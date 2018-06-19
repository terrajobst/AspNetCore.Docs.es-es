---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Introducción a la ampliación de SignalR 1.x | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: ee3384046bf8a0f363aa6801d7a46f68b2bf125a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043751"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Introducción a la ampliación de SignalR 1.x
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

En general, hay dos formas de escalar una aplicación web: *escalar verticalmente* y *escalar horizontalmente*.

- Escalar verticalmente mediante un servidor más grande (o una máquina virtual mayor) con más de RAM, CPU, etcetera de medios.
- Escalar horizontalmente significa agregar más servidores para administrar la carga.

El problema con el escalado es que rápidamente se alcanza un límite en el tamaño de la máquina. Además, deberá escalar horizontalmente. Sin embargo, al escalar horizontalmente, los clientes pueden obtener enrutar a diferentes servidores. Un cliente que está conectado a un servidor no recibirá mensajes enviados desde otro servidor.

![](scaleout-in-signalr/_static/image1.png)

Una solución consiste en reenviar los mensajes entre servidores, mediante un componente denominado un *backplane*. Con un backplane habilitada, cada instancia de la aplicación envía mensajes al backplane y backplane reenvía a las demás instancias de aplicación. (En electrónica, un backplane es un grupo de conectores paralelos. Por analogía, un backplane de SignalR conecta varios servidores.)

![](scaleout-in-signalr/_static/image2.png)

SignalR proporciona actualmente tres paneles posteriores:

- **Bus de servicio de Azure**. Bus de servicio es una infraestructura de mensajería que permite que los componentes enviar mensajes en modo de correspondencia imprecisa.
- **Redis**. Redis es un almacén de clave y valor en memoria. Redis admite un patrón de publicación/suscripción ("pub/sub") para enviar mensajes.
- **SQL Server**. El backplane de SQL Server escribe mensajes en tablas SQL. El backplane utiliza a Service Broker para la mensajería eficaz. Sin embargo, también funciona si Service Broker no está habilitado.

Si implementa la aplicación en Azure, considere el uso de la placa de Bus de servicio de Azure. Si va a implementar en su propio conjunto de servidores, considere la posibilidad de SQL Server o planos posteriores de Redis.

Los siguientes temas contienen tutoriales paso a paso para cada backplane:

- [Escalabilidad horizontal de SignalR con Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Escalabilidad horizontal de SignalR con Redis](scaleout-with-redis.md)
- [Escalabilidad horizontal de SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementación

En SignalR, cada mensaje se envía a través de un bus de mensajes. Implementa un bus de mensajes la [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaz, que proporciona una abstracción de publicación/suscripción. Los paneles posteriores de trabajo si se reemplaza el valor predeterminado **IMessageBus** con un bus, diseñado para ese backplane. Por ejemplo, el bus de mensajes para Redis es [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), y utiliza Redis [pub/sub](http://redis.io/topics/pubsub) mecanismo para enviar y recibir mensajes.

Cada instancia del servidor se conecta al backplane a través del bus. Cuando se envía un mensaje, entra en el plano posterior y el backplane lo envía a todos los servidores. Cuando un servidor recibe un mensaje del backplane, coloca el mensaje en su memoria caché local. El servidor, a continuación, envía mensajes a los clientes desde su caché local.

Para cada conexión de cliente, progreso del cliente en la secuencia de mensajes de lectura se realiza un seguimiento mediante un cursor. (Un cursor representa una posición en la secuencia de mensajes). Si un cliente se desconecta y, a continuación, se vuelve a conectar, solicita el bus de los mensajes que han llegado después del valor de cursor del cliente. Lo mismo ocurre cuando se utiliza una conexión [sondeo largo](../getting-started/introduction-to-signalr.md#transports). Una vez completada una solicitud de sondeo largo, el cliente abre una nueva conexión y pide a los mensajes recibidos después del cursor.

El funcionamiento de mecanismo de cursor incluso si un cliente se enruta a un servidor diferente en volver a conectar. El backplane es consciente de todos los servidores y no importa qué servidor se conecta un cliente.

## <a name="limitations"></a>Limitaciones

Con un backplane, el rendimiento máximo del mensaje es menor que lo es cuando los clientes comunicarse directamente con un solo nodo de servidor. Eso es porque el backplane reenvía todos los mensajes para cada nodo, por lo que el backplane puede convertirse en un cuello de botella. Si esta limitación es un problema depende de la aplicación. Por ejemplo, estos son algunos escenarios típicos de SignalR:

- [Difusión de servidor](tutorial-server-broadcast-with-aspnet-signalr.md) (p. ej., cotizaciones): planos posteriores funcionan bien para este escenario, debido a que el servidor controla la velocidad a la que se envían mensajes.
- [Cliente a](tutorial-getting-started-with-signalr.md) (p. ej., chat): en este escenario, el plano posterior puede ser un cuello de botella si ajusta el número de mensajes con el número de clientes; es decir, si la tasa de mensajes aumenta proporcionalmente como más los clientes se unen.
- [En tiempo real de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md) (por ejemplo, juegos en tiempo real): no se recomienda un backplane para este escenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Habilitar el seguimiento de ampliación de SignalR

Para habilitar el seguimiento de los planos posteriores, agregue las siguientes secciones en el archivo web.config, bajo la raíz de **configuración** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
