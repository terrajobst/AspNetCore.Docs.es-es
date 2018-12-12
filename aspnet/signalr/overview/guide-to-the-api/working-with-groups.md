---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Trabajar con grupos en SignalR | Microsoft Docs
author: pfletcher
description: Este tema describe cómo conservar la información de pertenencia de grupo con la API de concentrador.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: bf4b8597fe9c0bff06b95cfb9970211106191fd4
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287642"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="a89ce-103">Trabajar con grupos en SignalR</span><span class="sxs-lookup"><span data-stu-id="a89ce-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="a89ce-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a89ce-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="a89ce-105">Este tema describe cómo agregar usuarios a grupos y conservar la información de pertenencia a grupo.</span><span class="sxs-lookup"><span data-stu-id="a89ce-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a89ce-106">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="a89ce-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="a89ce-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a89ce-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="a89ce-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a89ce-108">.NET 4.5</span></span>
> - <span data-ttu-id="a89ce-109">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="a89ce-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="a89ce-110">Versiones anteriores de este tema.</span><span class="sxs-lookup"><span data-stu-id="a89ce-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="a89ce-111">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a89ce-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="a89ce-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="a89ce-112">Questions and comments</span></span>
>
> <span data-ttu-id="a89ce-113">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="a89ce-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a89ce-114">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a89ce-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="a89ce-115">Información general</span><span class="sxs-lookup"><span data-stu-id="a89ce-115">Overview</span></span>

