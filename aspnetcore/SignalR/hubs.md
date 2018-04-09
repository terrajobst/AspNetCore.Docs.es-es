---
title: Concentradores de uso de núcleo de ASP.NET SignalR
author: rachelappel
description: Obtenga información acerca de cómo usar los centros de ASP.NET Core SignalR.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Use los concentradores de SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)

## <a name="what-is-a-signalr-hub"></a>¿Qué es un concentrador SignalR

La API de concentradores de SignalR permite llamar a métodos en los clientes conectados desde el servidor. En el código del servidor, se definen métodos llamados por el cliente. En el código de cliente, se definen métodos que se llaman desde el servidor. SignalR se encarga de todo el contenido en segundo plano que permite las comunicaciones de cliente a servidor y cliente de servidor en tiempo real.

## <a name="configure-signalr-hubs"></a>Configurar los concentradores SignalR

El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

Al agregar la funcionalidad de SignalR a una aplicación de ASP.NET Core, el programa de instalación SignalR rutas mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a>Crear y usar los centros

Crear un concentrador mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos. Los clientes pueden llamar a métodos que se definen como `public`.

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#. SignalR controla la serialización y deserialización de objetos complejos y matrices en los parámetros y valores devueltos.

## <a name="the-clients-object"></a>El objeto de los clientes

Cada instancia de la `Hub` clase tiene una propiedad denominada `Clients` que contiene los miembros siguientes para la comunicación entre cliente y servidor:

| Property | Descripción |
| ------ | ----------- |
| `All` | Llama a un método en todos los clientes conectados |
| `Caller` | Llama a un método en el cliente que invoca el método de concentrador |
| `Others` | Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método |

Además, la `Hub` clase contiene los métodos siguientes:

| Método | Descripción |
| ------ | ----------- |
| `AllExcept` | Llama a un método en todos los clientes conectados, excepto para las conexiones especificadas |
| `Client` | Llama a un método en un cliente conectado específico |
| `Clients` | Llama a un método en los clientes conectados específicos |
| `Group` | Envía un mensaje a todas las conexiones en el grupo especificado  |
| `GroupExcept` | Envía un mensaje a todas las conexiones en el grupo especificado, excepto las conexiones especificadas |
| `Groups` | Envía un mensaje a varios grupos de conexiones  |
| `OthersInGroup` | Envía un mensaje a un grupo de conexiones, excepto al cliente que invoca el método de concentrador  |
| `User` | Envía un mensaje a todas las conexiones asociadas a un usuario específico |
| `Users` | Envía un mensaje a todas las conexiones asociadas a los usuarios especificados |

Cada propiedad o método de las tablas anteriores devuelve un objeto con un `SendAsync` método. El `SendAsync` método le permite especificar el nombre y los parámetros del método de cliente para llamar a.

## <a name="send-messages-to-clients"></a>Enviar mensajes a los clientes

Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto. En la siguiente en el ejemplo siguiente, la `SendMessageToCaller` método muestra cómo enviar un mensaje a la conexión que se invoca el método de concentrador. El `SendMessageToGroups` método envía un mensaje a los grupos almacenados en un `List` denominado `groups`.

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Controlar eventos de una conexión

La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones. Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a>Controlar errores

Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método. En el cliente de JavaScript, el `invoke` método devuelve un [compromiso de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y se pasan como JavaScript `Error` objeto.

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a>Recursos relacionados

[Introducción a ASP.NET Core SignalR](xref:signalr/introduction)