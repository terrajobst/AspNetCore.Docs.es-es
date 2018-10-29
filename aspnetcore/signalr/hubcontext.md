---
title: SignalR HubContext
author: tdykstra
description: Obtenga información sobre cómo usar el servicio de ASP.NET Core SignalR HubContext para enviar notificaciones a los clientes desde fuera de un concentrador.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 8be888e1f7b16d65ebbaa24b618e84fca029d80b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207958"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="442b2-103">Enviar mensajes desde fuera de un concentrador</span><span class="sxs-lookup"><span data-stu-id="442b2-103">Send messages from outside a hub</span></span>

<span data-ttu-id="442b2-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="442b2-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="442b2-105">El centro de SignalR es la abstracción principal para enviar mensajes a los clientes conectados al servidor de SignalR.</span><span class="sxs-lookup"><span data-stu-id="442b2-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="442b2-106">También es posible enviar mensajes desde otros lugares de la aplicación mediante el `IHubContext` service.</span><span class="sxs-lookup"><span data-stu-id="442b2-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="442b2-107">En este artículo se explica cómo obtener acceso a un SignalR `IHubContext` para enviar notificaciones a los clientes desde fuera de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="442b2-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="442b2-108">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="442b2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="442b2-109">Obtener una instancia de IHubContext</span><span class="sxs-lookup"><span data-stu-id="442b2-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="442b2-110">En ASP.NET Core SignalR, puede tener acceso a una instancia de `IHubContext` mediante la inserción de dependencia.</span><span class="sxs-lookup"><span data-stu-id="442b2-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="442b2-111">Puede insertar una instancia de `IHubContext` en un controlador, middleware u otro servicio de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="442b2-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="442b2-112">Utilice la instancia para enviar mensajes a los clientes.</span><span class="sxs-lookup"><span data-stu-id="442b2-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="442b2-113">Esto difiere de ASP.NET 4.x SignalR que usa GlobalHost para proporcionar acceso a la `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="442b2-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="442b2-114">ASP.NET Core tiene un marco de inserción de dependencia que elimina la necesidad de este singleton global.</span><span class="sxs-lookup"><span data-stu-id="442b2-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="442b2-115">Inyectar una instancia de IHubContext en un controlador</span><span class="sxs-lookup"><span data-stu-id="442b2-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="442b2-116">Puede insertar una instancia de `IHubContext` en un controlador al agregarlo a su constructor:</span><span class="sxs-lookup"><span data-stu-id="442b2-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="442b2-117">Ahora, con acceso a una instancia de `IHubContext`, puede llamar a métodos de concentrador como si estuviera en el centro de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="442b2-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="442b2-118">Obtener una instancia de IHubContext en middleware</span><span class="sxs-lookup"><span data-stu-id="442b2-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="442b2-119">Acceso a la `IHubContext` dentro de la canalización de middleware siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="442b2-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="442b2-120">Cuando se llaman a métodos de concentrador desde fuera de la `Hub` clase, no hay ningún autor de llamada asociada con la invocación.</span><span class="sxs-lookup"><span data-stu-id="442b2-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="442b2-121">Por lo tanto, no hay ningún acceso a la `ConnectionId`, `Caller`, y `Others` propiedades.</span><span class="sxs-lookup"><span data-stu-id="442b2-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="442b2-122">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="442b2-122">Related resources</span></span>

* [<span data-ttu-id="442b2-123">Introducción</span><span class="sxs-lookup"><span data-stu-id="442b2-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="442b2-124">Concentradores</span><span class="sxs-lookup"><span data-stu-id="442b2-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="442b2-125">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="442b2-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
