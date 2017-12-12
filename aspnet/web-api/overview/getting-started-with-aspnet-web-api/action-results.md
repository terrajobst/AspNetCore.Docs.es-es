---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Acción da como resultado una Web API 2 | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 68b82661b97434795e1c306b168033dfcde529bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="a0f3f-102">Resultados de la acción en Web API 2</span><span class="sxs-lookup"><span data-stu-id="a0f3f-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="a0f3f-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a0f3f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a0f3f-104">Este tema describe cómo ASP.NET Web API convierte el valor devuelto de una acción de controlador en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="a0f3f-105">Una acción de controlador de API Web puede devolver cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="a0f3f-106">void</span><span class="sxs-lookup"><span data-stu-id="a0f3f-106">void</span></span>
2. <span data-ttu-id="a0f3f-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="a0f3f-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="a0f3f-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="a0f3f-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="a0f3f-109">Algún otro tipo</span><span class="sxs-lookup"><span data-stu-id="a0f3f-109">Some other type</span></span>

<span data-ttu-id="a0f3f-110">Dependiendo de cuál de ellos se devuelve, API Web utiliza un mecanismo diferente para crear la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="a0f3f-111">Tipo devuelto</span><span class="sxs-lookup"><span data-stu-id="a0f3f-111">Return type</span></span> | <span data-ttu-id="a0f3f-112">La API Web crea la respuesta</span><span class="sxs-lookup"><span data-stu-id="a0f3f-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="a0f3f-113">void</span><span class="sxs-lookup"><span data-stu-id="a0f3f-113">void</span></span> | <span data-ttu-id="a0f3f-114">Devolver vacía 204 (sin contenido)</span><span class="sxs-lookup"><span data-stu-id="a0f3f-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="a0f3f-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="a0f3f-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="a0f3f-116">Convertir directamente en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="a0f3f-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="a0f3f-117">**IHttpActionResult**</span></span> | <span data-ttu-id="a0f3f-118">Llame a **ExecuteAsync** para crear un **HttpResponseMessage**, a continuación, convertir en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="a0f3f-119">Otro tipo</span><span class="sxs-lookup"><span data-stu-id="a0f3f-119">Other type</span></span> | <span data-ttu-id="a0f3f-120">Escribir el valor devuelto serializado en el cuerpo de respuesta; devolver 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="a0f3f-121">El resto de este tema describe cada opción con más detalle.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="a0f3f-122">void</span><span class="sxs-lookup"><span data-stu-id="a0f3f-122">void</span></span>

<span data-ttu-id="a0f3f-123">Si el tipo de valor devuelto es `void`, API Web simplemente devuelve una respuesta HTTP vacía con código de estado 204 (sin contenido).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="a0f3f-124">Controlador de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="a0f3f-125">Respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="a0f3f-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="a0f3f-126">HttpResponseMessage</span></span>

<span data-ttu-id="a0f3f-127">Si la acción devuelve un [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), API Web convierte el valor devuelto directamente en un mensaje de respuesta HTTP, utilizando las propiedades de la **HttpResponseMessage** objeto para llenar el respuesta.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="a0f3f-128">Esta opción proporciona un gran control sobre el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="a0f3f-129">Por ejemplo, la acción del controlador siguiente establece el encabezado Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="a0f3f-130">Respuesta:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="a0f3f-131">Si se pasa un modelo de dominio para la **CreateResponse** /método siguiente, la API Web utiliza un [formateador del contenido multimedia](../formats-and-model-binding/media-formatters.md) para escribir el modelo serializado en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="a0f3f-132">API Web usa el encabezado Accept en la solicitud para elegir al formateador.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="a0f3f-133">Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="a0f3f-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="a0f3f-134">IHttpActionResult</span></span>

<span data-ttu-id="a0f3f-135">El **IHttpActionResult** interfaz se introdujo en API Web 2.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="a0f3f-136">En esencia, define un **HttpResponseMessage** generador.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="a0f3f-137">Aquí le presentamos algunas ventajas del uso de la **IHttpActionResult** interfaz:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="a0f3f-138">Simplifica la [pruebas unitarias](../testing-and-debugging/unit-testing-controllers-in-web-api.md) los controladores.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="a0f3f-139">Mueve la lógica común para la creación de respuestas HTTP en clases independientes.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="a0f3f-140">Hace que la intención de la acción de controlador más clara, ocultando los detalles de bajo nivel de creación de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="a0f3f-141">**IHttpActionResult** contiene un método único, **ExecuteAsync**, que crea de forma asincrónica un **HttpResponseMessage** instancia.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="a0f3f-142">Si una acción de controlador devuelve un **IHttpActionResult**, llamadas de API de Web la **ExecuteAsync** método para crear un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="a0f3f-143">A continuación, convierte el **HttpResponseMessage** en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="a0f3f-144">Esta es una implementación simple que se haga de **IHttpActionResult** que crea una respuesta de texto sin formato:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="a0f3f-145">Acción de controlador de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="a0f3f-146">Respuesta:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="a0f3f-147">Con más frecuencia, usará el **IHttpActionResult** implementaciones definidas en el  **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)**  espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="a0f3f-148">El **ApiController** clase define los métodos auxiliares que devuelven estos resultados de acción integrados.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="a0f3f-149">En el ejemplo siguiente, si la solicitud no coincide con un identificador de producto existente, el controlador llama a [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) para crear una respuesta 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="a0f3f-150">En caso contrario, el controlador llama [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), que crea una respuesta 200 (OK) que contiene el producto.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="a0f3f-151">Otros tipos de valor devuelto</span><span class="sxs-lookup"><span data-stu-id="a0f3f-151">Other Return Types</span></span>

<span data-ttu-id="a0f3f-152">Para los demás tipos de valor devueltos, API Web utiliza un [formateador del contenido multimedia](../formats-and-model-binding/media-formatters.md) para serializar el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="a0f3f-153">API Web escribe el valor serializado en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="a0f3f-154">El código de estado de respuesta es 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="a0f3f-155">Una desventaja de este enfoque es que no se puede devolver directamente un código de error, por ejemplo, 404.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="a0f3f-156">Sin embargo, se puede producir un **HttpResponseException** para códigos de error.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="a0f3f-157">Para obtener más información, consulte [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="a0f3f-158">API Web usa el encabezado Accept en la solicitud para elegir al formateador.</span><span class="sxs-lookup"><span data-stu-id="a0f3f-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="a0f3f-159">Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="a0f3f-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="a0f3f-160">Solicitud de ejemplo</span><span class="sxs-lookup"><span data-stu-id="a0f3f-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="a0f3f-161">Respuesta de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a0f3f-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
