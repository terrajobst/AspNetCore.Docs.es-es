---
uid: signalr/overview/older-versions/working-with-groups
title: Trabajar con grupos en SignalR 1.x | Microsoft Docs
author: pfletcher
description: Este tema describe cómo conservar la información de pertenencia de grupo con la API de concentrador.
ms.author: riande
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5096e7a5eb8704762a21eeb88a128d92e75ede60
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287499"
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="dddd4-103">Trabajar con grupos en SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="dddd4-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="dddd4-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dddd4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="dddd4-105">Este tema describe cómo agregar usuarios a grupos y conservar la información de pertenencia a grupo.</span><span class="sxs-lookup"><span data-stu-id="dddd4-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="dddd4-106">Información general</span><span class="sxs-lookup"><span data-stu-id="dddd4-106">Overview</span></span>

<span data-ttu-id="dddd4-107">Los grupos en SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="dddd4-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="dddd4-108">Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="dddd4-109">No tiene que crear explícitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="dddd4-110">De hecho, un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a Groups.Add y se elimina cuando se quita la última conexión de la pertenencia a él.</span><span class="sxs-lookup"><span data-stu-id="dddd4-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="dddd4-111">Para obtener una introducción al uso de grupos, consulte [cómo administrar la pertenencia a grupos desde la clase Hub](index.md) en la API Hubs - servidor guía.</span><span class="sxs-lookup"><span data-stu-id="dddd4-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="dddd4-112">No hay ninguna API para obtener una lista de miembros de grupo o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="dddd4-113">SignalR envía mensajes a los clientes y grupos en función de un modelo pub/sub, y el servidor no mantiene las listas de grupos o pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="dddd4-114">Esto ayuda a maximizar la escalabilidad, ya que cada vez que agrega un nodo a una granja de servidores web, cualquier estado que mantiene SignalR se propaguen al nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="dddd4-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="dddd4-115">Cuando agrega un usuario a un grupo mediante la `Groups.Add` método, el usuario recibe mensajes dirigidos a ese grupo para la duración de la conexión actual, pero la pertenencia del usuario en ese grupo no se conserva más allá de la conexión actual.</span><span class="sxs-lookup"><span data-stu-id="dddd4-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="dddd4-116">Si desea conservar permanentemente la información sobre los grupos y pertenencia a grupos, debe almacenar esos datos en un repositorio como una base de datos o almacenamiento de tablas de Azure.</span><span class="sxs-lookup"><span data-stu-id="dddd4-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="dddd4-117">A continuación, cada vez que un usuario se conecta a la aplicación, recuperar desde el repositorio de qué grupos pertenece el usuario y agregue manualmente ese usuario a esos grupos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="dddd4-118">Al volver a conectarse después de una interrupción temporal, el usuario vuelve a une automáticamente los grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="dddd4-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="dddd4-119">Unir automáticamente a un grupo solo se aplica al conectarse de nuevo, no al establecer una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="dddd4-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="dddd4-120">Se pasa un token firmado digitalmente desde el cliente que contiene la lista de grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="dddd4-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="dddd4-121">Si desea comprobar si el usuario pertenece a los grupos solicitados, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dddd4-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="dddd4-122">En este tema, se incluyen las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="dddd4-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="dddd4-123">Agregar y quitar usuarios</span><span class="sxs-lookup"><span data-stu-id="dddd4-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="dddd4-124">Llamar a miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="dddd4-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="dddd4-125">Almacenamiento de pertenencia a grupos en una base de datos</span><span class="sxs-lookup"><span data-stu-id="dddd4-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="dddd4-126">Almacenamiento de pertenencia a grupos en Azure table storage</span><span class="sxs-lookup"><span data-stu-id="dddd4-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="dddd4-127">Comprobar la pertenencia al grupo al volver a conectarse</span><span class="sxs-lookup"><span data-stu-id="dddd4-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="dddd4-128">Agregar y quitar usuarios</span><span class="sxs-lookup"><span data-stu-id="dddd4-128">Adding and removing users</span></span>

