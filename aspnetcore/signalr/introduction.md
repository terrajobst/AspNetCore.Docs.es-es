---
title: Introducción a ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo la biblioteca de SignalR de ASP.NET Core simplifica la adición de funcionalidad en tiempo real a las aplicaciones.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 7108d9f223db78937dd1203a1cb4b890006b20ec
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963938"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a>Introducción a ASP.NET Core SignalR

## <a name="what-is-opno-locsignalr"></a>¿Qué es SignalR?

ASP.NET Core SignalR es una biblioteca de código abierto que simplifica la incorporación de funcionalidades Web en tiempo real a las aplicaciones. La funcionalidad web en tiempo real permite que el código del lado servidor Inserte contenido a los clientes al instante.

Buenos candidatos para SignalR:

* Aplicaciones que requieren actualizaciones de alta frecuencia del servidor. Algunos ejemplos son las aplicaciones de juegos, redes sociales, votación, subasta, mapas y GPS.
* Paneles y aplicaciones de supervisión. Algunos ejemplos son los paneles de la empresa, las actualizaciones de ventas instantáneas o las alertas de viaje.
* Aplicaciones de colaboración. Las aplicaciones de pizarra y el software de reunión de equipo son ejemplos de aplicaciones de colaboración.
* Aplicaciones que requieren notificaciones. Las redes sociales, el correo electrónico, el chat, los juegos, las alertas de viajes y muchas otras aplicaciones usan notificaciones.

SignalR proporciona una API para crear [llamadas a procedimiento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)de servidor a cliente. Las RPC llaman a funciones de JavaScript en los clientes desde el código de .NET Core del lado servidor.

Estas son algunas características de SignalR para ASP.NET Core:

* Controla la administración de conexiones automáticamente.
* Envía mensajes a todos los clientes conectados simultáneamente. Por ejemplo, un salón de chat.
* Envía mensajes a clientes o grupos de clientes específicos.
* Escala para controlar el aumento del tráfico.

El origen se hospeda en un [repositorio deSignalR en github](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Transportes

SignalR admite varias técnicas para controlar las comunicaciones en tiempo real:

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Eventos enviados por el servidor
* Sondeo largo

SignalR elige automáticamente el mejor método de transporte que se encuentra dentro de las capacidades del servidor y del cliente.

## <a name="hubs"></a>Concentradores

SignalR usa *hubs* para la comunicación entre clientes y servidores.

Un concentrador es una canalización de alto nivel que permite a un cliente y un servidor llamar a métodos entre sí. SignalR controla el envío automático de los límites de la máquina, lo que permite a los clientes llamar a métodos en el servidor y viceversa. Puede pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos. SignalR proporciona dos protocolos de concentrador integrados: un protocolo de texto basado en JSON y un protocolo binario basado en [MessagePack](https://msgpack.org/).  MessagePack suele crear mensajes más pequeños en comparación con JSON. Los exploradores más antiguos deben admitir el [nivel 2 de XHR](https://caniuse.com/#feat=xhr2) para proporcionar compatibilidad con el protocolo MessagePack.

Los concentradores llaman a código de cliente mediante el envío de mensajes que contienen el nombre y los parámetros del método del lado cliente. Los objetos enviados como parámetros de método se deserializan mediante el protocolo configurado. El cliente intenta hacer coincidir el nombre con un método en el código del lado cliente. Cuando el cliente encuentra una coincidencia, llama al método y le pasa los datos del parámetro deserializado.

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a SignalR para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
