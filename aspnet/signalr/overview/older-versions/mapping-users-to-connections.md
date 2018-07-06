---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Asignar usuarios de SignalR a las conexiones en SignalR 1.x | Microsoft Docs
author: pfletcher
description: En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 02ee9468ae4198af47226cdd5c22243f16e20da4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818118"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="7c437-103">Asignar usuarios de SignalR a las conexiones en SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="7c437-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="7c437-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7c437-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7c437-105">En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones.</span><span class="sxs-lookup"><span data-stu-id="7c437-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="7c437-106">Introducción</span><span class="sxs-lookup"><span data-stu-id="7c437-106">Introduction</span></span>

<span data-ttu-id="7c437-107">Cada cliente que se conecta a un concentrador pasa un identificador único de conexión. Puede recuperar este valor en el `Context.ConnectionId` propiedad del contexto del concentrador.</span><span class="sxs-lookup"><span data-stu-id="7c437-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="7c437-108">Si la aplicación necesita asignar un usuario para el identificador de conexión y almacenar esa asignación, puede usar uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="7c437-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="7c437-109">[Almacenamiento en memoria](#inmemory), por ejemplo, un diccionario</span><span class="sxs-lookup"><span data-stu-id="7c437-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="7c437-110">Grupo de SignalR para cada usuario</span><span class="sxs-lookup"><span data-stu-id="7c437-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="7c437-111">[Almacenamiento externo, permanente](#database), como una tabla de base de datos o almacenamiento de tablas de Azure</span><span class="sxs-lookup"><span data-stu-id="7c437-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="7c437-112">En este tema se muestra cada una de estas implementaciones.</span><span class="sxs-lookup"><span data-stu-id="7c437-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="7c437-113">Usa el `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos de la `Hub` clase para realizar un seguimiento del estado de conexión de usuario.</span><span class="sxs-lookup"><span data-stu-id="7c437-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="7c437-114">El mejor enfoque para la aplicación depende de:</span><span class="sxs-lookup"><span data-stu-id="7c437-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="7c437-115">El número de servidores web que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c437-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="7c437-116">Si necesita obtener una lista de los usuarios conectados actualmente.</span><span class="sxs-lookup"><span data-stu-id="7c437-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="7c437-117">Si necesita conservar información de usuario y grupo cuando se reinicia la aplicación o el servidor.</span><span class="sxs-lookup"><span data-stu-id="7c437-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="7c437-118">Si la latencia de la llamada a un servidor externo es un problema.</span><span class="sxs-lookup"><span data-stu-id="7c437-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="7c437-119">La siguiente tabla muestra qué enfoque funciona para estas consideraciones.</span><span class="sxs-lookup"><span data-stu-id="7c437-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="7c437-120">Más de un servidor</span><span class="sxs-lookup"><span data-stu-id="7c437-120">More than one server</span></span> | <span data-ttu-id="7c437-121">Obtiene una lista de los usuarios conectados actualmente</span><span class="sxs-lookup"><span data-stu-id="7c437-121">Get list of currently connected users</span></span> | <span data-ttu-id="7c437-122">Conservar la información una vez reiniciado</span><span class="sxs-lookup"><span data-stu-id="7c437-122">Persist information after restarts</span></span> | <span data-ttu-id="7c437-123">Obtener un rendimiento óptimo</span><span class="sxs-lookup"><span data-stu-id="7c437-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="7c437-124">En memoria</span><span class="sxs-lookup"><span data-stu-id="7c437-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="7c437-125">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="7c437-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="7c437-126">Permanente, externa</span><span class="sxs-lookup"><span data-stu-id="7c437-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="7c437-127">Almacenamiento en memoria</span><span class="sxs-lookup"><span data-stu-id="7c437-127">In-memory storage</span></span>

<span data-ttu-id="7c437-128">Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en un diccionario que se almacena en memoria.</span><span class="sxs-lookup"><span data-stu-id="7c437-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="7c437-129">Usa el diccionario un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7c437-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="7c437-130">Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="7c437-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="7c437-131">Si cierra la aplicación, se pierde toda la información, pero se vuelve a lo rellenará los usuarios restablecer sus conexiones.</span><span class="sxs-lookup"><span data-stu-id="7c437-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="7c437-132">Almacenamiento en memoria no funciona si su entorno incluye más de un servidor web, ya que cada servidor tendría una colección independiente de conexiones.</span><span class="sxs-lookup"><span data-stu-id="7c437-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="7c437-133">El primer ejemplo muestra una clase que administra la asignación de usuarios a las conexiones.</span><span class="sxs-lookup"><span data-stu-id="7c437-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="7c437-134">La clave para el objeto HashSet será el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="7c437-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="7c437-135">En el ejemplo siguiente se muestra cómo usar la clase de asignación de conexión de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="7c437-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="7c437-136">La instancia de la clase se almacena en un nombre de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="7c437-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="7c437-137">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="7c437-137">Single-user groups</span></span>

<span data-ttu-id="7c437-138">Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando desea llegar a solo ese usuario.</span><span class="sxs-lookup"><span data-stu-id="7c437-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="7c437-139">El nombre de cada grupo es el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="7c437-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="7c437-140">Si un usuario tiene más de una conexión, se agrega cada identificador de conexión para el grupo del usuario.</span><span class="sxs-lookup"><span data-stu-id="7c437-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="7c437-141">Se debería no quitar manualmente el usuario del grupo cuando el usuario se desconecta.</span><span class="sxs-lookup"><span data-stu-id="7c437-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="7c437-142">Esta acción se realiza automáticamente el marco de trabajo de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7c437-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="7c437-143">El ejemplo siguiente muestra cómo implementar grupos de usuario único.</span><span class="sxs-lookup"><span data-stu-id="7c437-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="7c437-144">Almacenamiento externo, permanente</span><span class="sxs-lookup"><span data-stu-id="7c437-144">Permanent, external storage</span></span>

<span data-ttu-id="7c437-145">En este tema se muestra cómo usar una base de datos o el almacenamiento de tablas de Azure para almacenar información de conexión.</span><span class="sxs-lookup"><span data-stu-id="7c437-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="7c437-146">Este enfoque funciona cuando tiene varios servidores web, ya que cada servidor web puede interactuar con el mismo repositorio de datos.</span><span class="sxs-lookup"><span data-stu-id="7c437-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="7c437-147">Si los servidores web deja de funcionar o se reinicie la aplicación, el `OnDisconnected` no se llama al método.</span><span class="sxs-lookup"><span data-stu-id="7c437-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="7c437-148">Por lo tanto, es posible que el repositorio de datos tendrá los registros para los identificadores de conexión que ya no son válidos.</span><span class="sxs-lookup"><span data-stu-id="7c437-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="7c437-149">Para limpiar estos registros huérfanos, puede que desee invalidar cualquier conexión que se creó fuera de un período de tiempo que sea relevante para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c437-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="7c437-150">Los ejemplos en esta sección incluyen un valor para el seguimiento cuando se creó la conexión, pero no se muestra cómo limpiar los registros antiguos, porque es posible que desee hacerlo como proceso en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7c437-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="7c437-151">Base de datos</span><span class="sxs-lookup"><span data-stu-id="7c437-151">Database</span></span>

<span data-ttu-id="7c437-152">Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c437-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="7c437-153">Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente se muestra cómo definir los modelos que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7c437-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="7c437-154">Estos modelos de entidad se corresponden con los campos y tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c437-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="7c437-155">La estructura de datos puede variar considerablemente según los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c437-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="7c437-156">El primer ejemplo muestra cómo definir una entidad de usuario que se puede asociar con muchas entidades de la conexión.</span><span class="sxs-lookup"><span data-stu-id="7c437-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="7c437-157">A continuación, desde el concentrador, puede seguir el estado de cada conexión con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7c437-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="7c437-158">Almacenamiento de tablas de Azure</span><span class="sxs-lookup"><span data-stu-id="7c437-158">Azure table storage</span></span>

<span data-ttu-id="7c437-159">En el siguiente ejemplo de almacenamiento de tablas de Azure es similar a la base de datos ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7c437-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="7c437-160">No incluir toda la información que necesita para empezar a trabajar con el servicio de Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="7c437-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="7c437-161">Para obtener información, consulte [cómo usar Table storage en .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="7c437-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="7c437-162">El ejemplo siguiente muestra una entidad de tabla para almacenar información de conexión.</span><span class="sxs-lookup"><span data-stu-id="7c437-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="7c437-163">Divide los datos por nombre de usuario y que identifica cada entidad con el identificador de la conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="7c437-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="7c437-164">En el centro, realizar un seguimiento del estado de conexión de cada usuario.</span><span class="sxs-lookup"><span data-stu-id="7c437-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
