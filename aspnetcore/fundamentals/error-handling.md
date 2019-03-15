---
title: Controlar errores en ASP.NET Core
author: tdykstra
description: Descubra cómo controlar errores en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665368"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="6e62a-103">Controlar errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e62a-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="6e62a-104">[Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6e62a-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6e62a-105">Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e62a-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="6e62a-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6e62a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="6e62a-107">Página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="6e62a-107">Developer Exception Page</span></span>

<span data-ttu-id="6e62a-108">Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones de solicitud, use la *página de excepciones para desarrolladores*.</span><span class="sxs-lookup"><span data-stu-id="6e62a-108">To configure an app to display a page that shows detailed information about request exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="6e62a-109">La página está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), a su vez disponible en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6e62a-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="6e62a-110">Agregue una línea al método `Startup.Configure` cuando la aplicación se ejecuta en el [entorno](xref:fundamentals/environments) de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="6e62a-110">Add a line to the `Startup.Configure` method when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

<span data-ttu-id="6e62a-111">Realice una llamada a <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> delante de cualquier middleware donde quiera capturar excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="6e62a-112">Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="6e62a-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="6e62a-113">No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción.</span><span class="sxs-lookup"><span data-stu-id="6e62a-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="6e62a-114">Para más información sobre la configuración de entornos, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="6e62a-115">Para ver la página de excepciones para el desarrollador, ejecute la aplicación de ejemplo con el entorno establecido en `Development` y agregue `?throw=true` a la URL base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e62a-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="6e62a-116">La página incluye la siguiente información sobre la excepción y la solicitud:</span><span class="sxs-lookup"><span data-stu-id="6e62a-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="6e62a-117">Seguimiento de la pila</span><span class="sxs-lookup"><span data-stu-id="6e62a-117">Stack trace</span></span>
* <span data-ttu-id="6e62a-118">Parámetros de cadena de consulta (si existen)</span><span class="sxs-lookup"><span data-stu-id="6e62a-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="6e62a-119">Cookies (si existen)</span><span class="sxs-lookup"><span data-stu-id="6e62a-119">Cookies (if any)</span></span>
* <span data-ttu-id="6e62a-120">Encabezados</span><span class="sxs-lookup"><span data-stu-id="6e62a-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="6e62a-121">Configurar una página personalizada de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="6e62a-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="6e62a-122">Cuando la aplicación no se ejecute en el entorno de desarrollo, llame al método de extensión <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> para agregar middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="6e62a-123">El middleware:</span><span class="sxs-lookup"><span data-stu-id="6e62a-123">The middleware:</span></span>

* <span data-ttu-id="6e62a-124">Captura las excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-124">Catches exceptions.</span></span>
* <span data-ttu-id="6e62a-125">Registra las excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-125">Logs exceptions.</span></span>
* <span data-ttu-id="6e62a-126">Vuelve a ejecutar la solicitud en una canalización alternativa correspondiente a la página o el controlador indicados.</span><span class="sxs-lookup"><span data-stu-id="6e62a-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="6e62a-127">La solicitud no se vuelve a ejecutar si se ha iniciado la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6e62a-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="6e62a-128">En el ejemplo siguiente de la aplicación de ejemplo, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> agrega middleware de control de excepciones en entornos que no son de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="6e62a-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="6e62a-129">El método de extensión especifica una página de error o un controlador en el punto de conexión `/Error` para las solicitudes ejecutadas de nuevo después de que se capturan y registran excepciones:</span><span class="sxs-lookup"><span data-stu-id="6e62a-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

<span data-ttu-id="6e62a-130">La plantilla de aplicación de Razor Pages proporciona una página de error (*.cshtml*) y una clase <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) en la carpeta Pages.</span><span class="sxs-lookup"><span data-stu-id="6e62a-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="6e62a-131">En una aplicación MVC, se incluye el siguiente método del controlador de errores en la plantilla de aplicación MVC y aparece en el controlador principal:</span><span class="sxs-lookup"><span data-stu-id="6e62a-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="6e62a-132">No decore el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="6e62a-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="6e62a-133">Los verbos explícitos impiden que algunas solicitudes lleguen al método.</span><span class="sxs-lookup"><span data-stu-id="6e62a-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="6e62a-134">Permita el acceso anónimo al método para que los usuarios no autenticados puedan recibir y ver el error.</span><span class="sxs-lookup"><span data-stu-id="6e62a-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="access-the-exception"></a><span data-ttu-id="6e62a-135">Acceso a la excepción</span><span class="sxs-lookup"><span data-stu-id="6e62a-135">Access the exception</span></span>

