---
title: Controlar errores en ASP.NET Core
author: ardalis
description: Descubra cómo controlar errores en aplicaciones ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: d7e60c0f615841461a17b093bffe5fb3f82f8616
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332280"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="485f5-103">Controlar errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="485f5-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="485f5-104">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="485f5-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="485f5-105">Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="485f5-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="485f5-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="485f5-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="485f5-107">Página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="485f5-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="485f5-108">Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones, use la *página de excepciones para desarrolladores*.</span><span class="sxs-lookup"><span data-stu-id="485f5-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="485f5-109">La página está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), a su vez disponible en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="485f5-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="485f5-110">Agregue una línea al método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="485f5-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="485f5-111">Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones, use la *página de excepciones para desarrolladores*.</span><span class="sxs-lookup"><span data-stu-id="485f5-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="485f5-112">La página está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), a su vez disponible en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="485f5-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="485f5-113">Agregue una línea al método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="485f5-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="485f5-114">Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones, use la *página de excepciones para desarrolladores*.</span><span class="sxs-lookup"><span data-stu-id="485f5-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="485f5-115">La página está disponible mediante la adición de una referencia de paquete para el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="485f5-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="485f5-116">Agregue una línea al método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="485f5-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="485f5-117">Realice una llamada a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) delante de cualquier middleware en el que quiera capturar excepciones, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="485f5-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="485f5-118">Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="485f5-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="485f5-119">No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción.</span><span class="sxs-lookup"><span data-stu-id="485f5-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="485f5-120">[Más información sobre la configuración de entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="485f5-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="485f5-121">Para ver la página de excepciones para el desarrollador, ejecute la aplicación de ejemplo con el entorno establecido en `Development` y agregue `?throw=true` a la URL base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="485f5-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="485f5-122">La página incluye varias pestañas con información sobre la excepción y la solicitud.</span><span class="sxs-lookup"><span data-stu-id="485f5-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="485f5-123">La primera pestaña incluye un seguimiento de la pila:</span><span class="sxs-lookup"><span data-stu-id="485f5-123">The first tab includes a stack trace:</span></span>

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="485f5-125">En la pestaña siguiente se muestran los parámetros de cadena de consulta, si los hay:</span><span class="sxs-lookup"><span data-stu-id="485f5-125">The next tab shows the query string parameters, if any:</span></span>

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="485f5-127">Si la solicitud tiene cookies, aparecen en la pestaña **Cookies**. Los encabezados se ven en la última pestaña:</span><span class="sxs-lookup"><span data-stu-id="485f5-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="485f5-129">Configuración de una página personalizada de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="485f5-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="485f5-130">Configure una página de controlador de excepciones para usarla cuando la aplicación no se ejecute en el entorno `Development`:</span><span class="sxs-lookup"><span data-stu-id="485f5-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="485f5-131">En una aplicación de Razor Pages, la plantilla de Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) facilita una página de error y una clase de modelo de página `ErrorModel` en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="485f5-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="485f5-132">En una aplicación MVC, no decore el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="485f5-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="485f5-133">Los verbos explícitos impiden que algunas solicitudes lleguen al método.</span><span class="sxs-lookup"><span data-stu-id="485f5-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="485f5-134">Permita el acceso anónimo al método para que los usuarios no autenticados puedan recibir y ver el error.</span><span class="sxs-lookup"><span data-stu-id="485f5-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="485f5-135">Por ejemplo, la plantilla de MVC [dotnet new](/dotnet/core/tools/dotnet-new) proporciona el siguiente método de controlador de errores y aparece en el controlador Home:</span><span class="sxs-lookup"><span data-stu-id="485f5-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="485f5-136">Configuración de páginas de códigos de estado</span><span class="sxs-lookup"><span data-stu-id="485f5-136">Configuring status code pages</span></span>

