---
uid: signalr/overview/older-versions/working-with-groups
title: Trabajar con grupos en SignalR 1.x | Documentos de Microsoft
author: pfletcher
description: "Este tema describe cómo conservar la información de pertenencia de grupo con la API de concentrador."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="3779b-103">Trabajar con grupos en SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="3779b-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="3779b-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3779b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3779b-105">En este tema se describe cómo agregar usuarios a grupos y conservar la información de pertenencia a grupo.</span><span class="sxs-lookup"><span data-stu-id="3779b-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="3779b-106">Información general</span><span class="sxs-lookup"><span data-stu-id="3779b-106">Overview</span></span>

<span data-ttu-id="3779b-107">Grupos de SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3779b-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="3779b-108">Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="3779b-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="3779b-109">No tienes que crear explícitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="3779b-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="3779b-110">De hecho, un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a Groups.Add y se elimina cuando se quita la última conexión de pertenencia en ella.</span><span class="sxs-lookup"><span data-stu-id="3779b-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="3779b-111">Para obtener una introducción al uso de grupos, consulte [cómo administrar la pertenencia a grupos de la clase de base de datos central](index.md) en la API de concentradores - Guía de servidor.</span><span class="sxs-lookup"><span data-stu-id="3779b-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="3779b-112">No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="3779b-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="3779b-113">SignalR envía mensajes a los clientes y grupos basados en un modelo de pub/sub, y el servidor no mantiene las listas de grupos o las pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="3779b-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="3779b-114">Esto ayuda a maximizar la escalabilidad, ya que cada vez que agregue un nodo a una granja de servidores web, cualquier estado que mantiene SignalR debe se propaguen en el nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="3779b-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="3779b-115">Al agregar un usuario a un grupo con el `Groups.Add` método, el usuario recibe mensajes dirigidos a ese grupo para la duración de la conexión actual, pero la pertenencia del usuario de ese grupo no se conserva más allá de la conexión actual.</span><span class="sxs-lookup"><span data-stu-id="3779b-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="3779b-116">Si desea conservar permanentemente la información sobre grupos y pertenencia a grupos, debe almacenar esos datos en un repositorio como una base de datos o almacenamiento de tabla de Azure.</span><span class="sxs-lookup"><span data-stu-id="3779b-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="3779b-117">A continuación, cada vez que un usuario se conecta a la aplicación, recuperar desde el repositorio de qué grupos pertenece el usuario y agrega manualmente ese usuario a esos grupos.</span><span class="sxs-lookup"><span data-stu-id="3779b-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="3779b-118">Cuando vuelve a conectar después de una interrupción temporal, el usuario vuelve a une automáticamente los grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="3779b-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="3779b-119">Automáticamente nueva unión a un grupo solo se aplica cuando vuelve a conectar, no al establecer una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="3779b-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="3779b-120">Se pasa un token firmado digitalmente desde el cliente que contiene la lista de grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="3779b-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="3779b-121">Si desea comprobar si el usuario pertenece a los grupos solicitados, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="3779b-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="3779b-122">En este tema, se incluyen las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="3779b-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="3779b-123">Agregar y quitar usuarios</span><span class="sxs-lookup"><span data-stu-id="3779b-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="3779b-124">Al llamar a los miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="3779b-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="3779b-125">Almacenamiento de pertenencia a grupos en una base de datos</span><span class="sxs-lookup"><span data-stu-id="3779b-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="3779b-126">Almacenamiento de pertenencia a grupos de almacenamiento de tabla de Azure</span><span class="sxs-lookup"><span data-stu-id="3779b-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="3779b-127">Comprobar la pertenencia al grupo cuando vuelve a conectar</span><span class="sxs-lookup"><span data-stu-id="3779b-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="3779b-128">Agregar y quitar usuarios</span><span class="sxs-lookup"><span data-stu-id="3779b-128">Adding and removing users</span></span>

