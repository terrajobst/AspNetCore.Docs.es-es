---
uid: webhooks/receiving/handlers
title: Controladores de ASP.NET WebHooks | Documentos de Microsoft
author: rick-anderson
description: "Cómo administrar las solicitudes en ASP.NET Webhook."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a>Controladores de Webhook de ASP.NET

Una vez que las solicitudes de Webhook se ha validado por un receptor de WebHook, está listo para ser procesados por el código de usuario. Aquí es donde *controladores* vienen en. Controladores que se derivan de la [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaz, pero normalmente utiliza la [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) clase en lugar de derivar directamente desde la interfaz.

Una solicitud de WebHook puede ser procesada por uno o varios controladores. Se llaman a controladores basándose en sus respectivas *orden* propiedad va de menor a mayor donde el orden es un entero simple (se recomienda un debe para estar entre 1 y 100):

![Diagrama de propiedad de orden de controlador de WebHook](_static/Handlers.png)

Opcionalmente, puede establecer un controlador de la *respuesta* propiedad en el WebHookHandlerContext lo que generará el procesamiento de detención y la respuesta se envían como la respuesta HTTP para el WebHook. En el caso anterior, controlador de C no llamará porque tiene un orden más alto de B y B establece la respuesta.

Configuración de la respuesta normalmente solo es pertinente para WebHooks donde la respuesta puede llevar información a la API de origen. Esto es, por ejemplo, el caso de Webhook de demora en la respuesta se vuelve a enviar el canal de dónde procede el WebHook. Controladores pueden establecer la propiedad de receptor si desean recibir WebHooks de dicho receptor concreto. Si no establecen el receptor se llaman para todas ellas.

Un otros usos comunes de una respuesta es usar un *410 desaparecido* respuesta para indicar que el WebHook ya no está activo y no hay más solicitudes que se deben enviar.

De forma predeterminada todos los destinatarios de WebHook llamará un controlador. Sin embargo, si la *receptor* propiedad se establece en el nombre de un controlador, a continuación, ese controlador sólo recibirá solicitudes de WebHook desde dicho receptor.

## <a name="processing-a-webhook"></a>Procesar un WebHook

Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud de WebHook. Los datos, normalmente el cuerpo de solicitud HTTP, están disponibles desde el *datos* propiedad.

El tipo de datos suele ser JSON o HTML de los datos de formulario, pero es posible convertir un tipo más específico si lo desea. Por ejemplo, el Webhook personalizado generado por WebHooks de ASP.NET puede convertirse al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) como se indica a continuación:

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

La mayoría de WebHook de remitentes se volverá a enviar un WebHook si no se genera una respuesta dentro de una serie de segundos. Esto significa que el controlador debe completar el procesamiento dentro de ese intervalo de tiempo en no para poder ser llama de nuevo.

Si el procesamiento tarda más tiempo, o que mejor se tratan por separado el [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) puede utilizarse para enviar la solicitud de WebHook a una cola, por ejemplo [cola de almacenamiento de Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

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
