---
title: Administrar usuarios y grupos en SignalR
author: bradygaster
description: Información general de administración de grupos y usuarios de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667757"
---
# <a name="manage-users-and-groups-in-signalr"></a>Administrar usuarios y grupos en SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como el nombre de grupos de conexiones.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Usuarios de SignalR

SignalR le permite enviar mensajes a todas las conexiones asociadas a un usuario específico. De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario. Un único usuario puede tener varias conexiones a una aplicación de SignalR. Por ejemplo, un usuario podría estar conectado en su escritorio, así como su teléfono. Cada dispositivo tiene una conexión SignalR independiente, pero que están todos los asociados con el mismo usuario. Si se envía un mensaje al usuario, todas las conexiones asociadas con la que el usuario recepción el mensaje. El identificador de usuario para una conexión puede tener acceso a la `Context.UserIdentifier` propiedad en el centro.

Enviar un mensaje a un usuario específico, pasando el identificador de usuario para el `User` funcionando en su método de concentrador, tal como se muestra en el ejemplo siguiente:

> [!NOTE]
> El identificador de usuario distingue mayúsculas de minúsculas.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a>Grupos en SignalR

Un grupo es una colección de conexiones asociado con un nombre. Los mensajes pueden enviarse a todas las conexiones en un grupo. Los grupos son el método recomendado para enviar a una conexión o varias conexiones porque los grupos administrados por la aplicación. Una conexión puede ser un miembro de varios grupos. Esto hace que los grupos ideal para algo parecido a una aplicación de chat, donde cada sala puede representarse como un grupo. Las conexiones se pueden sumar o quitan de los grupos a través de la `AddToGroupAsync` y `RemoveFromGroupAsync` métodos.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Pertenencia a grupos no se conserva cuando se vuelve a conectar en una conexión. Debe volver a unirse al grupo cuando se vuelve a establecer la conexión. No es posible contar a los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.

Para proteger el acceso a los recursos durante el uso de grupos, use [autenticación y autorización](xref:signalr/authn-and-authz) funcionalidad en ASP.NET Core. Si agrega solo los usuarios a un grupo cuando las credenciales son válidas para ese grupo, los mensajes enviados a ese grupo sólo irá a los usuarios autorizados. Sin embargo, los grupos no son una característica de seguridad. Notificaciones de autenticación tienen características que no los grupos, como la revocación y caducidad. Si se revoca el permiso de un usuario para acceder al grupo, tiene detectar y quitar del grupo manualmente.

> [!NOTE]
> Los nombres de grupo distinguen entre mayúsculas y minúsculas.

## <a name="related-resources"></a>Recursos relacionados

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
