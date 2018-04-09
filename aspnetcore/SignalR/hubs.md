---
title: Concentradores de uso de núcleo de ASP.NET SignalR
author: rachelappel
description: Obtenga información acerca de cómo usar los centros de ASP.NET Core SignalR.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="25aee-103">Use los concentradores de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25aee-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="25aee-104">Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="25aee-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="25aee-105">¿Qué es un concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="25aee-105">What is a SignalR hub</span></span>

<span data-ttu-id="25aee-106">La API de concentradores de SignalR permite llamar a métodos en los clientes conectados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="25aee-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="25aee-107">En el código del servidor, se definen métodos llamados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="25aee-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="25aee-108">En el código de cliente, se definen métodos que se llaman desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="25aee-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="25aee-109">SignalR se encarga de todo el contenido en segundo plano que permite las comunicaciones de cliente a servidor y cliente de servidor en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="25aee-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="25aee-110">Configurar los concentradores SignalR</span><span class="sxs-lookup"><span data-stu-id="25aee-110">Configure SignalR hubs</span></span>

<span data-ttu-id="25aee-111">El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="25aee-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="25aee-112">Al agregar la funcionalidad de SignalR a una aplicación de ASP.NET Core, el programa de instalación SignalR rutas mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="25aee-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="25aee-113">Crear y usar los centros</span><span class="sxs-lookup"><span data-stu-id="25aee-113">Create and use hubs</span></span>

<span data-ttu-id="25aee-114">Crear un concentrador mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="25aee-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="25aee-115">Los clientes pueden llamar a métodos que se definen como `public`.</span><span class="sxs-lookup"><span data-stu-id="25aee-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="25aee-116">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#.</span><span class="sxs-lookup"><span data-stu-id="25aee-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="25aee-117">SignalR controla la serialización y deserialización de objetos complejos y matrices en los parámetros y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="25aee-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="25aee-118">El objeto de los clientes</span><span class="sxs-lookup"><span data-stu-id="25aee-118">The Clients object</span></span>

<span data-ttu-id="25aee-119">Cada instancia de la `Hub` clase tiene una propiedad denominada `Clients` que contiene los miembros siguientes para la comunicación entre cliente y servidor:</span><span class="sxs-lookup"><span data-stu-id="25aee-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="25aee-120">Property</span><span class="sxs-lookup"><span data-stu-id="25aee-120">Property</span></span> | <span data-ttu-id="25aee-121">Descripción</span><span class="sxs-lookup"><span data-stu-id="25aee-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="25aee-122">Llama a un método en todos los clientes conectados</span><span class="sxs-lookup"><span data-stu-id="25aee-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="25aee-123">Llama a un método en el cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="25aee-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="25aee-124">Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método</span><span class="sxs-lookup"><span data-stu-id="25aee-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="25aee-125">Además, la `Hub` clase contiene los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="25aee-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="25aee-126">Método</span><span class="sxs-lookup"><span data-stu-id="25aee-126">Method</span></span> | <span data-ttu-id="25aee-127">Descripción</span><span class="sxs-lookup"><span data-stu-id="25aee-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="25aee-128">Llama a un método en todos los clientes conectados, excepto para las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="25aee-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="25aee-129">Llama a un método en un cliente conectado específico</span><span class="sxs-lookup"><span data-stu-id="25aee-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="25aee-130">Llama a un método en los clientes conectados específicos</span><span class="sxs-lookup"><span data-stu-id="25aee-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="25aee-131">Envía un mensaje a todas las conexiones en el grupo especificado</span><span class="sxs-lookup"><span data-stu-id="25aee-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="25aee-132">Envía un mensaje a todas las conexiones en el grupo especificado, excepto las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="25aee-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="25aee-133">Envía un mensaje a varios grupos de conexiones</span><span class="sxs-lookup"><span data-stu-id="25aee-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="25aee-134">Envía un mensaje a un grupo de conexiones, excepto al cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="25aee-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="25aee-135">Envía un mensaje a todas las conexiones asociadas a un usuario específico</span><span class="sxs-lookup"><span data-stu-id="25aee-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="25aee-136">Envía un mensaje a todas las conexiones asociadas a los usuarios especificados</span><span class="sxs-lookup"><span data-stu-id="25aee-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="25aee-137">Cada propiedad o método de las tablas anteriores devuelve un objeto con un `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="25aee-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="25aee-138">El `SendAsync` método le permite especificar el nombre y los parámetros del método de cliente para llamar a.</span><span class="sxs-lookup"><span data-stu-id="25aee-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="25aee-139">Enviar mensajes a los clientes</span><span class="sxs-lookup"><span data-stu-id="25aee-139">Send messages to clients</span></span>

<span data-ttu-id="25aee-140">Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="25aee-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="25aee-141">En la siguiente en el ejemplo siguiente, la `SendMessageToCaller` método muestra cómo enviar un mensaje a la conexión que se invoca el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="25aee-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="25aee-142">El `SendMessageToGroups` método envía un mensaje a los grupos almacenados en un `List` denominado `groups`.</span><span class="sxs-lookup"><span data-stu-id="25aee-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="25aee-143">Controlar eventos de una conexión</span><span class="sxs-lookup"><span data-stu-id="25aee-143">Handle events for a connection</span></span>

<span data-ttu-id="25aee-144">La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones.</span><span class="sxs-lookup"><span data-stu-id="25aee-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="25aee-145">Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.</span><span class="sxs-lookup"><span data-stu-id="25aee-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="25aee-146">Controlar errores</span><span class="sxs-lookup"><span data-stu-id="25aee-146">Handle errors</span></span>

<span data-ttu-id="25aee-147">Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método.</span><span class="sxs-lookup"><span data-stu-id="25aee-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="25aee-148">En el cliente de JavaScript, el `invoke` método devuelve un [compromiso de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="25aee-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="25aee-149">Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y se pasan como JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="25aee-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="25aee-150">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="25aee-150">Related resources</span></span>

[<span data-ttu-id="25aee-151">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="25aee-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)