<span data-ttu-id="dddd4-129">Para agregar o quitar usuarios de un grupo, se llama a la [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos y pase el identificador del usuario conexión y nombre del grupo como parámetros.</span><span class="sxs-lookup"><span data-stu-id="dddd4-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="dddd4-130">No es necesario quitar manualmente un usuario de un grupo cuando finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="dddd4-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="dddd4-131">El ejemplo siguiente se muestra el `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="dddd4-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="dddd4-132">El `Groups.Add` y `Groups.Remove` métodos se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="dddd4-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="dddd4-133">Si desea agregar a un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, deberá asegurarse de que el método Groups.Add finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="dddd4-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="dddd4-134">Ejemplos de código siguientes muestran cómo hacer esto, uno con un código que funciona en .NET 4.5 y uno mediante el uso de código que funciona en .NET 4.</span><span class="sxs-lookup"><span data-stu-id="dddd4-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="dddd4-135">Asincrónica .NET 4.5 ejemplo</span><span class="sxs-lookup"><span data-stu-id="dddd4-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="dddd4-136">Asincrónica .NET 4 ejemplo</span><span class="sxs-lookup"><span data-stu-id="dddd4-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="dddd4-137">En general, no debe incluir `await` al llamar a la `Groups.Remove` método porque el identificador de conexión que está intentando quitar es posible que ya no estarán disponible.</span><span class="sxs-lookup"><span data-stu-id="dddd4-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="dddd4-138">En ese caso, `TaskCanceledException` se produce después de la solicitud agota el tiempo. Si la aplicación debe asegurarse de que el usuario se quitó del grupo antes de enviar un mensaje al grupo, puede agregar `await` antes Groups.Remove y, a continuación, detectar la `TaskCanceledException` excepciones que puedan producirse.</span><span class="sxs-lookup"><span data-stu-id="dddd4-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="dddd4-139">Llamar a miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="dddd4-139">Calling members of a group</span></span>

<span data-ttu-id="dddd4-140">Puede enviar mensajes a todos los miembros de un grupo o solo los miembros especificados del grupo, tal como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="dddd4-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="dddd4-141">**Todos los** conectado a los clientes en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="dddd4-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="dddd4-142">Todos los clientes en un grupo especificado conectados **excepto los clientes especificados**, identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="dddd4-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="dddd4-143">Todos los clientes en un grupo especificado conectados **excepto el cliente que realiza la llamada**.</span><span class="sxs-lookup"><span data-stu-id="dddd4-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="dddd4-144">Almacenamiento de pertenencia a grupos en una base de datos</span><span class="sxs-lookup"><span data-stu-id="dddd4-144">Storing group membership in a database</span></span>

<span data-ttu-id="dddd4-145">Los ejemplos siguientes muestran cómo conservar la información de grupo y usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="dddd4-146">Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente se muestra cómo definir los modelos que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dddd4-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="dddd4-147">Estos modelos de entidad se corresponden con los campos y tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dddd4-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="dddd4-148">La estructura de datos puede variar considerablemente según los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dddd4-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="dddd4-149">En este ejemplo incluye una clase denominada `ConversationRoom` que sería única para una aplicación que permite a los usuarios para unirse a las conversaciones sobre diferentes temas, como deportes o jardinería.</span><span class="sxs-lookup"><span data-stu-id="dddd4-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="dddd4-150">En este ejemplo también incluye una clase para las conexiones.</span><span class="sxs-lookup"><span data-stu-id="dddd4-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="dddd4-151">La clase de conexión no es estrictamente necesaria para realizar el seguimiento de pertenencia a grupos, pero suele ser parte de una solución sólida para los usuarios de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="dddd4-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="dddd4-152">A continuación, en el concentrador, puede recuperar la información de grupo y usuario de la base de datos y agregar manualmente el usuario a los grupos adecuados.</span><span class="sxs-lookup"><span data-stu-id="dddd4-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="dddd4-153">El ejemplo no incluye código para realizar el seguimiento de las conexiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="dddd4-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="dddd4-154">En este ejemplo, el `await` no se aplica la palabra clave antes de `Groups.Add` porque no se envía inmediatamente un mensaje a los miembros del grupo.</span><span class="sxs-lookup"><span data-stu-id="dddd4-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="dddd4-155">Si desea enviar un mensaje a todos los miembros del grupo inmediatamente después de agregar el nuevo miembro, ¿desea aplicar el `await` palabra clave para asegurarse de que se ha completado la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="dddd4-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="dddd4-156">Almacenamiento de pertenencia a grupos en Azure table storage</span><span class="sxs-lookup"><span data-stu-id="dddd4-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="dddd4-157">Es similar al uso de una base de datos mediante Azure table storage para almacenar información de usuario y grupo.</span><span class="sxs-lookup"><span data-stu-id="dddd4-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="dddd4-158">El ejemplo siguiente muestra una entidad de tabla que almacena el nombre de usuario y el nombre del grupo.</span><span class="sxs-lookup"><span data-stu-id="dddd4-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="dddd4-159">En el concentrador, recuperar los grupos asignados cuando se conecta el usuario.</span><span class="sxs-lookup"><span data-stu-id="dddd4-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="dddd4-160">Comprobar la pertenencia al grupo al volver a conectarse</span><span class="sxs-lookup"><span data-stu-id="dddd4-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="dddd4-161">De forma predeterminada, SignalR volver a asigna automáticamente un usuario a los grupos adecuados cuando vuelva a conectar desde una interrupción temporal, por ejemplo, cuando una conexión se quita y vuelve a establecer antes de la conexión agota el tiempo. Información del grupo del usuario se pasa un token al conectarse de nuevo y ese token se comprueba en el servidor.</span><span class="sxs-lookup"><span data-stu-id="dddd4-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="dddd4-162">Para obtener información sobre el proceso de comprobación para unir a los usuarios a grupos, consulte [nueva unión a grupos al volver a conectarse](index.md).</span><span class="sxs-lookup"><span data-stu-id="dddd4-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="dddd4-163">En general, debe usar el comportamiento predeterminado de automáticamente unir a grupos en volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="dddd4-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="dddd4-164">Grupos de SignalR no están diseñados como un mecanismo de seguridad para restringir el acceso a datos confidenciales.</span><span class="sxs-lookup"><span data-stu-id="dddd4-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="dddd4-165">Sin embargo, si la aplicación debe comprobar pertenencia a grupos del usuario al conectarse de nuevo, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dddd4-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="dddd4-166">Cambiar el comportamiento predeterminado puede agregar una carga a la base de datos porque se debe recuperar la pertenencia a grupos del usuario para cada nueva conexión en lugar de simplemente cuando el usuario se conecta.</span><span class="sxs-lookup"><span data-stu-id="dddd4-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="dddd4-167">Si se debe comprobar la pertenencia a grupos en volver a conectarse, cree un nuevo módulo de canalización de concentrador que devuelve una lista de grupos asignados, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="dddd4-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="dddd4-168">A continuación, agregue ese módulo a la canalización de concentrador, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="dddd4-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
