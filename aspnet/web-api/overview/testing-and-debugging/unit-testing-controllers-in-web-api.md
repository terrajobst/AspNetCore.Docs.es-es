---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: En ASP.NET Web API 2 controladores de pruebas unitarias | Microsoft Docs
author: MikeWasson
description: En este tema se describe algunas técnicas específicas para las pruebas unitarias controladores en Web API 2. Antes de leer este tema, es posible que desea leer el tutorial unidad...
ms.author: aspnetcontent
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bc6d4d7e65c84036dc35fe39ed25685ed8b213b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823694"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="8737b-104">En ASP.NET Web API 2 controladores de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="8737b-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8737b-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8737b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8737b-106">En este tema se describe algunas técnicas específicas para las pruebas unitarias controladores en Web API 2.</span><span class="sxs-lookup"><span data-stu-id="8737b-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="8737b-107">Antes de leer este tema, es posible que desee leer el tutorial [unidad pruebas ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), que muestra cómo agregar un proyecto de prueba unitaria a la solución.</span><span class="sxs-lookup"><span data-stu-id="8737b-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8737b-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="8737b-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="8737b-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8737b-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="8737b-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="8737b-110">Web API 2</span></span>
> - <span data-ttu-id="8737b-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="8737b-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="8737b-112">Usé Moq, pero la misma idea se aplica a cualquier marco de simulación.</span><span class="sxs-lookup"><span data-stu-id="8737b-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="8737b-113">Moq 4.5.30 (y versiones posteriores) es compatible con Visual Studio 2017, Roslyn y .NET 4.5 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="8737b-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="8737b-114">Es un patrón común en las pruebas unitarias &quot;organizar de act-aserción&quot;:</span><span class="sxs-lookup"><span data-stu-id="8737b-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="8737b-115">Organizar: Configurar los requisitos previos para ejecutar la prueba.</span><span class="sxs-lookup"><span data-stu-id="8737b-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="8737b-116">ACT: Realizar la prueba.</span><span class="sxs-lookup"><span data-stu-id="8737b-116">Act: Perform the test.</span></span>
- <span data-ttu-id="8737b-117">Aserción: Compruebe que la prueba se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="8737b-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="8737b-118">En el paso acomodar, usará a menudo simulacro u objetos de código auxiliar.</span><span class="sxs-lookup"><span data-stu-id="8737b-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="8737b-119">Que minimiza el número de dependencias, por lo que la prueba se centra en las pruebas de una cosa.</span><span class="sxs-lookup"><span data-stu-id="8737b-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="8737b-120">Estos son algunos aspectos que debería realizar pruebas unitarias en sus controladores de API Web:</span><span class="sxs-lookup"><span data-stu-id="8737b-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="8737b-121">La acción devuelve el tipo de respuesta correcto.</span><span class="sxs-lookup"><span data-stu-id="8737b-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="8737b-122">Parámetros no válidos devuelven la respuesta de error correcto.</span><span class="sxs-lookup"><span data-stu-id="8737b-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="8737b-123">La acción llama al método correcto en la capa de repositorio o servicio.</span><span class="sxs-lookup"><span data-stu-id="8737b-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="8737b-124">Si la respuesta incluye un modelo de dominio, compruebe el tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="8737b-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="8737b-125">Estas son algunas cosas para probar, pero los detalles dependerán de la implementación del controlador.</span><span class="sxs-lookup"><span data-stu-id="8737b-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="8737b-126">En concreto, supone una gran diferencia si devuelven sus acciones de controlador **HttpResponseMessage** o **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="8737b-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="8737b-127">Para obtener más información acerca de estos tipos de resultados, vea [los resultados de acción en Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="8737b-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="8737b-128">Acciones de prueba que devuelven HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="8737b-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="8737b-129">Este es un ejemplo de un controlador de la devolución de cuyas acciones **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="8737b-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="8737b-130">Observe que el controlador utiliza la inserción de dependencias para insertar un `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="8737b-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="8737b-131">Esto hace que el controlador más estable, porque puede insertar un repositorio ficticio.</span><span class="sxs-lookup"><span data-stu-id="8737b-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="8737b-132">La siguiente prueba unitaria comprueba que el `Get` método escribe un `Product` al cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="8737b-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="8737b-133">Supongamos que `repository` es un simulacro `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="8737b-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="8737b-134">Es importante establecer **solicitar** y **configuración** en el controlador.</span><span class="sxs-lookup"><span data-stu-id="8737b-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="8737b-135">En caso contrario, se producirá un error la prueba con un **ArgumentNullException** o **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="8737b-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="8737b-136">Las pruebas de generación de vínculos</span><span class="sxs-lookup"><span data-stu-id="8737b-136">Testing Link Generation</span></span>

<span data-ttu-id="8737b-137">El `Post` llamadas al método **UrlHelper.Link** para crear vínculos en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="8737b-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="8737b-138">Esto requiere un poco más de configuración en la prueba unitaria:</span><span class="sxs-lookup"><span data-stu-id="8737b-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="8737b-139">El **UrlHelper** clase necesita los datos de ruta y la dirección URL de solicitud, por lo que la prueba debe establecer los valores para estos.</span><span class="sxs-lookup"><span data-stu-id="8737b-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="8737b-140">Otra opción es código auxiliar o simulacro **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="8737b-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="8737b-141">Con este enfoque, reemplaza el valor predeterminado de [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versión de simulacro o código auxiliar que devuelve un valor fijo.</span><span class="sxs-lookup"><span data-stu-id="8737b-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="8737b-142">Vamos a volver a escribir la prueba con el [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="8737b-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="8737b-143">Instalar el `Moq` paquete NuGet en el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="8737b-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="8737b-144">En esta versión, no necesita configurar los datos de ruta, dado que el simulacro **UrlHelper** devuelve una cadena de constante.</span><span class="sxs-lookup"><span data-stu-id="8737b-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="8737b-145">Acciones que devuelven IHttpActionResult de pruebas</span><span class="sxs-lookup"><span data-stu-id="8737b-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="8737b-146">En la Web API 2, puede devolver una acción de controlador **IHttpActionResult**, que es análogo a **ActionResult** en ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8737b-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="8737b-147">El **IHttpActionResult** interfaz define un patrón de comando para crear respuestas HTTP.</span><span class="sxs-lookup"><span data-stu-id="8737b-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="8737b-148">En lugar de crear directamente la respuesta, el controlador devuelve un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="8737b-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="8737b-149">Más adelante, se invoca la canalización la **IHttpActionResult** para crear la respuesta.</span><span class="sxs-lookup"><span data-stu-id="8737b-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="8737b-150">Este enfoque resulta más fácil escribir pruebas unitarias, porque puede omitir un lote de la configuración necesaria para **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="8737b-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="8737b-151">Aquí es un controlador de ejemplo cuya devolución acciones **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="8737b-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="8737b-152">En este ejemplo muestra algunos patrones comunes mediante **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="8737b-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="8737b-153">Veamos cómo prueba unitaria de ellos.</span><span class="sxs-lookup"><span data-stu-id="8737b-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="8737b-154">Acción devuelve 200 (correcto) con un cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="8737b-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="8737b-155">El `Get` llamadas al método `Ok(product)` si se encuentra el producto.</span><span class="sxs-lookup"><span data-stu-id="8737b-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="8737b-156">En la prueba unitaria, asegúrese de que el tipo de valor devuelto es **OkNegotiatedContentResult** y el producto devuelto tiene el identificador correcto.</span><span class="sxs-lookup"><span data-stu-id="8737b-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="8737b-157">Tenga en cuenta que la prueba unitaria no se ejecuta el resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="8737b-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="8737b-158">Puede asumir que el resultado de acción crea la respuesta HTTP correctamente.</span><span class="sxs-lookup"><span data-stu-id="8737b-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="8737b-159">(Que es el motivo por el marco Web API tiene sus propias pruebas unitarias!)</span><span class="sxs-lookup"><span data-stu-id="8737b-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="8737b-160">Acción devuelve 404 (no encontrado)</span><span class="sxs-lookup"><span data-stu-id="8737b-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="8737b-161">El `Get` llamadas al método `NotFound()` si no se encuentra el producto.</span><span class="sxs-lookup"><span data-stu-id="8737b-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="8737b-162">En este caso, la prueba unitaria simplemente comprueba si el tipo de valor devuelto es **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="8737b-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="8737b-163">Acción devuelve 200 (OK) sin cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="8737b-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="8737b-164">El `Delete` llamadas al método `Ok()` para devolver una respuesta HTTP 200 vacía.</span><span class="sxs-lookup"><span data-stu-id="8737b-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="8737b-165">Al igual que el ejemplo anterior, la prueba unitaria comprueba el tipo de valor devuelto, en este caso **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="8737b-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="8737b-166">Acción devuelve 201 (creado) con un encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="8737b-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="8737b-167">El `Post` llamadas al método `CreatedAtRoute` para devolver una respuesta HTTP 201 con un URI en el encabezado Location.</span><span class="sxs-lookup"><span data-stu-id="8737b-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="8737b-168">En la prueba unitaria, compruebe que la acción establece los valores correctos de enrutamientos.</span><span class="sxs-lookup"><span data-stu-id="8737b-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="8737b-169">Acción devuelve 2xx otra con un cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="8737b-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="8737b-170">El `Put` llamadas al método `Content` para devolver una respuesta HTTP 202 (aceptado) con un cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="8737b-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="8737b-171">En este caso es similar a devuelve 200 (correcto), pero la prueba unitaria también debe comprobar el código de estado.</span><span class="sxs-lookup"><span data-stu-id="8737b-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="8737b-172">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8737b-172">Additional Resources</span></span>

- [<span data-ttu-id="8737b-173">Simular Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="8737b-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="8737b-174">[Escribir pruebas para un servicio ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog de Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="8737b-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="8737b-175">Depuración ASP.NET Web API con Route Debugger</span><span class="sxs-lookup"><span data-stu-id="8737b-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
