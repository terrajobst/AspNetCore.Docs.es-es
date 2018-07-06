---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Da como resultado de acción en Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808932"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="54b7b-102">Resultados de acción en Web API 2</span><span class="sxs-lookup"><span data-stu-id="54b7b-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="54b7b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="54b7b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="54b7b-104">Este tema describe cómo ASP.NET Web API convierte el valor devuelto de una acción de controlador en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="54b7b-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="54b7b-105">Una acción de controlador Web API puede devolver cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="54b7b-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="54b7b-106">void</span><span class="sxs-lookup"><span data-stu-id="54b7b-106">void</span></span>
2. <span data-ttu-id="54b7b-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="54b7b-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="54b7b-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="54b7b-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="54b7b-109">Algún otro tipo</span><span class="sxs-lookup"><span data-stu-id="54b7b-109">Some other type</span></span>

<span data-ttu-id="54b7b-110">Dependiendo de cuál de estos se devuelven, API Web usa un mecanismo diferente para crear la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="54b7b-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="54b7b-111">Tipo devuelto</span><span class="sxs-lookup"><span data-stu-id="54b7b-111">Return type</span></span> | <span data-ttu-id="54b7b-112">Cómo se crea la respuesta Web API</span><span class="sxs-lookup"><span data-stu-id="54b7b-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="54b7b-113">void</span><span class="sxs-lookup"><span data-stu-id="54b7b-113">void</span></span> | <span data-ttu-id="54b7b-114">Devolver vacía 204 (sin contenido)</span><span class="sxs-lookup"><span data-stu-id="54b7b-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="54b7b-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="54b7b-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="54b7b-116">Convertir directamente en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="54b7b-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="54b7b-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="54b7b-117">**IHttpActionResult**</span></span> | <span data-ttu-id="54b7b-118">Llame a **ExecuteAsync** para crear un **HttpResponseMessage**, a continuación, convertir en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="54b7b-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="54b7b-119">Otro tipo</span><span class="sxs-lookup"><span data-stu-id="54b7b-119">Other type</span></span> | <span data-ttu-id="54b7b-120">Escribir el valor devuelto serializado en el cuerpo de respuesta; Devuelve 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="54b7b-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="54b7b-121">El resto de este tema describe cada opción con más detalle.</span><span class="sxs-lookup"><span data-stu-id="54b7b-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="54b7b-122">void</span><span class="sxs-lookup"><span data-stu-id="54b7b-122">void</span></span>

<span data-ttu-id="54b7b-123">Si el tipo de valor devuelto es `void`, Web API simplemente devuelve una respuesta HTTP vacía con el código de estado 204 (sin contenido).</span><span class="sxs-lookup"><span data-stu-id="54b7b-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="54b7b-124">Controlador de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54b7b-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="54b7b-125">Respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="54b7b-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="54b7b-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="54b7b-126">HttpResponseMessage</span></span>

<span data-ttu-id="54b7b-127">Si la acción devuelve un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API convierte el valor devuelto directamente en un mensaje de respuesta HTTP, mediante las propiedades de la **HttpResponseMessage** objeto para rellenar el respuesta.</span><span class="sxs-lookup"><span data-stu-id="54b7b-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="54b7b-128">Esta opción proporciona un gran control sobre el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="54b7b-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="54b7b-129">Por ejemplo, la siguiente acción del controlador establece el encabezado Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="54b7b-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="54b7b-130">Respuesta:</span><span class="sxs-lookup"><span data-stu-id="54b7b-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="54b7b-131">Si se pasa un modelo de dominio para el **CreateResponse** método, Web API usa un [formateador media](../formats-and-model-binding/media-formatters.md) para escribir el modelo serializado en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="54b7b-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="54b7b-132">API Web usa el encabezado Accept en la solicitud para elegir al formateador.</span><span class="sxs-lookup"><span data-stu-id="54b7b-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="54b7b-133">Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="54b7b-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="54b7b-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="54b7b-134">IHttpActionResult</span></span>

