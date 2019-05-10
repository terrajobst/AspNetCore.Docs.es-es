---
title: Administrar usuarios y grupos en SignalR
author: bradygaster
description: Información general de administración de grupos y usuarios de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 180f8b4551eea39cc340bf1d250f4575cb5f71ed
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087428"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="53066-103">Administrar usuarios y grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="53066-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="53066-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="53066-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="53066-105">SignalR permite que los mensajes se envíen a todas las conexiones asociadas a un usuario específico, así como el nombre de grupos de conexiones.</span><span class="sxs-lookup"><span data-stu-id="53066-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="53066-106">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="53066-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="53066-107">Usuarios de SignalR</span><span class="sxs-lookup"><span data-stu-id="53066-107">Users in SignalR</span></span>

<span data-ttu-id="53066-108">SignalR le permite enviar mensajes a todas las conexiones asociadas a un usuario específico.</span><span class="sxs-lookup"><span data-stu-id="53066-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="53066-109">De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="53066-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="53066-110">Un único usuario puede tener varias conexiones a una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="53066-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="53066-111">Por ejemplo, un usuario podría estar conectado en su escritorio, así como su teléfono.</span><span class="sxs-lookup"><span data-stu-id="53066-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="53066-112">Cada dispositivo tiene una conexión SignalR independiente, pero que están todos los asociados con el mismo usuario.</span><span class="sxs-lookup"><span data-stu-id="53066-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="53066-113">Si se envía un mensaje al usuario, todas las conexiones asociadas con la que el usuario recepción el mensaje.</span><span class="sxs-lookup"><span data-stu-id="53066-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="53066-114">El identificador de usuario para una conexión puede tener acceso a la `Context.UserIdentifier` propiedad en el centro.</span><span class="sxs-lookup"><span data-stu-id="53066-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="53066-115">Enviar un mensaje a un usuario específico, pasando el identificador de usuario para el `User` funcionando en su método de concentrador, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="53066-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="53066-116">El identificador de usuario distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="53066-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a><span data-ttu-id="53066-117">Grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="53066-117">Groups in SignalR</span></span>

<span data-ttu-id="53066-118">Un grupo es una colección de conexiones asociado con un nombre.</span><span class="sxs-lookup"><span data-stu-id="53066-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="53066-119">Los mensajes pueden enviarse a todas las conexiones en un grupo.</span><span class="sxs-lookup"><span data-stu-id="53066-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="53066-120">Los grupos son el método recomendado para enviar a una conexión o varias conexiones porque los grupos administrados por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53066-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="53066-121">Una conexión puede ser un miembro de varios grupos.</span><span class="sxs-lookup"><span data-stu-id="53066-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="53066-122">Esto hace que los grupos ideal para algo parecido a una aplicación de chat, donde cada sala puede representarse como un grupo.</span><span class="sxs-lookup"><span data-stu-id="53066-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="53066-123">Las conexiones se pueden sumar o quitan de los grupos a través de la `AddToGroupAsync` y `RemoveFromGroupAsync` métodos.</span><span class="sxs-lookup"><span data-stu-id="53066-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="53066-124">Pertenencia a grupos no se conserva cuando se vuelve a conectar en una conexión.</span><span class="sxs-lookup"><span data-stu-id="53066-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="53066-125">Debe volver a unirse al grupo cuando se vuelve a establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="53066-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="53066-126">No es posible contar a los miembros de un grupo, puesto que esta información no está disponible si la aplicación se escala a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="53066-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="53066-127">Para proteger el acceso a los recursos durante el uso de grupos, use [autenticación y autorización](xref:signalr/authn-and-authz) funcionalidad en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53066-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="53066-128">Si agrega solo los usuarios a un grupo cuando las credenciales son válidas para ese grupo, los mensajes enviados a ese grupo sólo irá a los usuarios autorizados.</span><span class="sxs-lookup"><span data-stu-id="53066-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="53066-129">Sin embargo, los grupos no son una característica de seguridad.</span><span class="sxs-lookup"><span data-stu-id="53066-129">However, groups are not a security feature.</span></span> <span data-ttu-id="53066-130">Notificaciones de autenticación tienen características que no los grupos, como la revocación y caducidad.</span><span class="sxs-lookup"><span data-stu-id="53066-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="53066-131">Si se revoca el permiso de un usuario para acceder al grupo, tiene detectar y quitar del grupo manualmente.</span><span class="sxs-lookup"><span data-stu-id="53066-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="53066-132">Los nombres de grupo distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="53066-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="53066-133">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="53066-133">Related resources</span></span>

* [<span data-ttu-id="53066-134">Introducción</span><span class="sxs-lookup"><span data-stu-id="53066-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="53066-135">Concentradores</span><span class="sxs-lookup"><span data-stu-id="53066-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="53066-136">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="53066-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
