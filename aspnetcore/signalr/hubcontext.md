---
title: SignalR HubContext
author: rachelappel
description: Obtenga información acerca de cómo usar el servicio de ASP.NET Core SignalR HubContext para enviar notificaciones a los clientes desde fuera de un concentrador.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277766"
---
# <a name="send-messages-from-outside-a-hub"></a>Enviar mensajes desde fuera de un concentrador

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

El concentrador de SignalR es la abstracción básica para enviar mensajes a los clientes conectados al servidor de SignalR. También es posible enviar mensajes desde otros lugares en la aplicación mediante el `IHubContext` service. Este artículo explica cómo obtener acceso a un SignalR `IHubContext` para enviar notificaciones a los clientes desde fuera de un concentrador.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtener una instancia de `IHubContext`

En ASP.NET Core SignalR, puede tener acceso a una instancia de `IHubContext` a través de la inserción de dependencias. También puede insertar una instancia de `IHubContext` en un controlador, middleware u otro servicio de DI. Utilice la instancia para enviar mensajes a los clientes.

> [!NOTE]
> Esto difiere de ASP.NET SignalR que utiliza GlobalHost para proporcionar acceso a la `IHubContext`. ASP.NET Core tiene un marco de inyección de dependencia que elimina la necesidad de este singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Insertar una instancia de `IHubContext` en un controlador

También puede insertar una instancia de `IHubContext` en un controlador, éste se agrega a su constructor:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Ahora, con acceso a una instancia de `IHubContext`, puede llamar a métodos de concentrador como si estuviera en el concentrador de sí mismo.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtener una instancia de `IHubContext` de middleware

Acceso a la `IHubContext` dentro de la canalización de middleware de este modo:

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
> Cuando se llaman a métodos de concentrador desde fuera de la `Hub` clase, no hay ningún autor de llamada asociado a la invocación. Por lo tanto, no hay ningún acceso a la `ConnectionId`, `Caller`, y `Others` propiedades.

## <a name="related-resources"></a>Recursos relacionados

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