<span data-ttu-id="54b7b-135">El **IHttpActionResult** interfaz se introdujo en Web API 2.</span><span class="sxs-lookup"><span data-stu-id="54b7b-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="54b7b-136">Básicamente, define un **HttpResponseMessage** factory.</span><span class="sxs-lookup"><span data-stu-id="54b7b-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="54b7b-137">Estas son algunas ventajas de utilizar el **IHttpActionResult** interfaz:</span><span class="sxs-lookup"><span data-stu-id="54b7b-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="54b7b-138">Simplifica [las pruebas unitarias](../testing-and-debugging/unit-testing-controllers-in-web-api.md) los controladores.</span><span class="sxs-lookup"><span data-stu-id="54b7b-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="54b7b-139">Mueve la lógica común para crear respuestas HTTP en clases independientes.</span><span class="sxs-lookup"><span data-stu-id="54b7b-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="54b7b-140">Hace que la intención de la acción del controlador es más clara, ocultando los detalles de bajo nivel de construir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="54b7b-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="54b7b-141">**IHttpActionResult** contiene un método único, **ExecuteAsync**, que crea de forma asincrónica un **HttpResponseMessage** instancia.</span><span class="sxs-lookup"><span data-stu-id="54b7b-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="54b7b-142">Si una acción de controlador devuelve un **IHttpActionResult**, llamadas de API Web la **ExecuteAsync** método para crear un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="54b7b-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="54b7b-143">A continuación, convierte el **HttpResponseMessage** en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="54b7b-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="54b7b-144">Esta es una implementación simple que haga de **IHttpActionResult** que crea una respuesta de texto sin formato:</span><span class="sxs-lookup"><span data-stu-id="54b7b-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="54b7b-145">Acción de controlador de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54b7b-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="54b7b-146">Respuesta:</span><span class="sxs-lookup"><span data-stu-id="54b7b-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="54b7b-147">Más a menudo, usará el **IHttpActionResult** implementaciones definidas en el **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="54b7b-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="54b7b-148">El **ApiController** clase define los métodos auxiliares que devuelven estos resultados de acción integrada.</span><span class="sxs-lookup"><span data-stu-id="54b7b-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="54b7b-149">En el ejemplo siguiente, si la solicitud no coincide con un identificador de producto existente, el controlador llama a [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para crear una respuesta 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="54b7b-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="54b7b-150">En caso contrario, el controlador llama a [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), que crea una respuesta 200 (OK) que contiene el producto.</span><span class="sxs-lookup"><span data-stu-id="54b7b-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="54b7b-151">Otros tipos de valor devuelto</span><span class="sxs-lookup"><span data-stu-id="54b7b-151">Other Return Types</span></span>

<span data-ttu-id="54b7b-152">Para los demás tipos de valor devueltos, API Web usa un [formateador media](../formats-and-model-binding/media-formatters.md) para serializar el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="54b7b-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="54b7b-153">API Web escribe el valor serializado en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="54b7b-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="54b7b-154">El código de estado de respuesta es 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="54b7b-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="54b7b-155">Una desventaja de este enfoque es que directamente no se puede devolver un código de error, por ejemplo, 404.</span><span class="sxs-lookup"><span data-stu-id="54b7b-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="54b7b-156">Sin embargo, se puede producir un **HttpResponseException** para códigos de error.</span><span class="sxs-lookup"><span data-stu-id="54b7b-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="54b7b-157">Para obtener más información, consulte [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="54b7b-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="54b7b-158">API Web usa el encabezado Accept en la solicitud para elegir al formateador.</span><span class="sxs-lookup"><span data-stu-id="54b7b-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="54b7b-159">Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="54b7b-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="54b7b-160">Solicitud de ejemplo</span><span class="sxs-lookup"><span data-stu-id="54b7b-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="54b7b-161">Respuesta de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54b7b-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