<span data-ttu-id="a89ce-116">Los grupos en SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="a89ce-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="a89ce-117">Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="a89ce-118">No tiene que crear explícitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="a89ce-119">De hecho, un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a Groups.Add y se elimina cuando se quita la última conexión de la pertenencia a él.</span><span class="sxs-lookup"><span data-stu-id="a89ce-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="a89ce-120">Para obtener una introducción al uso de grupos, consulte [cómo administrar la pertenencia a grupos desde la clase Hub](hubs-api-guide-server.md#groupsfromhub) en la API Hubs - servidor guía.</span><span class="sxs-lookup"><span data-stu-id="a89ce-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="a89ce-121">No hay ninguna API para obtener una lista de miembros de grupo o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="a89ce-122">SignalR envía mensajes a los clientes y grupos en función de un modelo pub/sub, y el servidor no mantiene las listas de grupos o pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="a89ce-123">Esto ayuda a maximizar la escalabilidad, ya que cada vez que agrega un nodo a una granja de servidores web, cualquier estado que mantiene SignalR se propaguen al nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="a89ce-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="a89ce-124">Cuando agrega un usuario a un grupo mediante la `Groups.Add` método, el usuario recibe mensajes dirigidos a ese grupo para la duración de la conexión actual, pero la pertenencia del usuario en ese grupo no se conserva más allá de la conexión actual.</span><span class="sxs-lookup"><span data-stu-id="a89ce-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="a89ce-125">Si desea conservar permanentemente la información sobre los grupos y pertenencia a grupos, debe almacenar esos datos en un repositorio como una base de datos o almacenamiento de tablas de Azure.</span><span class="sxs-lookup"><span data-stu-id="a89ce-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="a89ce-126">A continuación, cada vez que un usuario se conecta a la aplicación, recuperar desde el repositorio de qué grupos pertenece el usuario y agregue manualmente ese usuario a esos grupos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="a89ce-127">Al volver a conectarse después de una interrupción temporal, el usuario vuelve a une automáticamente los grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="a89ce-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="a89ce-128">Unir automáticamente a un grupo solo se aplica al conectarse de nuevo, no al establecer una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="a89ce-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="a89ce-129">Se pasa un token firmado digitalmente desde el cliente que contiene la lista de grupos asignados previamente.</span><span class="sxs-lookup"><span data-stu-id="a89ce-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="a89ce-130">Si desea comprobar si el usuario pertenece a los grupos solicitados, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a89ce-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="a89ce-131">En este tema, se incluyen las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="a89ce-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="a89ce-132">Agregar y quitar usuarios</span><span class="sxs-lookup"><span data-stu-id="a89ce-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="a89ce-133">Llamar a miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="a89ce-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="a89ce-134">Almacenamiento de pertenencia a grupos en una base de datos</span><span class="sxs-lookup"><span data-stu-id="a89ce-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="a89ce-135">Almacenamiento de pertenencia a grupos en Azure table storage</span><span class="sxs-lookup"><span data-stu-id="a89ce-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="a89ce-136">Comprobar la pertenencia al grupo al volver a conectarse</span><span class="sxs-lookup"><span data-stu-id="a89ce-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="a89ce-137">Agregar y quitar usuarios</span><span class="sxs-lookup"><span data-stu-id="a89ce-137">Adding and removing users</span></span>

<span data-ttu-id="a89ce-138">Para agregar o quitar usuarios de un grupo, se llama a la [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos y pase el identificador del usuario conexión y nombre del grupo como parámetros.</span><span class="sxs-lookup"><span data-stu-id="a89ce-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="a89ce-139">No es necesario quitar manualmente un usuario de un grupo cuando finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="a89ce-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="a89ce-140">El ejemplo siguiente se muestra el `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="a89ce-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="a89ce-141">El `Groups.Add` y `Groups.Remove` métodos se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="a89ce-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="a89ce-142">Si desea agregar a un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, deberá asegurarse de que el método Groups.Add finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="a89ce-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="a89ce-143">Ejemplos de código siguientes muestran cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="a89ce-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="a89ce-144">En general, no debe incluir `await` al llamar a la `Groups.Remove` método porque el identificador de conexión que está intentando quitar es posible que ya no estarán disponible.</span><span class="sxs-lookup"><span data-stu-id="a89ce-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="a89ce-145">En ese caso, `TaskCanceledException` se produce después de la solicitud agota el tiempo. Si la aplicación debe asegurarse de que el usuario se quitó del grupo antes de enviar un mensaje al grupo, puede agregar `await` antes `Groups.Remove`y, a continuación, detecte el `TaskCanceledException` excepciones que puedan producirse.</span><span class="sxs-lookup"><span data-stu-id="a89ce-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="a89ce-146">Llamar a miembros de un grupo</span><span class="sxs-lookup"><span data-stu-id="a89ce-146">Calling members of a group</span></span>

<span data-ttu-id="a89ce-147">Puede enviar mensajes a todos los miembros de un grupo o solo los miembros especificados del grupo, tal como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="a89ce-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="a89ce-148">**Todos los** conectado a los clientes en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="a89ce-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="a89ce-149">Todos los clientes en un grupo especificado conectados **excepto los clientes especificados**, identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="a89ce-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="a89ce-150">Todos los clientes en un grupo especificado conectados **excepto el cliente que realiza la llamada**.</span><span class="sxs-lookup"><span data-stu-id="a89ce-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="a89ce-151">Almacenamiento de pertenencia a grupos en una base de datos</span><span class="sxs-lookup"><span data-stu-id="a89ce-151">Storing group membership in a database</span></span>

<span data-ttu-id="a89ce-152">Los ejemplos siguientes muestran cómo conservar la información de grupo y usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="a89ce-153">Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente se muestra cómo definir los modelos que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a89ce-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="a89ce-154">Estos modelos de entidad se corresponden con los campos y tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="a89ce-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="a89ce-155">La estructura de datos puede variar considerablemente según los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a89ce-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="a89ce-156">En este ejemplo incluye una clase denominada `ConversationRoom` que sería única para una aplicación que permite a los usuarios para unirse a las conversaciones sobre diferentes temas, como deportes o jardinería.</span><span class="sxs-lookup"><span data-stu-id="a89ce-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="a89ce-157">En este ejemplo también incluye una clase para las conexiones.</span><span class="sxs-lookup"><span data-stu-id="a89ce-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="a89ce-158">La clase de conexión no es estrictamente necesaria para realizar el seguimiento de pertenencia a grupos, pero suele ser parte de una solución sólida para los usuarios de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a89ce-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="a89ce-159">A continuación, en el concentrador, puede recuperar la información de grupo y usuario de la base de datos y agregar manualmente el usuario a los grupos adecuados.</span><span class="sxs-lookup"><span data-stu-id="a89ce-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="a89ce-160">El ejemplo no incluye código para realizar el seguimiento de las conexiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="a89ce-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="a89ce-161">En este ejemplo, el `await` no se aplica la palabra clave antes de `Groups.Add` porque no se envía inmediatamente un mensaje a los miembros del grupo.</span><span class="sxs-lookup"><span data-stu-id="a89ce-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="a89ce-162">Si desea enviar un mensaje a todos los miembros del grupo inmediatamente después de agregar el nuevo miembro, ¿desea aplicar el `await` palabra clave para asegurarse de que se ha completado la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="a89ce-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="a89ce-163">Almacenamiento de pertenencia a grupos en Azure table storage</span><span class="sxs-lookup"><span data-stu-id="a89ce-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="a89ce-164">Es similar al uso de una base de datos mediante Azure table storage para almacenar información de usuario y grupo.</span><span class="sxs-lookup"><span data-stu-id="a89ce-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="a89ce-165">El ejemplo siguiente muestra una entidad de tabla que almacena el nombre de usuario y el nombre del grupo.</span><span class="sxs-lookup"><span data-stu-id="a89ce-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="a89ce-166">En el concentrador, recuperar los grupos asignados cuando se conecta el usuario.</span><span class="sxs-lookup"><span data-stu-id="a89ce-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="a89ce-167">Comprobar la pertenencia al grupo al volver a conectarse</span><span class="sxs-lookup"><span data-stu-id="a89ce-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="a89ce-168">De forma predeterminada, SignalR volver a asigna automáticamente un usuario a los grupos adecuados cuando vuelva a conectar desde una interrupción temporal, por ejemplo, cuando una conexión se quita y vuelve a establecer antes de la conexión agota el tiempo. Información del grupo del usuario se pasa un token al conectarse de nuevo y ese token se comprueba en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a89ce-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="a89ce-169">Para obtener información sobre el proceso de comprobación para unir a los usuarios a grupos, consulte [nueva unión a grupos al volver a conectarse](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="a89ce-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="a89ce-170">En general, debe usar el comportamiento predeterminado de automáticamente unir a grupos en volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="a89ce-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="a89ce-171">Grupos de SignalR no están diseñados como un mecanismo de seguridad para restringir el acceso a datos confidenciales.</span><span class="sxs-lookup"><span data-stu-id="a89ce-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="a89ce-172">Sin embargo, si la aplicación debe comprobar pertenencia a grupos del usuario al conectarse de nuevo, puede invalidar el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a89ce-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="a89ce-173">Cambiar el comportamiento predeterminado puede agregar una carga a la base de datos porque se debe recuperar la pertenencia a grupos del usuario para cada nueva conexión en lugar de simplemente cuando el usuario se conecta.</span><span class="sxs-lookup"><span data-stu-id="a89ce-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="a89ce-174">Si se debe comprobar la pertenencia a grupos en volver a conectarse, cree un nuevo módulo de canalización de concentrador que devuelve una lista de grupos asignados, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="a89ce-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="a89ce-175">A continuación, agregue ese módulo a la canalización de concentrador, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="a89ce-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
