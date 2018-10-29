---
title: Administrar usuarios y grupos en SignalR
author: tdykstra
description: Información general de administración de grupos y usuarios de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d3e580dfc42a36762358899892831c8b68f544b0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207165"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="a8748-103">Administrar usuarios y grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="a8748-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="a8748-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="a8748-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="a8748-105">SignalR permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como el nombre de grupos de conexiones.</span><span class="sxs-lookup"><span data-stu-id="a8748-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="a8748-106">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a8748-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="a8748-107">Usuarios de SignalR</span><span class="sxs-lookup"><span data-stu-id="a8748-107">Users in SignalR</span></span>

<span data-ttu-id="a8748-108">SignalR le permite enviar mensajes a todas las conexiones asociadas a un usuario específico.</span><span class="sxs-lookup"><span data-stu-id="a8748-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="a8748-109">De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="a8748-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="a8748-110">Un único usuario puede tener varias conexiones a una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8748-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="a8748-111">Por ejemplo, un usuario podría estar conectado en su escritorio, así como su teléfono.</span><span class="sxs-lookup"><span data-stu-id="a8748-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="a8748-112">Cada dispositivo tiene una conexión SignalR independiente, pero que están todos los asociados con el mismo usuario.</span><span class="sxs-lookup"><span data-stu-id="a8748-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="a8748-113">Si se envía un mensaje al usuario, todas las conexiones asociadas con la que el usuario recepción el mensaje.</span><span class="sxs-lookup"><span data-stu-id="a8748-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="a8748-114">El identificador de usuario para una conexión puede tener acceso a la `Context.UserIdentifier` propiedad en el centro.</span><span class="sxs-lookup"><span data-stu-id="a8748-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="a8748-115">Enviar un mensaje a un usuario específico, pasando el identificador de usuario para el `User` funcionando en su método de concentrador, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a8748-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="a8748-116">El identificador de usuario distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a8748-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="a8748-117">El identificador de usuario se puede personalizar mediante la creación de un `IUserIdProvider`y registrarlo en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8748-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="a8748-118">AddSignalR debe llamarse antes de registrar sus servicios personalizados de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8748-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="a8748-119">Grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="a8748-119">Groups in SignalR</span></span>

<span data-ttu-id="a8748-120">Un grupo es una colección de conexiones asociado con un nombre.</span><span class="sxs-lookup"><span data-stu-id="a8748-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="a8748-121">Los mensajes pueden enviarse a todas las conexiones en un grupo.</span><span class="sxs-lookup"><span data-stu-id="a8748-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="a8748-122">Los grupos son el método recomendado para enviar a una conexión o varias conexiones porque los grupos administrados por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8748-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="a8748-123">Una conexión puede ser un miembro de varios grupos.</span><span class="sxs-lookup"><span data-stu-id="a8748-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="a8748-124">Esto hace que los grupos ideal para algo parecido a una aplicación de chat, donde cada sala puede representarse como un grupo.</span><span class="sxs-lookup"><span data-stu-id="a8748-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="a8748-125">Las conexiones se pueden sumar o quitan de los grupos a través de la `AddToGroupAsync` y `RemoveFromGroupAsync` métodos.</span><span class="sxs-lookup"><span data-stu-id="a8748-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="a8748-126">Pertenencia a grupos no se conserva cuando se vuelve a conectar en una conexión.</span><span class="sxs-lookup"><span data-stu-id="a8748-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="a8748-127">Debe volver a unirse al grupo cuando se vuelve a establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="a8748-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="a8748-128">No es posible contar a los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="a8748-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="a8748-129">Los nombres de grupo distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a8748-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="a8748-130">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="a8748-130">Related resources</span></span>

* [<span data-ttu-id="a8748-131">Introducción</span><span class="sxs-lookup"><span data-stu-id="a8748-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="a8748-132">Concentradores</span><span class="sxs-lookup"><span data-stu-id="a8748-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a8748-133">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="a8748-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
