---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Asignar usuarios de SignalR a las conexiones | Microsoft Docs
author: bradygaster
description: En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones. Patrick Fletcher ayudó a escribir en este tema. Versiones de software que se usa en este tema...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 786fc6bbc0b8d430770cf19d1647dbdba26347aa
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837200"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="5058b-105">Asignar usuarios de SignalR a las conexiones</span><span class="sxs-lookup"><span data-stu-id="5058b-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="5058b-106">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5058b-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5058b-107">En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones.</span><span class="sxs-lookup"><span data-stu-id="5058b-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="5058b-108">Patrick Fletcher ayudó a escribir en este tema.</span><span class="sxs-lookup"><span data-stu-id="5058b-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5058b-109">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="5058b-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5058b-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5058b-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="5058b-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5058b-111">.NET 4.5</span></span>
> - <span data-ttu-id="5058b-112">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="5058b-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5058b-113">Versiones anteriores de este tema.</span><span class="sxs-lookup"><span data-stu-id="5058b-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5058b-114">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5058b-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5058b-115">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="5058b-115">Questions and comments</span></span>
>
> <span data-ttu-id="5058b-116">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="5058b-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5058b-117">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5058b-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="5058b-118">Introducción</span><span class="sxs-lookup"><span data-stu-id="5058b-118">Introduction</span></span>

