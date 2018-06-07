---
title: Concentradores de uso de núcleo de ASP.NET SignalR
author: rachelappel
description: Obtenga información acerca de cómo usar los centros de ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 495aa156dd5e4641d688d7b16df1e5814c9607f4
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819089"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="34b88-103">Use los concentradores de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34b88-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="34b88-104">Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="34b88-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="34b88-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="34b88-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="34b88-106">¿Qué es un concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="34b88-106">What is a SignalR hub</span></span>

<span data-ttu-id="34b88-107">La API de concentradores de SignalR permite llamar a métodos en los clientes conectados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="34b88-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="34b88-108">En el código del servidor, se definen métodos llamados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="34b88-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="34b88-109">En el código de cliente, se definen métodos que se llaman desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="34b88-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="34b88-110">SignalR se encarga de todo el contenido en segundo plano que permite las comunicaciones de cliente a servidor y cliente de servidor en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="34b88-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="34b88-111">Configurar los concentradores SignalR</span><span class="sxs-lookup"><span data-stu-id="34b88-111">Configure SignalR hubs</span></span>

<span data-ttu-id="34b88-112">El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="34b88-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="34b88-113">Al agregar la funcionalidad de SignalR a una aplicación de ASP.NET Core, el programa de instalación SignalR rutas mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="34b88-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="34b88-114">Crear y usar los centros</span><span class="sxs-lookup"><span data-stu-id="34b88-114">Create and use hubs</span></span>

<span data-ttu-id="34b88-115">Crear un concentrador mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="34b88-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="34b88-116">Los clientes pueden llamar a métodos que se definen como `public`.</span><span class="sxs-lookup"><span data-stu-id="34b88-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="34b88-117">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#.</span><span class="sxs-lookup"><span data-stu-id="34b88-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="34b88-118">SignalR controla la serialización y deserialización de objetos complejos y matrices en los parámetros y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="34b88-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="34b88-119">El objeto de los clientes</span><span class="sxs-lookup"><span data-stu-id="34b88-119">The Clients object</span></span>

<span data-ttu-id="34b88-120">Cada instancia de la `Hub` clase tiene una propiedad denominada `Clients` que contiene los miembros siguientes para la comunicación entre cliente y servidor:</span><span class="sxs-lookup"><span data-stu-id="34b88-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="34b88-121">Property</span><span class="sxs-lookup"><span data-stu-id="34b88-121">Property</span></span> | <span data-ttu-id="34b88-122">Descripción</span><span class="sxs-lookup"><span data-stu-id="34b88-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="34b88-123">Llama a un método en todos los clientes conectados</span><span class="sxs-lookup"><span data-stu-id="34b88-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="34b88-124">Llama a un método en el cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="34b88-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="34b88-125">Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método</span><span class="sxs-lookup"><span data-stu-id="34b88-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="34b88-126">Además, `Hub.Clients` contiene los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="34b88-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="34b88-127">Método</span><span class="sxs-lookup"><span data-stu-id="34b88-127">Method</span></span> | <span data-ttu-id="34b88-128">Descripción</span><span class="sxs-lookup"><span data-stu-id="34b88-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="34b88-129">Llama a un método en todos los clientes conectados, excepto para las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="34b88-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="34b88-130">Llama a un método en un cliente conectado específico</span><span class="sxs-lookup"><span data-stu-id="34b88-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="34b88-131">Llama a un método en los clientes conectados específicos</span><span class="sxs-lookup"><span data-stu-id="34b88-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="34b88-132">Llama a un método a todas las conexiones en el grupo especificado</span><span class="sxs-lookup"><span data-stu-id="34b88-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="34b88-133">Llama a un método a todas las conexiones en el grupo especificado, excepto las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="34b88-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="34b88-134">Llama a un método a varios grupos de conexiones</span><span class="sxs-lookup"><span data-stu-id="34b88-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="34b88-135">Llama a un método a un grupo de conexiones, excepto al cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="34b88-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="34b88-136">Llama a un método para todas las conexiones asociadas a un usuario específico</span><span class="sxs-lookup"><span data-stu-id="34b88-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="34b88-137">Llama a un método para todas las conexiones asociadas a los usuarios especificados</span><span class="sxs-lookup"><span data-stu-id="34b88-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="34b88-138">Cada propiedad o método de las tablas anteriores devuelve un objeto con un `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="34b88-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="34b88-139">El `SendAsync` método le permite especificar el nombre y los parámetros del método de cliente para llamar a.</span><span class="sxs-lookup"><span data-stu-id="34b88-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="34b88-140">Enviar mensajes a los clientes</span><span class="sxs-lookup"><span data-stu-id="34b88-140">Send messages to clients</span></span>

<span data-ttu-id="34b88-141">Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="34b88-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="34b88-142">En el ejemplo siguiente, la `SendMessageToCaller` método muestra cómo enviar un mensaje a la conexión que se invoca el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="34b88-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="34b88-143">El `SendMessageToGroups` método envía un mensaje a los grupos almacenados en un `List` denominado `groups`.</span><span class="sxs-lookup"><span data-stu-id="34b88-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="34b88-144">Controlar eventos de una conexión</span><span class="sxs-lookup"><span data-stu-id="34b88-144">Handle events for a connection</span></span>

<span data-ttu-id="34b88-145">La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones.</span><span class="sxs-lookup"><span data-stu-id="34b88-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="34b88-146">Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.</span><span class="sxs-lookup"><span data-stu-id="34b88-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="34b88-147">Controlar errores</span><span class="sxs-lookup"><span data-stu-id="34b88-147">Handle errors</span></span>

<span data-ttu-id="34b88-148">Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método.</span><span class="sxs-lookup"><span data-stu-id="34b88-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="34b88-149">En el cliente de JavaScript, el `invoke` método devuelve un [compromiso de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="34b88-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="34b88-150">Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y se pasan como JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="34b88-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="34b88-151">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="34b88-151">Related resources</span></span>

* [<span data-ttu-id="34b88-152">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="34b88-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="34b88-153">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b88-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="34b88-154">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="34b88-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
