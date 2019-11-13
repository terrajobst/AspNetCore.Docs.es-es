---
title: ASP.NET Core SignalR el hospedaje y el escalado de producción
author: bradygaster
description: Aprenda a evitar problemas de rendimiento y escalado en las aplicaciones que usan ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
no-loc:
- SignalR
uid: signalr/scale
ms.openlocfilehash: 7fc767939996a489174be949742637030924616d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963749"
---
# <a name="aspnet-core-opno-locsignalr-hosting-and-scaling"></a>Hospedaje y escalado de SignalR de ASP.NET Core

Por [Andrew Stanton-enfermera](https://twitter.com/anurse), [Brady transgastor](https://twitter.com/bradygaster)y [Tom Dykstra](https://github.com/tdykstra),

En este artículo se explican las consideraciones de hospedaje y escalado para las aplicaciones de alto tráfico que usan ASP.NET Core SignalR.

## <a name="sticky-sessions"></a>Sesiones permanentes

SignalR requiere que todas las solicitudes HTTP para una conexión específica se controlen mediante el mismo proceso de servidor. Cuando SignalR se está ejecutando en una granja de servidores (varios servidores), se deben usar "sesiones permanentes". En algunos equilibradores de carga también se denominan "sesiones permanentes". Azure App Service usa el [enrutamiento de solicitud de aplicaciones](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (arr) para enrutar las solicitudes. Al habilitar la configuración de "afinidad ARR" en el Azure App Service, se habilitarán las "sesiones permanentes". Las únicas circunstancias en las que no se requieren sesiones permanentes son:

1. Al hospedar en un solo servidor, en un único proceso.
1. Al usar el servicio Azure SignalR.
1. Cuando todos los clientes están configurados para usar **solo** WebSockets **y** el [valor SkipNegotiation](xref:signalr/configuration#configure-additional-options) está habilitado en la configuración del cliente.

En todas las demás circunstancias (incluso cuando se usa el backplane de Redis), el entorno de servidor debe configurarse para sesiones permanentes.

Para obtener instrucciones sobre la configuración de Azure App Service para SignalR, consulte <xref:signalr/publish-to-azure-web-app>.

## <a name="tcp-connection-resources"></a>Recursos de conexión TCP

El número de conexiones TCP simultáneas que un servidor web puede admitir es limitado. Los clientes HTTP estándar usan conexiones *efímeras* . Estas conexiones se pueden cerrar cuando el cliente se queda inactivo y se vuelve a abrir posteriormente. Por otro lado, una conexión SignalR es *persistente*. SignalR conexiones permanecen abiertas incluso cuando el cliente se queda inactivo. En una aplicación de alto tráfico que sirve a muchos clientes, estas conexiones persistentes pueden hacer que los servidores alcancen el número máximo de conexiones.

Las conexiones persistentes también consumen memoria adicional, para realizar un seguimiento de cada conexión.

El uso intensivo de los recursos relacionados con la conexión por SignalR puede afectar a otras aplicaciones web que se hospedan en el mismo servidor. Cuando SignalR se abre y contiene las últimas conexiones TCP disponibles, otras aplicaciones web en el mismo servidor tampoco tienen más conexiones disponibles.

Si un servidor se queda sin conexiones, verá errores de socket aleatorios y errores de restablecimiento de conexión. Por ejemplo:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Para evitar que SignalR uso de recursos provoque errores en otras aplicaciones Web, ejecute SignalR en servidores diferentes a los de otras aplicaciones Web.

Para evitar que SignalR uso de recursos provoque errores en una aplicación SignalR, escale horizontalmente para limitar el número de conexiones que un servidor tiene que controlar.

## <a name="scale-out"></a>Escalar

Una aplicación que usa SignalR debe realizar un seguimiento de todas sus conexiones, lo que crea problemas para una granja de servidores. Agregue un servidor y obtenga las nuevas conexiones que los otros servidores no conocen. Por ejemplo, SignalR en cada servidor del diagrama siguiente no es consciente de las conexiones en los otros servidores. Cuando SignalR en uno de los servidores desea enviar un mensaje a todos los clientes, el mensaje solo se dirige a los clientes conectados a ese servidor.

![Escalado [! Operador. NO-LOC (Signalr)] sin un backplane](scale/_static/scale-no-backplane.png)

Las opciones para resolver este problema son el [servicio Azure SignalR](#azure-signalr-service) y el [backplane Redis](#redis-backplane).

## <a name="azure-opno-locsignalr-service"></a>Servicio Azure SignalR

El servicio Azure SignalR es un proxy en lugar de un plano posterior. Cada vez que un cliente inicia una conexión al servidor, el cliente se redirige para conectarse al servicio. Ese proceso se ilustra en el diagrama siguiente:

![Establecer una conexión con Azure [! Operador. Servicio NO-LOC (Signalr)]](scale/_static/azure-signalr-service-one-connection.png)

El resultado es que el servicio administra todas las conexiones de cliente, mientras que cada servidor solo necesita un pequeño número constante de conexiones al servicio, tal y como se muestra en el diagrama siguiente:

![Clientes conectados al servicio, servidores conectados al servicio](scale/_static/azure-signalr-service-multiple-connections.png)

Este enfoque para el escalado horizontal tiene varias ventajas sobre la alternativa de backplane de Redis:

* Las sesiones permanentes, también conocidas como [afinidad de cliente](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), no son necesarias, ya que los clientes se redirigen inmediatamente al servicio Azure SignalR cuando se conectan.
* Una aplicación SignalR se puede escalar horizontalmente en función del número de mensajes enviados, mientras que el servicio Azure SignalR se escala automáticamente para controlar cualquier número de conexiones. Por ejemplo, puede haber miles de clientes, pero si solo se envían unos pocos mensajes por segundo, la aplicación SignalR no necesitará escalar horizontalmente a varios servidores solo para administrar las propias conexiones.
* Una aplicación SignalR no usará significativamente más recursos de conexión que una aplicación web sin SignalR.

Por estas razones, se recomienda el servicio Azure SignalR para todas las aplicaciones ASP.NET Core SignalR hospedadas en Azure, incluidas App Service, máquinas virtuales y contenedores.

Para obtener más información, consulte la [documentación del servicio Azure SignalR](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Backplane de Redis

[Redis](https://redis.io/) es un almacén de clave-valor en memoria que admite un sistema de mensajería con un modelo de publicación/suscripción. El SignalR backplane de Redis usa la característica pub/sub para reenviar mensajes a otros servidores. Cuando un cliente realiza una conexión, la información de conexión se pasa al backplane. Cuando un servidor desea enviar un mensaje a todos los clientes, envía al plano posterior. El backplane sabe todos los clientes conectados y los servidores en los que se encuentran. Envía el mensaje a todos los clientes a través de sus servidores respectivos. Este proceso se ilustra en el diagrama siguiente:

![Backplane de Redis, mensaje enviado desde un servidor a todos los clientes](scale/_static/redis-backplane.png)

El backplane de Redis es el enfoque de escalado horizontal recomendado para las aplicaciones hospedadas en su propia infraestructura. El servicio Azure SignalR no es una opción práctica para el uso de producción con aplicaciones locales debido a la latencia de conexión entre su centro de datos y un centro de datos de Azure.

Las ventajas del servicio Azure SignalR que se han indicado anteriormente son inconvenientes para el backplane de Redis:

* Las sesiones permanentes, también conocidas como [afinidad de cliente](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), son necesarias. Una vez que se inicia una conexión en un servidor, la conexión tiene que permanecer en ese servidor.
* Una aplicación SignalR debe escalarse horizontalmente en función del número de clientes, incluso si se envían pocos mensajes.
* Una aplicación SignalR usa significativamente más recursos de conexión que una aplicación web sin SignalR.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Documentación del servicio Azure SignalR](/azure/azure-signalr/signalr-overview)
* [Configuración de un backplane de Redis](xref:signalr/redis-backplane)