<span data-ttu-id="5058b-119">Cada cliente que se conecta a un concentrador pasa un identificador único de conexión. Puede recuperar este valor en el `Context.ConnectionId` propiedad del contexto del concentrador.</span><span class="sxs-lookup"><span data-stu-id="5058b-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="5058b-120">Si la aplicación necesita asignar un usuario para el identificador de conexión y almacenar esa asignación, puede usar uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5058b-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="5058b-121">El proveedor de Id. de usuario (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="5058b-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="5058b-122">[Almacenamiento en memoria](#inmemory), por ejemplo, un diccionario</span><span class="sxs-lookup"><span data-stu-id="5058b-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="5058b-123">Grupo de SignalR para cada usuario</span><span class="sxs-lookup"><span data-stu-id="5058b-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="5058b-124">[Almacenamiento externo, permanente](#database), como una tabla de base de datos o almacenamiento de tablas de Azure</span><span class="sxs-lookup"><span data-stu-id="5058b-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="5058b-125">En este tema se muestra cada una de estas implementaciones.</span><span class="sxs-lookup"><span data-stu-id="5058b-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="5058b-126">Usa el `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos de la `Hub` clase para realizar un seguimiento del estado de conexión de usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="5058b-127">El mejor enfoque para la aplicación depende de:</span><span class="sxs-lookup"><span data-stu-id="5058b-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="5058b-128">El número de servidores web que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5058b-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="5058b-129">Si necesita obtener una lista de los usuarios conectados actualmente.</span><span class="sxs-lookup"><span data-stu-id="5058b-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="5058b-130">Si necesita conservar información de usuario y grupo cuando se reinicia la aplicación o el servidor.</span><span class="sxs-lookup"><span data-stu-id="5058b-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="5058b-131">Si la latencia de la llamada a un servidor externo es un problema.</span><span class="sxs-lookup"><span data-stu-id="5058b-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="5058b-132">La siguiente tabla muestra qué enfoque funciona para estas consideraciones.</span><span class="sxs-lookup"><span data-stu-id="5058b-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="5058b-133">Más de un servidor</span><span class="sxs-lookup"><span data-stu-id="5058b-133">More than one server</span></span> | <span data-ttu-id="5058b-134">Obtiene una lista de los usuarios conectados actualmente</span><span class="sxs-lookup"><span data-stu-id="5058b-134">Get list of currently connected users</span></span> | <span data-ttu-id="5058b-135">Conservar la información una vez reiniciado</span><span class="sxs-lookup"><span data-stu-id="5058b-135">Persist information after restarts</span></span> | <span data-ttu-id="5058b-136">Obtener un rendimiento óptimo</span><span class="sxs-lookup"><span data-stu-id="5058b-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="5058b-137">Proveedor de identificador de usuario</span><span class="sxs-lookup"><span data-stu-id="5058b-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="5058b-138">En memoria</span><span class="sxs-lookup"><span data-stu-id="5058b-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="5058b-139">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="5058b-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="5058b-140">Permanente, externa</span><span class="sxs-lookup"><span data-stu-id="5058b-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="5058b-141">Proveedor IUserID</span><span class="sxs-lookup"><span data-stu-id="5058b-141">IUserID provider</span></span>

<span data-ttu-id="5058b-142">Esta característica permite a los usuarios especificar lo que es el identificador de usuario en función de un objeto IRequest a través de una nueva interfaz IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="5058b-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="5058b-143">**El IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="5058b-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="5058b-144">De forma predeterminada, será una implementación que usa el usuario `IPrincipal.Identity.Name` como el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="5058b-145">Para cambiar esto, registrar su implementación de `IUserIdProvider` con el host global cuando se inicia la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5058b-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="5058b-146">Desde dentro de un concentrador, podrá enviar mensajes a estos usuarios a través de la siguiente API:</span><span class="sxs-lookup"><span data-stu-id="5058b-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="5058b-147">**Enviar un mensaje a un usuario específico**</span><span class="sxs-lookup"><span data-stu-id="5058b-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="5058b-148">Almacenamiento en memoria</span><span class="sxs-lookup"><span data-stu-id="5058b-148">In-memory storage</span></span>

<span data-ttu-id="5058b-149">Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en un diccionario que se almacena en memoria.</span><span class="sxs-lookup"><span data-stu-id="5058b-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="5058b-150">Usa el diccionario un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5058b-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="5058b-151">Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5058b-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="5058b-152">Si cierra la aplicación, se pierde toda la información, pero se vuelve a lo rellenará los usuarios restablecer sus conexiones.</span><span class="sxs-lookup"><span data-stu-id="5058b-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="5058b-153">Almacenamiento en memoria no funciona si su entorno incluye más de un servidor web, ya que cada servidor tendría una colección independiente de conexiones.</span><span class="sxs-lookup"><span data-stu-id="5058b-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="5058b-154">El primer ejemplo muestra una clase que administra la asignación de usuarios a las conexiones.</span><span class="sxs-lookup"><span data-stu-id="5058b-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="5058b-155">La clave para el objeto HashSet será el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="5058b-156">En el ejemplo siguiente se muestra cómo usar la clase de asignación de conexión de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="5058b-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="5058b-157">La instancia de la clase se almacena en un nombre de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="5058b-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="5058b-158">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="5058b-158">Single-user groups</span></span>

<span data-ttu-id="5058b-159">Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando desea llegar a solo ese usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="5058b-160">El nombre de cada grupo es el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="5058b-161">Si un usuario tiene más de una conexión, se agrega cada identificador de conexión para el grupo del usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="5058b-162">Se debería no quitar manualmente el usuario del grupo cuando el usuario se desconecta.</span><span class="sxs-lookup"><span data-stu-id="5058b-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="5058b-163">Esta acción se realiza automáticamente el marco de trabajo de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5058b-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="5058b-164">El ejemplo siguiente muestra cómo implementar grupos de usuario único.</span><span class="sxs-lookup"><span data-stu-id="5058b-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="5058b-165">Almacenamiento externo, permanente</span><span class="sxs-lookup"><span data-stu-id="5058b-165">Permanent, external storage</span></span>

<span data-ttu-id="5058b-166">En este tema se muestra cómo usar una base de datos o el almacenamiento de tablas de Azure para almacenar información de conexión.</span><span class="sxs-lookup"><span data-stu-id="5058b-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="5058b-167">Este enfoque funciona cuando tiene varios servidores web, ya que cada servidor web puede interactuar con el mismo repositorio de datos.</span><span class="sxs-lookup"><span data-stu-id="5058b-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="5058b-168">Si los servidores web deja de funcionar o se reinicie la aplicación, el `OnDisconnected` no se llama al método.</span><span class="sxs-lookup"><span data-stu-id="5058b-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="5058b-169">Por lo tanto, es posible que el repositorio de datos tendrá los registros para los identificadores de conexión que ya no son válidos.</span><span class="sxs-lookup"><span data-stu-id="5058b-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="5058b-170">Para limpiar estos registros huérfanos, puede que desee invalidar cualquier conexión que se creó fuera de un período de tiempo que sea relevante para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5058b-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="5058b-171">Los ejemplos en esta sección incluyen un valor para el seguimiento cuando se creó la conexión, pero no se muestra cómo limpiar los registros antiguos, porque es posible que desee hacerlo como proceso en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="5058b-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="5058b-172">Base de datos</span><span class="sxs-lookup"><span data-stu-id="5058b-172">Database</span></span>

<span data-ttu-id="5058b-173">Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5058b-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="5058b-174">Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente se muestra cómo definir los modelos que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5058b-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="5058b-175">Estos modelos de entidad se corresponden con los campos y tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="5058b-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="5058b-176">La estructura de datos puede variar considerablemente según los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5058b-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="5058b-177">El primer ejemplo muestra cómo definir una entidad de usuario que se puede asociar con muchas entidades de la conexión.</span><span class="sxs-lookup"><span data-stu-id="5058b-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="5058b-178">A continuación, desde el concentrador, puede seguir el estado de cada conexión con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5058b-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="5058b-179">Almacenamiento de tablas de Azure</span><span class="sxs-lookup"><span data-stu-id="5058b-179">Azure table storage</span></span>

<span data-ttu-id="5058b-180">En el siguiente ejemplo de almacenamiento de tablas de Azure es similar a la base de datos ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5058b-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="5058b-181">No incluir toda la información que necesita para empezar a trabajar con el servicio de Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="5058b-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="5058b-182">Para obtener información, consulte [cómo usar Table storage en .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="5058b-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="5058b-183">El ejemplo siguiente muestra una entidad de tabla para almacenar información de conexión.</span><span class="sxs-lookup"><span data-stu-id="5058b-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="5058b-184">Divide los datos por nombre de usuario y que identifica cada entidad con el identificador de la conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="5058b-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="5058b-185">En el centro, realizar un seguimiento del estado de conexión de cada usuario.</span><span class="sxs-lookup"><span data-stu-id="5058b-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
