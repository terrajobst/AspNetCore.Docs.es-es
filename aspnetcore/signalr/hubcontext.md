---
title: SignalR HubContext
author: bradygaster
description: Obtenga información sobre cómo usar el servicio de ASP.NET Core SignalR HubContext para enviar notificaciones a los clientes desde fuera de un concentrador.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: 7ec52d4711fc191dcb83120cf54b1dc28c41f947
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894482"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="4b115-103">Enviar mensajes desde fuera de un concentrador</span><span class="sxs-lookup"><span data-stu-id="4b115-103">Send messages from outside a hub</span></span>

<span data-ttu-id="4b115-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="4b115-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="4b115-105">El centro de SignalR es la abstracción principal para enviar mensajes a los clientes conectados al servidor de SignalR.</span><span class="sxs-lookup"><span data-stu-id="4b115-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="4b115-106">También es posible enviar mensajes desde otros lugares de la aplicación mediante el `IHubContext` service.</span><span class="sxs-lookup"><span data-stu-id="4b115-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="4b115-107">En este artículo se explica cómo obtener acceso a un SignalR `IHubContext` para enviar notificaciones a los clientes desde fuera de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="4b115-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="4b115-108">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4b115-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="4b115-109">Obtener una instancia de IHubContext</span><span class="sxs-lookup"><span data-stu-id="4b115-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="4b115-110">En ASP.NET Core SignalR, puede tener acceso a una instancia de `IHubContext` mediante la inserción de dependencia.</span><span class="sxs-lookup"><span data-stu-id="4b115-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="4b115-111">Puede insertar una instancia de `IHubContext` en un controlador, middleware u otro servicio de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="4b115-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="4b115-112">Utilice la instancia para enviar mensajes a los clientes.</span><span class="sxs-lookup"><span data-stu-id="4b115-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="4b115-113">Esto difiere de ASP.NET 4.x SignalR que usa GlobalHost para proporcionar acceso a la `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="4b115-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="4b115-114">ASP.NET Core tiene un marco de inserción de dependencia que elimina la necesidad de este singleton global.</span><span class="sxs-lookup"><span data-stu-id="4b115-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="4b115-115">Inyectar una instancia de IHubContext en un controlador</span><span class="sxs-lookup"><span data-stu-id="4b115-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="4b115-116">Puede insertar una instancia de `IHubContext` en un controlador al agregarlo a su constructor:</span><span class="sxs-lookup"><span data-stu-id="4b115-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="4b115-117">Ahora, con acceso a una instancia de `IHubContext`, puede llamar a métodos de concentrador como si estuviera en el centro de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="4b115-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="4b115-118">Obtener una instancia de IHubContext en middleware</span><span class="sxs-lookup"><span data-stu-id="4b115-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="4b115-119">Acceso a la `IHubContext` dentro de la canalización de middleware siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="4b115-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="4b115-120">Cuando se llaman a métodos de concentrador desde fuera de la `Hub` clase, no hay ningún autor de llamada asociada con la invocación.</span><span class="sxs-lookup"><span data-stu-id="4b115-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="4b115-121">Por lo tanto, no hay ningún acceso a la `ConnectionId`, `Caller`, y `Others` propiedades.</span><span class="sxs-lookup"><span data-stu-id="4b115-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

### <a name="inject-a-strongly-typed-hubcontext"></a><span data-ttu-id="4b115-122">Insertar un HubContext fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="4b115-122">Inject a strongly-typed HubContext</span></span>

<span data-ttu-id="4b115-123">Para insertar un HubContext fuertemente tipado, asegúrese de que hereda de su centro de `Hub<T>`.</span><span class="sxs-lookup"><span data-stu-id="4b115-123">To inject a strongly-typed HubContext, ensure your Hub inherits from `Hub<T>`.</span></span> <span data-ttu-id="4b115-124">Insertar mediante el `IHubContext<THub, T>` interfaz en lugar de `IHubContext<THub>`.</span><span class="sxs-lookup"><span data-stu-id="4b115-124">Inject it using the `IHubContext<THub, T>` interface rather than `IHubContext<THub>`.</span></span>

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a><span data-ttu-id="4b115-125">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="4b115-125">Related resources</span></span>

* [<span data-ttu-id="4b115-126">Introducción</span><span class="sxs-lookup"><span data-stu-id="4b115-126">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4b115-127">Concentradores</span><span class="sxs-lookup"><span data-stu-id="4b115-127">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4b115-128">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="4b115-128">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
