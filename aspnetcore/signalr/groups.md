---
title: Administrar usuarios y grupos de SignalR
author: rachelappel
description: Información general de administración de grupos y usuarios de ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358451"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="a9439-103">Administrar usuarios y grupos de SignalR</span><span class="sxs-lookup"><span data-stu-id="a9439-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="a9439-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="a9439-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="a9439-105">SignalR permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como el nombre de grupos de conexiones.</span><span class="sxs-lookup"><span data-stu-id="a9439-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="a9439-106">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a9439-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="a9439-107">Usuarios de SignalR</span><span class="sxs-lookup"><span data-stu-id="a9439-107">Users in SignalR</span></span>

<span data-ttu-id="a9439-108">SignalR le permite enviar mensajes a todas las conexiones asociadas a un usuario específico.</span><span class="sxs-lookup"><span data-stu-id="a9439-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="a9439-109">De forma predeterminada se usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="a9439-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="a9439-110">Un único usuario puede tener varias conexiones a una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a9439-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="a9439-111">Por ejemplo, un usuario podría estar conectado en el escritorio, así como el número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="a9439-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="a9439-112">Cada dispositivo tiene una conexión SignalR independiente, pero están asociadas con el mismo usuario.</span><span class="sxs-lookup"><span data-stu-id="a9439-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="a9439-113">Si se envía un mensaje al usuario, todas las conexiones asociadas a ese usuario recibe el mensaje.</span><span class="sxs-lookup"><span data-stu-id="a9439-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="a9439-114">Enviar un mensaje a un usuario específico, pase el identificador de usuario para el `User` funcionando en su método de concentrador, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9439-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="a9439-115">El identificador de usuario distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a9439-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="a9439-116">El identificador de usuario se puede personalizar mediante la creación de un `IUserIdProvider`y registrarlo en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9439-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="a9439-117">AddSignalR debe llamarse antes de registrar los servicios SignalR personalizados.</span><span class="sxs-lookup"><span data-stu-id="a9439-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="a9439-118">Grupos de SignalR</span><span class="sxs-lookup"><span data-stu-id="a9439-118">Groups in SignalR</span></span>

<span data-ttu-id="a9439-119">Un grupo es una colección de conexiones asociadas a un nombre.</span><span class="sxs-lookup"><span data-stu-id="a9439-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="a9439-120">Los mensajes pueden enviarse a todas las conexiones en un grupo.</span><span class="sxs-lookup"><span data-stu-id="a9439-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="a9439-121">Los grupos son el método recomendado para enviar a una conexión o varias conexiones porque los grupos administrados por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a9439-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="a9439-122">Una conexión puede ser un miembro de varios grupos.</span><span class="sxs-lookup"><span data-stu-id="a9439-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="a9439-123">Esto hace que los grupos ideal para algo parecido a una aplicación de chat, donde cada habitación puede representarse como un grupo.</span><span class="sxs-lookup"><span data-stu-id="a9439-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="a9439-124">Las conexiones se pueden agregar a o quitar de los grupos a través de la `AddToGroupAsync` y `RemoveFromGroupAsync` métodos.</span><span class="sxs-lookup"><span data-stu-id="a9439-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="a9439-125">Pertenencia a grupos no se conserva cuando se vuelve a conectar una conexión.</span><span class="sxs-lookup"><span data-stu-id="a9439-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="a9439-126">Debe volver a unirse al grupo cuando se vuelve a establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="a9439-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="a9439-127">No es posible contar a los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="a9439-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="a9439-128">Nombres de grupo distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a9439-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="a9439-129">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="a9439-129">Related resources</span></span>

* [<span data-ttu-id="a9439-130">Introducción</span><span class="sxs-lookup"><span data-stu-id="a9439-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="a9439-131">Concentradores</span><span class="sxs-lookup"><span data-stu-id="a9439-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a9439-132">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="a9439-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