<span data-ttu-id="6e62a-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> para acceder a la ruta de acceso a la solicitud original o a la excepción en un controlador o página:</span><span class="sxs-lookup"><span data-stu-id="6e62a-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception or the original request path in a controller or page:</span></span>

* <span data-ttu-id="6e62a-137">La ruta de acceso está disponible en la propiedad <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-137">The path is available from the <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> property.</span></span>
* <span data-ttu-id="6e62a-138">Lea <xref:System.Exception?displayProperty=fullName> de la propiedad [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) heredada.</span><span class="sxs-lookup"><span data-stu-id="6e62a-138">Read the <xref:System.Exception?displayProperty=fullName> from the inherited [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) property.</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> <span data-ttu-id="6e62a-139">**No** proporcione información de errores confidencial de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> a los clientes.</span><span class="sxs-lookup"><span data-stu-id="6e62a-139">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="6e62a-140">Proporcionar información de los errores es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="6e62a-140">Serving errors is a security risk.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="6e62a-141">Configuración del código personalizado de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="6e62a-141">Configure custom exception handling code</span></span>

<span data-ttu-id="6e62a-142">Una alternativa a proporcionar un punto de conexión para los errores con una [página personalizada de control de excepciones](#configure-a-custom-exception-handling-page) es proporcionar una función lambda a <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-142">An alternative to serving an endpoint for errors with a [custom exception handling page](#configure-a-custom-exception-handling-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="6e62a-143">Usar una función lambda con <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> permite acceder al error antes de devolver la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6e62a-143">Using a lambda with <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> allows access to the error before returning the response.</span></span>

<span data-ttu-id="6e62a-144">La aplicación de ejemplo muestra el código personalizado de control de excepciones en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6e62a-144">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="6e62a-145">Desencadene una excepción con el vínculo **Iniciar excepción** en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="6e62a-145">Trigger an exception with the **Throw Exception** link on the Index page.</span></span> <span data-ttu-id="6e62a-146">Se ejecuta esta función lambda:</span><span class="sxs-lookup"><span data-stu-id="6e62a-146">The following lambda runs:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> <span data-ttu-id="6e62a-147">**No** proporcione información de errores confidencial de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> a los clientes.</span><span class="sxs-lookup"><span data-stu-id="6e62a-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="6e62a-148">Proporcionar información de los errores es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="6e62a-148">Serving errors is a security risk.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="6e62a-149">Configurar páginas de códigos de estado</span><span class="sxs-lookup"><span data-stu-id="6e62a-149">Configure status code pages</span></span>

<span data-ttu-id="6e62a-150">Una aplicación ASP.NET Core no proporciona de forma predeterminada una página de códigos de estado para los códigos de estado HTTP, como *404 - No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="6e62a-150">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="6e62a-151">La aplicación devuelve un código de estado y un cuerpo de respuesta vacío.</span><span class="sxs-lookup"><span data-stu-id="6e62a-151">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="6e62a-152">Para proporcionar páginas de códigos de estado, use el middleware de páginas de código de estado.</span><span class="sxs-lookup"><span data-stu-id="6e62a-152">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="6e62a-153">El middleware está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), a su vez disponible en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6e62a-153">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6e62a-154">Agregue una línea al método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6e62a-154">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="6e62a-155">Llame al método <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> antes del middleware de control de solicitudes (por ejemplo, middleware de archivos estáticos y middleware de MVC).</span><span class="sxs-lookup"><span data-stu-id="6e62a-155">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="6e62a-156">El middleware de páginas de códigos de estado agrega de forma predeterminada controladores de solo texto para los códigos de estado comunes, como *404 - No encontrado*:</span><span class="sxs-lookup"><span data-stu-id="6e62a-156">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="6e62a-157">El middleware admite varios métodos de extensión que le permiten personalizar su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="6e62a-157">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="6e62a-158">Una sobrecarga de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> toma una expresión lambda, que se puede usar para procesar la lógica personalizada de control de errores y escribir manualmente la respuesta:</span><span class="sxs-lookup"><span data-stu-id="6e62a-158">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="6e62a-159">Una sobrecarga de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> toma una cadena de tipo de contenido y formato, que se puede usar para personalizar el tipo de contenido y el texto de respuesta:</span><span class="sxs-lookup"><span data-stu-id="6e62a-159">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="6e62a-160">Métodos de extensión de redirección y nueva ejecución</span><span class="sxs-lookup"><span data-stu-id="6e62a-160">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="6e62a-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="6e62a-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="6e62a-162">Envía un código de estado *302 - Encontrado* al cliente.</span><span class="sxs-lookup"><span data-stu-id="6e62a-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="6e62a-163">Redirige al cliente a la ubicación proporcionada en la plantilla de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="6e62a-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="6e62a-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> se usa normalmente cuando la aplicación:</span><span class="sxs-lookup"><span data-stu-id="6e62a-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="6e62a-165">Debe redirigir al cliente a un punto de conexión diferente, normalmente en casos en los que una aplicación diferente procesa el error.</span><span class="sxs-lookup"><span data-stu-id="6e62a-165">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="6e62a-166">En el caso de aplicaciones web, la barra de direcciones del explorador del cliente refleja el punto de conexión redirigido.</span><span class="sxs-lookup"><span data-stu-id="6e62a-166">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="6e62a-167">No debe conservar ni devolver el código de estado original con la respuesta de redirección inicial.</span><span class="sxs-lookup"><span data-stu-id="6e62a-167">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="6e62a-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="6e62a-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="6e62a-169">Devuelve el código de estado original al cliente.</span><span class="sxs-lookup"><span data-stu-id="6e62a-169">Returns the original status code to the client.</span></span>
* <span data-ttu-id="6e62a-170">Genera el cuerpo de respuesta, para lo cual vuelve a ejecutar la canalización de solicitud mediante una ruta de acceso alternativa.</span><span class="sxs-lookup"><span data-stu-id="6e62a-170">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="6e62a-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> se usa normalmente cuando la aplicación debe:</span><span class="sxs-lookup"><span data-stu-id="6e62a-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="6e62a-172">Procesar la solicitud sin redirigirla a un punto de conexión diferente.</span><span class="sxs-lookup"><span data-stu-id="6e62a-172">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="6e62a-173">En el caso de aplicaciones web, la barra de direcciones del explorador del cliente refleja el punto de conexión solicitado originalmente.</span><span class="sxs-lookup"><span data-stu-id="6e62a-173">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="6e62a-174">Conservar y devolver el código de estado original con la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6e62a-174">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="6e62a-175">Las plantillas pueden incluir un marcador de posición (`{0}`) relativo al código de estado.</span><span class="sxs-lookup"><span data-stu-id="6e62a-175">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="6e62a-176">La plantilla debe empezar con una barra diagonal (`/`).</span><span class="sxs-lookup"><span data-stu-id="6e62a-176">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="6e62a-177">Cuando se use un marcador de posición, confirme que el punto de conexión (página o controlador) puede procesar el segmento de línea.</span><span class="sxs-lookup"><span data-stu-id="6e62a-177">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="6e62a-178">Por ejemplo, una página de errores de Razor debe aceptar el valor del segmento de línea opcional con la directiva `@page`:</span><span class="sxs-lookup"><span data-stu-id="6e62a-178">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="6e62a-179">Las páginas de códigos de estado se pueden deshabilitar en solicitudes específicas en un método de controlador de páginas de Razor o en un controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="6e62a-179">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="6e62a-180">Para deshabilitar páginas de códigos de estado, intente recuperar <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> de la colección [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) de la solicitud y deshabilite la característica si está disponible:</span><span class="sxs-lookup"><span data-stu-id="6e62a-180">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="6e62a-181">Para usar una sobrecarga <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> que apunta a un punto de conexión dentro de la aplicación, cree una vista de MVC o una página de Razor Pages para ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="6e62a-181">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="6e62a-182">Por ejemplo, la plantilla de aplicación de Razor Pages genera la página y la clase de modelo de página siguientes:</span><span class="sxs-lookup"><span data-stu-id="6e62a-182">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="6e62a-183">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6e62a-183">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="6e62a-184">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6e62a-184">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="6e62a-185">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6e62a-185">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="6e62a-186">Código de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="6e62a-186">Exception-handling code</span></span>

