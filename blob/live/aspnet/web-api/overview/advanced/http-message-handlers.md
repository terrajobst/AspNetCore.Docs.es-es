---
uid: web-api/overview/advanced/http-message-handlers
title: Controladores de mensajes HTTP en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="ce6b6-102">Controladores de mensajes HTTP en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ce6b6-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ce6b6-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ce6b6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ce6b6-104">A *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="ce6b6-105">Controladores de mensajes que se derivan de la clase abstracta **HttpMessageHandler** clase.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="ce6b6-106">Normalmente, una serie de controladores de mensajes están encadenados entre sí.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="ce6b6-107">El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y le ofrece la solicitud al siguiente controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="ce6b6-108">En algún momento, la respuesta se crea y vuelve a la cadena.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="ce6b6-109">Este patrón se denomina un *delegar* controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="ce6b6-110">Controladores de mensajes de servidor</span><span class="sxs-lookup"><span data-stu-id="ce6b6-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="ce6b6-111">En el servidor, la canalización de Web API usa algunos controladores de mensajes integrada:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="ce6b6-112">**Servidor HTTP** Obtiene la solicitud desde el host.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="ce6b6-113">**HttpRoutingDispatcher** envía la solicitud en función de la ruta.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="ce6b6-114">**HttpControllerDispatcher** envía la solicitud a un controlador de Web API.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="ce6b6-115">Puede agregar controladores personalizados a la canalización.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="ce6b6-116">Controladores de mensajes son buenos para preocupaciones transversales que operan en el nivel de HTTP mensajes (en lugar de las acciones de controlador).</span><span class="sxs-lookup"><span data-stu-id="ce6b6-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="ce6b6-117">Por ejemplo, puede que un controlador de mensajes:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-117">For example, a message handler might:</span></span>

