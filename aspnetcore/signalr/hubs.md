---
title: Usar concentradores en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo usar concentradores en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 244ddc40e647bfcc3ca8cda2797c51bc49174822
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320152"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Usar concentradores de SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>¿Qué es un concentrador SignalR

La API de concentradores de SignalR le permite llamar a métodos en los clientes conectados desde el servidor. En el código del servidor, se definen métodos llamados por el cliente. En el código de cliente, se definen los métodos que se llaman desde el servidor. SignalR se ocupa de todo en segundo plano que hace posible la comunicación de servidor de cliente y servidor a cliente en tiempo real.

## <a name="configure-signalr-hubs"></a>Configurar los concentradores de SignalR

El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Al agregar la funcionalidad de SignalR a una aplicación ASP.NET Core, configurar las rutas de SignalR mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Crear y usar concentradores

Crear un centro mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos. Los clientes pueden llamar a métodos que se definen como `public`.

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#. SignalR controla la serialización y deserialización de objetos complejos y matrices en sus parámetros y valores devueltos.

> [!NOTE]
> Los concentradores son transitorios:
>
> * No almacene el estado en una propiedad de la clase de concentrador. Cada llamada al método de concentrador se ejecuta en una nueva instancia de concentrador.  
> * Use `await` al llamar a métodos asincrónicos que dependen del concentrador permanecer activo. Por ejemplo, un método como `Clients.All.SendAsync(...)` puede producir un error si se llama sin `await` y el método de concentrador se completa antes de `SendAsync` finaliza.

## <a name="the-context-object"></a>El objeto de contexto

El `Hub` clase tiene un `Context` propiedad que contiene las siguientes propiedades con información sobre la conexión:

| Property | Descripción |
| ------ | ----------- |
| `ConnectionId` | Obtiene el identificador único para la conexión, asignada por SignalR. Hay un identificador de conexión para cada conexión.|
| `UserIdentifier` | Obtiene el [useridentifier](xref:signalr/groups). De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario. |
| `User` | Obtiene el `ClaimsPrincipal` asociada con el usuario actual. |
| `Items` | Obtiene una colección de pares clave-valor que se puede usar para compartir datos dentro del ámbito de esta conexión. Se pueden almacenar datos en esta colección y conservará para la conexión entre las invocaciones de método de concentrador. |
| `Features` | Obtiene la colección de las características disponibles en la conexión. Por ahora, esta colección no es necesario en la mayoría de los escenarios, por lo que no se documentan detalladamente todavía. |
| `ConnectionAborted` | Obtiene un `CancellationToken` que le notifica cuando se anula la conexión. |

`Hub.Context` También contiene los métodos siguientes:

| Método | Descripción |
| ------ | ----------- |
| `GetHttpContext` | Devuelve el `HttpContext` para la conexión, o `null` si la conexión no está asociada a una solicitud HTTP. Para las conexiones HTTP, puede usar este método para obtener información como los encabezados HTTP y las cadenas de consulta. |
| `Abort` | Anula la conexión. |

## <a name="the-clients-object"></a>El objeto de los clientes

El `Hub` clase tiene un `Clients` propiedad que contiene las siguientes propiedades para la comunicación entre cliente y servidor:

| Property | Descripción |
| ------ | ----------- |
| `All` | Llama a un método en todos los clientes conectados |
| `Caller` | Llama a un método en el cliente que invoca el método de concentrador |
| `Others` | Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método |

`Hub.Clients` También contiene los métodos siguientes:

| Método | Descripción |
| ------ | ----------- |
| `AllExcept` | Llama a un método en todos los clientes conectados, excepto las conexiones especificadas |
| `Client` | Llama a un método en un cliente específico conectado |
| `Clients` | Llame a un método específicos clientes conectados |
| `Group` | Llama a un método en todas las conexiones del grupo especificado  |
| `GroupExcept` | Llama a un método en todas las conexiones en el grupo especificado, excepto las conexiones especificadas |
| `Groups` | Llama a un método en varios grupos de conexiones  |
| `OthersInGroup` | Llama a un método en un grupo de conexiones, excepto al cliente que invoca el método de concentrador  |
| `User` | Llama a un método en todas las conexiones asociadas a un usuario específico |
| `Users` | Llama a un método en todas las conexiones asociadas a los usuarios especificados |

