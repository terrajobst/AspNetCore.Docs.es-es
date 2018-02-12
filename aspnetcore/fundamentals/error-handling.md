---
title: Control de errores en ASP.NET Core
author: ardalis
description: "Descubra cómo controlar errores en aplicaciones ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="454a9-103">Introducción al control de errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="454a9-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="454a9-104">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="454a9-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="454a9-105">Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="454a9-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="454a9-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="454a9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="454a9-107">Página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="454a9-107">The developer exception page</span></span>

<span data-ttu-id="454a9-108">Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones, instale el paquete NuGet `Microsoft.AspNetCore.Diagnostics` y agregue una línea al [método Configure de la clase Startup](startup.md):</span><span class="sxs-lookup"><span data-stu-id="454a9-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="454a9-109">Coloque `UseDeveloperExceptionPage` antes del software intermedio en el que quiera capturar excepciones, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="454a9-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="454a9-110">Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="454a9-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="454a9-111">No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción.</span><span class="sxs-lookup"><span data-stu-id="454a9-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="454a9-112">[Más información sobre la configuración de entornos](environments.md).</span><span class="sxs-lookup"><span data-stu-id="454a9-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="454a9-113">Para ver la página de excepciones para el desarrollador, ejecute la aplicación de ejemplo con el entorno establecido en `Development` y agregue `?throw=true` a la URL base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="454a9-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="454a9-114">La página incluye varias pestañas con información sobre la excepción y la solicitud.</span><span class="sxs-lookup"><span data-stu-id="454a9-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="454a9-115">La primera pestaña incluye un seguimiento de la pila.</span><span class="sxs-lookup"><span data-stu-id="454a9-115">The first tab includes a stack trace.</span></span> 

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="454a9-117">En la pestaña siguiente se muestran los parámetros de cadena de consulta, si los hay.</span><span class="sxs-lookup"><span data-stu-id="454a9-117">The next tab shows the query string parameters, if any.</span></span>

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="454a9-119">Esta solicitud no tenía cookies, pero en caso de que las tuviera, aparecerían en la pestaña **Cookies**. Puede ver los encabezados que se han pasado en la última pestaña.</span><span class="sxs-lookup"><span data-stu-id="454a9-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="454a9-121">Configuración de una página personalizada de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="454a9-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="454a9-122">Se recomienda configurar una página de controlador de excepciones para usarla cuando la aplicación no se ejecute en el entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="454a9-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="454a9-123">En una aplicación MVC, no decore explícitamente el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="454a9-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="454a9-124">El uso de verbos explícitos podría impedir que algunas solicitudes llegasen al método.</span><span class="sxs-lookup"><span data-stu-id="454a9-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="454a9-125">Configuración de páginas de códigos de estado</span><span class="sxs-lookup"><span data-stu-id="454a9-125">Configuring status code pages</span></span>

<span data-ttu-id="454a9-126">De manera predeterminada, la aplicación no proporciona una página de códigos de estado enriquecida para códigos de estado HTTP como 500 (Error interno del servidor) o 404 (No encontrado).</span><span class="sxs-lookup"><span data-stu-id="454a9-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="454a9-127">Para configurar `StatusCodePagesMiddleware`, agregue una línea al método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="454a9-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="454a9-128">De forma predeterminada, este software intermedio agrega controladores simples de solo texto para códigos de estado comunes, como 404:</span><span class="sxs-lookup"><span data-stu-id="454a9-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="454a9-130">El software intermedio admite diversos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="454a9-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="454a9-131">Uno toma una expresión lambda, el otro toma un tipo de contenido y una cadena de formato.</span><span class="sxs-lookup"><span data-stu-id="454a9-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="454a9-132">También hay métodos de extensión de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="454a9-132">There are also redirect extension methods.</span></span> <span data-ttu-id="454a9-133">Uno envía un código de estado 302 al cliente, mientras que el otro devuelve el código de estado original al cliente, pero también ejecuta el controlador para la dirección URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="454a9-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="454a9-134">Si necesita deshabilitar las páginas de códigos de estado para algunas solicitudes, puede hacerlo de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="454a9-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="454a9-135">Código de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="454a9-135">Exception-handling code</span></span>