- <span data-ttu-id="ce6b6-118">Leer o modificar los encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-118">Read or modify request headers.</span></span>
- <span data-ttu-id="ce6b6-119">Agregar un encabezado de respuesta para las respuestas.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-119">Add a response header to responses.</span></span>
- <span data-ttu-id="ce6b6-120">Validar las solicitudes antes de alcanzar el controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="ce6b6-121">Este diagrama muestra dos controladores personalizados que se insertan en la canalización:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="ce6b6-122">En el lado cliente, HttpClient también utiliza los controladores de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="ce6b6-123">Para obtener más información, consulte [controladores de mensajes HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ce6b6-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="ce6b6-124">Controladores de mensajes personalizado</span><span class="sxs-lookup"><span data-stu-id="ce6b6-124">Custom Message Handlers</span></span>

<span data-ttu-id="ce6b6-125">Para escribir un controlador de mensajes personalizado, derive de **System.Net.Http.DelegatingHandler** e invalide el **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="ce6b6-126">Este método tiene la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="ce6b6-127">El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="ce6b6-128">Una implementación típica hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="ce6b6-129">Procesar el mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-129">Process the request message.</span></span>
2. <span data-ttu-id="ce6b6-130">Llame a `base.SendAsync` para enviar la solicitud al controlador interno.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="ce6b6-131">El controlador interno devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-131">The inner handler returns a response message.</span></span> <span data-ttu-id="ce6b6-132">(Este paso es asíncrono).</span><span class="sxs-lookup"><span data-stu-id="ce6b6-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="ce6b6-133">Procesar la respuesta y devolver al llamador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="ce6b6-134">Este es un ejemplo trivial:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="ce6b6-135">La llamada a `base.SendAsync` es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="ce6b6-136">Si el controlador no hace nada después de esta llamada, use la **await** palabra clave, como se muestra.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="ce6b6-137">Un controlador de delegación puede omitir el controlador interno y crear directamente la respuesta:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="ce6b6-138">Si una delegación de controlador crea la respuesta sin llamar a `base.SendAsync`, la solicitud omite el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="ce6b6-139">Esto puede ser útil para un controlador que valida la solicitud (creación de una respuesta de error).</span><span class="sxs-lookup"><span data-stu-id="ce6b6-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="ce6b6-140">Agrega un controlador a la canalización</span><span class="sxs-lookup"><span data-stu-id="ce6b6-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="ce6b6-141">Para agregar un controlador de mensajes en el servidor, agregue el controlador a la **HttpConfiguration.MessageHandlers** colección.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="ce6b6-142">Si utiliza la plantilla de "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, puede hacer esta dentro el **WebApiConfig** clase:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="ce6b6-143">Se denominan controladores de mensajes en el mismo orden que aparecen en **MessageHandlers** colección.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="ce6b6-144">Porque están anidadas, el mensaje de respuesta se transmite en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="ce6b6-145">Es decir, el último controlador es el primero que se va a obtener el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="ce6b6-146">Tenga en cuenta que no es necesario establecer los controladores internos; el marco Web API conecta automáticamente a los controladores de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="ce6b6-147">Si está [hospeda a sí mismo](../older-versions/self-host-a-web-api.md), cree una instancia de la **HttpSelfHostConfiguration** clase y agregue los controladores para la **MessageHandlers** colección.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="ce6b6-148">Ahora veamos algunos ejemplos de controladores de mensajes personalizados.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="ce6b6-149">Ejemplo: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="ce6b6-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="ce6b6-150">X-HTTP-Method-Override es un encabezado HTTP no estándar.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="ce6b6-151">Está diseñado para los clientes que no se pueden enviar a ciertos tipos de solicitud HTTP, como PUT o DELETE.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="ce6b6-152">En su lugar, el cliente envía una solicitud POST y establece el encabezado X-HTTP-Method-Override para el método que desee.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="ce6b6-153">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="ce6b6-154">Este es un controlador de mensajes que agrega compatibilidad para X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="ce6b6-155">En el **SendAsync** método, el controlador comprueba si el mensaje de solicitud es una solicitud POST y comprobar si contiene el encabezado X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="ce6b6-156">Si es así, valida el valor del encabezado y, a continuación, modifica el método de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="ce6b6-157">Por último, el controlador llama `base.SendAsync` para pasar el mensaje al siguiente controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="ce6b6-158">Cuando la solicitud alcanza el **HttpControllerDispatcher** (clase), **HttpControllerDispatcher** se usará para enrutar la solicitud en función del método de solicitud actualizada.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="ce6b6-159">Ejemplo: Agregar un encabezado de respuesta personalizado</span><span class="sxs-lookup"><span data-stu-id="ce6b6-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="ce6b6-160">Este es un controlador de mensajes que agrega un encabezado personalizado a cada mensaje de respuesta:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="ce6b6-161">En primer lugar, el controlador llama `base.SendAsync` para pasar la solicitud al controlador de mensajes interna.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="ce6b6-162">El controlador interno devuelve un mensaje de respuesta, pero lo hace de forma asincrónica utilizando una **tarea&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="ce6b6-163">No está disponible hasta que el mensaje de respuesta `base.SendAsync` completa de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="ce6b6-164">Este ejemplo se utiliza la **await** palabra clave que se va a realizar el trabajo asincrónicamente después `SendAsync` completa.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="ce6b6-165">Si se establece como destino .NET Framework 4.0, utilice el **tarea**&lt;T&gt;**. Método ContinueWith** método:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="ce6b6-166">Ejemplo: Buscar una clave de API</span><span class="sxs-lookup"><span data-stu-id="ce6b6-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="ce6b6-167">Algunos servicios web requieren que los clientes incluir una clave de API en su solicitud.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="ce6b6-168">En el ejemplo siguiente se muestra cómo un controlador de mensajes puede comprobar las solicitudes de una clave de API válida:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="ce6b6-169">Este controlador busca la clave de API en la cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="ce6b6-170">(En este ejemplo, suponemos que la clave es una cadena estática.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="ce6b6-171">Una implementación real probablemente utilizará una validación más compleja.) Si la cadena de consulta contiene la clave, el controlador pasa la solicitud al controlador interno.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="ce6b6-172">Si la solicitud no tiene una clave válida, el controlador crea un mensaje de respuesta con código de estado 403, prohibido.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="ce6b6-173">En este caso, el controlador no llama a `base.SendAsync`, por lo que el controlador interno nunca recibe la solicitud, ni tampoco el controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="ce6b6-174">Por lo tanto, el controlador puede suponer que todas las solicitudes entrantes tienen una clave de API válida.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="ce6b6-175">Si la clave de API solo se aplica a determinadas acciones de controlador, considere la posibilidad de utilizar un filtro de acción en lugar de un controlador de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="ce6b6-176">Filtros de acción se ejecutan después de que se realiza el enrutamiento de URI.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="ce6b6-177">Controladores de mensajes por ruta</span><span class="sxs-lookup"><span data-stu-id="ce6b6-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="ce6b6-178">Los controladores en el **HttpConfiguration.MessageHandlers** colección se aplican globalmente.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="ce6b6-179">Como alternativa, puede agregar un controlador de mensajes a una ruta específica cuando se define la ruta:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="ce6b6-180">En este ejemplo, si el URI de solicitud coincide con "Route2", la solicitud se envía a `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="ce6b6-181">El siguiente diagrama muestra la canalización para estas dos rutas:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="ce6b6-182">Tenga en cuenta que `MessageHandler2` reemplaza el valor predeterminado **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="ce6b6-183">En este ejemplo, `MessageHandler2` crea la respuesta, y las solicitudes que coinciden con "Route2" nunca se vaya a un controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="ce6b6-184">Esto le permite reemplazar todo el mecanismo de controlador de API Web con su propio extremo personalizado.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="ce6b6-185">Como alternativa, puede delegar un controlador de mensajes por la ruta a **HttpControllerDispatcher**, que luego se envía a un controlador.</span><span class="sxs-lookup"><span data-stu-id="ce6b6-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="ce6b6-186">El código siguiente muestra cómo configurar esta ruta:</span><span class="sxs-lookup"><span data-stu-id="ce6b6-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
