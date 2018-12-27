---
title: Introducción a ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo la biblioteca de ASP.NET Core SignalR simplifica la adición de funcionalidad en tiempo real a las aplicaciones.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: da18837c690d2182589db5f486ae74e537ade931
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425151"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introducción a ASP.NET Core SignalR

## <a name="what-is-signalr"></a>¿Qué es SignalR?

ASP.NET Core SignalR es una biblioteca de código abierto que simplifica agregar la funcionalidad a las aplicaciones web en tiempo real. La funcionalidad web en tiempo real permite que el código de servidor para insertar contenido a los clientes al instante.

Buenos candidatos para SignalR:

* Aplicaciones que requieren actualizaciones de alta frecuencia desde el servidor. Algunos ejemplos son juegos, redes sociales, votación, subastas, mapas y aplicaciones GPS.
* Los paneles y aplicaciones de supervisión. Los ejemplos incluyen paneles empresariales, actualizaciones de venta instantáneas o alertas de viaje.
* Aplicaciones de colaboración. Las aplicaciones de pizarra y software de reuniones de equipo son ejemplos de aplicaciones de colaboración.
* Aplicaciones que requieren las notificaciones. Redes sociales, correo electrónico, chat, juegos, alertas de viaje y muchas otras aplicaciones utilizan notificaciones.

SignalR proporciona una API para crear el servidor a cliente [llamadas a procedimiento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Las RPC llamar a funciones de JavaScript en los clientes desde el código de .NET Core en el servidor.

Estas son algunas características de SignalR para ASP.NET Core:

* Controla automáticamente la administración de conexiones.
* Envía mensajes a todos los clientes conectados al mismo tiempo. Por ejemplo, un salón de chat.
* Envía mensajes a clientes específicos o grupos de clientes.
* Se puede ampliar para controlar el creciente tráfico.

El origen se hospeda en un [SignalR repositorio de GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Transportes

SignalR admite varias técnicas para controlar las comunicaciones en tiempo real:

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Eventos enviados por el servidor
* Sondeo largo

SignalR elige automáticamente el mejor método de transporte que se encuentra dentro de las capacidades del servidor y cliente.

## <a name="hubs"></a>Concentradores

SignalR usa *hubs* para la comunicación entre clientes y servidores.

Un concentrador es una canalización de alto nivel que permite que un cliente y servidor llamar a métodos entre sí. SignalR controla el envío a través de los límites del equipo automáticamente, permitir que los clientes llamar a métodos en el servidor y viceversa. Puede pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos. SignalR proporciona dos protocolos de concentrador integrada: un protocolo de texto basado en JSON y un protocolo binario según [MessagePack](https://msgpack.org/).  MessagePack generalmente crea mensajes más pequeños en comparación con JSON. Deben ser compatible con los exploradores más antiguos [nivel XHR 2](https://caniuse.com/#feat=xhr2) para proporcionar compatibilidad con el protocolo MessagePack.

Centros de llamar a código de cliente mediante el envío de mensajes que contienen el nombre y los parámetros del método del lado cliente. Se deserializan los objetos que se envía como parámetros de método mediante el protocolo configurado. El cliente intenta hacer coincidir el nombre a un método en el código del lado cliente. Cuando el cliente encuentra una coincidencia, llama al método y le pasa los datos deserializados.

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a SignalR para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