<span data-ttu-id="485f5-137">Una aplicación no proporciona de forma predeterminada una página de códigos de estado enriquecida para códigos de estado HTTP como *404 No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="485f5-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="485f5-138">Para proporcionarlas, configure el middleware de páginas de código de estado agregando una línea al método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="485f5-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="485f5-139">El middleware de páginas de código de estado agrega de forma predeterminada controladores de solo texto para códigos de estado comunes, como 404:</span><span class="sxs-lookup"><span data-stu-id="485f5-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![Página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="485f5-141">El middleware admite diversos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="485f5-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="485f5-142">Uno de ellos es una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="485f5-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="485f5-143">Otro toma un tipo de contenido y una cadena de formato:</span><span class="sxs-lookup"><span data-stu-id="485f5-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="485f5-144">También hay métodos de extensión de redireccionamiento y de nueva ejecución.</span><span class="sxs-lookup"><span data-stu-id="485f5-144">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="485f5-145">El método de redireccionamiento envía un código de estado *302 Found* al cliente y lo redirige a la plantilla de la dirección URL de la ubicación facilitada.</span><span class="sxs-lookup"><span data-stu-id="485f5-145">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="485f5-146">La plantilla puede incluir un marcador de posición `{0}` relativo al código de estado.</span><span class="sxs-lookup"><span data-stu-id="485f5-146">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="485f5-147">Las direcciones URL que empiezan por `~` tienen la ruta de acceso base antepuesta.</span><span class="sxs-lookup"><span data-stu-id="485f5-147">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="485f5-148">Las direcciones URL que no empiezan por `~` se usan tal cual.</span><span class="sxs-lookup"><span data-stu-id="485f5-148">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="485f5-149">El método de reejecución devuelve el código de estado original al cliente y especifica que el cuerpo de la respuesta se debe generar volviendo a ejecutar la canalización de la solicitud mediante una ruta de acceso alternativa.</span><span class="sxs-lookup"><span data-stu-id="485f5-149">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="485f5-150">Es posible que esta ruta de acceso contenga un marcador de posición `{0}` relativo al código de estado:</span><span class="sxs-lookup"><span data-stu-id="485f5-150">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="485f5-151">Las páginas de códigos de estado se pueden deshabilitar en solicitudes específicas en un método de controlador de páginas de Razor o en un controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="485f5-151">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="485f5-152">Para deshabilitar páginas de códigos de estado, intente recuperar [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) de la colección [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) de la solicitud y deshabilite la característica si está disponible:</span><span class="sxs-lookup"><span data-stu-id="485f5-152">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="485f5-153">Si se usa una sobrecarga `UseStatusCodePages*` que apunta a un punto de conexión dentro de la aplicación, cree una vista de MVC o una página de Razor Pages para ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="485f5-153">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="485f5-154">Por ejemplo, la plantilla [dotnet new](/dotnet/core/tools/dotnet-new) de una aplicación de Razor Pages genera la página y la clase de modelo de página siguientes:</span><span class="sxs-lookup"><span data-stu-id="485f5-154">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="485f5-155">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="485f5-155">*Error.cshtml*:</span></span>

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

<span data-ttu-id="485f5-156">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="485f5-156">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="485f5-157">Código de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="485f5-157">Exception-handling code</span></span>

<span data-ttu-id="485f5-158">El código de las páginas de control de excepciones puede producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="485f5-158">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="485f5-159">Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.</span><span class="sxs-lookup"><span data-stu-id="485f5-159">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="485f5-160">Además, tenga en cuenta que una vez que se hayan enviado los encabezados de una respuesta, no se podrá cambiar el código de estado de la respuesta ni se podrá ejecutar ninguna página de excepción o controlador.</span><span class="sxs-lookup"><span data-stu-id="485f5-160">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="485f5-161">Deberá completarse la respuesta o anularse la conexión.</span><span class="sxs-lookup"><span data-stu-id="485f5-161">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="485f5-162">Control de excepciones del servidor</span><span class="sxs-lookup"><span data-stu-id="485f5-162">Server exception handling</span></span>

