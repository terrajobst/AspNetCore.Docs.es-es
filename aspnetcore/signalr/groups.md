---
title: Administrar usuarios y grupos en SignalR
author: bradygaster
description: Información general de ASP.NET Core SignalR la administración de usuarios y grupos.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 59e90042ecbaf936602643bbdc3965e036426b26
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963808"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a>Administrar usuarios y grupos en SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como a grupos de conexiones con nombre.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargarlo)](xref:index#how-to-download-a-sample)

## <a name="users-in-opno-locsignalr"></a>Usuarios en SignalR

SignalR permite enviar mensajes a todas las conexiones asociadas a un usuario específico. De forma predeterminada, SignalR usa el `ClaimTypes.NameIdentifier` de la `ClaimsPrincipal` asociada a la conexión como identificador de usuario. Un solo usuario puede tener varias conexiones a una aplicación SignalR. Por ejemplo, un usuario podría estar conectado a su escritorio, así como a su teléfono. Cada dispositivo tiene una conexión SignalR independiente, pero todos están asociados al mismo usuario. Si se envía un mensaje al usuario, todas las conexiones asociadas a ese usuario reciben el mensaje. El identificador de usuario de una conexión puede tener acceso a la propiedad `Context.UserIdentifier` del centro.

Envíe un mensaje a un usuario específico pasando el identificador de usuario a la función `User` en el método de concentrador, tal como se muestra en el ejemplo siguiente:

> [!NOTE]
> El identificador de usuario distingue entre mayúsculas y minúsculas.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a>Grupos en SignalR

Un grupo es una colección de conexiones asociadas a un nombre. Los mensajes se pueden enviar a todas las conexiones de un grupo. Los grupos son el método recomendado para enviar a una conexión o varias conexiones, ya que la aplicación administra los grupos. Una conexión puede ser miembro de varios grupos. Esto hace que los grupos sean ideales para algo como una aplicación de chat, donde cada habitación puede representarse como un grupo. Las conexiones se pueden agregar o quitar de los grupos a través de los métodos `AddToGroupAsync` y `RemoveFromGroupAsync`.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

La pertenencia a grupos no se conserva cuando se vuelve a conectar una conexión. La conexión debe volver a unirse al grupo cuando se restablece. No es posible contar los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.

Para proteger el acceso a los recursos al usar grupos, use la funcionalidad de [autenticación y autorización](xref:signalr/authn-and-authz) en ASP.net Core. Si solo agrega usuarios a un grupo cuando las credenciales son válidas para ese grupo, los mensajes enviados a ese grupo solo se dirigirán a los usuarios autorizados. Sin embargo, los grupos no son una característica de seguridad. Las notificaciones de autenticación tienen características que los grupos no, como la expiración y la revocación. Si se revoca el permiso de un usuario para obtener acceso al grupo, debe detectarlo manualmente y quitarlo del grupo.

> [!NOTE]
> Los nombres de grupo distinguen mayúsculas de minúsculas.

## <a name="related-resources"></a>Recursos relacionados

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
