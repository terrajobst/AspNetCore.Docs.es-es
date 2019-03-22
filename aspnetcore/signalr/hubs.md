---
title: Usar concentradores en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo usar concentradores en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: f037c1f6fd7ef773b8e7b2fd4fdf6e28222c441a
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327266"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="5cece-103">Usar concentradores de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cece-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="5cece-104">Por [Rachel Appel](https://twitter.com/rachelappel) y [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="5cece-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="5cece-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="5cece-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="5cece-106">¿Qué es un concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="5cece-106">What is a SignalR hub</span></span>

<span data-ttu-id="5cece-107">La API de concentradores de SignalR le permite llamar a métodos en los clientes conectados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="5cece-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="5cece-108">En el código del servidor, se definen métodos llamados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="5cece-109">En el código de cliente, se definen los métodos que se llaman desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="5cece-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="5cece-110">SignalR se ocupa de todo en segundo plano que hace posible la comunicación de servidor de cliente y servidor a cliente en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="5cece-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="5cece-111">Configurar los concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="5cece-111">Configure SignalR hubs</span></span>

<span data-ttu-id="5cece-112">El middleware de SignalR requiere algunos servicios, que se configuran mediante una llamada a `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="5cece-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="5cece-113">Al agregar la funcionalidad de SignalR a una aplicación ASP.NET Core, configurar las rutas de SignalR mediante una llamada a `app.UseSignalR` en el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="5cece-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="5cece-114">Crear y usar concentradores</span><span class="sxs-lookup"><span data-stu-id="5cece-114">Create and use hubs</span></span>

<span data-ttu-id="5cece-115">Crear un centro mediante la declaración de una clase que hereda de `Hub`y agregar métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="5cece-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="5cece-116">Los clientes pueden llamar a métodos que se definen como `public`.</span><span class="sxs-lookup"><span data-stu-id="5cece-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="5cece-117">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#.</span><span class="sxs-lookup"><span data-stu-id="5cece-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="5cece-118">SignalR controla la serialización y deserialización de objetos complejos y matrices en sus parámetros y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="5cece-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="5cece-119">Los concentradores son transitorios:</span><span class="sxs-lookup"><span data-stu-id="5cece-119">Hubs are transient:</span></span>
>
> * <span data-ttu-id="5cece-120">No almacene el estado en una propiedad de la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5cece-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="5cece-121">Cada llamada al método de concentrador se ejecuta en una nueva instancia de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5cece-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="5cece-122">Use `await` al llamar a métodos asincrónicos que dependen del concentrador permanecer activo.</span><span class="sxs-lookup"><span data-stu-id="5cece-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="5cece-123">Por ejemplo, un método como `Clients.All.SendAsync(...)` puede producir un error si se llama sin `await` y el método de concentrador se completa antes de `SendAsync` finaliza.</span><span class="sxs-lookup"><span data-stu-id="5cece-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="5cece-124">El objeto de contexto</span><span class="sxs-lookup"><span data-stu-id="5cece-124">The Context object</span></span>

<span data-ttu-id="5cece-125">El `Hub` clase tiene un `Context` propiedad que contiene las siguientes propiedades con información sobre la conexión:</span><span class="sxs-lookup"><span data-stu-id="5cece-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="5cece-126">Property</span><span class="sxs-lookup"><span data-stu-id="5cece-126">Property</span></span> | <span data-ttu-id="5cece-127">Descripción</span><span class="sxs-lookup"><span data-stu-id="5cece-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="5cece-128">Obtiene el identificador único para la conexión, asignada por SignalR.</span><span class="sxs-lookup"><span data-stu-id="5cece-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="5cece-129">Hay un identificador de conexión para cada conexión.</span><span class="sxs-lookup"><span data-stu-id="5cece-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="5cece-130">Obtiene el [useridentifier](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="5cece-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="5cece-131">De forma predeterminada, usa SignalR el `ClaimTypes.NameIdentifier` desde el `ClaimsPrincipal` asociado a la conexión como el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="5cece-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="5cece-132">Obtiene el `ClaimsPrincipal` asociada con el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="5cece-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="5cece-133">Obtiene una colección de pares clave-valor que se puede usar para compartir datos dentro del ámbito de esta conexión.</span><span class="sxs-lookup"><span data-stu-id="5cece-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="5cece-134">Se pueden almacenar datos en esta colección y conservará para la conexión entre las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5cece-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="5cece-135">Obtiene la colección de las características disponibles en la conexión.</span><span class="sxs-lookup"><span data-stu-id="5cece-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="5cece-136">Por ahora, esta colección no es necesario en la mayoría de los escenarios, por lo que no se documentan detalladamente todavía.</span><span class="sxs-lookup"><span data-stu-id="5cece-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="5cece-137">Obtiene un `CancellationToken` que le notifica cuando se anula la conexión.</span><span class="sxs-lookup"><span data-stu-id="5cece-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="5cece-138">`Hub.Context` También contiene los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5cece-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="5cece-139">Método</span><span class="sxs-lookup"><span data-stu-id="5cece-139">Method</span></span> | <span data-ttu-id="5cece-140">Descripción</span><span class="sxs-lookup"><span data-stu-id="5cece-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="5cece-141">Devuelve el `HttpContext` para la conexión, o `null` si la conexión no está asociada a una solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="5cece-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="5cece-142">Para las conexiones HTTP, puede usar este método para obtener información como los encabezados HTTP y las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="5cece-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="5cece-143">Anula la conexión.</span><span class="sxs-lookup"><span data-stu-id="5cece-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="5cece-144">El objeto de los clientes</span><span class="sxs-lookup"><span data-stu-id="5cece-144">The Clients object</span></span>

<span data-ttu-id="5cece-145">El `Hub` clase tiene un `Clients` propiedad que contiene las siguientes propiedades para la comunicación entre cliente y servidor:</span><span class="sxs-lookup"><span data-stu-id="5cece-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="5cece-146">Property</span><span class="sxs-lookup"><span data-stu-id="5cece-146">Property</span></span> | <span data-ttu-id="5cece-147">Descripción</span><span class="sxs-lookup"><span data-stu-id="5cece-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="5cece-148">Llama a un método en todos los clientes conectados</span><span class="sxs-lookup"><span data-stu-id="5cece-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="5cece-149">Llama a un método en el cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="5cece-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="5cece-150">Llama a un método en todos los clientes conectados, excepto el cliente que invocó el método</span><span class="sxs-lookup"><span data-stu-id="5cece-150">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="5cece-151">`Hub.Clients` También contiene los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5cece-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="5cece-152">Método</span><span class="sxs-lookup"><span data-stu-id="5cece-152">Method</span></span> | <span data-ttu-id="5cece-153">Descripción</span><span class="sxs-lookup"><span data-stu-id="5cece-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="5cece-154">Llama a un método en todos los clientes conectados, excepto las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="5cece-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="5cece-155">Llama a un método en un cliente específico conectado</span><span class="sxs-lookup"><span data-stu-id="5cece-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="5cece-156">Llame a un método específicos clientes conectados</span><span class="sxs-lookup"><span data-stu-id="5cece-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="5cece-157">Llama a un método en todas las conexiones del grupo especificado</span><span class="sxs-lookup"><span data-stu-id="5cece-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="5cece-158">Llama a un método en todas las conexiones en el grupo especificado, excepto las conexiones especificadas</span><span class="sxs-lookup"><span data-stu-id="5cece-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="5cece-159">Llama a un método en varios grupos de conexiones</span><span class="sxs-lookup"><span data-stu-id="5cece-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="5cece-160">Llama a un método en un grupo de conexiones, excepto al cliente que invoca el método de concentrador</span><span class="sxs-lookup"><span data-stu-id="5cece-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="5cece-161">Llama a un método en todas las conexiones asociadas a un usuario específico</span><span class="sxs-lookup"><span data-stu-id="5cece-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="5cece-162">Llama a un método en todas las conexiones asociadas a los usuarios especificados</span><span class="sxs-lookup"><span data-stu-id="5cece-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="5cece-163">Cada propiedad o método en las tablas anteriores devuelve un objeto con un `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="5cece-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="5cece-164">El `SendAsync` método permite proporcionar el nombre y los parámetros del método para llamar al cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="5cece-165">Enviar mensajes a los clientes</span><span class="sxs-lookup"><span data-stu-id="5cece-165">Send messages to clients</span></span>

