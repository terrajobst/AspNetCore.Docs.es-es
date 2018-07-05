---
uid: webhooks/receiving/handlers
title: Controladores de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Cómo administrar las solicitudes en ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.openlocfilehash: 7e45a97ac9d61b2d046984e5ede3be158b741b7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372704"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="0cfe8-103">Controladores de ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="0cfe8-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="0cfe8-104">Una vez que las solicitudes de WebHooks ha sido validada por un receptor de WebHook, está listo para ser procesados por el código de usuario.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="0cfe8-105">Aquí es donde *controladores* venir.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-105">This is where *handlers* come in.</span></span> <span data-ttu-id="0cfe8-106">Los controladores que derivan de la [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaz, pero normalmente usa el [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) clase en lugar de derivar directamente desde la interfaz.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="0cfe8-107">Una solicitud de WebHook se puede procesar por uno o varios controladores.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="0cfe8-108">Se llama a los controladores en orden según sus respectivas *orden* propiedad va de menor a mayor que el orden es un número entero simple (se recomienda estar comprendido entre 1 y 100):</span><span class="sxs-lookup"><span data-stu-id="0cfe8-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagrama de propiedad de orden de controlador de WebHook](_static/Handlers.png)

<span data-ttu-id="0cfe8-110">Opcionalmente, puede definir un controlador de la *respuesta* propiedad en el WebHookHandlerContext lo que conllevará el procesamiento de detención y la respuesta se envían como la respuesta HTTP al WebHook.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="0cfe8-111">En el caso anterior, no obtener llamará al controlador C porque tiene un orden más alto de B y B establece la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="0cfe8-112">Establecimiento de la respuesta normalmente solo es pertinente para WebHooks donde la respuesta puede transmitir información a la API original.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="0cfe8-113">Por ejemplo, este es el caso con WebHooks de Slack donde se publica la respuesta de vuelta al canal de dónde procede el WebHook.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="0cfe8-114">Los controladores pueden establecer la propiedad de receptor si desean recibir WebHooks de receptor en particular.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="0cfe8-115">Si no establecen el receptor se denominan para todas ellas.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="0cfe8-116">Un otro uso común de una respuesta es utilizar un *410 eliminado* respuesta para indicar que el WebHook, ya no está activo y no se debe enviar ninguna solicitud adicional.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="0cfe8-117">De forma predeterminada un controlador se llamará a todos los receptores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="0cfe8-118">Sin embargo, si la *receptor* propiedad está establecida en el nombre de un controlador, a continuación, ese controlador solo recibirá solicitudes de WebHook desde ese receptor.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="0cfe8-119">Procesamiento de un WebHook</span><span class="sxs-lookup"><span data-stu-id="0cfe8-119">Processing a WebHook</span></span>

<span data-ttu-id="0cfe8-120">Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud de WebHook.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="0cfe8-121">Los datos, normalmente el cuerpo de solicitud HTTP, están disponibles desde el *datos* propiedad.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="0cfe8-122">El tipo de datos normalmente es datos de formulario HTML o de JSON, pero es posible convertir a un tipo más específico, si lo desea.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="0cfe8-123">Por ejemplo, los WebHooks personalizados generados por ASP.NET WebHooks puede convertirse al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) como sigue:</span><span class="sxs-lookup"><span data-stu-id="0cfe8-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="0cfe8-124">Procesamiento en cola</span><span class="sxs-lookup"><span data-stu-id="0cfe8-124">Queued Processing</span></span>

<span data-ttu-id="0cfe8-125">La mayoría de los remitentes de WebHook se volverá a enviar a un WebHook si no se genera una respuesta dentro de unos cuantos segundos.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="0cfe8-126">Esto significa que el controlador debe completar el procesamiento dentro de ese período de tiempo en no para que ésta vuelva a llamar.</span><span class="sxs-lookup"><span data-stu-id="0cfe8-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="0cfe8-127">Si el procesamiento tarda más tiempo o se controla mejor por separado el [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) puede usarse para enviar la solicitud de WebHook a una cola, por ejemplo [cola de Azure Storage](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="0cfe8-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="0cfe8-128">Un esquema de un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) aquí se proporciona la implementación:</span><span class="sxs-lookup"><span data-stu-id="0cfe8-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
