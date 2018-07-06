---
uid: webhooks/receiving/handlers
title: Controladores de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Cómo administrar las solicitudes en ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: bc8f4ef3f4ade775b395d73dfa8d73fec92fba3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841427"
---
# <a name="aspnet-webhooks-handlers"></a>Controladores de ASP.NET WebHooks

Una vez que las solicitudes de WebHooks ha sido validada por un receptor de WebHook, está listo para ser procesados por el código de usuario. Aquí es donde *controladores* venir. Los controladores que derivan de la [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaz, pero normalmente usa el [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) clase en lugar de derivar directamente desde la interfaz.

Una solicitud de WebHook se puede procesar por uno o varios controladores. Se llama a los controladores en orden según sus respectivas *orden* propiedad va de menor a mayor que el orden es un número entero simple (se recomienda estar comprendido entre 1 y 100):

![Diagrama de propiedad de orden de controlador de WebHook](_static/Handlers.png)

Opcionalmente, puede definir un controlador de la *respuesta* propiedad en el WebHookHandlerContext lo que conllevará el procesamiento de detención y la respuesta se envían como la respuesta HTTP al WebHook. En el caso anterior, no obtener llamará al controlador C porque tiene un orden más alto de B y B establece la respuesta.

Establecimiento de la respuesta normalmente solo es pertinente para WebHooks donde la respuesta puede transmitir información a la API original. Por ejemplo, este es el caso con WebHooks de Slack donde se publica la respuesta de vuelta al canal de dónde procede el WebHook. Los controladores pueden establecer la propiedad de receptor si desean recibir WebHooks de receptor en particular. Si no establecen el receptor se denominan para todas ellas.

Un otro uso común de una respuesta es utilizar un *410 eliminado* respuesta para indicar que el WebHook, ya no está activo y no se debe enviar ninguna solicitud adicional.

De forma predeterminada un controlador se llamará a todos los receptores de WebHook. Sin embargo, si la *receptor* propiedad está establecida en el nombre de un controlador, a continuación, ese controlador solo recibirá solicitudes de WebHook desde ese receptor.

## <a name="processing-a-webhook"></a>Procesamiento de un WebHook

Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud de WebHook. Los datos, normalmente el cuerpo de solicitud HTTP, están disponibles desde el *datos* propiedad.

El tipo de datos normalmente es datos de formulario HTML o de JSON, pero es posible convertir a un tipo más específico, si lo desea. Por ejemplo, los WebHooks personalizados generados por ASP.NET WebHooks puede convertirse al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) como sigue:

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>Procesamiento en cola

La mayoría de los remitentes de WebHook se volverá a enviar a un WebHook si no se genera una respuesta dentro de unos cuantos segundos. Esto significa que el controlador debe completar el procesamiento dentro de ese período de tiempo en no para que ésta vuelva a llamar.

Si el procesamiento tarda más tiempo o se controla mejor por separado el [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) puede usarse para enviar la solicitud de WebHook a una cola, por ejemplo [cola de Azure Storage](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Un esquema de un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) aquí se proporciona la implementación:

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