<span data-ttu-id="6e62a-187">El código de las páginas de control de excepciones puede producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-187">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="6e62a-188">Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.</span><span class="sxs-lookup"><span data-stu-id="6e62a-188">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="6e62a-189">Además, tenga en cuenta que después de que se envían los encabezados de una respuesta:</span><span class="sxs-lookup"><span data-stu-id="6e62a-189">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="6e62a-190">La aplicación no puede cambiar el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6e62a-190">The app can't change the response's status code.</span></span>
* <span data-ttu-id="6e62a-191">No se pueden ejecutar páginas o controladores de excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-191">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="6e62a-192">Deberá completarse la respuesta o anularse la conexión.</span><span class="sxs-lookup"><span data-stu-id="6e62a-192">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="6e62a-193">Control de excepciones del servidor</span><span class="sxs-lookup"><span data-stu-id="6e62a-193">Server exception handling</span></span>

<span data-ttu-id="6e62a-194">Además de la lógica de control de excepciones de la aplicación, la [implementación del servidor](xref:fundamentals/servers/index) puede controlar algunas excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-194">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="6e62a-195">Si el servidor almacena en caché una excepción antes de que se envíen los encabezados de respuesta, envía una respuesta *500 - Error interno del servidor* sin cuerpo.</span><span class="sxs-lookup"><span data-stu-id="6e62a-195">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="6e62a-196">Si el servidor almacena en caché una excepción después de que se envían los encabezados de respuesta, cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="6e62a-196">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="6e62a-197">El servidor controla las solicitudes que no controla la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e62a-197">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="6e62a-198">El control de excepciones del servidor controla cualquier excepción que se produzca cuando el servidor controle la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6e62a-198">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="6e62a-199">Las páginas de error personalizadas, el middleware de control de excepciones y los filtros de la aplicación no afectan este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="6e62a-199">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="6e62a-200">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="6e62a-200">Startup exception handling</span></span>

