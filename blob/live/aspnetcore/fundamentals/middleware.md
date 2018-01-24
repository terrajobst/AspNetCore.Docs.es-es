---
title: ASP.NET Core Middleware
author: rick-anderson
description: "Obtenga información sobre el software intermedio ASP.NET Core y la canalización de solicitudes."
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ef130e736e2f32fa134156d979ce5bfbedcae828
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="793e5-103">Conceptos básicos de Middleware de núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="793e5-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="793e5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="793e5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="793e5-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="793e5-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="793e5-106">¿Qué es middleware?</span><span class="sxs-lookup"><span data-stu-id="793e5-106">What is middleware?</span></span>

<span data-ttu-id="793e5-107">Software intermedio es un software que se monta en una canalización de la aplicación para controlar las solicitudes y respuestas.</span><span class="sxs-lookup"><span data-stu-id="793e5-107">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="793e5-108">Cada componente:</span><span class="sxs-lookup"><span data-stu-id="793e5-108">Each component:</span></span>

* <span data-ttu-id="793e5-109">Elige si se debe pasar la solicitud al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="793e5-110">Puede realizar el trabajo antes y después se invoca el componente siguiente en la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="793e5-111">Los delegados de la solicitud se utilizan para crear la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="793e5-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="793e5-112">Los delegados de solicitud controlen cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="793e5-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="793e5-113">Solicitar los delegados se configuran mediante [ejecutar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), y [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="793e5-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="793e5-114">Un delegado de solicitud individual puede ser especificado en línea como un método anónimo (denominado middleware en línea) o puede definirse en una clase reutilizable.</span><span class="sxs-lookup"><span data-stu-id="793e5-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="793e5-115">Estas clases reutilizables y métodos anónimos en línea están *middleware*, o *componentes de middleware*.</span><span class="sxs-lookup"><span data-stu-id="793e5-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="793e5-116">Cada componente de middleware en la canalización de solicitud es responsable de invocar el siguiente componente de la canalización o evaluación "cortocircuitada" de la cadena si es necesario.</span><span class="sxs-lookup"><span data-stu-id="793e5-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="793e5-117">[Migrar módulos HTTP de middleware](../migration/http-modules.md) explica la diferencia entre las canalizaciones de solicitud de ASP.NET Core y las versiones anteriores y proporciona más ejemplos de middleware.</span><span class="sxs-lookup"><span data-stu-id="793e5-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="793e5-118">Crear una canalización de middleware con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="793e5-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="793e5-119">La canalización de solicitudes de ASP.NET Core consta de una secuencia de delegados de la solicitud, denominada uno tras otro, como muestra en este diagrama (el subproceso de ejecución sigue las flechas negras):</span><span class="sxs-lookup"><span data-stu-id="793e5-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Patrón de procesamiento de solicitud que muestra una solicitud que llega, el procesamiento a través de tres middlewares y la respuesta salir de la aplicación.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="793e5-123">Cada delegado puede realizar operaciones antes y después de la siguiente delegado.</span><span class="sxs-lookup"><span data-stu-id="793e5-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="793e5-124">También puede decidir que un delegado no pasar una solicitud para el delegado siguiente, que se denomina evaluación "cortocircuitada" de la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="793e5-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="793e5-125">Evaluación "cortocircuitada" es a menudo deseable porque evita el trabajo innecesario.</span><span class="sxs-lookup"><span data-stu-id="793e5-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="793e5-126">Por ejemplo, el middleware de archivos estáticos puede devolver una solicitud de un archivo estático y el resto de la canalización de cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="793e5-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="793e5-127">Los delegados de control de excepciones deben llamarse al principio de la canalización, por lo que pueden detectar las excepciones que se producen en fases posteriores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="793e5-128">La aplicación de ASP.NET Core más simple posible establece un delegado de solicitud único que controla todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="793e5-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="793e5-129">En este caso no incluye una canalización de solicitud real.</span><span class="sxs-lookup"><span data-stu-id="793e5-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="793e5-130">En su lugar, se llama a una función anónima única en respuesta a cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="793e5-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="793e5-131">La primera [aplicación. Ejecutar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegado finaliza la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="793e5-132">Puede encadenar varios delegados de la solicitud junto con [aplicación. Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="793e5-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="793e5-133">El `next` parámetro representa el delegado siguiente en la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="793e5-134">(Recuerde que puede cortocircuita la canalización por *no* al llamar a la *siguiente* parámetro.) Por lo general puede realizar acciones antes y después de la siguiente delegado, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="793e5-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="793e5-135">No llame a `next.Invoke` una vez enviada la respuesta al cliente.</span><span class="sxs-lookup"><span data-stu-id="793e5-135">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="793e5-136">Cambia a `HttpResponse` una vez iniciada la respuesta se iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="793e5-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="793e5-137">Por ejemplo, los cambios, como establecer los encabezados, código de estado, etcetera, iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="793e5-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="793e5-138">Escribir en el cuerpo de respuesta después de llamar a `next`:</span><span class="sxs-lookup"><span data-stu-id="793e5-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="793e5-139">Puede provocar una infracción del protocolo.</span><span class="sxs-lookup"><span data-stu-id="793e5-139">May cause a protocol violation.</span></span> <span data-ttu-id="793e5-140">Por ejemplo, escribir más de los indicados `content-length`.</span><span class="sxs-lookup"><span data-stu-id="793e5-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="793e5-141">Podría dañarse el formato del cuerpo.</span><span class="sxs-lookup"><span data-stu-id="793e5-141">May corrupt the body format.</span></span> <span data-ttu-id="793e5-142">Por ejemplo, escribiendo un pie de página HTML en un archivo CSS.</span><span class="sxs-lookup"><span data-stu-id="793e5-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="793e5-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) es una sugerencia útil para indicar si se han enviado los encabezados o el cuerpo se ha escrito en.</span><span class="sxs-lookup"><span data-stu-id="793e5-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="793e5-144">Ordenación</span><span class="sxs-lookup"><span data-stu-id="793e5-144">Ordering</span></span>

<span data-ttu-id="793e5-145">El orden en que se agregan los componentes de middleware en la `Configure` método define el orden en el que se invocan en las solicitudes y el orden inverso para la respuesta.</span><span class="sxs-lookup"><span data-stu-id="793e5-145">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="793e5-146">Esta ordenación es fundamental para la seguridad, rendimiento y funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="793e5-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="793e5-147">El método Configure (se muestra a continuación) agrega los siguientes componentes de software intermedio:</span><span class="sxs-lookup"><span data-stu-id="793e5-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="793e5-148">Control de excepciones y errores</span><span class="sxs-lookup"><span data-stu-id="793e5-148">Exception/error handling</span></span>
2. <span data-ttu-id="793e5-149">Servidor de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="793e5-149">Static file server</span></span>
3. <span data-ttu-id="793e5-150">Autenticación</span><span class="sxs-lookup"><span data-stu-id="793e5-150">Authentication</span></span>
4. <span data-ttu-id="793e5-151">MVC</span><span class="sxs-lookup"><span data-stu-id="793e5-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="793e5-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="793e5-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="793e5-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="793e5-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="793e5-154">En el código anterior, `UseExceptionHandler` es el primer componente de middleware que se agrega a la canalización, por lo tanto, detecta cualquier excepción que se produce en las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="793e5-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="793e5-155">El middleware de archivos estáticos se llama al principio de la canalización para que pueda controlar las solicitudes y sin tener que pasar a través de los demás componentes de cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="793e5-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="793e5-156">Proporciona el middleware de archivos estáticos **sin** comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="793e5-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="793e5-157">Los archivos atendido por él, las de incluidas *wwwroot*, estén disponibles públicamente.</span><span class="sxs-lookup"><span data-stu-id="793e5-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="793e5-158">Vea [trabajar con archivos estáticos](xref:fundamentals/static-files) para obtener un enfoque proteger los archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="793e5-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="793e5-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="793e5-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="793e5-160">Si la solicitud no está controlada por el middleware de archivos estáticos, se pasa el middleware de identidad (`app.UseAuthentication`), que realiza la autenticación.</span><span class="sxs-lookup"><span data-stu-id="793e5-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="793e5-161">Identidad no cortocircuita las solicitudes no autenticadas.</span><span class="sxs-lookup"><span data-stu-id="793e5-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="793e5-162">Aunque identidad autentica las solicitudes, autorización (y rechazo) se produce después de MVC selecciona un específico Razor página o acción y controlador.</span><span class="sxs-lookup"><span data-stu-id="793e5-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="793e5-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="793e5-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="793e5-164">Si la solicitud no está controlada por el middleware de archivos estáticos, se pasa el middleware de identidad (`app.UseIdentity`), que realiza la autenticación.</span><span class="sxs-lookup"><span data-stu-id="793e5-164">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="793e5-165">Identidad no cortocircuita las solicitudes no autenticadas.</span><span class="sxs-lookup"><span data-stu-id="793e5-165">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="793e5-166">Aunque identidad autentica las solicitudes, autorización (y rechazo) se produce después de MVC selecciona una acción y controlador específico.</span><span class="sxs-lookup"><span data-stu-id="793e5-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="793e5-167">En el ejemplo siguiente se muestra un middleware de ordenación que se administran las solicitudes de archivos estáticos por el middleware de archivos estáticos antes el middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="793e5-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="793e5-168">Archivos estáticos no se comprimen con esta ordenación del middleware de.</span><span class="sxs-lookup"><span data-stu-id="793e5-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="793e5-169">Las respuestas MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) se pueden comprimir.</span><span class="sxs-lookup"><span data-stu-id="793e5-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="793e5-170">Usar, ejecutar y asignar</span><span class="sxs-lookup"><span data-stu-id="793e5-170">Use, Run, and Map</span></span>

<span data-ttu-id="793e5-171">Configurar la canalización HTTP con `Use`, `Run`, y `Map`.</span><span class="sxs-lookup"><span data-stu-id="793e5-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="793e5-172">El `Use` método puede cortocircuito la canalización (es decir, si no llama al método un `next` delegado de la solicitud).</span><span class="sxs-lookup"><span data-stu-id="793e5-172">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="793e5-173">`Run`es una convención y pueden exponer algunos componentes de middleware `Run[Middleware]` métodos que se ejecutan al final de la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="793e5-174">`Map*`las extensiones se utilizan como una convención para la bifurcación de la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="793e5-175">[Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) bifurcaciones de la canalización de solicitudes basándose en coincidencias de la ruta de acceso de solicitud dada.</span><span class="sxs-lookup"><span data-stu-id="793e5-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="793e5-176">Si la ruta de acceso de solicitud empieza con la ruta de acceso especificada, se ejecuta la rama.</span><span class="sxs-lookup"><span data-stu-id="793e5-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="793e5-177">La siguiente tabla muestra las solicitudes y respuestas de `http://localhost:1234` utilizando el código anterior:</span><span class="sxs-lookup"><span data-stu-id="793e5-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="793e5-178">Solicitud</span><span class="sxs-lookup"><span data-stu-id="793e5-178">Request</span></span> | <span data-ttu-id="793e5-179">Respuesta</span><span class="sxs-lookup"><span data-stu-id="793e5-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="793e5-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="793e5-180">localhost:1234</span></span> | <span data-ttu-id="793e5-181">Hola desde el delegado de no asignación.</span><span class="sxs-lookup"><span data-stu-id="793e5-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="793e5-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="793e5-182">localhost:1234/map1</span></span> | <span data-ttu-id="793e5-183">Prueba 1 de mapa</span><span class="sxs-lookup"><span data-stu-id="793e5-183">Map Test 1</span></span> |
| <span data-ttu-id="793e5-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="793e5-184">localhost:1234/map2</span></span> | <span data-ttu-id="793e5-185">Prueba 2 de mapa</span><span class="sxs-lookup"><span data-stu-id="793e5-185">Map Test 2</span></span> |
| <span data-ttu-id="793e5-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="793e5-186">localhost:1234/map3</span></span> | <span data-ttu-id="793e5-187">Hola desde el delegado de no asignación.</span><span class="sxs-lookup"><span data-stu-id="793e5-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="793e5-188">Cuando `Map` es utilizado, se quitan los segmentos de ruta de acceso coincidentes de `HttpRequest.Path` y agrega a `HttpRequest.PathBase` para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="793e5-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="793e5-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) bifurcaciones de la canalización de solicitudes en función del resultado del predicado proporcionado.</span><span class="sxs-lookup"><span data-stu-id="793e5-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="793e5-190">Cualquier predicado de tipo `Func<HttpContext, bool>` puede usarse para asignar solicitudes a una nueva bifurcación de la canalización.</span><span class="sxs-lookup"><span data-stu-id="793e5-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="793e5-191">En el ejemplo siguiente, se utiliza un predicado para detectar la presencia de una variable de cadena de consulta `branch`:</span><span class="sxs-lookup"><span data-stu-id="793e5-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="793e5-192">La siguiente tabla muestra las solicitudes y respuestas de `http://localhost:1234` utilizando el código anterior:</span><span class="sxs-lookup"><span data-stu-id="793e5-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="793e5-193">Solicitud</span><span class="sxs-lookup"><span data-stu-id="793e5-193">Request</span></span> | <span data-ttu-id="793e5-194">Respuesta</span><span class="sxs-lookup"><span data-stu-id="793e5-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="793e5-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="793e5-195">localhost:1234</span></span> | <span data-ttu-id="793e5-196">Hola desde el delegado de no asignación.</span><span class="sxs-lookup"><span data-stu-id="793e5-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="793e5-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="793e5-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="793e5-198">Bifurcación usa = master</span><span class="sxs-lookup"><span data-stu-id="793e5-198">Branch used = master</span></span>|

