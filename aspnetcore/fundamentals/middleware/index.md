---
title: Middleware de ASP.NET Core
author: rick-anderson
description: "Obtenga información sobre el middleware de ASP.NET Core y la canalización de solicitudes."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 186faa4c02275ae1f4be53f4a2dd4f8325397bd2
ms.sourcegitcommit: c5ecda3c5b1674b62294cfddcb104e7f0b9ce465
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="f0f54-103">Middleware de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0f54-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="f0f54-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f0f54-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f0f54-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0f54-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="f0f54-106">¿Qué es el middleware?</span><span class="sxs-lookup"><span data-stu-id="f0f54-106">What is middleware?</span></span>

<span data-ttu-id="f0f54-107">El middleware es un software que se ensambla a una canalización de una aplicación para controlar las solicitudes y las respuestas.</span><span class="sxs-lookup"><span data-stu-id="f0f54-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="f0f54-108">Cada componente puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f0f54-108">Each component:</span></span>

* <span data-ttu-id="f0f54-109">Elegir si se pasa la solicitud al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="f0f54-110">Realizar trabajos antes y después de invocar al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="f0f54-111">Los delegados de solicitudes se usan para crear la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f0f54-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="f0f54-112">Estos también controlan las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0f54-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="f0f54-113">Los delegados de solicitudes se configuran con los métodos de extensión [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) y [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="f0f54-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="f0f54-114">Un delegado de solicitudes se puede especificar en línea como un método anónimo (denominado middleware en línea) o se puede definir en una clase reutilizable.</span><span class="sxs-lookup"><span data-stu-id="f0f54-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="f0f54-115">Estas clases reutilizables y métodos anónimos en línea son *middleware* o *componentes de middleware*.</span><span class="sxs-lookup"><span data-stu-id="f0f54-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="f0f54-116">Cada componente de middleware de la canalización de solicitudes es responsable de invocar al siguiente componente de la canalización o de cortocircuitar la cadena en caso de ser necesario.</span><span class="sxs-lookup"><span data-stu-id="f0f54-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="f0f54-117">En [Migrating HTTP Modules to Middleware](xref:migration/http-modules) (Migración de módulos HTTP a middleware) se explica la diferencia entre las canalizaciones de solicitudes en ASP.NET Core y ASP.NET 4.x y se proporcionan más ejemplos de middleware.</span><span class="sxs-lookup"><span data-stu-id="f0f54-117">[Migrating HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="f0f54-118">Creación de una canalización de middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="f0f54-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="f0f54-119">La canalización de solicitudes de ASP.NET Core está compuesta por una secuencia de delegados de solicitudes, a los que se llama uno después de otro, tal y como se muestra en este diagrama (el hilo de ejecución sigue las flechas negras):</span><span class="sxs-lookup"><span data-stu-id="f0f54-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![En el patrón de procesamiento de solicitudes se muestra una solicitud que llega, el procesamiento a través de tres middleware y la respuesta que abandona la aplicación.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="f0f54-123">Cada delegado puede realizar operaciones antes y después del siguiente.</span><span class="sxs-lookup"><span data-stu-id="f0f54-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="f0f54-124">También puede decidir no pasar una solicitud al siguiente delegado, lo que se denomina "cortocircuitar" la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f0f54-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="f0f54-125">Este proceso es necesario muchas veces, ya que previene la realización de trabajo innecesario.</span><span class="sxs-lookup"><span data-stu-id="f0f54-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="f0f54-126">Por ejemplo, el middleware de archivos estáticos puede devolver una solicitud para un archivo estático y cortocircuitar el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="f0f54-127">Los delegados que controlan excepciones deben llamarse al principio de la canalización para que puedan capturar las excepciones que se producen en las fases siguientes de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="f0f54-128">La aplicación ASP.NET Core más sencilla posible configura un solo delegado de solicitudes que controla todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f0f54-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="f0f54-129">En este caso no se incluye una canalización de solicitudes real.</span><span class="sxs-lookup"><span data-stu-id="f0f54-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="f0f54-130">En su lugar, solo se llama a una única función anónima en respuesta a todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0f54-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="f0f54-131">El primer delegado [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) finaliza la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="f0f54-132">Puede encadenar varios delegados de solicitudes con [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="f0f54-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="f0f54-133">El parámetro `next` representa el siguiente delegado de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="f0f54-134">Recuerde que puede cortocircuitar la canalización si *no* llama al *siguiente* parámetro. Normalmente puede realizar acciones antes y después del siguiente delegado, tal y como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0f54-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="f0f54-135">No llame a `next.Invoke` después de haber enviado la respuesta al cliente.</span><span class="sxs-lookup"><span data-stu-id="f0f54-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="f0f54-136">Se producirá una excepción si se modifica `HttpResponse` después de haber iniciado la respuesta.</span><span class="sxs-lookup"><span data-stu-id="f0f54-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="f0f54-137">Por ejemplo, se producirá una excepción al realizar cambios como el establecimiento de encabezados, el código de estado, etc.</span><span class="sxs-lookup"><span data-stu-id="f0f54-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="f0f54-138">Si escribe en el cuerpo de la respuesta después de llamar a `next`:</span><span class="sxs-lookup"><span data-stu-id="f0f54-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="f0f54-139">Puede provocar una infracción del protocolo.</span><span class="sxs-lookup"><span data-stu-id="f0f54-139">May cause a protocol violation.</span></span> <span data-ttu-id="f0f54-140">Por ejemplo, si escribe más de la longitud `content-length` establecida.</span><span class="sxs-lookup"><span data-stu-id="f0f54-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="f0f54-141">Puede dañar el formato del cuerpo.</span><span class="sxs-lookup"><span data-stu-id="f0f54-141">May corrupt the body format.</span></span> <span data-ttu-id="f0f54-142">Por ejemplo, si escribe un pie de página en HTML en un archivo CSS.</span><span class="sxs-lookup"><span data-stu-id="f0f54-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="f0f54-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) es una sugerencia útil para indicar si se han enviado los encabezados o si se han realizado escrituras en el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="f0f54-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="f0f54-144">Ordenación</span><span class="sxs-lookup"><span data-stu-id="f0f54-144">Ordering</span></span>

<span data-ttu-id="f0f54-145">El orden en el que se agregan los componentes de middleware en el método `Configure` define el orden en el que se invocarán en las solicitudes y el orden inverso de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="f0f54-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="f0f54-146">Este orden es esencial por motivos de seguridad, rendimiento y funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="f0f54-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="f0f54-147">El método Configure (que se muestra aquí) agrega los siguientes componentes de middleware:</span><span class="sxs-lookup"><span data-stu-id="f0f54-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="f0f54-148">Control de errores y excepciones</span><span class="sxs-lookup"><span data-stu-id="f0f54-148">Exception/error handling</span></span>
2. <span data-ttu-id="f0f54-149">Servidor de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="f0f54-149">Static file server</span></span>
3. <span data-ttu-id="f0f54-150">Autenticación</span><span class="sxs-lookup"><span data-stu-id="f0f54-150">Authentication</span></span>
4. <span data-ttu-id="f0f54-151">MVC</span><span class="sxs-lookup"><span data-stu-id="f0f54-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0f54-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0f54-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0f54-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0f54-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="f0f54-154">En el código anterior, `UseExceptionHandler` es el primer componente de middleware que se agrega a la canalización. Por tanto, captura todas las excepciones que se puedan producir en las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="f0f54-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="f0f54-155">El middleware de archivos estáticos se llama al principio de la canalización para que pueda controlar solicitudes y realizar cortocircuitos sin pasar por los componentes restantes.</span><span class="sxs-lookup"><span data-stu-id="f0f54-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="f0f54-156">Este middleware **no** proporciona comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="f0f54-157">Los archivos que proporciona, incluidos los de *wwwroot*, están disponibles de forma pública.</span><span class="sxs-lookup"><span data-stu-id="f0f54-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="f0f54-158">Vea [Working with static files](xref:fundamentals/static-files) (Trabajo con archivos estáticos) para obtener información sobre cómo proteger archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="f0f54-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0f54-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0f54-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="f0f54-160">Si el middleware de archivos estáticos no controla la solicitud, se pasa al middleware de identidad (`app.UseAuthentication`), que realiza la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="f0f54-161">Este middleware no cortocircuita las solicitudes sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="f0f54-162">Aunque autentique solicitudes, la autorización (y el rechazo) se producen después de que MVC seleccione una página de Razor o un control y una acción concretos.</span><span class="sxs-lookup"><span data-stu-id="f0f54-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0f54-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0f54-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f0f54-164">Si el middleware de archivos estáticos no controla la solicitud, se pasa al middleware de identidad (`app.UseIdentity`), que realiza la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="f0f54-165">Este middleware no cortocircuita las solicitudes sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="f0f54-166">Aunque autentique solicitudes, la autorización (y el rechazo) se producen después de que MVC seleccione un control y una acción concretos.</span><span class="sxs-lookup"><span data-stu-id="f0f54-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="f0f54-167">En el ejemplo siguiente se muestra un orden de middleware en el que el middleware de archivos estáticos controla las solicitudes de archivos estáticos antes del middleware de compresión de respuestas.</span><span class="sxs-lookup"><span data-stu-id="f0f54-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="f0f54-168">Los archivos estáticos no se comprimen con este orden de middleware.</span><span class="sxs-lookup"><span data-stu-id="f0f54-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="f0f54-169">Las respuestas MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) se pueden comprimir.</span><span class="sxs-lookup"><span data-stu-id="f0f54-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="f0f54-170">Use, Run y Map</span><span class="sxs-lookup"><span data-stu-id="f0f54-170">Use, Run, and Map</span></span>

<span data-ttu-id="f0f54-171">Puede configurar la canalización HTTP con `Use`, `Run` y `Map`.</span><span class="sxs-lookup"><span data-stu-id="f0f54-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="f0f54-172">El método `Use` puede cortocircuitar la canalización (solo si no llama a un delegado de solicitudes `next`).</span><span class="sxs-lookup"><span data-stu-id="f0f54-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="f0f54-173">`Run` es una convención y es posible que algunos componentes de middleware expongan métodos `Run[Middleware]` que se ejecutan al final de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="f0f54-174">Las extensiones `Map*` se usan como convenciones para la creación de ramas en la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="f0f54-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) crea una rama de la canalización de solicitudes según las coincidencias de la ruta de solicitud proporcionada.</span><span class="sxs-lookup"><span data-stu-id="f0f54-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="f0f54-176">Si la ruta de solicitud comienza con la ruta proporcionada, se ejecuta la creación de la rama.</span><span class="sxs-lookup"><span data-stu-id="f0f54-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="f0f54-177">En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior:</span><span class="sxs-lookup"><span data-stu-id="f0f54-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="f0f54-178">Solicitud</span><span class="sxs-lookup"><span data-stu-id="f0f54-178">Request</span></span> | <span data-ttu-id="f0f54-179">Respuesta</span><span class="sxs-lookup"><span data-stu-id="f0f54-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="f0f54-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="f0f54-180">localhost:1234</span></span> | <span data-ttu-id="f0f54-181">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="f0f54-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="f0f54-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="f0f54-182">localhost:1234/map1</span></span> | <span data-ttu-id="f0f54-183">Prueba 1 de Map</span><span class="sxs-lookup"><span data-stu-id="f0f54-183">Map Test 1</span></span> |
| <span data-ttu-id="f0f54-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="f0f54-184">localhost:1234/map2</span></span> | <span data-ttu-id="f0f54-185">Prueba 2 de Map</span><span class="sxs-lookup"><span data-stu-id="f0f54-185">Map Test 2</span></span> |
| <span data-ttu-id="f0f54-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="f0f54-186">localhost:1234/map3</span></span> | <span data-ttu-id="f0f54-187">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="f0f54-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="f0f54-188">Cuando se usa `Map`, los segmentos de ruta que coincidan se eliminan de `HttpRequest.Path` y se anexan a `HttpRequest.PathBase` por cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="f0f54-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="f0f54-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) crea una rama de la canalización de solicitudes según el resultado del predicado proporcionado.</span><span class="sxs-lookup"><span data-stu-id="f0f54-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="f0f54-190">Se puede usar cualquier predicado de tipo `Func<HttpContext, bool>` para asignar solicitudes a nuevas ramas de la canalización.</span><span class="sxs-lookup"><span data-stu-id="f0f54-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="f0f54-191">En el ejemplo siguiente se usa un predicado para detectar la presencia de una `branch` variable de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="f0f54-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="f0f54-192">En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior:</span><span class="sxs-lookup"><span data-stu-id="f0f54-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="f0f54-193">Solicitud</span><span class="sxs-lookup"><span data-stu-id="f0f54-193">Request</span></span> | <span data-ttu-id="f0f54-194">Respuesta</span><span class="sxs-lookup"><span data-stu-id="f0f54-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="f0f54-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="f0f54-195">localhost:1234</span></span> | <span data-ttu-id="f0f54-196">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="f0f54-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="f0f54-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="f0f54-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="f0f54-198">Rama usada = master</span><span class="sxs-lookup"><span data-stu-id="f0f54-198">Branch used = master</span></span>|

<span data-ttu-id="f0f54-199">`Map` admite la anidación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0f54-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="f0f54-200">`Map` puede hacer coincidir varios segmentos a la vez, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0f54-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="f0f54-201">Middleware integrado</span><span class="sxs-lookup"><span data-stu-id="f0f54-201">Built-in middleware</span></span>

<span data-ttu-id="f0f54-202">ASP.NET Core incluye los siguientes componentes de middleware, así como una descripción del orden en el que se deberían agregar:</span><span class="sxs-lookup"><span data-stu-id="f0f54-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="f0f54-203">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="f0f54-203">Middleware</span></span> | <span data-ttu-id="f0f54-204">Description</span><span class="sxs-lookup"><span data-stu-id="f0f54-204">Description</span></span> | <span data-ttu-id="f0f54-205">Orden</span><span class="sxs-lookup"><span data-stu-id="f0f54-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="f0f54-206">Autenticación</span><span class="sxs-lookup"><span data-stu-id="f0f54-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="f0f54-207">Proporciona compatibilidad con autenticación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-207">Provides authentication support.</span></span> | <span data-ttu-id="f0f54-208">Antes de que se necesite `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="f0f54-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="f0f54-209">Terminal para devoluciones de llamadas OAuth.</span><span class="sxs-lookup"><span data-stu-id="f0f54-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="f0f54-210">CORS</span><span class="sxs-lookup"><span data-stu-id="f0f54-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="f0f54-211">Configura el uso compartido de recursos entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="f0f54-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="f0f54-212">Antes de los componentes que usan CORS.</span><span class="sxs-lookup"><span data-stu-id="f0f54-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="f0f54-213">Diagnóstico</span><span class="sxs-lookup"><span data-stu-id="f0f54-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="f0f54-214">Configura el diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="f0f54-214">Configures diagnostics.</span></span> | <span data-ttu-id="f0f54-215">Antes de los componentes que generan errores.</span><span class="sxs-lookup"><span data-stu-id="f0f54-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="f0f54-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="f0f54-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="f0f54-217">Reenvía encabezados con proxy a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="f0f54-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="f0f54-218">Antes de los componentes que consumen los campos actualizados (ejemplos: Scheme, Host, ClientIP, Method).</span><span class="sxs-lookup"><span data-stu-id="f0f54-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="f0f54-219">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="f0f54-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="f0f54-220">Proporciona compatibilidad con la captura de respuestas.</span><span class="sxs-lookup"><span data-stu-id="f0f54-220">Provides support for caching responses.</span></span> | <span data-ttu-id="f0f54-221">Antes de los componentes que requieren el almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="f0f54-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="f0f54-222">Compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="f0f54-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="f0f54-223">Proporciona compatibilidad con la compresión de respuestas.</span><span class="sxs-lookup"><span data-stu-id="f0f54-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="f0f54-224">Antes de los componentes que requieren compresión.</span><span class="sxs-lookup"><span data-stu-id="f0f54-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="f0f54-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="f0f54-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="f0f54-226">Proporciona compatibilidad con ubicación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-226">Provides localization support.</span></span> | <span data-ttu-id="f0f54-227">Antes de los componentes que dependen de la ubicación.</span><span class="sxs-lookup"><span data-stu-id="f0f54-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="f0f54-228">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="f0f54-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="f0f54-229">Define y restringe las rutas de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="f0f54-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="f0f54-230">Terminal para rutas que coincidan.</span><span class="sxs-lookup"><span data-stu-id="f0f54-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="f0f54-231">Sesión</span><span class="sxs-lookup"><span data-stu-id="f0f54-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="f0f54-232">Proporciona compatibilidad con la administración de sesiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="f0f54-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="f0f54-233">Antes de los componentes que requieren Session.</span><span class="sxs-lookup"><span data-stu-id="f0f54-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="f0f54-234">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="f0f54-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="f0f54-235">Proporciona compatibilidad con la proporción de archivos estáticos y la exploración de directorios.</span><span class="sxs-lookup"><span data-stu-id="f0f54-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="f0f54-236">Terminal si hay una solicitud que coincida con archivos.</span><span class="sxs-lookup"><span data-stu-id="f0f54-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="f0f54-237">Reescritura de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="f0f54-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="f0f54-238">Proporciona compatibilidad con la reescritura de direcciones URL y la redirección de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f0f54-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="f0f54-239">Antes de los componentes que consumen la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="f0f54-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="f0f54-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="f0f54-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="f0f54-241">Habilita el protocolo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f0f54-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="f0f54-242">Antes de los componentes necesarios para aceptar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f0f54-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="f0f54-243">Escritura de middleware</span><span class="sxs-lookup"><span data-stu-id="f0f54-243">Writing middleware</span></span>

<span data-ttu-id="f0f54-244">El middleware normalmente está encapsulado en una clase y se expone con un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="f0f54-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="f0f54-245">Use el siguiente middleware a modo de ejemplo. En este se establece la referencia cultural de la solicitud actual a partir de la cadena de solicitud:</span><span class="sxs-lookup"><span data-stu-id="f0f54-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="f0f54-246">Nota: El código de ejemplo anterior se usa para mostrar la creación de un componente de middleware.</span><span class="sxs-lookup"><span data-stu-id="f0f54-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="f0f54-247">Vea [Globalization and localization](xref:fundamentals/localization) (Globalización y localización) para ver la compatibilidad con localización integrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0f54-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="f0f54-248">Puede probar el middleware pasando la referencia cultural, por ejemplo, `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="f0f54-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="f0f54-249">El código siguiente mueve el delegado de middleware a una clase:</span><span class="sxs-lookup"><span data-stu-id="f0f54-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="f0f54-250">En ASP.NET Core 1.x, el nombre del método del middleware `Task` debe ser `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="f0f54-250">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="f0f54-251">En ASP.NET Core 2.0 o posterior, el nombre puede ser `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0f54-251">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="f0f54-252">El método de extensión siguiente expone el middleware mediante [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="f0f54-252">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="f0f54-253">El código siguiente llama al middleware desde `Configure`:</span><span class="sxs-lookup"><span data-stu-id="f0f54-253">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f0f54-254">El middleware debería seguir el [principio de dependencias explicitas](http://deviq.com/explicit-dependencies-principle/) mediante la exposición de sus dependencias en el constructor.</span><span class="sxs-lookup"><span data-stu-id="f0f54-254">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="f0f54-255">El middleware se construye una vez por *duración de la aplicación*.</span><span class="sxs-lookup"><span data-stu-id="f0f54-255">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="f0f54-256">Vea *Dependencias bajo solicitud* a continuación si necesita compartir servicios con middleware en una solicitud.</span><span class="sxs-lookup"><span data-stu-id="f0f54-256">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="f0f54-257">Los componentes de middleware pueden resolver las dependencias de una inserción de dependencias mediante parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="f0f54-257">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="f0f54-258">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) también puede aceptar parámetros adicionales directamente.</span><span class="sxs-lookup"><span data-stu-id="f0f54-258">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="f0f54-259">Dependencias bajo solicitud</span><span class="sxs-lookup"><span data-stu-id="f0f54-259">Per-request dependencies</span></span>

<span data-ttu-id="f0f54-260">Dado que el middleware se construye al inicio de la aplicación y no bajo solicitud, los servicios de duración *con ámbito* que usan los constructores de middleware no se comparten con otros tipos insertados mediante dependencias durante cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="f0f54-260">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="f0f54-261">Si debe compartir un servicio *con ámbito* entre su middleware y otros tipos, agregue esos servicios a la signatura del método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="f0f54-261">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="f0f54-262">El método `Invoke` puede aceptar parámetros adicionales que propaga la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="f0f54-262">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="f0f54-263">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0f54-263">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="f0f54-264">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f0f54-264">Additional resources</span></span>

* [<span data-ttu-id="f0f54-265">Migración de módulos HTTP a middleware</span><span class="sxs-lookup"><span data-stu-id="f0f54-265">Migrating HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="f0f54-266">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="f0f54-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="f0f54-267">Solicitud de características</span><span class="sxs-lookup"><span data-stu-id="f0f54-267">Request Features</span></span>](xref:fundamentals/request-features)
* <span data-ttu-id="f0f54-268">[Factory-based middleware activation](xref:fundamentals/middleware/extensibility) (Activación de middleware basada en Factory)</span><span class="sxs-lookup"><span data-stu-id="f0f54-268">[Factory-based middleware activation](xref:fundamentals/middleware/extensibility)</span></span>
* [<span data-ttu-id="f0f54-269">Activación de middleware con un contenedor de terceros</span><span class="sxs-lookup"><span data-stu-id="f0f54-269">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