<span data-ttu-id="3779b-129">Para agregar o quitar usuarios de un grupo, se llama a la [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos y pase el identificador del usuario conexión y nombre del grupo como parámetros.</span><span class="sxs-lookup"><span data-stu-id="3779b-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="3779b-130">No es necesario quitar manualmente un usuario de un grupo cuando finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="3779b-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="3779b-131">El siguiente ejemplo se muestra la `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="3779b-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="3779b-132">El `Groups.Add` y `Groups.Remove` métodos ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3779b-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="3779b-133">Si desea agregar a un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, se tiene que asegurarse de que el método Groups.Add finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="3779b-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="3779b-134">Ejemplos de código siguientes muestran cómo hacerlo, uno con un código que funciona en .NET 4.5 y otro con un código que funciona en .NET 4.</span><span class="sxs-lookup"><span data-stu-id="3779b-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="3779b-135">Asincrónica .NET 4.5 ejemplo</span><span class="sxs-lookup"><span data-stu-id="3779b-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="3779b-136">Asincrónica .NET 4 ejemplo</span><span class="sxs-lookup"><span data-stu-id="3779b-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="3779b-137">En general, no es necesario incluir `await` al llamar a la `Groups.Remove` método porque el identificador de conexión que está intentando quitar ya no estén disponible.</span><span class="sxs-lookup"><span data-stu-id="3779b-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="3779b-138">En ese caso, `TaskCanceledException` se produce cuando se agota el tiempo de espera de la solicitud. Si la aplicación debe asegurarse de que el usuario se ha quitado del grupo antes de enviar un mensaje al grupo, puede agregar `await` antes de Groups.Remove y, a continuación, detectar el `TaskCanceledException` excepción que se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="3779b-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="3779b-139">Al llamar a los miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="3779b-139">Calling members of a group</span></span>

<span data-ttu-id="3779b-140">Puede enviar mensajes a todos los miembros de un grupo o solo los miembros especificados del grupo, tal como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="3779b-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="3779b-141">**Todos los** conectado los clientes en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="3779b-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="3779b-142">Todos los clientes en un grupo especificado conectados **excepto los clientes especificados**, identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="3779b-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="3779b-143">Todos los clientes en un grupo especificado conectados **excepto el cliente que realiza la llamada**.</span><span class="sxs-lookup"><span data-stu-id="3779b-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="3779b-144">Almacenamiento de pertenencia a grupos en una base de datos</span><span class="sxs-lookup"><span data-stu-id="3779b-144">Storing group membership in a database</span></span>

<span data-ttu-id="3779b-145">Los ejemplos siguientes muestran cómo conservar la información de usuario y de grupo en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3779b-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="3779b-146">Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente muestra cómo definir los modelos que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3779b-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="3779b-147">Estos modelos de entidad corresponden a los campos y tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3779b-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="3779b-148">La estructura de datos puede variar considerablemente según los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3779b-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="3779b-149">Este ejemplo incluye una clase denominada `ConversationRoom` que debería ser único para una aplicación que permite a los usuarios para unirse a las conversaciones sobre temas diferentes, como deportes o matriz de procesos.</span><span class="sxs-lookup"><span data-stu-id="3779b-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="3779b-150">En este ejemplo también incluye una clase para las conexiones.</span><span class="sxs-lookup"><span data-stu-id="3779b-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="3779b-151">La clase de conexión no es absolutamente necesaria para realizar el seguimiento de pertenencia a grupos, pero suele ser parte de una solución sólida para los usuarios de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="3779b-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="3779b-152">A continuación, en el concentrador, puede recuperar la información de grupo y usuario de la base de datos y agregar manualmente el usuario a los grupos adecuados.</span><span class="sxs-lookup"><span data-stu-id="3779b-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="3779b-153">El ejemplo no incluye código para realizar el seguimiento de las conexiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="3779b-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="3779b-154">En este ejemplo, el `await` palabra clave no se aplica antes de `Groups.Add` porque no se envía inmediatamente un mensaje a los miembros del grupo.</span><span class="sxs-lookup"><span data-stu-id="3779b-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="3779b-155">Si desea enviar un mensaje a todos los miembros del grupo inmediatamente después de agregar el nuevo miembro, sería conveniente aplicar el `await` palabra clave para asegurarse de que se complete la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3779b-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="3779b-156">Almacenamiento de pertenencia a grupos de almacenamiento de tabla de Azure</span><span class="sxs-lookup"><span data-stu-id="3779b-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="3779b-157">Es similar al uso de una base de datos mediante almacenamiento de tabla de Azure para almacenar información de usuario y de grupo.</span><span class="sxs-lookup"><span data-stu-id="3779b-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="3779b-158">En el ejemplo siguiente se muestra una entidad de tabla que almacena el nombre de usuario y el nombre del grupo.</span><span class="sxs-lookup"><span data-stu-id="3779b-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="3779b-159">En el concentrador, recuperar los grupos asignados cuando se conecta el usuario.</span><span class="sxs-lookup"><span data-stu-id="3779b-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="3779b-160">Comprobar la pertenencia al grupo cuando vuelve a conectar</span><span class="sxs-lookup"><span data-stu-id="3779b-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="3779b-161">De forma predeterminada, SignalR volver a asigna automáticamente un usuario a los grupos correspondientes cuando vuelve a conectar desde una interrupción temporal, por ejemplo, cuando una conexión se quita y se restablece antes agote el tiempo de espera de la conexión. Información del grupo del usuario se pasa en un token cuando vuelve a conectar, y ese token se comprueba en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3779b-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="3779b-162">Para obtener información sobre el proceso de comprobación para volver a unir a los usuarios a grupos, consulte [nueva unión a grupos cuando vuelve a conectar](index.md).</span><span class="sxs-lookup"><span data-stu-id="3779b-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="3779b-163">En general, debe usar el comportamiento predeterminado de automáticamente nueva unión a grupos en volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="3779b-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="3779b-164">Grupos de SignalR no están diseñados como un mecanismo de seguridad para restringir el acceso a información confidencial.</span><span class="sxs-lookup"><span data-stu-id="3779b-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="3779b-165">Sin embargo, si la aplicación debe comprobar la pertenencia a grupos del usuario cuando vuelve a conectar, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="3779b-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="3779b-166">Cambiar el comportamiento predeterminado puede agregar una carga a la base de datos porque se debe recuperar la pertenencia a grupos del usuario para cada nueva conexión en lugar de simplemente cuando el usuario se conecta.</span><span class="sxs-lookup"><span data-stu-id="3779b-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="3779b-167">Si debe comprobar la pertenencia a grupos de volver a conectarse, cree un nuevo módulo de canalización de concentrador que devuelve una lista de grupos asignados, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3779b-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="3779b-168">A continuación, agregue ese módulo a la canalización de concentrador, como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="3779b-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
