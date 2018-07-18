---
title: Introducción a ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo la biblioteca de ASP.NET Core SignalR simplifica la adición de funcionalidad en tiempo real a las aplicaciones.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095394"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introducción a ASP.NET Core SignalR

Por [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>¿Qué es SignalR?

ASP.NET Core SignalR es una biblioteca que simplifica agregar la funcionalidad a las aplicaciones web en tiempo real. La funcionalidad web en tiempo real permite que el código de servidor para insertar contenido a los clientes al instante.

Buenos candidatos para SignalR:

* Aplicaciones que requieren actualizaciones de alta frecuencia desde el servidor. Algunos ejemplos son juegos, redes sociales, votación, subastas, mapas y aplicaciones GPS.
* Los paneles y aplicaciones de supervisión. Los ejemplos incluyen paneles empresariales, actualizaciones de venta instantáneas o alertas de viaje.
* Aplicaciones de colaboración. Las aplicaciones de pizarra y software de reuniones de equipo son ejemplos de aplicaciones de colaboración.
* Aplicaciones que requieren las notificaciones. Redes sociales, correo electrónico, chat, juegos, alertas de viaje y muchas otras aplicaciones utilizan notificaciones.

SignalR proporciona una API para crear el servidor a cliente [llamadas a procedimiento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Las RPC llamar a funciones de JavaScript en los clientes desde el código de .NET Core en el servidor.

SignalR para ASP.NET Core:

* Controla automáticamente la administración de conexiones.
* Permite difundir mensajes a todos los clientes conectados al mismo tiempo. Por ejemplo, un salón de chat.
* Habilita el envío de mensajes a clientes específicos o grupos de clientes.
* Está abierto en [GitHub](https://github.com/aspnet/signalr).
* Escalable.

La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP.

## <a name="transports"></a>Transportes

Resúmenes de SignalR a través de una serie de técnicas para crear aplicaciones web en tiempo real. [WebSockets](https://tools.ietf.org/html/rfc7118) es el transporte óptimo, pero se pueden usar otras técnicas, como los eventos y Long Polling cuando los que no están disponibles. SignalR detectará automáticamente e inicializar el transporte adecuado en función de las características admitidas en el servidor y cliente.

## <a name="hubs"></a>Concentradores

SignalR usa concentradores para la comunicación entre clientes y servidores.

Un concentrador es una canalización de alto nivel que permite que el cliente y servidor llamar a métodos entre sí. SignalR controla el envío a través de los límites del equipo automáticamente, permitir que los clientes llamar a métodos en el servidor como fácilmente como métodos locales y viceversa. Concentradores permiten pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos. SignalR proporciona dos protocolos de concentrador integrada: un protocolo de texto basado en JSON y un protocolo binario según [MessagePack](https://msgpack.org/).  MessagePack generalmente crea mensajes más pequeños que cuando se usan JSON. Deben ser compatible con los exploradores más antiguos [nivel XHR 2](https://caniuse.com/#feat=xhr2) para proporcionar compatibilidad con el protocolo MessagePack.

Concentradores de llamar a código de cliente mediante el envío de mensajes mediante el transporte activo. Los mensajes contienen el nombre y los parámetros del método del lado cliente. Se deserializan los objetos que se envía como parámetros de método mediante el protocolo configurado. El cliente intenta hacer coincidir el nombre a un método en el código del lado cliente. Cuando se produce una coincidencia, el método de cliente se ejecuta con los datos deserializados.

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a SignalR para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