<span data-ttu-id="6e62a-201">Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e62a-201">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="6e62a-202">Mediante el [host web](xref:fundamentals/host/web-host), puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](xref:fundamentals/host/web-host#detailed-errors) con las claves `captureStartupErrors` y `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="6e62a-202">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="6e62a-203">El hospedaje solo puede mostrar una página de error para un error de inicio capturado si este se produce después del enlace de puerto/dirección del host.</span><span class="sxs-lookup"><span data-stu-id="6e62a-203">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="6e62a-204">Si algún enlace produce un error por cualquier motivo:</span><span class="sxs-lookup"><span data-stu-id="6e62a-204">If any binding fails for any reason:</span></span>

* <span data-ttu-id="6e62a-205">La capa de hospedaje registra una excepción crítica.</span><span class="sxs-lookup"><span data-stu-id="6e62a-205">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="6e62a-206">El proceso de dotnet se bloquea.</span><span class="sxs-lookup"><span data-stu-id="6e62a-206">The dotnet process crashes.</span></span>
* <span data-ttu-id="6e62a-207">No se muestra ninguna página de error cuando la aplicación se ejecuta en el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6e62a-207">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="6e62a-208">Si se ejecuta en [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) devuelve un *error de proceso 502.5* si el proceso no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="6e62a-208">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="6e62a-209">Para obtener más información, vea <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-209">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="6e62a-210">Para obtener información sobre cómo solucionar problemas de inicio con Azure App Service, vea <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-210">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="6e62a-211">Control de errores de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6e62a-211">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="6e62a-212">Las aplicaciones [MVC](xref:mvc/overview) tienen algunas opciones adicionales para controlar errores, como la configuración de filtros de excepciones y la realización de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="6e62a-212">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="6e62a-213">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="6e62a-213">Exception filters</span></span>

<span data-ttu-id="6e62a-214">Los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción, en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="6e62a-214">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="6e62a-215">Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro.</span><span class="sxs-lookup"><span data-stu-id="6e62a-215">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="6e62a-216">En caso contrario, no se llama a estos filtros.</span><span class="sxs-lookup"><span data-stu-id="6e62a-216">These filters aren't called otherwise.</span></span> <span data-ttu-id="6e62a-217">Para obtener más información, vea <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-217">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="6e62a-218">Los filtros de excepciones son útiles para interceptar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="6e62a-218">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="6e62a-219">Se recomienda usar el middleware.</span><span class="sxs-lookup"><span data-stu-id="6e62a-219">We recommend using the middleware.</span></span> <span data-ttu-id="6e62a-220">Use filtros únicamente cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida.</span><span class="sxs-lookup"><span data-stu-id="6e62a-220">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="6e62a-221">Control de errores de estado del modelo</span><span class="sxs-lookup"><span data-stu-id="6e62a-221">Handle model state errors</span></span>

<span data-ttu-id="6e62a-222">La [validación de modelos](xref:mvc/models/validation) se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="6e62a-222">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="6e62a-223">Algunas aplicaciones optan por seguir una convención estándar para tratar los errores de [validación de modelos](xref:mvc/models/validation), en cuyo caso un [filtro](xref:mvc/controllers/filters) podría ser el lugar adecuado para implementar esta directiva.</span><span class="sxs-lookup"><span data-stu-id="6e62a-223">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="6e62a-224">Debe probar cómo se comportan las acciones con estados de modelo no válidos.</span><span class="sxs-lookup"><span data-stu-id="6e62a-224">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="6e62a-225">Para obtener más información, vea <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="6e62a-225">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e62a-226">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6e62a-226">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