<span data-ttu-id="793e5-199">`Map`admite la anidación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="793e5-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="793e5-200">`Map`puede hacer coincidir varios segmentos a la vez, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="793e5-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="793e5-201">Middleware integrado</span><span class="sxs-lookup"><span data-stu-id="793e5-201">Built-in middleware</span></span>

<span data-ttu-id="793e5-202">ASP.NET Core incluye los siguientes componentes de software intermedio, así como una descripción del orden en el que se debe agregar:</span><span class="sxs-lookup"><span data-stu-id="793e5-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="793e5-203">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="793e5-203">Middleware</span></span> | <span data-ttu-id="793e5-204">Descripción</span><span class="sxs-lookup"><span data-stu-id="793e5-204">Description</span></span> | <span data-ttu-id="793e5-205">Orden</span><span class="sxs-lookup"><span data-stu-id="793e5-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="793e5-206">Autenticación</span><span class="sxs-lookup"><span data-stu-id="793e5-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="793e5-207">Proporciona compatibilidad con la autenticación.</span><span class="sxs-lookup"><span data-stu-id="793e5-207">Provides authentication support.</span></span> | <span data-ttu-id="793e5-208">Antes de `HttpContext.User` es necesaria.</span><span class="sxs-lookup"><span data-stu-id="793e5-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="793e5-209">Terminal para las devoluciones de llamada de OAuth.</span><span class="sxs-lookup"><span data-stu-id="793e5-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="793e5-210">CORS</span><span class="sxs-lookup"><span data-stu-id="793e5-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="793e5-211">Define el uso compartido de recursos entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="793e5-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="793e5-212">Antes de componentes que usan CORS.</span><span class="sxs-lookup"><span data-stu-id="793e5-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="793e5-213">Diagnóstico</span><span class="sxs-lookup"><span data-stu-id="793e5-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="793e5-214">Configura los diagnósticos.</span><span class="sxs-lookup"><span data-stu-id="793e5-214">Configures diagnostics.</span></span> | <span data-ttu-id="793e5-215">Antes de componentes que generan errores.</span><span class="sxs-lookup"><span data-stu-id="793e5-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="793e5-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="793e5-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="793e5-217">Reenvía encabezados procesadas por el proxy en la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="793e5-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="793e5-218">Antes de los componentes que utilizan los campos actualizados (ejemplos: esquema, Host, IP de cliente, método).</span><span class="sxs-lookup"><span data-stu-id="793e5-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="793e5-219">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="793e5-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="793e5-220">Proporciona compatibilidad para almacenar en caché las respuestas.</span><span class="sxs-lookup"><span data-stu-id="793e5-220">Provides support for caching responses.</span></span> | <span data-ttu-id="793e5-221">Antes de componentes que requieren almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="793e5-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="793e5-222">Compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="793e5-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="793e5-223">Proporciona compatibilidad para la compresión de las respuestas.</span><span class="sxs-lookup"><span data-stu-id="793e5-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="793e5-224">Antes de componentes que requieren la compresión.</span><span class="sxs-lookup"><span data-stu-id="793e5-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="793e5-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="793e5-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="793e5-226">Proporciona compatibilidad de localización.</span><span class="sxs-lookup"><span data-stu-id="793e5-226">Provides localization support.</span></span> | <span data-ttu-id="793e5-227">Antes de los componentes de localización confidenciales.</span><span class="sxs-lookup"><span data-stu-id="793e5-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="793e5-228">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="793e5-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="793e5-229">Define y restringe las rutas de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="793e5-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="793e5-230">Terminal de rutas coincidentes.</span><span class="sxs-lookup"><span data-stu-id="793e5-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="793e5-231">Sesión</span><span class="sxs-lookup"><span data-stu-id="793e5-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="793e5-232">Proporciona compatibilidad para administrar sesiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="793e5-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="793e5-233">Antes de componentes que requieren la sesión.</span><span class="sxs-lookup"><span data-stu-id="793e5-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="793e5-234">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="793e5-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="793e5-235">Proporciona compatibilidad para servir archivos estáticos y examen de directorios.</span><span class="sxs-lookup"><span data-stu-id="793e5-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="793e5-236">Terminal de si una solicitud coincide con los archivos.</span><span class="sxs-lookup"><span data-stu-id="793e5-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="793e5-237">Reescritura de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="793e5-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="793e5-238">Proporciona compatibilidad para volver a escribir las direcciones URL y redirigir las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="793e5-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="793e5-239">Antes de los componentes que utilizan la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="793e5-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="793e5-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="793e5-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="793e5-241">Habilita el protocolo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="793e5-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="793e5-242">Antes de componentes que se necesitan para aceptar las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="793e5-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="793e5-243">Middleware de escritura</span><span class="sxs-lookup"><span data-stu-id="793e5-243">Writing middleware</span></span>

