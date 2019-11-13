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
# <a name="manage-users-and-groups-in-opno-locsignalr"></a><span data-ttu-id="ad7b6-103">Administrar usuarios y grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="ad7b6-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="ad7b6-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ad7b6-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="ad7b6-105"> permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como a grupos de conexiones con nombre.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-105"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="ad7b6-106">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargarlo)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ad7b6-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-opno-locsignalr"></a><span data-ttu-id="ad7b6-107">Usuarios en SignalR</span><span class="sxs-lookup"><span data-stu-id="ad7b6-107">Users in SignalR</span></span>

SignalR<span data-ttu-id="ad7b6-108"> permite enviar mensajes a todas las conexiones asociadas a un usuario específico.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-108"> allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="ad7b6-109">De forma predeterminada, SignalR usa el `ClaimTypes.NameIdentifier` de la `ClaimsPrincipal` asociada a la conexión como identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="ad7b6-110">Un solo usuario puede tener varias conexiones a una aplicación SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="ad7b6-111">Por ejemplo, un usuario podría estar conectado a su escritorio, así como a su teléfono.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="ad7b6-112">Cada dispositivo tiene una conexión SignalR independiente, pero todos están asociados al mismo usuario.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="ad7b6-113">Si se envía un mensaje al usuario, todas las conexiones asociadas a ese usuario reciben el mensaje.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="ad7b6-114">El identificador de usuario de una conexión puede tener acceso a la propiedad `Context.UserIdentifier` del centro.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="ad7b6-115">Envíe un mensaje a un usuario específico pasando el identificador de usuario a la función `User` en el método de concentrador, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ad7b6-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="ad7b6-116">El identificador de usuario distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a><span data-ttu-id="ad7b6-117">Grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="ad7b6-117">Groups in SignalR</span></span>

<span data-ttu-id="ad7b6-118">Un grupo es una colección de conexiones asociadas a un nombre.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="ad7b6-119">Los mensajes se pueden enviar a todas las conexiones de un grupo.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="ad7b6-120">Los grupos son el método recomendado para enviar a una conexión o varias conexiones, ya que la aplicación administra los grupos.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="ad7b6-121">Una conexión puede ser miembro de varios grupos.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="ad7b6-122">Esto hace que los grupos sean ideales para algo como una aplicación de chat, donde cada habitación puede representarse como un grupo.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="ad7b6-123">Las conexiones se pueden agregar o quitar de los grupos a través de los métodos `AddToGroupAsync` y `RemoveFromGroupAsync`.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="ad7b6-124">La pertenencia a grupos no se conserva cuando se vuelve a conectar una conexión.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="ad7b6-125">La conexión debe volver a unirse al grupo cuando se restablece.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="ad7b6-126">No es posible contar los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="ad7b6-127">Para proteger el acceso a los recursos al usar grupos, use la funcionalidad de [autenticación y autorización](xref:signalr/authn-and-authz) en ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="ad7b6-128">Si solo agrega usuarios a un grupo cuando las credenciales son válidas para ese grupo, los mensajes enviados a ese grupo solo se dirigirán a los usuarios autorizados.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="ad7b6-129">Sin embargo, los grupos no son una característica de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-129">However, groups are not a security feature.</span></span> <span data-ttu-id="ad7b6-130">Las notificaciones de autenticación tienen características que los grupos no, como la expiración y la revocación.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="ad7b6-131">Si se revoca el permiso de un usuario para obtener acceso al grupo, debe detectarlo manualmente y quitarlo del grupo.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="ad7b6-132">Los nombres de grupo distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="ad7b6-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="ad7b6-133">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="ad7b6-133">Related resources</span></span>

* [<span data-ttu-id="ad7b6-134">Introducción</span><span class="sxs-lookup"><span data-stu-id="ad7b6-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ad7b6-135">Concentradores</span><span class="sxs-lookup"><span data-stu-id="ad7b6-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ad7b6-136">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="ad7b6-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