<span data-ttu-id="454a9-136">El código de las páginas de control de excepciones puede producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="454a9-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="454a9-137">Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.</span><span class="sxs-lookup"><span data-stu-id="454a9-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="454a9-138">Además, tenga en cuenta que una vez que se hayan enviado los encabezados de una respuesta, no se podrá cambiar el código de estado de la respuesta ni se podrá ejecutar ninguna página de excepción o controlador.</span><span class="sxs-lookup"><span data-stu-id="454a9-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="454a9-139">Deberá completarse la respuesta o anularse la conexión.</span><span class="sxs-lookup"><span data-stu-id="454a9-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="454a9-140">Control de excepciones del servidor</span><span class="sxs-lookup"><span data-stu-id="454a9-140">Server exception handling</span></span>

<span data-ttu-id="454a9-141">Además de la lógica de control de excepciones de la aplicación, el [servidor](servers/index.md) que hospede la aplicación llevará a cabo el control de algunas excepciones.</span><span class="sxs-lookup"><span data-stu-id="454a9-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="454a9-142">Si el servidor detecta una excepción antes de que se envíen los encabezados, enviará una respuesta "500 Error interno del servidor" sin cuerpo.</span><span class="sxs-lookup"><span data-stu-id="454a9-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="454a9-143">Si el servidor detecta una excepción después de que se hayan enviado los encabezados, cerrará la conexión.</span><span class="sxs-lookup"><span data-stu-id="454a9-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="454a9-144">El servidor controla las solicitudes que no controla la aplicación.</span><span class="sxs-lookup"><span data-stu-id="454a9-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="454a9-145">El control de excepciones del servidor controla todas las excepciones que se producen.</span><span class="sxs-lookup"><span data-stu-id="454a9-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="454a9-146">Las páginas de error personalizadas y el software intermedio o filtros de control de excepciones que se hayan configurado no afectarán a este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="454a9-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="454a9-147">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="454a9-147">Startup exception handling</span></span>

<span data-ttu-id="454a9-148">Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="454a9-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="454a9-149">También puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](hosting.md#detailed-errors) con `captureStartupErrors` y la clave `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="454a9-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="454a9-150">El hospedaje solo puede mostrar una página de error para un error de inicio capturado si este se produce después del enlace de puerto/dirección del host.</span><span class="sxs-lookup"><span data-stu-id="454a9-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="454a9-151">Si se produce un error en algún enlace por cualquier motivo, el nivel de hospedaje registra una excepción crítica, el proceso de dotnet se bloquea y no se muestra ninguna página de error.</span><span class="sxs-lookup"><span data-stu-id="454a9-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="454a9-152">Control de errores de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="454a9-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="454a9-153">Las aplicaciones [MVC](xref:mvc/overview) tienen algunas opciones adicionales para controlar errores, como la configuración de filtros de excepciones y la realización de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="454a9-153">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="454a9-154">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="454a9-154">Exception Filters</span></span>

<span data-ttu-id="454a9-155">Los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción, en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="454a9-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="454a9-156">Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro; en caso contrario, no se les llama.</span><span class="sxs-lookup"><span data-stu-id="454a9-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="454a9-157">Encontrará más información sobre los filtros de excepciones en [Filtros](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="454a9-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="454a9-158">Los filtros de excepciones están indicados para capturar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el software intermedio de control de errores.</span><span class="sxs-lookup"><span data-stu-id="454a9-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="454a9-159">Use de preferencia el software intermedio para los casos generales y aplique filtros solo cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida.</span><span class="sxs-lookup"><span data-stu-id="454a9-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="454a9-160">Control de errores de estado del modelo</span><span class="sxs-lookup"><span data-stu-id="454a9-160">Handling Model State Errors</span></span>

<span data-ttu-id="454a9-161">La [validación de modelos](../mvc/models/validation.md) se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="454a9-161">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="454a9-162">Algunas aplicaciones optarán por seguir una convención estándar para tratar los errores de validación de modelos, en cuyo caso un [filtro](../mvc/controllers/filters.md) podría ser el lugar adecuado para implementar esta directiva.</span><span class="sxs-lookup"><span data-stu-id="454a9-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="454a9-163">Debe probar cómo se comportan las acciones con estados de modelo no válidos.</span><span class="sxs-lookup"><span data-stu-id="454a9-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="454a9-164">Obtenga más información en [Probar la lógica del controlador](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="454a9-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



