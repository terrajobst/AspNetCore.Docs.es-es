---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Controladores de mensajes de HttpClient en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="97c49-102">Controladores de mensajes de HttpClient en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="97c49-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="97c49-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="97c49-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="97c49-104">A *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="97c49-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="97c49-105">Normalmente, una serie de controladores de mensajes están encadenados entre sí.</span><span class="sxs-lookup"><span data-stu-id="97c49-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="97c49-106">El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y le ofrece la solicitud al siguiente controlador.</span><span class="sxs-lookup"><span data-stu-id="97c49-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="97c49-107">En algún momento, la respuesta se crea y vuelve a la cadena.</span><span class="sxs-lookup"><span data-stu-id="97c49-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="97c49-108">Este patrón se denomina un *delegar* controlador.</span><span class="sxs-lookup"><span data-stu-id="97c49-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="97c49-109">En el lado del cliente, el **HttpClient** clase utiliza un controlador de mensajes para procesar solicitudes.</span><span class="sxs-lookup"><span data-stu-id="97c49-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="97c49-110">El controlador predeterminado es **HttpClientHandler**, que envía la solicitud a través de la red y obtenga la respuesta del servidor.</span><span class="sxs-lookup"><span data-stu-id="97c49-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="97c49-111">Puede insertar controladores de mensajes personalizados en la canalización de cliente:</span><span class="sxs-lookup"><span data-stu-id="97c49-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="97c49-112">ASP.NET Web API también utiliza los controladores de mensajes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="97c49-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="97c49-113">Para obtener más información, consulte [controladores de mensajes HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="97c49-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="97c49-114">Controladores de mensajes personalizado</span><span class="sxs-lookup"><span data-stu-id="97c49-114">Custom Message Handlers</span></span>

<span data-ttu-id="97c49-115">Para escribir un controlador de mensajes personalizado, derive de **System.Net.Http.DelegatingHandler** e invalide el **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="97c49-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="97c49-116">Aquí se muestra la signatura de método:</span><span class="sxs-lookup"><span data-stu-id="97c49-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="97c49-117">El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="97c49-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="97c49-118">Una implementación típica hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="97c49-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="97c49-119">Procesar el mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="97c49-119">Process the request message.</span></span>
2. <span data-ttu-id="97c49-120">Llame a `base.SendAsync` para enviar la solicitud al controlador interno.</span><span class="sxs-lookup"><span data-stu-id="97c49-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="97c49-121">El controlador interno devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="97c49-121">The inner handler returns a response message.</span></span> <span data-ttu-id="97c49-122">(Este paso es asíncrono).</span><span class="sxs-lookup"><span data-stu-id="97c49-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="97c49-123">Procesar la respuesta y devolver al llamador.</span><span class="sxs-lookup"><span data-stu-id="97c49-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="97c49-124">En el ejemplo siguiente se muestra un controlador de mensajes que agrega un encabezado personalizado a la solicitud de salida:</span><span class="sxs-lookup"><span data-stu-id="97c49-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="97c49-125">La llamada a `base.SendAsync` es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="97c49-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="97c49-126">Si el controlador no hace nada después de esta llamada, use la **await** palabra clave que se va a reanudar la ejecución una vez finalizado el método.</span><span class="sxs-lookup"><span data-stu-id="97c49-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="97c49-127">En el ejemplo siguiente se muestra un controlador que registra los códigos de error.</span><span class="sxs-lookup"><span data-stu-id="97c49-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="97c49-128">El registro de sí mismo no es muy interesante, pero en el ejemplo se muestra cómo obtener la respuesta dentro del controlador.</span><span class="sxs-lookup"><span data-stu-id="97c49-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="97c49-129">Agregar controladores de mensajes a la canalización de cliente</span><span class="sxs-lookup"><span data-stu-id="97c49-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="97c49-130">Para agregar controladores personalizados para **HttpClient**, use la **HttpClientFactory.Create** método:</span><span class="sxs-lookup"><span data-stu-id="97c49-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="97c49-131">Se denominan controladores de mensajes en el orden en que se pasan en el **Create** método.</span><span class="sxs-lookup"><span data-stu-id="97c49-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="97c49-132">Porque los controladores están anidados, el mensaje de respuesta se transmite en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="97c49-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="97c49-133">Es decir, el último controlador es el primero que se va a obtener el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="97c49-133">That is, the last handler is the first to get the response message.</span></span>
