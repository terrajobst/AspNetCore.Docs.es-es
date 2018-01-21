---
title: Control de errores en ASP.NET Core
author: ardalis
description: "Descubra cómo controlar errores en las aplicaciones de ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49507e90cd659be5da08df17e175297adad0fea1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="6114a-103">Introducción a control de errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6114a-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="6114a-104">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="6114a-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="6114a-105">Este artículo tratan appoaches común para controlar los errores en las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6114a-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="6114a-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6114a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="6114a-107">La página de excepción para desarrolladores</span><span class="sxs-lookup"><span data-stu-id="6114a-107">The developer exception page</span></span>

<span data-ttu-id="6114a-108">Para configurar una aplicación para mostrar una página que muestra información detallada sobre las excepciones, instale el `Microsoft.AspNetCore.Diagnostics` NuGet empaquetar y agregue una línea a la [Configurar método de la clase de inicio](startup.md):</span><span class="sxs-lookup"><span data-stu-id="6114a-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="6114a-109">Colocar `UseDeveloperExceptionPage` antes de cualquier middleware que desea detectar excepciones, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="6114a-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="6114a-110">Habilitar la página de excepción para desarrolladores **sólo cuando la aplicación se ejecuta en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="6114a-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="6114a-111">No desea compartir públicamente información de excepción detallada cuando se ejecute la aplicación en producción.</span><span class="sxs-lookup"><span data-stu-id="6114a-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="6114a-112">[Más información acerca de cómo configurar entornos](environments.md).</span><span class="sxs-lookup"><span data-stu-id="6114a-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="6114a-113">Para ver la página de excepción para desarrolladores, ejecute la aplicación de ejemplo con el entorno establecido en `Development`y agregue `?throw=true` a la dirección URL base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6114a-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="6114a-114">La página incluye varias pestañas con información sobre la excepción y la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6114a-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="6114a-115">La primera ficha incluye un seguimiento de pila.</span><span class="sxs-lookup"><span data-stu-id="6114a-115">The first tab includes a stack trace.</span></span> 

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="6114a-117">La ficha siguiente muestra la consulta de parámetros de cadena, si existe.</span><span class="sxs-lookup"><span data-stu-id="6114a-117">The next tab shows the query string parameters, if any.</span></span>

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="6114a-119">Esta solicitud no tenía ninguna cookie, pero si lo hiciera, aparecerían en el **Cookies** ficha. Puede ver los encabezados que se pasaron en la última ficha.</span><span class="sxs-lookup"><span data-stu-id="6114a-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="6114a-121">Configurar una página de control de excepciones personalizadas</span><span class="sxs-lookup"><span data-stu-id="6114a-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="6114a-122">Es una buena idea para configurar una página de controlador de excepción que se usará cuando la aplicación no se está ejecutando el `Development` entorno.</span><span class="sxs-lookup"><span data-stu-id="6114a-122">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="6114a-123">En una aplicación MVC, no explícitamente decorar el método de acción de controlador de errores con atributos del método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="6114a-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="6114a-124">Uso de verbos explícitos podría impedir que algunas solicitudes de alcanzar el método.</span><span class="sxs-lookup"><span data-stu-id="6114a-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="6114a-125">Configuración de páginas de códigos de estado</span><span class="sxs-lookup"><span data-stu-id="6114a-125">Configuring status code pages</span></span>

