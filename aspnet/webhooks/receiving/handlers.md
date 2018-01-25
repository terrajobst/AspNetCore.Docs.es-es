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
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="22123-103">Controladores de Webhook de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="22123-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="22123-104">Una vez que las solicitudes de Webhook se ha validado por un receptor de WebHook, está listo para ser procesados por el código de usuario.</span><span class="sxs-lookup"><span data-stu-id="22123-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="22123-105">Aquí es donde *controladores* vienen en.</span><span class="sxs-lookup"><span data-stu-id="22123-105">This is where *handlers* come in.</span></span> <span data-ttu-id="22123-106">Controladores que se derivan de la [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaz, pero normalmente utiliza la [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) clase en lugar de derivar directamente desde la interfaz.</span><span class="sxs-lookup"><span data-stu-id="22123-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="22123-107">Una solicitud de WebHook puede ser procesada por uno o varios controladores.</span><span class="sxs-lookup"><span data-stu-id="22123-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="22123-108">Se llaman a controladores basándose en sus respectivas *orden* propiedad va de menor a mayor donde el orden es un entero simple (se recomienda un debe para estar entre 1 y 100):</span><span class="sxs-lookup"><span data-stu-id="22123-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagrama de propiedad de orden de controlador de WebHook](_static/Handlers.png)

<span data-ttu-id="22123-110">Opcionalmente, puede establecer un controlador de la *respuesta* propiedad en el WebHookHandlerContext lo que generará el procesamiento de detención y la respuesta se envían como la respuesta HTTP para el WebHook.</span><span class="sxs-lookup"><span data-stu-id="22123-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="22123-111">En el caso anterior, controlador de C no llamará porque tiene un orden más alto de B y B establece la respuesta.</span><span class="sxs-lookup"><span data-stu-id="22123-111">In the case above, Handler C won’t get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="22123-112">Configuración de la respuesta normalmente solo es pertinente para WebHooks donde la respuesta puede llevar información a la API de origen.</span><span class="sxs-lookup"><span data-stu-id="22123-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="22123-113">Esto es, por ejemplo, el caso de Webhook de demora en la respuesta se vuelve a enviar el canal de dónde procede el WebHook.</span><span class="sxs-lookup"><span data-stu-id="22123-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="22123-114">Controladores pueden establecer la propiedad de receptor si desean recibir WebHooks de dicho receptor concreto.</span><span class="sxs-lookup"><span data-stu-id="22123-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="22123-115">Si no establecen el receptor se llaman para todas ellas.</span><span class="sxs-lookup"><span data-stu-id="22123-115">If they don’t set the receiver they are called for all of them.</span></span>

<span data-ttu-id="22123-116">Un otros usos comunes de una respuesta es usar un *410 desaparecido* respuesta para indicar que el WebHook ya no está activo y no hay más solicitudes que se deben enviar.</span><span class="sxs-lookup"><span data-stu-id="22123-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="22123-117">De forma predeterminada todos los destinatarios de WebHook llamará un controlador.</span><span class="sxs-lookup"><span data-stu-id="22123-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="22123-118">Sin embargo, si la *receptor* propiedad se establece en el nombre de un controlador, a continuación, ese controlador sólo recibirá solicitudes de WebHook desde dicho receptor.</span><span class="sxs-lookup"><span data-stu-id="22123-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="22123-119">Procesar un WebHook</span><span class="sxs-lookup"><span data-stu-id="22123-119">Processing a WebHook</span></span>

<span data-ttu-id="22123-120">Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud de WebHook.</span><span class="sxs-lookup"><span data-stu-id="22123-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="22123-121">Los datos, normalmente el cuerpo de solicitud HTTP, están disponibles desde el *datos* propiedad.</span><span class="sxs-lookup"><span data-stu-id="22123-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="22123-122">El tipo de datos suele ser JSON o HTML de los datos de formulario, pero es posible convertir un tipo más específico si lo desea.</span><span class="sxs-lookup"><span data-stu-id="22123-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="22123-123">Por ejemplo, el Webhook personalizado generado por WebHooks de ASP.NET puede convertirse al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="22123-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="22123-124">Procesamiento en cola</span><span class="sxs-lookup"><span data-stu-id="22123-124">Queued Processing</span></span>

<span data-ttu-id="22123-125">La mayoría de WebHook de remitentes se volverá a enviar un WebHook si no se genera una respuesta dentro de una serie de segundos.</span><span class="sxs-lookup"><span data-stu-id="22123-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="22123-126">Esto significa que el controlador debe completar el procesamiento dentro de ese intervalo de tiempo en no para poder ser llama de nuevo.</span><span class="sxs-lookup"><span data-stu-id="22123-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="22123-127">Si el procesamiento tarda más tiempo, o que mejor se tratan por separado el [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) puede utilizarse para enviar la solicitud de WebHook a una cola, por ejemplo [cola de almacenamiento de Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="22123-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="22123-128">Un esquema de un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) aquí se proporciona la implementación:</span><span class="sxs-lookup"><span data-stu-id="22123-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