<span data-ttu-id="5cece-166">Para realizar llamadas a clientes específicos, use las propiedades de la `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="5cece-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="5cece-167">En el ejemplo siguiente, hay tres métodos de concentrador:</span><span class="sxs-lookup"><span data-stu-id="5cece-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="5cece-168">`SendMessage` envía un mensaje a todos los clientes conectados, mediante `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="5cece-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="5cece-169">`SendMessageToCaller` envía un mensaje de vuelta al llamador, mediante `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="5cece-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="5cece-170">`SendMessageToGroups` envía un mensaje a todos los clientes de la `SignalR Users` grupo.</span><span class="sxs-lookup"><span data-stu-id="5cece-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="5cece-171">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="5cece-171">Strongly typed hubs</span></span>

<span data-ttu-id="5cece-172">Una desventaja de utilizar `SendAsync` es que se basa en una cadena mágica para especificar que se llame al método de cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="5cece-173">Esto deja el código abierto a los errores de tiempo de ejecución si el nombre del método está mal escrito o no aparece en el cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="5cece-174">Una alternativa al uso `SendAsync` es asignar rigurosamente los `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span><span class="sxs-lookup"><span data-stu-id="5cece-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span></span> <span data-ttu-id="5cece-175">En el ejemplo siguiente, la `ChatHub` métodos de cliente se han extraído alejar en una interfaz denominada `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="5cece-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="5cece-176">Esta interfaz puede usarse para refactorizar anterior `ChatHub` ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5cece-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="5cece-177">Uso de `Hub<IChatClient>` habilita la comprobación de tiempo de compilación de los métodos de cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="5cece-178">Esto evita problemas causados por usar cadenas mágicas, ya que `Hub<T>` solo puede proporcionar acceso a los métodos definidos en la interfaz.</span><span class="sxs-lookup"><span data-stu-id="5cece-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="5cece-179">Usar fuertemente tipado `Hub<T>` deshabilita la posibilidad de usar `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="5cece-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="5cece-180">Cualquier método definido en la interfaz todavía se puede definir como asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="5cece-180">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="5cece-181">De hecho, cada uno de estos métodos debe devolver un `Task`.</span><span class="sxs-lookup"><span data-stu-id="5cece-181">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="5cece-182">Puesto que es una interfaz, no use el `async` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="5cece-182">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="5cece-183">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5cece-183">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="5cece-184">El `Async` sufijo no se quitará el nombre del método.</span><span class="sxs-lookup"><span data-stu-id="5cece-184">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="5cece-185">A menos que se define el método de cliente con `.on('MyMethodAsync')`, no debe usar `MyMethodAsync` como un nombre.</span><span class="sxs-lookup"><span data-stu-id="5cece-185">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="5cece-186">Cambiar el nombre de un método de concentrador</span><span class="sxs-lookup"><span data-stu-id="5cece-186">Change the name of a hub method</span></span>

<span data-ttu-id="5cece-187">De forma predeterminada, un nombre de método de concentrador de servidor es el nombre del método. NET.</span><span class="sxs-lookup"><span data-stu-id="5cece-187">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="5cece-188">Sin embargo, puede usar el [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) atributo para cambiar este comportamiento predeterminado y especificar manualmente un nombre para el método.</span><span class="sxs-lookup"><span data-stu-id="5cece-188">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="5cece-189">El cliente debería utilizar este nombre, en lugar del nombre de método. NET, al invocar el método.</span><span class="sxs-lookup"><span data-stu-id="5cece-189">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="5cece-190">Controlar los eventos de una conexión</span><span class="sxs-lookup"><span data-stu-id="5cece-190">Handle events for a connection</span></span>

<span data-ttu-id="5cece-191">La API de concentradores de SignalR proporciona el `OnConnectedAsync` y `OnDisconnectedAsync` métodos virtuales para administrar y realizar un seguimiento de las conexiones.</span><span class="sxs-lookup"><span data-stu-id="5cece-191">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="5cece-192">Invalidar el `OnConnectedAsync` método virtual para realizar acciones cuando un cliente se conecta al concentrador, por ejemplo, éste se agrega a un grupo.</span><span class="sxs-lookup"><span data-stu-id="5cece-192">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="5cece-193">Invalidar el `OnDisconnectedAsync` método virtual para realizar acciones cuando un cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="5cece-193">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="5cece-194">Si el cliente se desconecta de forma intencionada (mediante una llamada a `connection.stop()`, por ejemplo), el `exception` parámetro será `null`.</span><span class="sxs-lookup"><span data-stu-id="5cece-194">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="5cece-195">Sin embargo, si el cliente se desconecta debido a un error (por ejemplo, un error de red), el `exception` parámetro contendrá una excepción que describe el error.</span><span class="sxs-lookup"><span data-stu-id="5cece-195">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="5cece-196">Control de errores</span><span class="sxs-lookup"><span data-stu-id="5cece-196">Handle errors</span></span>

<span data-ttu-id="5cece-197">Las excepciones producidas en los métodos de concentrador se envían al cliente que invocó el método.</span><span class="sxs-lookup"><span data-stu-id="5cece-197">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="5cece-198">En el cliente de JavaScript, el `invoke` método devuelve un [promesa de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="5cece-198">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="5cece-199">Cuando el cliente recibe un error con un controlador asociado a la promesa mediante `catch`, se invoca y pasado como un JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="5cece-199">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="5cece-200">Si su centro, produce una excepción, no se cierran las conexiones.</span><span class="sxs-lookup"><span data-stu-id="5cece-200">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="5cece-201">De forma predeterminada, SignalR devuelve un mensaje de error genérico al cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-201">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="5cece-202">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5cece-202">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="5cece-203">Excepciones inesperadas suelen contengan información confidencial, como el nombre de un servidor de base de datos en una excepción que se desencadena cuando se produce un error en la conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="5cece-203">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="5cece-204">SignalR no expone estos mensajes de error detallados de forma predeterminada como medida de seguridad.</span><span class="sxs-lookup"><span data-stu-id="5cece-204">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="5cece-205">Consulte la [artículo de consideraciones de seguridad](xref:signalr/security#exceptions) para obtener más información sobre por qué se suprimen los detalles de la excepción.</span><span class="sxs-lookup"><span data-stu-id="5cece-205">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="5cece-206">Si tiene una excepcional de condición *hacer* desea propagar al cliente, puede usar el `HubException` clase.</span><span class="sxs-lookup"><span data-stu-id="5cece-206">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="5cece-207">Si produce un `HubException` desde su método de concentrador SignalR **le** enviar todo el mensaje al cliente, sin modificar.</span><span class="sxs-lookup"><span data-stu-id="5cece-207">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="5cece-208">SignalR solo envía el `Message` propiedad de la excepción al cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-208">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="5cece-209">El seguimiento de pila y otras propiedades en la excepción no están disponibles para el cliente.</span><span class="sxs-lookup"><span data-stu-id="5cece-209">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="5cece-210">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="5cece-210">Related resources</span></span>

* [<span data-ttu-id="5cece-211">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="5cece-211">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="5cece-212">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5cece-212">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="5cece-213">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="5cece-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
