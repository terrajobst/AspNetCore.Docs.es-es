---
title: Usar concentradores en ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo usar concentradores en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 17e3ee23967bc1097a3121b3e3e5b58cebe3887d
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538367"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="220bf-103">Usar concentradores de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="220bf-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="220bf-104">Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="220bf-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="220bf-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="220bf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="220bf-106">¿Qué es un concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="220bf-106">What is a SignalR hub</span></span>

<span data-ttu-id="220bf-107">La API de concentradores de SignalR le permite llamar a métodos en los clientes conectados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="220bf-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="220bf-108">En el código del servidor, se definen métodos llamados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="220bf-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="220bf-109">En el código de cliente, se definen los métodos que se llaman desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="220bf-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="220bf-110">SignalR se ocupa de todo en segundo plano que hace posible la comunicación de servidor de cliente y servidor a cliente en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="220bf-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="220bf-111">Configurar los concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="220bf-111">Configure SignalR hubs</span></span>

<span data-ttu-id="220bf-112">El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="220bf-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="220bf-113">Al agregar la funcionalidad de SignalR a una aplicación ASP.NET Core, configurar las rutas de SignalR mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="220bf-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="220bf-114">Crear y usar concentradores</span><span class="sxs-lookup"><span data-stu-id="220bf-114">Create and use hubs</span></span>

<span data-ttu-id="220bf-115">Crear un centro mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="220bf-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="220bf-116">Los clientes pueden llamar a métodos que se definen como `public`.</span><span class="sxs-lookup"><span data-stu-id="220bf-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="220bf-117">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#.</span><span class="sxs-lookup"><span data-stu-id="220bf-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="220bf-118">SignalR controla la serialización y deserialización de objetos complejos y matrices en sus parámetros y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="220bf-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="220bf-119">El objeto de contexto</span><span class="sxs-lookup"><span data-stu-id="220bf-119">The Context object</span></span>

