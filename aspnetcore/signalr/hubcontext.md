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
# <a name="send-messages-from-outside-a-hub"></a>Enviar mensajes desde fuera de un concentrador

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

El centro de SignalR es la abstracción principal para enviar mensajes a los clientes conectados al servidor de SignalR. También es posible enviar mensajes desde otros lugares de la aplicación mediante el `IHubContext` service. En este artículo se explica cómo obtener acceso a un SignalR `IHubContext` para enviar notificaciones a los clientes desde fuera de un concentrador.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtener una instancia de IHubContext

En ASP.NET Core SignalR, puede tener acceso a una instancia de `IHubContext` mediante la inserción de dependencia. Puede insertar una instancia de `IHubContext` en un controlador, middleware u otro servicio de inserción de dependencias. Utilice la instancia para enviar mensajes a los clientes.

> [!NOTE]
> Esto difiere de ASP.NET 4.x SignalR que usa GlobalHost para proporcionar acceso a la `IHubContext`. ASP.NET Core tiene un marco de inserción de dependencia que elimina la necesidad de este singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Inyectar una instancia de IHubContext en un controlador

Puede insertar una instancia de `IHubContext` en un controlador al agregarlo a su constructor:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Ahora, con acceso a una instancia de `IHubContext`, puede llamar a métodos de concentrador como si estuviera en el centro de sí mismo.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtener una instancia de IHubContext en middleware

Acceso a la `IHubContext` dentro de la canalización de middleware siguiente manera:

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Cuando se llaman a métodos de concentrador desde fuera de la `Hub` clase, no hay ningún autor de llamada asociada con la invocación. Por lo tanto, no hay ningún acceso a la `ConnectionId`, `Caller`, y `Others` propiedades.

### <a name="inject-a-strongly-typed-hubcontext"></a>Insertar un HubContext fuertemente tipados

Para insertar un HubContext fuertemente tipado, asegúrese de que hereda de su centro de `Hub<T>`. Insertar mediante el `IHubContext<THub, T>` interfaz en lugar de `IHubContext<THub>`.

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

## <a name="related-resources"></a>Recursos relacionados

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