<span data-ttu-id="793e5-244">Middleware en general se encapsulan en una clase y exponen a través de un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="793e5-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="793e5-245">Tenga en cuenta el siguiente middleware, que establece la referencia cultural de la solicitud actual de la cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="793e5-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="793e5-246">Nota: El código de ejemplo anterior se utiliza para mostrar cómo crear un componente de middleware.</span><span class="sxs-lookup"><span data-stu-id="793e5-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="793e5-247">Vea [ globalización y localización](xref:fundamentals/localization) para la compatibilidad de localización integrado del núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="793e5-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="793e5-248">Puede probar el middleware pasando en la referencia cultural, por ejemplo `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="793e5-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="793e5-249">El siguiente código mueve el delegado de middleware a una clase:</span><span class="sxs-lookup"><span data-stu-id="793e5-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="793e5-250">El siguiente método de extensión expone el middleware a través de [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="793e5-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="793e5-251">El código siguiente llama el middleware de `Configure`:</span><span class="sxs-lookup"><span data-stu-id="793e5-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="793e5-252">Middleware debe seguir la [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/) mediante la exposición de sus dependencias en su constructor.</span><span class="sxs-lookup"><span data-stu-id="793e5-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="793e5-253">Middleware se construye una vez por *duración de la aplicación*.</span><span class="sxs-lookup"><span data-stu-id="793e5-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="793e5-254">Vea *dependencias por solicitud* siguiente si se tienen que compartir servicios con middleware dentro de una solicitud.</span><span class="sxs-lookup"><span data-stu-id="793e5-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="793e5-255">Componentes de software intermedio pueden resolver sus dependencias de inyección de dependencia a través de los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="793e5-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="793e5-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)También se puede aceptar parámetros adicionales directamente.</span><span class="sxs-lookup"><span data-stu-id="793e5-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="793e5-257">Dependencias de cada solicitud</span><span class="sxs-lookup"><span data-stu-id="793e5-257">Per-request dependencies</span></span>

<span data-ttu-id="793e5-258">Dado que se construye middleware al iniciar la aplicación, no por solicitud, *ámbito* usados por los constructores de middleware de servicios de duración no se comparten con otros tipos de dependencia inyectado durante cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="793e5-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="793e5-259">Si debe compartir un *ámbito* entre el middleware y otros tipos de servicio, agregue estos servicios para el `Invoke` la firma del método.</span><span class="sxs-lookup"><span data-stu-id="793e5-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="793e5-260">El `Invoke` método puede aceptar parámetros adicionales que se rellenan con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="793e5-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="793e5-261">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="793e5-261">For example:</span></span>

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

## <a name="resources"></a><span data-ttu-id="793e5-262">Recursos</span><span class="sxs-lookup"><span data-stu-id="793e5-262">Resources</span></span>

* [<span data-ttu-id="793e5-263">Código de ejemplo utilizado en este documento</span><span class="sxs-lookup"><span data-stu-id="793e5-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="793e5-264">Migración de módulos HTTP a middleware</span><span class="sxs-lookup"><span data-stu-id="793e5-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="793e5-265">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="793e5-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="793e5-266">Solicitud de características</span><span class="sxs-lookup"><span data-stu-id="793e5-266">Request Features</span></span>](request-features.md)