<span data-ttu-id="485f5-163">Además de la lógica de control de excepciones de la aplicación, el [servidor](xref:fundamentals/servers/index) que hospede la aplicación llevará a cabo el control de algunas excepciones.</span><span class="sxs-lookup"><span data-stu-id="485f5-163">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="485f5-164">Si el servidor detecta una excepción antes de que se envíen los encabezados, enviará una respuesta *500 Error interno del servidor* sin cuerpo.</span><span class="sxs-lookup"><span data-stu-id="485f5-164">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="485f5-165">Si el servidor detecta una excepción después de que se hayan enviado los encabezados, cerrará la conexión.</span><span class="sxs-lookup"><span data-stu-id="485f5-165">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="485f5-166">El servidor controla las solicitudes que no controla la aplicación.</span><span class="sxs-lookup"><span data-stu-id="485f5-166">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="485f5-167">El control de excepciones del servidor controla todas las excepciones que se producen.</span><span class="sxs-lookup"><span data-stu-id="485f5-167">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="485f5-168">Las páginas de error personalizadas y el software intermedio o filtros de control de excepciones que se hayan configurado no afectarán a este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="485f5-168">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="485f5-169">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="485f5-169">Startup exception handling</span></span>

<span data-ttu-id="485f5-170">Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="485f5-170">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="485f5-171">Mediante el [host de web](xref:fundamentals/host/web-host) también puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](xref:fundamentals/host/web-host#detailed-errors) con las claves `captureStartupErrors` y `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="485f5-171">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="485f5-172">El hospedaje solo puede mostrar una página de error para un error de inicio capturado si este se produce después del enlace de puerto/dirección del host.</span><span class="sxs-lookup"><span data-stu-id="485f5-172">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="485f5-173">Si se produce un error en algún enlace por cualquier motivo, el nivel de hospedaje registra una excepción crítica, el proceso de dotnet se bloquea y no se muestra ninguna página de error si la aplicación se está ejecutando en el servidor de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="485f5-173">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="485f5-174">Si se ejecuta en [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) devuelve un *error de proceso 502.5* en caso de que el proceso no se pueda iniciar.</span><span class="sxs-lookup"><span data-stu-id="485f5-174">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="485f5-175">Para obtener información sobre cómo solucionar problemas de inicio al hospedar con IIS, vea <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="485f5-175">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="485f5-176">Para obtener información sobre cómo solucionar problemas de inicio con Azure App Service, vea <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="485f5-176">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="485f5-177">Control de errores de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="485f5-177">ASP.NET MVC error handling</span></span>

<span data-ttu-id="485f5-178">Las aplicaciones [MVC](xref:mvc/overview) tienen algunas opciones adicionales para controlar errores, como la configuración de filtros de excepciones y la realización de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="485f5-178">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="485f5-179">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="485f5-179">Exception filters</span></span>

<span data-ttu-id="485f5-180">Los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción, en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="485f5-180">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="485f5-181">Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro.</span><span class="sxs-lookup"><span data-stu-id="485f5-181">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="485f5-182">En caso contrario, no se llama a estos filtros.</span><span class="sxs-lookup"><span data-stu-id="485f5-182">These filters aren't called otherwise.</span></span> <span data-ttu-id="485f5-183">Para obtener más información, vea [Filtros](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="485f5-183">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="485f5-184">Los filtros de excepciones están indicados para capturar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el middleware de control de errores.</span><span class="sxs-lookup"><span data-stu-id="485f5-184">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="485f5-185">Es recomendable dar preferencia al uso de middleware y usar filtros solo cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida.</span><span class="sxs-lookup"><span data-stu-id="485f5-185">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="485f5-186">Control de errores de estado del modelo</span><span class="sxs-lookup"><span data-stu-id="485f5-186">Handling model state errors</span></span>

<span data-ttu-id="485f5-187">La [validación de modelos](xref:mvc/models/validation) se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="485f5-187">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="485f5-188">Algunas aplicaciones optan por seguir una convención estándar para tratar los errores de validación de modelos, en cuyo caso un [filtro](xref:mvc/controllers/filters) podría ser el lugar adecuado para implementar esta directiva.</span><span class="sxs-lookup"><span data-stu-id="485f5-188">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="485f5-189">Debe probar cómo se comportan las acciones con estados de modelo no válidos.</span><span class="sxs-lookup"><span data-stu-id="485f5-189">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="485f5-190">Obtenga más información en [Probar la lógica del controlador](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="485f5-190">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="485f5-191">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="485f5-191">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
