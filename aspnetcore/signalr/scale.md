---
title: Hospedaje de producción de ASP.NET Core SignalR y escalado
author: bradygaster
description: Obtenga información sobre cómo evitar el rendimiento y escalado de problemas en las aplicaciones que usan ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836927"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>Hospedaje de ASP.NET Core SignalR y escalado

Por [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), y [Tom Dykstra](https://github.com/tdykstra),

En este artículo se explica de hospedaje y la escala para aplicaciones de alto tráfico que usan ASP.NET Core SignalR.

## <a name="tcp-connection-resources"></a>Recursos de la conexión TCP

Se limita el número de conexiones TCP simultáneas que puede admitir un servidor web. Usan los clientes HTTP estándares *efímero* conexiones. Cuando el cliente pasa a estado inactivo y se vuelve a abrir más adelante, se pueden cerrar estas conexiones. Por otro lado, es una conexión SignalR *persistente*. Las conexiones de SignalR permanecen abierta, incluso cuando el cliente pasa a estado inactivo. En una aplicación con mucho tráfico que sirve a muchos clientes, estas conexiones persistentes pueden provocar que los servidores alcance su número máximo de conexiones.

Las conexiones persistentes también consumen memoria adicional, para realizar un seguimiento de cada conexión.

Un uso intensivo de recursos relacionados con la conexión signalr puede afectar a otras aplicaciones web que se hospedan en el mismo servidor. Cuando SignalR se abre y contiene el último conexiones TCP disponibles, otras aplicaciones web en el mismo servidor también tienen no hay más conexiones disponibles para ellos.

Si un servidor se ejecuta fuera de las conexiones, verá errores aleatorios de socket y errores de restablecimiento de conexión. Por ejemplo:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Para evitar el uso de recursos de SignalR causando errores en otras aplicaciones web, ejecute SignalR en distintos servidores de las otras aplicaciones web.

Para evitar el uso de recursos de SignalR causando errores en una aplicación de SignalR, escale horizontalmente para limitar el número de conexiones de que un servidor tiene que administrar.

## <a name="scale-out"></a>Escalar

Una aplicación que usa SignalR necesita realizar un seguimiento de todas sus conexiones, lo que crea problemas de una granja de servidores. Agregar un servidor y obtiene nuevas conexiones que no conocen los demás servidores. Por ejemplo, no es consciente de las conexiones de los demás servidores SignalR en cada servidor en el diagrama siguiente. Cuando desea SignalR en uno de los servidores enviar un mensaje a todos los clientes, el mensaje sólo se dirige a los clientes conectados a ese servidor.

![El escalado sin un backplane SignalR](scale/_static/scale-no-backplane.png)

Las opciones para solucionar este problema son las [Azure SignalR Service](#azure-signalr-service) y [Redis backplane](#redis-backplane).

## <a name="azure-signalr-service"></a>Servicio Azure SignalR

Azure SignalR Service es un servidor proxy en lugar de como un backplane. Cada vez que un cliente inicia una conexión al servidor, se redirige el cliente para conectarse al servicio. Este proceso se ilustra en el diagrama siguiente:

![Establecer una conexión a Azure SignalR Service](scale/_static/azure-signalr-service-one-connection.png)

El resultado es que el servicio administra todas las conexiones de cliente, mientras que cada servidor necesita solo un pequeño número constante de las conexiones al servicio, tal como se muestra en el diagrama siguiente:

![Clientes conectados al servicio, los servidores conectados al servicio](scale/_static/azure-signalr-service-multiple-connections.png)

Este enfoque de escalado tiene varias ventajas respecto a la alternativa de backplane de Redis:

* Sesiones permanentes, también conocidas como [afinidad del cliente](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), no es necesario, porque los clientes se redirigen inmediatamente a Azure SignalR Service cuando se conectan.
* Una aplicación puede escalar horizontalmente de SignalR en función del número de mensajes enviados, mientras que el servicio Azure SignalR se escala automáticamente para administrar cualquier número de conexiones. Por ejemplo, podría haber miles de clientes, pero si solo se envían algunos mensajes por segundo, la aplicación de SignalR no tendrá que escalar horizontalmente a varios servidores para controlar las conexiones a sí mismos.
* Una aplicación de SignalR que no utilice muchos más recursos de conexión que una aplicación web sin SignalR.

Por estas razones, se recomienda Azure SignalR Service para todas las aplicaciones de ASP.NET Core SignalR hospedadas en Azure, como App Service, las máquinas virtuales y contenedores.

Para obtener más información, consulte el [documentación de Azure SignalR Service](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Backplane de Redis

[Redis](https://redis.io/) es un almacén de pares clave-valor en memoria que admite un sistema de mensajería con un modelo de publicación/suscripción. El backplane SignalR Redis usa la característica de pub/sub para reenviar los mensajes a otros servidores. Cuando un cliente realiza una conexión, la información de conexión se pasa al backplane. Cuando un servidor desea enviar un mensaje a todos los clientes, envía al backplane. El backplane sabe conectados todos los clientes y que los servidores que están incluidas en. Envía el mensaje a todos los clientes a través de sus respectivos servidores. Este proceso se ilustra en el diagrama siguiente:

![Backplane, mensaje enviado desde un servidor a todos los clientes de Redis](scale/_static/redis-backplane.png)

El backplane de Redis es el enfoque recomendado de escalabilidad horizontal para aplicaciones hospedadas en su propia infraestructura. Azure SignalR Service no es una opción práctica para su uso en producción con aplicaciones locales debido a la latencia de la conexión entre su centro de datos y un centro de datos de Azure.

Las ventajas de Azure SignalR Service se ha indicado anteriormente son las desventajas de la placa de Redis:

* Sesiones permanentes, también conocidas como [afinidad del cliente](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), es necesario. Una vez que se inicia una conexión en un servidor, la conexión debe permanecer en ese servidor.
* Una aplicación de SignalR debe escalar horizontalmente basada en el número de clientes, incluso si algunos de los mensajes se envíen.
* Una aplicación de SignalR usa significativamente más recursos de conexión que una aplicación web sin SignalR.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Documentación de Azure SignalR Service](/azure/azure-signalr/signalr-overview)
* [Configurar un backplane de Redis](xref:signalr/redis-backplane)