Cada propiedad o método en las tablas anteriores devuelve un objeto con un `SendAsync` método. El `SendAsync` método permite proporcionar el nombre y los parámetros del método para llamar al cliente.

## <a name="send-messages-to-clients"></a>Enviar mensajes a los clientes

Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto. En el ejemplo siguiente, hay tres métodos de concentrador:

* `SendMessage` envía un mensaje a todos los clientes conectados, mediante `Clients.All`.
* `SendMessageToCaller` envía un mensaje de vuelta al llamador, mediante `Clients.Caller`.
* `SendMessageToGroups` envía un mensaje a todos los clientes de la `SignalR Users` grupo.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>Concentradores fuertemente tipados

Una desventaja de utilizar `SendAsync` es que se basa en una cadena mágica para especificar que se llame al método de cliente. Esto deja el código abierto a los errores de tiempo de ejecución si el nombre del método está mal escrito o no aparece en el cliente.

Una alternativa al uso `SendAsync` es asignar rigurosamente los `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub`1>. En el ejemplo siguiente, la `ChatHub` métodos de cliente se han extraído alejar en una interfaz denominada `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Esta interfaz puede usarse para refactorizar anterior `ChatHub` ejemplo.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

Uso de `Hub<IChatClient>` habilita la comprobación de tiempo de compilación de los métodos de cliente. Esto evita problemas causados por usar cadenas mágicas, ya que `Hub<T>` solo puede proporcionar acceso a los métodos definidos en la interfaz.

Usar fuertemente tipado `Hub<T>` deshabilita la posibilidad de usar `SendAsync`. Cualquier método definido en la interfaz todavía se puede definir como asincrónicas. De hecho, cada uno de estos métodos debe devolver un `Task`. Puesto que es una interfaz, no use el `async` palabra clave. Por ejemplo:

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> El `Async` sufijo no se quitará el nombre del método. A menos que se define el método de cliente con `.on('MyMethodAsync')`, no debe usar `MyMethodAsync` como un nombre.

## <a name="change-the-name-of-a-hub-method"></a>Cambiar el nombre de un método de concentrador

De forma predeterminada, un nombre de método de concentrador de servidor es el nombre del método. NET. Sin embargo, puede usar el [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) atributo para cambiar este comportamiento predeterminado y especificar manualmente un nombre para el método. El cliente debería utilizar este nombre, en lugar del nombre de método. NET, al invocar el método.

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>Controlar los eventos de una conexión

La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones. Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

Invalidar el `OnDisconnectedAsync` método virtual para realizar acciones cuando un cliente se desconecta. Si el cliente se desconecta de forma intencionada (mediante una llamada a `connection.stop()`, por ejemplo), el `exception` parámetro será `null`. Sin embargo, si el cliente se desconecta debido a un error (por ejemplo, un error de red), el `exception` parámetro contendrá una excepción que describe el error.

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>Control de errores

Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método. En el cliente de JavaScript, el `invoke` método devuelve un [promesa de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y pasado como un JavaScript `Error` objeto.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

Si su centro, produce una excepción, no se cierran las conexiones. De forma predeterminada, SignalR devuelve un mensaje de error genérico al cliente. Por ejemplo:

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

Excepciones inesperadas suelen contengan información confidencial, como el nombre de un servidor de base de datos en una excepción que se desencadena cuando se produce un error en la conexión de base de datos. SignalR no expone estos mensajes de error detallados de forma predeterminada como medida de seguridad. Consulte la [artículo de consideraciones de seguridad](xref:signalr/security#exceptions) para obtener más información sobre por qué se suprimen los detalles de la excepción.

Si tiene una excepcional de condición *hacer* desea propagar al cliente, puede usar el `HubException` clase. Si produce un `HubException` desde su método de concentrador SignalR **le** enviar todo el mensaje al cliente, sin modificar.

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR solo envía el `Message` propiedad de la excepción al cliente. El seguimiento de pila y otras propiedades en la excepción no están disponibles para el cliente.

## <a name="related-resources"></a>Recursos relacionados

* [Introducción a ASP.NET Core SignalR](xref:signalr/introduction)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