<span data-ttu-id="6114a-126">De forma predeterminada, la aplicación no proporcionará una página de códigos de estado enriquecido para los códigos de estado HTTP como 500 (Error interno del servidor) o 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="6114a-126">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="6114a-127">Puede configurar la `StatusCodePagesMiddleware` agregando una línea a la `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="6114a-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="6114a-128">De forma predeterminada, este middleware agrega controladores simples, de sólo texto para los códigos de estado comunes, por ejemplo, 404:</span><span class="sxs-lookup"><span data-stu-id="6114a-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="6114a-130">El middleware admite varios métodos de extensión distinto.</span><span class="sxs-lookup"><span data-stu-id="6114a-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="6114a-131">Una toma una expresión lambda, el otro toma una cadena de contenido del tipo y el formato.</span><span class="sxs-lookup"><span data-stu-id="6114a-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="6114a-132">También hay métodos de extensión de redirección.</span><span class="sxs-lookup"><span data-stu-id="6114a-132">There are also redirect extension methods.</span></span> <span data-ttu-id="6114a-133">Uno envía un código de 302 estado al cliente, y uno devuelve el código de estado original al cliente, pero también ejecuta el controlador para la dirección URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="6114a-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="6114a-134">Si tiene que deshabilitar páginas de códigos de estado para algunas solicitudes, puede hacerlo:</span><span class="sxs-lookup"><span data-stu-id="6114a-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="6114a-135">Código de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="6114a-135">Exception-handling code</span></span>

<span data-ttu-id="6114a-136">Código de páginas de control de excepciones puede producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="6114a-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="6114a-137">A menudo resulta una buena idea de páginas de errores de producción para que se componga de contenido estático únicamente.</span><span class="sxs-lookup"><span data-stu-id="6114a-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="6114a-138">Además, tenga en cuenta que una vez que se ha enviado los encabezados de respuesta, no se puede cambiar el código de estado de la respuesta, ni puede ejecutar ningún controlador o páginas de excepción.</span><span class="sxs-lookup"><span data-stu-id="6114a-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="6114a-139">La respuesta debe completarse o anuló la conexión.</span><span class="sxs-lookup"><span data-stu-id="6114a-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="6114a-140">Control de excepciones de servidor</span><span class="sxs-lookup"><span data-stu-id="6114a-140">Server exception handling</span></span>

<span data-ttu-id="6114a-141">Además de la lógica de la aplicación de control de excepciones del [server](servers/index.md) hospede su aplicación realiza algunas excepciones.</span><span class="sxs-lookup"><span data-stu-id="6114a-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="6114a-142">Si el servidor detecta una excepción antes de que se envían los encabezados, el servidor envía una respuesta de Error de servidor interno 500 sin cuerpo.</span><span class="sxs-lookup"><span data-stu-id="6114a-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="6114a-143">Si el servidor detecta una excepción después de han enviado los encabezados, el servidor cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="6114a-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="6114a-144">El servidor controla las solicitudes no están administradas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6114a-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="6114a-145">Cualquier excepción que se está controlando de excepción del servidor de control.</span><span class="sxs-lookup"><span data-stu-id="6114a-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="6114a-146">Cualquier configurar páginas de errores personalizadas o middleware de control de excepciones o filtros no afectan a este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="6114a-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="6114a-147">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="6114a-147">Startup exception handling</span></span>

<span data-ttu-id="6114a-148">Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6114a-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="6114a-149">También puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](hosting.md#detailed-errors) con `captureStartupErrors` y `detailedErrors` clave.</span><span class="sxs-lookup"><span data-stu-id="6114a-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="6114a-150">Hospedaje sólo puede mostrar una página de error para un error de inicio capturada si el error se produce después de enlace de dirección/puerto de host.</span><span class="sxs-lookup"><span data-stu-id="6114a-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="6114a-151">Si se produce un error en cualquier enlace por cualquier motivo, el nivel de hospedaje registra una excepción crítica, el bloqueo del proceso de dotnet, y no se muestra ninguna página de error.</span><span class="sxs-lookup"><span data-stu-id="6114a-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="6114a-152">Control de errores de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6114a-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="6114a-153">[MVC](../mvc/index.md) aplicaciones tienen algunas opciones adicionales para controlar los errores, como la configuración de filtros de excepciones y realizar la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="6114a-153">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="6114a-154">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="6114a-154">Exception Filters</span></span>

<span data-ttu-id="6114a-155">Filtros de excepciones se pueden configurar globalmente o según una por controlador o por cada acción en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="6114a-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="6114a-156">Estos filtros administrar cualquier excepción no controlada que se produce durante la ejecución de una acción de controlador o de otro filtro y no se llaman en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="6114a-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="6114a-157">Más información acerca de los filtros de excepción en [filtros](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="6114a-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="6114a-158">Los filtros de excepción son buenos para la captura de excepciones que se producen dentro de las acciones de MVC, pero no son lo más flexibles middleware de control de errores.</span><span class="sxs-lookup"><span data-stu-id="6114a-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="6114a-159">Prefiere middleware para el caso general y usar filtros sólo donde es necesario para realizar el control de errores *diferente* en función de qué acción de MVC se ha elegido.</span><span class="sxs-lookup"><span data-stu-id="6114a-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="6114a-160">Estado del modelo de control de errores</span><span class="sxs-lookup"><span data-stu-id="6114a-160">Handling Model State Errors</span></span>

<span data-ttu-id="6114a-161">[Validación de modelos](../mvc/models/validation.md) se produce antes de cada acción de controlador que se invoca, y es responsabilidad del método de acción para inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="6114a-161">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="6114a-162">Algunas aplicaciones opte por seguir una convención estándar para tratar los errores de validación del modelo, en cuyo caso una [filtro](../mvc/controllers/filters.md) puede ser un lugar adecuado para implementar esta directiva.</span><span class="sxs-lookup"><span data-stu-id="6114a-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="6114a-163">Debe probar cómo se comportan las acciones con los Estados del modelo no válido.</span><span class="sxs-lookup"><span data-stu-id="6114a-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="6114a-164">Obtenga más información en [para probar la lógica de controlador](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="6114a-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



