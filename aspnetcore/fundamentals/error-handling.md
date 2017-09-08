---
title: Control de errores en ASP.NET Core
author: ardalis
description: "Explica cómo controlar los errores en las aplicaciones de ASP.NET Core"
keywords: "Núcleo de ASP.NET, control de errores, control de excepciones"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5898892c63e978adfabf9939394fef4ea1848d49
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="54b36-104">Introducción a control de errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54b36-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="54b36-105">Por [Steve Smith](http://ardalis.com) y [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="54b36-105">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="54b36-106">Este artículo tratan appoaches común para controlar los errores en las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54b36-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="54b36-107">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="54b36-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a><span data-ttu-id="54b36-108">La página de excepción para desarrolladores</span><span class="sxs-lookup"><span data-stu-id="54b36-108">The developer exception page</span></span>

<span data-ttu-id="54b36-109">Para configurar una aplicación para mostrar una página que muestra información detallada sobre las excepciones, instale el `Microsoft.AspNetCore.Diagnostics` NuGet empaquetar y agregue una línea a la [Configurar método de la clase de inicio](startup.md):</span><span class="sxs-lookup"><span data-stu-id="54b36-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

<span data-ttu-id="54b36-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="54b36-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span></span>

<span data-ttu-id="54b36-111">Colocar `UseDeveloperExceptionPage` antes de cualquier middleware que desea detectar excepciones, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="54b36-111">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="54b36-112">Habilitar la página de excepción para desarrolladores **sólo cuando la aplicación se ejecuta en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="54b36-112">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="54b36-113">No desea compartir públicamente información de excepción detallada cuando se ejecute la aplicación en producción.</span><span class="sxs-lookup"><span data-stu-id="54b36-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="54b36-114">[Más información acerca de cómo configurar entornos](environments.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-114">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="54b36-115">Para ver la página de excepción para desarrolladores, ejecute la aplicación de ejemplo con el entorno establecido en `Development`y agregue `?throw=true` a la dirección URL base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54b36-115">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="54b36-116">La página incluye varias pestañas con información sobre la excepción y la solicitud.</span><span class="sxs-lookup"><span data-stu-id="54b36-116">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="54b36-117">La primera ficha incluye un seguimiento de pila.</span><span class="sxs-lookup"><span data-stu-id="54b36-117">The first tab includes a stack trace.</span></span> 

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="54b36-119">La ficha siguiente muestra la consulta de parámetros de cadena, si existe.</span><span class="sxs-lookup"><span data-stu-id="54b36-119">The next tab shows the query string parameters, if any.</span></span>

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="54b36-121">Esta solicitud no tenía ninguna cookie, pero si lo hiciera, aparecerían en el **Cookies** ficha.</span><span class="sxs-lookup"><span data-stu-id="54b36-121">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab.</span></span> <span data-ttu-id="54b36-122">Puede ver los encabezados que se pasaron en la última ficha.</span><span class="sxs-lookup"><span data-stu-id="54b36-122">You can see the headers that were passed in the last tab.</span></span>

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="54b36-124">Configurar una página de control de excepciones personalizadas</span><span class="sxs-lookup"><span data-stu-id="54b36-124">Configuring a custom exception handling page</span></span>

<span data-ttu-id="54b36-125">Es una buena idea para configurar una página de controlador de excepción que se usará cuando la aplicación no se está ejecutando el `Development` entorno.</span><span class="sxs-lookup"><span data-stu-id="54b36-125">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

<span data-ttu-id="54b36-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="54b36-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span></span>

<span data-ttu-id="54b36-127">En una aplicación MVC, no explícitamente decorar el método de acción de controlador de errores con atributos del método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="54b36-127">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="54b36-128">Uso de verbos explícitos podría impedir que algunas solicitudes de alcanzar el método.</span><span class="sxs-lookup"><span data-stu-id="54b36-128">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="54b36-129">Configuración de páginas de códigos de estado</span><span class="sxs-lookup"><span data-stu-id="54b36-129">Configuring status code pages</span></span>

<span data-ttu-id="54b36-130">De forma predeterminada, la aplicación no proporcionará una página de códigos de estado enriquecido para los códigos de estado HTTP como 500 (Error interno del servidor) o 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="54b36-130">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="54b36-131">Puede configurar la `StatusCodePagesMiddleware` agregando una línea a la `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="54b36-131">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="54b36-132">De forma predeterminada, este middleware agrega controladores simples, de sólo texto para los códigos de estado comunes, por ejemplo, 404:</span><span class="sxs-lookup"><span data-stu-id="54b36-132">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="54b36-134">El middleware admite varios métodos de extensión distinto.</span><span class="sxs-lookup"><span data-stu-id="54b36-134">The middleware supports several different extension methods.</span></span> <span data-ttu-id="54b36-135">Una toma una expresión lambda, el otro toma una cadena de contenido del tipo y el formato.</span><span class="sxs-lookup"><span data-stu-id="54b36-135">One takes a lambda expression, another takes a content type and format string.</span></span>

<span data-ttu-id="54b36-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span><span class="sxs-lookup"><span data-stu-id="54b36-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="54b36-137">También hay métodos de extensión de redirección.</span><span class="sxs-lookup"><span data-stu-id="54b36-137">There are also redirect extension methods.</span></span> <span data-ttu-id="54b36-138">Uno envía un código de 302 estado al cliente, y uno devuelve el código de estado original al cliente, pero también ejecuta el controlador para la dirección URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="54b36-138">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

<span data-ttu-id="54b36-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span><span class="sxs-lookup"><span data-stu-id="54b36-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="54b36-140">Si tiene que deshabilitar páginas de códigos de estado para algunas solicitudes, puede hacerlo:</span><span class="sxs-lookup"><span data-stu-id="54b36-140">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="54b36-141">Código de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="54b36-141">Exception-handling code</span></span>

<span data-ttu-id="54b36-142">Código de páginas de control de excepciones puede producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="54b36-142">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="54b36-143">A menudo resulta una buena idea de páginas de errores de producción para que se componga de contenido estático únicamente.</span><span class="sxs-lookup"><span data-stu-id="54b36-143">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="54b36-144">Además, tenga en cuenta que una vez que se ha enviado los encabezados de respuesta, no se puede cambiar el código de estado de la respuesta, ni puede ejecutar ningún controlador o páginas de excepción.</span><span class="sxs-lookup"><span data-stu-id="54b36-144">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="54b36-145">La respuesta debe completarse o anuló la conexión.</span><span class="sxs-lookup"><span data-stu-id="54b36-145">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="54b36-146">Control de excepciones de servidor</span><span class="sxs-lookup"><span data-stu-id="54b36-146">Server exception handling</span></span>

<span data-ttu-id="54b36-147">Además de la lógica de la aplicación de control de excepciones del [server](servers/index.md) hospeda la aplicación realizará algunas excepciones.</span><span class="sxs-lookup"><span data-stu-id="54b36-147">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app will perform some exception handling.</span></span> <span data-ttu-id="54b36-148">Si el servidor detecta una excepción antes de que se han enviado los encabezados que envía una respuesta de Error de servidor interno 500 sin cuerpo.</span><span class="sxs-lookup"><span data-stu-id="54b36-148">If the server catches an exception before the headers have been sent it sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="54b36-149">Si detecta una excepción después de han enviado los encabezados, cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="54b36-149">If it catches an exception after the headers have been sent, it closes the connection.</span></span> <span data-ttu-id="54b36-150">Las solicitudes que no son controladas por la aplicación será procesadas por el servidor y controlará cualquier excepción que se produce por la excepción del servidor de control.</span><span class="sxs-lookup"><span data-stu-id="54b36-150">Requests that are not handled by your app will be handled by the server, and any exception that occurs will be handled by the server's exception handling.</span></span> <span data-ttu-id="54b36-151">Las páginas de errores personalizadas o control middleware o ha configurado para la aplicación de filtros de excepciones no afectará a este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="54b36-151">Any custom error pages or exception handling middleware or filters you have configured for your app will not affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="54b36-152">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="54b36-152">Startup exception handling</span></span>

<span data-ttu-id="54b36-153">Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54b36-153">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="54b36-154">Las excepciones que se producen durante el inicio de la aplicación pueden afectar al comportamiento del servidor.</span><span class="sxs-lookup"><span data-stu-id="54b36-154">Exceptions that occur during app startup can impact server behavior.</span></span> <span data-ttu-id="54b36-155">Por ejemplo, si se produce una excepción antes de llamar a `KestrelServerOptions.UseHttps`, el nivel de hospedaje detecta la excepción, inicia el servidor y se muestra una página de error en el puerto no SSL.</span><span class="sxs-lookup"><span data-stu-id="54b36-155">For example, if an exception happens before you call `KestrelServerOptions.UseHttps`, the hosting layer catches the exception, starts the server, and displays an error page on the non-SSL port.</span></span> <span data-ttu-id="54b36-156">Si se produce una excepción cuando se ejecuta esa línea, se sirve la página de error a través de HTTPS en su lugar.</span><span class="sxs-lookup"><span data-stu-id="54b36-156">If an exception happens after that line executes, the error page is served over HTTPS instead.</span></span>

<span data-ttu-id="54b36-157">También puede [configurar cómo se comportará el host en respuesta a errores durante el inicio](hosting.md#configuring-a-host) con `CaptureStartupErrors` y `detailedErrors` clave.</span><span class="sxs-lookup"><span data-stu-id="54b36-157">You can [configure how the host will behave in response to errors during startup](hosting.md#configuring-a-host) using `CaptureStartupErrors` and the `detailedErrors` key.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="54b36-158">Control de errores de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="54b36-158">ASP.NET MVC error handling</span></span>

<span data-ttu-id="54b36-159">[MVC](../mvc/index.md) aplicaciones tienen algunas opciones adicionales para controlar los errores, como la configuración de filtros de excepciones y realizar la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="54b36-159">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="54b36-160">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="54b36-160">Exception Filters</span></span>

<span data-ttu-id="54b36-161">Filtros de excepciones se pueden configurar globalmente o según una por controlador o por cada acción en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="54b36-161">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="54b36-162">Estos filtros administrar cualquier excepción no controlada que se produce durante la ejecución de una acción de controlador o de otro filtro y no se llaman en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="54b36-162">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="54b36-163">Más información acerca de los filtros de excepción en [filtros](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-163">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="54b36-164">Los filtros de excepción son buenos para la captura de excepciones que se producen dentro de las acciones de MVC, pero no son lo más flexibles middleware de control de errores.</span><span class="sxs-lookup"><span data-stu-id="54b36-164">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="54b36-165">Prefiere middleware para el caso general y usar filtros sólo donde es necesario para realizar el control de errores *diferente* en función de qué acción de MVC se ha elegido.</span><span class="sxs-lookup"><span data-stu-id="54b36-165">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="54b36-166">Estado del modelo de control de errores</span><span class="sxs-lookup"><span data-stu-id="54b36-166">Handling Model State Errors</span></span>

<span data-ttu-id="54b36-167">[Validación de modelos](../mvc/models/validation.md) se produce antes de cada acción de controlador que se invoca, y es responsabilidad del método de acción para inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="54b36-167">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="54b36-168">Algunas aplicaciones opte por seguir una convención estándar para tratar los errores de validación del modelo, en cuyo caso una [filtro](../mvc/controllers/filters.md) puede ser un lugar adecuado para implementar esta directiva.</span><span class="sxs-lookup"><span data-stu-id="54b36-168">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="54b36-169">Debe probar cómo se comportan las acciones con los Estados del modelo no válido.</span><span class="sxs-lookup"><span data-stu-id="54b36-169">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="54b36-170">Obtenga más información en [para probar la lógica de controlador](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-170">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



