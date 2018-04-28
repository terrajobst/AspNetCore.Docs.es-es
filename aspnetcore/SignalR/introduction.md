---
title: Introducción a ASP.NET Core SignalR
author: rachelappel
description: Obtenga información acerca de cómo la biblioteca ASP.NET Core SignalR simplifica la adición de funcionalidad en tiempo real a las aplicaciones.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: 190dfe9eac95be646b458870ac4ee95f681f45d7
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introducción a ASP.NET Core SignalR

Por [Rachel Appel](https://twitter.com/rachelappel)


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a>¿Qué es SignalR?

Núcleo de ASP.NET SignalR es una biblioteca que simplifica la agregación de funcionalidades de web en tiempo real a las aplicaciones. La funcionalidad web en tiempo real permite que el código de servidor para el contenido de inserción a los clientes al instante.

Buenos candidatos para SignalR:

* Aplicaciones que requieren alta frecuencia actualizaciones desde el servidor. Se trata de juegos, redes sociales, votar, subasta, mapas y aplicaciones GPS.
* Los paneles y supervisión de aplicaciones. Los ejemplos incluyen paneles de la empresa, las actualizaciones de ventas instantáneas, o alertas de viaje.
* Aplicaciones de colaboración. Aplicaciones de pizarra y reunión software de equipo son ejemplos de aplicaciones de colaboración.
* Aplicaciones que requieren las notificaciones. Las redes sociales, correo electrónico, chat, juegos, alertas de viaje y muchas otras aplicaciones utilizan notificaciones.

SignalR proporciona una API para la creación de servidor a cliente [llamadas a procedimiento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Las RPC llamar a funciones de JavaScript en los clientes desde el código de .NET Core en el servidor de.

SignalR principales de ASP.NET:

* Controla automáticamente la administración de conexiones.
* Permite difundir mensajes a todos los clientes conectados al mismo tiempo. Por ejemplo, un salón de chat.
* Habilita el envío de mensajes a clientes específicos o grupos de clientes.
* Es código abierto en [GitHub](https://github.com/aspnet/signalr).
* Escalable.

La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP.

## <a name="transports"></a>Transportes

Resúmenes de SignalR a través de una serie de técnicas para crear aplicaciones web en tiempo real. [WebSockets](https://tools.ietf.org/html/rfc7118) es el transporte óptimo, pero se pueden usar otras técnicas como eventos de Server-Sent y sondeo largo cuando los que no están disponibles. SignalR detectará automáticamente e inicializar el transporte adecuado en función de las características admitidas en el servidor y el cliente.

## <a name="hubs"></a>Concentradores

SignalR usa centros para la comunicación entre clientes y servidores.

Un concentrador es una canalización de alto nivel que permite que el cliente y servidor para llamar a métodos en entre sí. SignalR administra el envío a través de los límites del equipo automáticamente, permitiendo a los clientes llamar a métodos en el servidor como fácilmente como métodos locales y viceversa. Los concentradores permiten pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos. SignalR proporciona dos protocolos de concentrador integrada: un protocolo de texto en función de JSON y un protocolo binario basado en [MessagePack](https://msgpack.org/).  Por lo general, MessagePack crea mensajes más pequeños que cuando se usan JSON. Deben ser compatible con los exploradores más antiguos [nivel XHR 2](https://caniuse.com/#feat=xhr2) para proporcionar compatibilidad con el protocolo MessagePack.

Los centros de llamar a código de cliente mediante el envío de mensajes mediante el transporte activo. Los mensajes contienen el nombre y los parámetros del método de cliente. Objetos que se envíen como parámetros de método se deserializan mediante el protocolo configurado. El cliente intenta hacer coincidir el nombre a un método en el código de cliente. Cuando se produce una coincidencia, el método de cliente se ejecuta con los datos del parámetro deserializado.

## <a name="additional-resources"></a>Recursos adicionales

* [Empezar a trabajar con SignalR para ASP.NET Core](xref:signalr/get-started)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
