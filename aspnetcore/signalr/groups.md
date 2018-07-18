---
title: Administrar usuarios y grupos en SignalR
author: tdykstra
description: Información general de administración de grupos y usuarios de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d54ab2a113345f98e26425a88cad165d67b8d456
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095026"
---
# <a name="manage-users-and-groups-in-signalr"></a>Administrar usuarios y grupos en SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como el nombre de grupos de conexiones.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Usuarios de SignalR

SignalR le permite enviar mensajes a todas las conexiones asociadas a un usuario específico. De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario. Un único usuario puede tener varias conexiones a una aplicación de SignalR. Por ejemplo, un usuario podría estar conectado en su escritorio, así como su teléfono. Cada dispositivo tiene una conexión SignalR independiente, pero que están todos los asociados con el mismo usuario. Si se envía un mensaje al usuario, todas las conexiones asociadas con la que el usuario recepción el mensaje. El identificador de usuario para una conexión puede tener acceso a la `Context.UserIdentifier` propiedad en el centro.

Enviar un mensaje a un usuario específico, pasando el identificador de usuario para el `User` funcionando en su método de concentrador, tal como se muestra en el ejemplo siguiente:

> [!NOTE]
> El identificador de usuario distingue mayúsculas de minúsculas.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

El identificador de usuario se puede personalizar mediante la creación de un `IUserIdProvider`y registrarlo en `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR debe llamarse antes de registrar sus servicios personalizados de SignalR.

## <a name="groups-in-signalr"></a>Grupos en SignalR

Un grupo es una colección de conexiones asociado con un nombre. Los mensajes pueden enviarse a todas las conexiones en un grupo. Los grupos son el método recomendado para enviar a una conexión o varias conexiones porque los grupos administrados por la aplicación. Una conexión puede ser un miembro de varios grupos. Esto hace que los grupos ideal para algo parecido a una aplicación de chat, donde cada sala puede representarse como un grupo. Las conexiones se pueden sumar o quitan de los grupos a través de la `AddToGroupAsync` y `RemoveFromGroupAsync` métodos.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Pertenencia a grupos no se conserva cuando se vuelve a conectar en una conexión. Debe volver a unirse al grupo cuando se vuelve a establecer la conexión. No es posible contar a los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.

> [!NOTE]
> Los nombres de grupo distinguen entre mayúsculas y minúsculas.

## <a name="related-resources"></a>Recursos relacionados

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