<span data-ttu-id="220bf-120">El `Hub` clase tiene un `Context` propiedad que contiene las siguientes propiedades con información sobre la conexión:</span><span class="sxs-lookup"><span data-stu-id="220bf-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="220bf-121">Property</span><span class="sxs-lookup"><span data-stu-id="220bf-121">Property</span></span> | <span data-ttu-id="220bf-122">Descripción</span><span class="sxs-lookup"><span data-stu-id="220bf-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="220bf-123">Obtiene el identificador único para la conexión, asignada por SignalR.</span><span class="sxs-lookup"><span data-stu-id="220bf-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="220bf-124">Hay un identificador de conexión para cada conexión.</span><span class="sxs-lookup"><span data-stu-id="220bf-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="220bf-125">Obtiene el [useridentifier](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="220bf-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="220bf-126">De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="220bf-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="220bf-127">Obtiene el `ClaimsPrincipal` asociada con el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="220bf-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="220bf-128">Obtiene una colección de pares clave-valor que se puede usar para compartir datos dentro del ámbito de esta conexión.</span><span class="sxs-lookup"><span data-stu-id="220bf-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="220bf-129">Se pueden almacenar datos en esta colección y conservará para la conexión entre las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="220bf-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="220bf-130">Obtiene la colección de las características disponibles en la conexión.</span><span class="sxs-lookup"><span data-stu-id="220bf-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="220bf-131">Por ahora, esta colección no es necesario en la mayoría de los escenarios, por lo que no se documentan detalladamente todavía.</span><span class="sxs-lookup"><span data-stu-id="220bf-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="220bf-132">Obtiene un `CancellationToken` que le notifica cuando se anula la conexión.</span><span class="sxs-lookup"><span data-stu-id="220bf-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="220bf-133">`Hub.Context` También contiene los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="220bf-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="220bf-134">Método</span><span class="sxs-lookup"><span data-stu-id="220bf-134">Method</span></span> | <span data-ttu-id="220bf-135">Descripción</span><span class="sxs-lookup"><span data-stu-id="220bf-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="220bf-136">Devuelve el `HttpContext` para la conexión, o `null` si la conexión no está asociada a una solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="220bf-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="220bf-137">Para las conexiones HTTP, puede usar este método para obtener información como los encabezados HTTP y las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="220bf-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="220bf-138">Anula la conexión.</span><span class="sxs-lookup"><span data-stu-id="220bf-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="220bf-139">El objeto de los clientes</span><span class="sxs-lookup"><span data-stu-id="220bf-139">The Clients object</span></span>

<span data-ttu-id="220bf-140">El `Hub` clase tiene un `Clients` propiedad que contiene las siguientes propiedades para la comunicación entre cliente y servidor:</span><span class="sxs-lookup"><span data-stu-id="220bf-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="220bf-141">Property</span><span class="sxs-lookup"><span data-stu-id="220bf-141">Property</span></span> | <span data-ttu-id="220bf-142">Descripción</span><span class="sxs-lookup"><span data-stu-id="220bf-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="220bf-143">Llama a un método en todos los clientes conectados</span><span class="sxs-lookup"><span data-stu-id="220bf-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="220bf-144">Llama a un método en el cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="220bf-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="220bf-145">Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método</span><span class="sxs-lookup"><span data-stu-id="220bf-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="220bf-146">`Hub.Clients` También contiene los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="220bf-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="220bf-147">Método</span><span class="sxs-lookup"><span data-stu-id="220bf-147">Method</span></span> | <span data-ttu-id="220bf-148">Descripción</span><span class="sxs-lookup"><span data-stu-id="220bf-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="220bf-149">Llama a un método en todos los clientes conectados, excepto las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="220bf-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="220bf-150">Llama a un método en un cliente específico conectado</span><span class="sxs-lookup"><span data-stu-id="220bf-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="220bf-151">Llame a un método específicos clientes conectados</span><span class="sxs-lookup"><span data-stu-id="220bf-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="220bf-152">Llama a un método a todas las conexiones del grupo especificado</span><span class="sxs-lookup"><span data-stu-id="220bf-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="220bf-153">Llama a un método a todas las conexiones en el grupo especificado, excepto las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="220bf-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="220bf-154">Llama a un método a varios grupos de conexiones</span><span class="sxs-lookup"><span data-stu-id="220bf-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="220bf-155">Llama a un método a un grupo de conexiones, excepto al cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="220bf-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="220bf-156">Llama a un método para todas las conexiones asociadas a un usuario específico</span><span class="sxs-lookup"><span data-stu-id="220bf-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="220bf-157">Llama a un método para todas las conexiones asociadas a los usuarios especificados</span><span class="sxs-lookup"><span data-stu-id="220bf-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="220bf-158">Cada propiedad o método en las tablas anteriores devuelve un objeto con un `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="220bf-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="220bf-159">El `SendAsync` método permite proporcionar el nombre y los parámetros del método para llamar al cliente.</span><span class="sxs-lookup"><span data-stu-id="220bf-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="220bf-160">Enviar mensajes a los clientes</span><span class="sxs-lookup"><span data-stu-id="220bf-160">Send messages to clients</span></span>

<span data-ttu-id="220bf-161">Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="220bf-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="220bf-162">En el ejemplo siguiente, la `SendMessageToCaller` método muestra cómo enviar un mensaje a la conexión que invoca el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="220bf-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="220bf-163">El `SendMessageToGroups` método envía un mensaje a los grupos que se almacenan en un `List` denominado `groups`.</span><span class="sxs-lookup"><span data-stu-id="220bf-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="220bf-164">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="220bf-164">Strongly typed hubs</span></span>

<span data-ttu-id="220bf-165">Una desventaja de utilizar `SendAsync` es que se basa en una cadena mágica para especificar que se llame al método de cliente.</span><span class="sxs-lookup"><span data-stu-id="220bf-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="220bf-166">Esto deja el código abierto a los errores de tiempo de ejecución si el nombre del método está mal escrito o no aparece en el cliente.</span><span class="sxs-lookup"><span data-stu-id="220bf-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="220bf-167">Una alternativa al uso `SendAsync` es asignar rigurosamente los `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="220bf-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="220bf-168">En el ejemplo siguiente, la `ChatHub` métodos de cliente se han extraído alejar en una interfaz denominada `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="220bf-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="220bf-169">Esta interfaz puede usarse para refactorizar anterior `ChatHub` ejemplo.</span><span class="sxs-lookup"><span data-stu-id="220bf-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="220bf-170">Uso de `Hub<IChatClient>` habilita la comprobación de tiempo de compilación de los métodos de cliente.</span><span class="sxs-lookup"><span data-stu-id="220bf-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="220bf-171">Esto evita problemas causados por usar cadenas mágicas, ya que `Hub<T>` solo puede proporcionar acceso a los métodos definidos en la interfaz.</span><span class="sxs-lookup"><span data-stu-id="220bf-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="220bf-172">Usar fuertemente tipado `Hub<T>` deshabilita la posibilidad de usar `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="220bf-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="220bf-173">Controlar los eventos de una conexión</span><span class="sxs-lookup"><span data-stu-id="220bf-173">Handle events for a connection</span></span>

<span data-ttu-id="220bf-174">La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones.</span><span class="sxs-lookup"><span data-stu-id="220bf-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="220bf-175">Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.</span><span class="sxs-lookup"><span data-stu-id="220bf-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="220bf-176">Controlar los errores</span><span class="sxs-lookup"><span data-stu-id="220bf-176">Handle errors</span></span>

<span data-ttu-id="220bf-177">Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método.</span><span class="sxs-lookup"><span data-stu-id="220bf-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="220bf-178">En el cliente de JavaScript, el `invoke` método devuelve un [promesa de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="220bf-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="220bf-179">Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y pasado como un JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="220bf-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="220bf-180">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="220bf-180">Related resources</span></span>

* [<span data-ttu-id="220bf-181">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="220bf-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="220bf-182">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="220bf-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="220bf-183">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="220bf-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
