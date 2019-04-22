---
title: Controlar errores en ASP.NET Core
author: tdykstra
description: Descubra cómo controlar errores en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/07/2019
uid: fundamentals/error-handling
ms.openlocfilehash: cbb9462a3c6010e074dc391aa128ac2cbb901456
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705583"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="42070-103">Controlar errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42070-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="42070-104">[Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="42070-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="42070-105">Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42070-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="42070-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="42070-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="42070-107">([Cómo descargarlo](xref:index#how-to-download-a-sample)). El artículo incluye instrucciones sobre cómo establecer directivas de preprocesador (`#if`, `#endif` y `#define`) en la aplicación de ejemplo para habilitar escenarios diferentes.</span><span class="sxs-lookup"><span data-stu-id="42070-107">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="42070-108">Página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="42070-108">Developer Exception Page</span></span>

<span data-ttu-id="42070-109">En la *Página de excepciones para el desarrollador* se muestra información detallada sobre las excepciones de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="42070-109">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="42070-110">La página está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), que se encuentra en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="42070-110">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="42070-111">Agregue código al método `Startup.Configure` para habilitar la página cuando la aplicación se ejecuta en el [entorno](xref:fundamentals/environments) de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="42070-111">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="42070-112">Realice una llamada a <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> antes de cualquier middleware donde quiera capturar excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-112">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="42070-113">Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="42070-113">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="42070-114">No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción.</span><span class="sxs-lookup"><span data-stu-id="42070-114">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="42070-115">Para más información sobre la configuración de entornos, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="42070-115">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="42070-116">La página incluye la siguiente información sobre la excepción y la solicitud:</span><span class="sxs-lookup"><span data-stu-id="42070-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="42070-117">Seguimiento de la pila</span><span class="sxs-lookup"><span data-stu-id="42070-117">Stack trace</span></span>
* <span data-ttu-id="42070-118">Parámetros de cadena de consulta (si existen)</span><span class="sxs-lookup"><span data-stu-id="42070-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="42070-119">Cookies (si existen)</span><span class="sxs-lookup"><span data-stu-id="42070-119">Cookies (if any)</span></span>
* <span data-ttu-id="42070-120">Encabezados</span><span class="sxs-lookup"><span data-stu-id="42070-120">Headers</span></span>

<span data-ttu-id="42070-121">Para ver la Página de excepciones para el desarrollador en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use la directiva de preprocesador `DevEnvironment` y seleccione **Trigger an exception** (Desencadenar una excepción) en la página principal.</span><span class="sxs-lookup"><span data-stu-id="42070-121">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="42070-122">Página del controlador de excepciones</span><span class="sxs-lookup"><span data-stu-id="42070-122">Exception handler page</span></span>

<span data-ttu-id="42070-123">Para configurar una página de control de errores personalizada para el entorno de producción, use el middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-123">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="42070-124">El middleware:</span><span class="sxs-lookup"><span data-stu-id="42070-124">The middleware:</span></span>

* <span data-ttu-id="42070-125">Captura y registra las excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-125">Catches and logs exceptions.</span></span>
* <span data-ttu-id="42070-126">Vuelve a ejecutar la solicitud en una canalización alternativa correspondiente a la página o el controlador indicados.</span><span class="sxs-lookup"><span data-stu-id="42070-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="42070-127">La solicitud no se vuelve a ejecutar si se ha iniciado la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42070-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="42070-128">En el ejemplo siguiente, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> agrega middleware de control de excepciones en entornos que no son de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="42070-128">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="42070-129">La plantilla de aplicación de Razor Pages proporciona una página de error (*.cshtml*) y una clase <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="42070-129">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="42070-130">Para una aplicación de MVC, la plantilla de proyecto incluye un método de acción para el error y una vista del error.</span><span class="sxs-lookup"><span data-stu-id="42070-130">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="42070-131">Este es el método de acción:</span><span class="sxs-lookup"><span data-stu-id="42070-131">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="42070-132">No decore el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="42070-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="42070-133">Los verbos explícitos impiden que algunas solicitudes lleguen al método.</span><span class="sxs-lookup"><span data-stu-id="42070-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="42070-134">Permita el acceso anónimo al método para que los usuarios no autenticados puedan recibir y ver el error.</span><span class="sxs-lookup"><span data-stu-id="42070-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="42070-135">Acceso a la excepción</span><span class="sxs-lookup"><span data-stu-id="42070-135">Access the exception</span></span>

<span data-ttu-id="42070-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> para acceder a la ruta de acceso a la solicitud original y a la excepción en una página o un controlador de errores:</span><span class="sxs-lookup"><span data-stu-id="42070-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="42070-137">**No** proporcione información de errores confidencial a los clientes.</span><span class="sxs-lookup"><span data-stu-id="42070-137">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="42070-138">Proporcionar información de los errores es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="42070-138">Serving errors is a security risk.</span></span>

<span data-ttu-id="42070-139">Para ver la página de control de excepciones en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use las directivas de preprocesador `ProdEnvironment` y `ErrorHandlerPage` y, después, seleccione **Trigger an exception** (Desencadenar una excepción) en la página principal.</span><span class="sxs-lookup"><span data-stu-id="42070-139">To see the exception handling page in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="42070-140">Lambda del controlador de excepciones</span><span class="sxs-lookup"><span data-stu-id="42070-140">Exception handler lambda</span></span>

<span data-ttu-id="42070-141">Una alternativa a una [página del controlador de excepciones personalizada](#exception-handler-page) es proporcionar una expresión lambda en <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="42070-141">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="42070-142">Usar una expresión lambda permite acceder al error antes de devolver la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42070-142">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="42070-143">Este es un ejemplo del uso de una expresión lambda para el control de excepciones:</span><span class="sxs-lookup"><span data-stu-id="42070-143">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="42070-144">**No** proporcione información de errores confidencial de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> a los clientes.</span><span class="sxs-lookup"><span data-stu-id="42070-144">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="42070-145">Proporcionar información de los errores es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="42070-145">Serving errors is a security risk.</span></span>

<span data-ttu-id="42070-146">Para ver el resultado de la expresión lambda de control de excepciones en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use las directivas de preprocesador `ProdEnvironment` y `ErrorHandlerLambda` y, después, seleccione **Trigger an exception** (Desencadenar una excepción) en la página principal.</span><span class="sxs-lookup"><span data-stu-id="42070-146">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="42070-147">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="42070-147">UseStatusCodePages</span></span>

<span data-ttu-id="42070-148">Una aplicación ASP.NET Core no proporciona de forma predeterminada una página de códigos de estado para los códigos de estado HTTP, como *404 - No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="42070-148">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="42070-149">La aplicación devuelve un código de estado y un cuerpo de respuesta vacío.</span><span class="sxs-lookup"><span data-stu-id="42070-149">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="42070-150">Para proporcionar páginas de códigos de estado, use el middleware de páginas de códigos de estado.</span><span class="sxs-lookup"><span data-stu-id="42070-150">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="42070-151">El middleware está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), que se encuentra en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="42070-151">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="42070-152">Para habilitar los controladores de solo texto predeterminados para los códigos de estado de error comunes, llame a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> en el método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="42070-152">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="42070-153">Llame a `UseStatusCodePages` antes del middleware de control de solicitudes (por ejemplo, middleware de archivos estáticos y middleware de MVC).</span><span class="sxs-lookup"><span data-stu-id="42070-153">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="42070-154">Este es un ejemplo del texto que muestran los controladores predeterminados:</span><span class="sxs-lookup"><span data-stu-id="42070-154">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="42070-155">Para ver alguno de los distintos formatos de la página de códigos de estado en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use una de las directivas de preprocesador que comienzan por `StatusCodePages` y seleccione **Trigger a 404** (Desencadenar un error 404) en la página principal.</span><span class="sxs-lookup"><span data-stu-id="42070-155">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="42070-156">UseStatusCodePages con cadena de formato</span><span class="sxs-lookup"><span data-stu-id="42070-156">UseStatusCodePages with format string</span></span>

<span data-ttu-id="42070-157">Para personalizar el texto y el tipo de contenido de la respuesta, use la sobrecarga de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> que adopta una cadena de tipo de contenido y formato:</span><span class="sxs-lookup"><span data-stu-id="42070-157">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="42070-158">UseStatusCodePages con una expresión lambda</span><span class="sxs-lookup"><span data-stu-id="42070-158">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="42070-159">Para especificar el código de escritura de respuesta y control de errores personalizado, use la sobrecarga de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> que adopta una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="42070-159">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a><span data-ttu-id="42070-160">UseStatusCodePagesWithRedirect</span><span class="sxs-lookup"><span data-stu-id="42070-160">UseStatusCodePagesWithRedirect</span></span>

<span data-ttu-id="42070-161">Método de extensión <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="42070-161">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="42070-162">Envía un código de estado *302 - Encontrado* al cliente.</span><span class="sxs-lookup"><span data-stu-id="42070-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="42070-163">Redirige al cliente a la ubicación proporcionada en la plantilla de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="42070-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="42070-164">La plantilla de dirección URL puede incluir un marcador de posición `{0}` para el código de estado, como se muestra en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="42070-164">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="42070-165">Si la plantilla de dirección URL comienza con una tilde (~), esta se sustituye por el atributo `PathBase` de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="42070-165">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="42070-166">Si apunta a un punto de conexión dentro de la aplicación, cree una vista de MVC o una página de Razor Pages para ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="42070-166">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="42070-167">Para consultar un ejemplo de Razor Pages, vea [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="42070-167">For a Razor Pages example, see [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="42070-168">Este método se usa normalmente cuando la aplicación:</span><span class="sxs-lookup"><span data-stu-id="42070-168">This method is commonly used when the app:</span></span>

* <span data-ttu-id="42070-169">Debe redirigir al cliente a un punto de conexión diferente, normalmente en casos en los que una aplicación diferente procesa el error.</span><span class="sxs-lookup"><span data-stu-id="42070-169">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="42070-170">En el caso de aplicaciones web, la barra de direcciones del explorador del cliente refleja el punto de conexión redirigido.</span><span class="sxs-lookup"><span data-stu-id="42070-170">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="42070-171">No debe conservar ni devolver el código de estado original con la respuesta de redirección inicial.</span><span class="sxs-lookup"><span data-stu-id="42070-171">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="42070-172">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="42070-172">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="42070-173">Método de extensión <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="42070-173">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="42070-174">Devuelve el código de estado original al cliente.</span><span class="sxs-lookup"><span data-stu-id="42070-174">Returns the original status code to the client.</span></span>
* <span data-ttu-id="42070-175">Genera el cuerpo de respuesta, para lo cual vuelve a ejecutar la canalización de solicitud mediante una ruta de acceso alternativa.</span><span class="sxs-lookup"><span data-stu-id="42070-175">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="42070-176">Si apunta a un punto de conexión dentro de la aplicación, cree una vista de MVC o una página de Razor Pages para ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="42070-176">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="42070-177">Para consultar un ejemplo de Razor Pages, vea [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="42070-177">For a Razor Pages example, see [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="42070-178">Este método se usa normalmente cuando la aplicación debe:</span><span class="sxs-lookup"><span data-stu-id="42070-178">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="42070-179">Procesar la solicitud sin redirigirla a un punto de conexión diferente.</span><span class="sxs-lookup"><span data-stu-id="42070-179">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="42070-180">En el caso de aplicaciones web, la barra de direcciones del explorador del cliente refleja el punto de conexión solicitado originalmente.</span><span class="sxs-lookup"><span data-stu-id="42070-180">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="42070-181">Conservar y devolver el código de estado original con la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42070-181">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="42070-182">Las plantillas de dirección URL y cadena de consulta pueden incluir un marcador de posición (`{0}`) relativo al código de estado.</span><span class="sxs-lookup"><span data-stu-id="42070-182">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="42070-183">La plantilla de dirección URL debe empezar con una barra diagonal (`/`).</span><span class="sxs-lookup"><span data-stu-id="42070-183">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="42070-184">Cuando se use un marcador de posición en la ruta de acceso, confirme que el punto de conexión (página o controlador) puede procesar el segmento de línea.</span><span class="sxs-lookup"><span data-stu-id="42070-184">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="42070-185">Por ejemplo, una página de errores de Razor debe aceptar el valor del segmento de línea opcional con la directiva `@page`:</span><span class="sxs-lookup"><span data-stu-id="42070-185">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="42070-186">El punto de conexión que procesa el error puede obtener la dirección URL original que generó el error, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="42070-186">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="42070-187">Deshabilitar las páginas de códigos de estado</span><span class="sxs-lookup"><span data-stu-id="42070-187">Disable status code pages</span></span>

<span data-ttu-id="42070-188">Las páginas de códigos de estado se pueden deshabilitar en solicitudes específicas en un método de controlador de páginas de Razor o en un controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="42070-188">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="42070-189">Para deshabilitar las páginas de códigos de estado, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span><span class="sxs-lookup"><span data-stu-id="42070-189">To disable status code pages, use the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="42070-190">Código de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="42070-190">Exception-handling code</span></span>

<span data-ttu-id="42070-191">El código de las páginas de control de excepciones puede producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-191">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="42070-192">Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.</span><span class="sxs-lookup"><span data-stu-id="42070-192">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="42070-193">Encabezados de respuesta</span><span class="sxs-lookup"><span data-stu-id="42070-193">Response headers</span></span>

<span data-ttu-id="42070-194">Una vez enviados los encabezados de una respuesta:</span><span class="sxs-lookup"><span data-stu-id="42070-194">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="42070-195">La aplicación no puede cambiar el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42070-195">The app can't change the response's status code.</span></span>
* <span data-ttu-id="42070-196">No se pueden ejecutar páginas o controladores de excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-196">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="42070-197">Deberá completarse la respuesta o anularse la conexión.</span><span class="sxs-lookup"><span data-stu-id="42070-197">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="42070-198">Control de excepciones del servidor</span><span class="sxs-lookup"><span data-stu-id="42070-198">Server exception handling</span></span>

<span data-ttu-id="42070-199">Además de la lógica de control de excepciones de la aplicación, la [implementación del servidor HTTP](xref:fundamentals/servers/index) puede controlar algunas excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-199">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="42070-200">Si el servidor almacena en caché una excepción antes de que se envíen los encabezados de respuesta, envía una respuesta *500 - Error interno del servidor* sin cuerpo.</span><span class="sxs-lookup"><span data-stu-id="42070-200">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="42070-201">Si el servidor almacena en caché una excepción después de que se envían los encabezados de respuesta, cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="42070-201">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="42070-202">El servidor controla las solicitudes que no controla la aplicación.</span><span class="sxs-lookup"><span data-stu-id="42070-202">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="42070-203">El control de excepciones del servidor controla cualquier excepción que se produzca cuando el servidor controle la solicitud.</span><span class="sxs-lookup"><span data-stu-id="42070-203">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="42070-204">Las páginas de error personalizadas, el middleware de control de excepciones y los filtros de la aplicación no afectan este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="42070-204">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="42070-205">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="42070-205">Startup exception handling</span></span>

<span data-ttu-id="42070-206">Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="42070-206">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="42070-207">El host puede configurarse para [capturar errores de inicio](xref:fundamentals/host/web-host#capture-startup-errors) y [capturar errores detallados](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="42070-207">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="42070-208">La capa de hospedaje puede mostrar una página de error para un error de inicio capturado solo si este se produce después del enlace de puerto/dirección del host.</span><span class="sxs-lookup"><span data-stu-id="42070-208">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="42070-209">Si se produce un error de enlace:</span><span class="sxs-lookup"><span data-stu-id="42070-209">If binding fails:</span></span>

* <span data-ttu-id="42070-210">La capa de hospedaje registra una excepción crítica.</span><span class="sxs-lookup"><span data-stu-id="42070-210">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="42070-211">El proceso de dotnet se bloquea.</span><span class="sxs-lookup"><span data-stu-id="42070-211">The dotnet process crashes.</span></span>
* <span data-ttu-id="42070-212">No se muestra ninguna página de error si el servidor HTTP es [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="42070-212">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="42070-213">Si se ejecuta en [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) devuelve un *error de proceso 502.5* si el proceso no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="42070-213">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="42070-214">Para obtener más información, vea <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="42070-214">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="42070-215">Para obtener información sobre cómo solucionar problemas de inicio con Azure App Service, vea <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="42070-215">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="42070-216">Página de error de la base de datos</span><span class="sxs-lookup"><span data-stu-id="42070-216">Database error page</span></span>

<span data-ttu-id="42070-217">El middleware de la [página de error de la base de datos](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) captura excepciones relacionadas con la base de datos que se pueden resolver mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="42070-217">The [Database Error Page](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="42070-218">Cuando se producen estas excepciones, se genera una respuesta HTML con los detalles de las acciones posibles para resolver el problema.</span><span class="sxs-lookup"><span data-stu-id="42070-218">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="42070-219">Esta página debe habilitarse solo en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="42070-219">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="42070-220">Habilitar la página mediante la adición de código a `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="42070-220">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a><span data-ttu-id="42070-221">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="42070-221">Exception filters</span></span>

<span data-ttu-id="42070-222">En las aplicaciones de MVC, los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción.</span><span class="sxs-lookup"><span data-stu-id="42070-222">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="42070-223">En las aplicaciones de Razor Pages, se pueden configurar a nivel global o por modelo de página.</span><span class="sxs-lookup"><span data-stu-id="42070-223">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="42070-224">Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro.</span><span class="sxs-lookup"><span data-stu-id="42070-224">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="42070-225">Para obtener más información, vea <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="42070-225">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="42070-226">Los filtros de excepciones son útiles para interceptar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="42070-226">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="42070-227">Se recomienda usar el middleware.</span><span class="sxs-lookup"><span data-stu-id="42070-227">We recommend using the middleware.</span></span> <span data-ttu-id="42070-228">Use filtros únicamente cuando deba realizar el control de errores de manera diferente según la acción de MVC elegida.</span><span class="sxs-lookup"><span data-stu-id="42070-228">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="42070-229">Errores de estado del modelo</span><span class="sxs-lookup"><span data-stu-id="42070-229">Model state errors</span></span>

<span data-ttu-id="42070-230">Para obtener información sobre cómo controlar los errores de estado de los modelos, vea [Enlace de modelos](xref:mvc/models/model-binding) y [Validación de modelos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="42070-230">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42070-231">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="42070-231">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
