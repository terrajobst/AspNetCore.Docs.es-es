---
title: Usar concentradores en ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo usar concentradores en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095286"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Usar concentradores de SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>¿Qué es un concentrador SignalR

La API de concentradores de SignalR le permite llamar a métodos en los clientes conectados desde el servidor. En el código del servidor, se definen métodos llamados por el cliente. En el código de cliente, se definen los métodos que se llaman desde el servidor. SignalR se ocupa de todo en segundo plano que hace posible la comunicación de servidor de cliente y servidor a cliente en tiempo real.

## <a name="configure-signalr-hubs"></a>Configurar los concentradores de SignalR

El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Al agregar la funcionalidad de SignalR a una aplicación ASP.NET Core, configurar las rutas de SignalR mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Crear y usar concentradores

Crear un centro mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos. Los clientes pueden llamar a métodos que se definen como `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#. SignalR controla la serialización y deserialización de objetos complejos y matrices en sus parámetros y valores devueltos.

## <a name="the-clients-object"></a>El objeto de los clientes

Cada instancia de la `Hub` clase tiene una propiedad denominada `Clients` que contiene los siguientes miembros para la comunicación entre cliente y servidor:

| Property | Descripción |
| ------ | ----------- |
| `All` | Llama a un método en todos los clientes conectados |
| `Caller` | Llama a un método en el cliente que invoca el método de concentrador |
| `Others` | Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método |


Además, `Hub.Clients` contiene los siguientes métodos:

| Método | Descripción |
| ------ | ----------- |
| `AllExcept` | Llama a un método en todos los clientes conectados, excepto las conexiones especificadas |
| `Client` | Llama a un método en un cliente específico conectado |
| `Clients` | Llame a un método específicos clientes conectados |
| `Group` | Llama a un método a todas las conexiones del grupo especificado  |
| `GroupExcept` | Llama a un método a todas las conexiones en el grupo especificado, excepto las conexiones especificadas |
| `Groups` | Llama a un método a varios grupos de conexiones  |
| `OthersInGroup` | Llama a un método a un grupo de conexiones, excepto al cliente que invoca el método de concentrador  |
| `User` | Llama a un método para todas las conexiones asociadas a un usuario específico |
| `Users` | Llama a un método para todas las conexiones asociadas a los usuarios especificados |

Cada propiedad o método en las tablas anteriores devuelve un objeto con un `SendAsync` método. El `SendAsync` método permite proporcionar el nombre y los parámetros del método para llamar al cliente.

## <a name="send-messages-to-clients"></a>Enviar mensajes a los clientes

Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto. En el ejemplo siguiente, la `SendMessageToCaller` método muestra cómo enviar un mensaje a la conexión que invoca el método de concentrador. El `SendMessageToGroups` método envía un mensaje a los grupos que se almacenan en un `List` denominado `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Controlar los eventos de una conexión

La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones. Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Controlar los errores

Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método. En el cliente de JavaScript, el `invoke` método devuelve un [promesa de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y pasado como un JavaScript `Error` objeto.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Recursos relacionados

* [Introducción a ASP.NET Core SignalR](xref:signalr/introduction)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
