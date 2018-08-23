---
title: SignalR HubContext
author: tdykstra
description: Obtenga información sobre cómo usar el servicio de ASP.NET Core SignalR HubContext para enviar notificaciones a los clientes desde fuera de un concentrador.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: a02588dc98283a375e9deb7c8561c59f6d886eb0
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835569"
---
# <a name="send-messages-from-outside-a-hub"></a>Enviar mensajes desde fuera de un concentrador

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

El centro de SignalR es la abstracción principal para enviar mensajes a los clientes conectados al servidor de SignalR. También es posible enviar mensajes desde otros lugares de la aplicación mediante el `IHubContext` service. En este artículo se explica cómo obtener acceso a un SignalR `IHubContext` para enviar notificaciones a los clientes desde fuera de un concentrador.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtener una instancia de `IHubContext`

En ASP.NET Core SignalR, puede tener acceso a una instancia de `IHubContext` mediante la inserción de dependencia. Puede insertar una instancia de `IHubContext` en un controlador, middleware u otro servicio de inserción de dependencias. Utilice la instancia para enviar mensajes a los clientes.

> [!NOTE]
> Esto difiere de ASP.NET 4.x SignalR que usa GlobalHost para proporcionar acceso a la `IHubContext`. ASP.NET Core tiene un marco de inserción de dependencia que elimina la necesidad de este singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Inyectar una instancia de `IHubContext` en un controlador

Puede insertar una instancia de `IHubContext` en un controlador al agregarlo a su constructor:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Ahora, con acceso a una instancia de `IHubContext`, puede llamar a métodos de concentrador como si estuviera en el centro de sí mismo.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtener una instancia de `IHubContext` en middleware

Acceso a la `IHubContext` dentro de la canalización de middleware siguiente manera:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Cuando se llaman a métodos de concentrador desde fuera de la `Hub` clase, no hay ningún autor de llamada asociada con la invocación. Por lo tanto, no hay ningún acceso a la `ConnectionId`, `Caller`, y `Others` propiedades.

## <a name="related-resources"></a>Recursos relacionados

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
