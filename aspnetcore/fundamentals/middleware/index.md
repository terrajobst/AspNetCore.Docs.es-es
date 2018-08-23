---
title: Middleware de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre el middleware de ASP.NET Core y la canalización de solicitudes.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870952"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="0c507-103">Middleware de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c507-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="0c507-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0c507-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0c507-105">El software intermedio es un software que se ensambla en una canalización de una aplicación para controlar las solicitudes y las respuestas.</span><span class="sxs-lookup"><span data-stu-id="0c507-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="0c507-106">Cada componente puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0c507-106">Each component:</span></span>

* <span data-ttu-id="0c507-107">Elegir si se pasa la solicitud al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="0c507-108">Realizar trabajos antes y después de invocar al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="0c507-109">Los delegados de solicitudes se usan para crear la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="0c507-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="0c507-110">Estos también controlan las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c507-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="0c507-111">Los delegados de solicitudes se configuran con los métodos de extensión <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> y <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="0c507-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="0c507-112">Un delegado de solicitudes se puede especificar en línea como un método anónimo (denominado middleware en línea) o se puede definir en una clase reutilizable.</span><span class="sxs-lookup"><span data-stu-id="0c507-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="0c507-113">Estas clases reutilizables y métodos anónimos en línea se conocen como *software intermedio* o *componentes de software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="0c507-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="0c507-114">Cada componente de software intermedio de la canalización de solicitudes es responsable de invocar el siguiente componente de la canalización o de cortocircuitar la canalización, en caso de ser necesario.</span><span class="sxs-lookup"><span data-stu-id="0c507-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="0c507-115">En <xref:migration/http-modules> se explica la diferencia entre las canalizaciones de solicitudes en ASP.NET Core y ASP.NET 4.x y se proporcionan más ejemplos de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="0c507-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="0c507-116">Creación de una canalización de software intermedio con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="0c507-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="0c507-117">La canalización de solicitudes de ASP.NET Core consiste en una secuencia de delegados de solicitud a los que se llama de uno en uno.</span><span class="sxs-lookup"><span data-stu-id="0c507-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="0c507-118">En el siguiente diagrama se muestra este concepto.</span><span class="sxs-lookup"><span data-stu-id="0c507-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="0c507-119">El subproceso de ejecución sigue las flechas negras.</span><span class="sxs-lookup"><span data-stu-id="0c507-119">The thread of execution follows the black arrows.</span></span>

![En el patrón de procesamiento de solicitudes se muestra una solicitud entrante, el procesamiento a través de tres softwares intermedios y la respuesta saliente de la aplicación.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="0c507-123">Cada delegado puede realizar operaciones antes y después del siguiente.</span><span class="sxs-lookup"><span data-stu-id="0c507-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="0c507-124">También puede decidir no pasar una solicitud al siguiente delegado, lo que se denomina *cortocircuitar la canalización de solicitudes*.</span><span class="sxs-lookup"><span data-stu-id="0c507-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="0c507-125">Este proceso es necesario muchas veces, ya que previene la realización de trabajo innecesario.</span><span class="sxs-lookup"><span data-stu-id="0c507-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="0c507-126">Por ejemplo, el software intermedio de archivos estáticos puede devolver una solicitud para un archivo estático y cortocircuitar el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="0c507-127">Los delegados que controlan excepciones se llaman al principio de la canalización para que puedan capturar las excepciones que se produzcan en las fases siguientes de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="0c507-128">La aplicación ASP.NET Core más sencilla posible configura un solo delegado de solicitudes que controla todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="0c507-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="0c507-129">En este caso no se incluye una canalización de solicitudes real.</span><span class="sxs-lookup"><span data-stu-id="0c507-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="0c507-130">En su lugar, solo se llama a una única función anónima en respuesta a todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c507-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="0c507-131">El primer delegado de <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> finaliza la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="0c507-132">Encadene varios delegados de solicitudes con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="0c507-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="0c507-133">El parámetro `next` representa el siguiente delegado de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="0c507-134">Si *no* llama al *siguiente* parámetro, puede cortocircuitar la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="0c507-135">Normalmente, puede realizar acciones antes y después del siguiente delegado, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0c507-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="0c507-136">No llame a `next.Invoke` después de haber enviado la respuesta al cliente.</span><span class="sxs-lookup"><span data-stu-id="0c507-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="0c507-137">Si se modifica <xref:Microsoft.AspNetCore.Http.HttpResponse> después de haber iniciado la respuesta, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="0c507-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="0c507-138">Por ejemplo, se producirá una excepción al realizar cambios tales como el establecimiento de encabezados o el código.</span><span class="sxs-lookup"><span data-stu-id="0c507-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="0c507-139">Si escribe en el cuerpo de la respuesta después de llamar a `next`:</span><span class="sxs-lookup"><span data-stu-id="0c507-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="0c507-140">Puede provocar una infracción del protocolo.</span><span class="sxs-lookup"><span data-stu-id="0c507-140">May cause a protocol violation.</span></span> <span data-ttu-id="0c507-141">Por ejemplo, si escribe más de la longitud `Content-Length` establecida.</span><span class="sxs-lookup"><span data-stu-id="0c507-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="0c507-142">Puede dañar el formato del cuerpo.</span><span class="sxs-lookup"><span data-stu-id="0c507-142">May corrupt the body format.</span></span> <span data-ttu-id="0c507-143">Por ejemplo, si escribe un pie de página en HTML en un archivo CSS.</span><span class="sxs-lookup"><span data-stu-id="0c507-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="0c507-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> es una sugerencia útil para indicar si se han enviado los encabezados o se han realizado escrituras en el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="0c507-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="0c507-145">Orden</span><span class="sxs-lookup"><span data-stu-id="0c507-145">Order</span></span>

<span data-ttu-id="0c507-146">El orden en el que se agregan los componentes de software intermedio en el método `Startup.Configure` define el orden en el que se invocarán los componentes de software intermedio en las solicitudes y el orden inverso de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0c507-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="0c507-147">Por motivos de seguridad, rendimiento y funcionalidad, el orden es básico.</span><span class="sxs-lookup"><span data-stu-id="0c507-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="0c507-148">El método `Configure` siguiente agrega los siguientes componentes de software intermedio:</span><span class="sxs-lookup"><span data-stu-id="0c507-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="0c507-149">Control de errores y excepciones</span><span class="sxs-lookup"><span data-stu-id="0c507-149">Exception/error handling</span></span>
2. <span data-ttu-id="0c507-150">Servidor de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="0c507-150">Static file server</span></span>
3. <span data-ttu-id="0c507-151">Autenticación</span><span class="sxs-lookup"><span data-stu-id="0c507-151">Authentication</span></span>
4. <span data-ttu-id="0c507-152">MVC</span><span class="sxs-lookup"><span data-stu-id="0c507-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="0c507-153">En el código anterior, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> es el primer componente de software intermedio que se agrega a la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-153">In the code above, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="0c507-154">Por lo tanto, el software intermedio del controlador de excepciones detectará las excepciones que se produzcan en las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="0c507-154">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="0c507-155">El software intermedio de archivos estáticos se llama al principio de la canalización para que pueda controlar solicitudes y realizar cortocircuitos sin pasar por los componentes restantes.</span><span class="sxs-lookup"><span data-stu-id="0c507-155">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="0c507-156">Este software intermedio **no** proporciona comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="0c507-156">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="0c507-157">Los archivos que proporciona, incluidos los de *wwwroot*, están disponibles de forma pública.</span><span class="sxs-lookup"><span data-stu-id="0c507-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="0c507-158">Para obtener más información sobre cómo proteger este tipo de archivos, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="0c507-158">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0c507-159">Si el software intermedio de archivos estáticos no controla la solicitud, se pasará al software intermedio de autenticación (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), que realizará la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c507-159">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="0c507-160">Este software intermedio no cortocircuita las solicitudes sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c507-160">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="0c507-161">Aunque autentique solicitudes, la autorización (y también el rechazo) se producirán después de que MVC seleccione una página de Razor o un control y una acción de MVC concretos.</span><span class="sxs-lookup"><span data-stu-id="0c507-161">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0c507-162">Si el software intermedio de archivos estáticos no controla la solicitud, se pasará al software intermedio de identidad (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), que realizará la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c507-162">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="0c507-163">Este middleware no cortocircuita las solicitudes sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c507-163">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="0c507-164">Aunque autentique solicitudes, la autorización (y también el rechazo) se producirán después de que MVC seleccione un control y una acción concretos.</span><span class="sxs-lookup"><span data-stu-id="0c507-164">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="0c507-165">En el ejemplo siguiente se muestra un orden de software intermedio en el que el software intermedio de archivos estáticos controla las solicitudes de archivos estáticos antes del software intermedio de compresión de respuestas.</span><span class="sxs-lookup"><span data-stu-id="0c507-165">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="0c507-166">Los archivos estáticos no se comprimen en este orden de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="0c507-166">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="0c507-167">Las respuestas de MVC de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> se pueden comprimir.</span><span class="sxs-lookup"><span data-stu-id="0c507-167">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="0c507-168">Use, Run y Map</span><span class="sxs-lookup"><span data-stu-id="0c507-168">Use, Run, and Map</span></span>

<span data-ttu-id="0c507-169">Puede configurar la canalización HTTP con `Use`, `Run` y `Map`.</span><span class="sxs-lookup"><span data-stu-id="0c507-169">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="0c507-170">El método `Use` puede cortocircuitar la canalización (solo si no llama a un delegado de solicitudes `next`).</span><span class="sxs-lookup"><span data-stu-id="0c507-170">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="0c507-171">`Run` es una convención y es posible que algunos componentes de middleware expongan métodos `Run[Middleware]` que se ejecutan al final de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-171">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="0c507-172">Las extensiones <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> se usan como convenciones para la creación de ramas en la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-172"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="0c507-173">`Map*` crea una rama de la canalización de solicitudes según las coincidencias de la ruta de solicitud proporcionada.</span><span class="sxs-lookup"><span data-stu-id="0c507-173">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="0c507-174">Si la ruta de solicitud comienza con la ruta proporcionada, se ejecuta la creación de la rama.</span><span class="sxs-lookup"><span data-stu-id="0c507-174">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="0c507-175">En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior.</span><span class="sxs-lookup"><span data-stu-id="0c507-175">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="0c507-176">Solicitud</span><span class="sxs-lookup"><span data-stu-id="0c507-176">Request</span></span>             | <span data-ttu-id="0c507-177">Respuesta</span><span class="sxs-lookup"><span data-stu-id="0c507-177">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="0c507-178">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="0c507-178">localhost:1234</span></span>      | <span data-ttu-id="0c507-179">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="0c507-179">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="0c507-180">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="0c507-180">localhost:1234/map1</span></span> | <span data-ttu-id="0c507-181">Prueba 1 de Map</span><span class="sxs-lookup"><span data-stu-id="0c507-181">Map Test 1</span></span>                   |
| <span data-ttu-id="0c507-182">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="0c507-182">localhost:1234/map2</span></span> | <span data-ttu-id="0c507-183">Prueba 2 de Map</span><span class="sxs-lookup"><span data-stu-id="0c507-183">Map Test 2</span></span>                   |
| <span data-ttu-id="0c507-184">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="0c507-184">localhost:1234/map3</span></span> | <span data-ttu-id="0c507-185">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="0c507-185">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="0c507-186">Cuando se usa `Map`, los segmentos de ruta que coincidan se eliminan de `HttpRequest.Path` y se anexan a `HttpRequest.PathBase` por cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="0c507-186">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="0c507-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea una rama de la canalización de solicitudes según el resultado del predicado proporcionado.</span><span class="sxs-lookup"><span data-stu-id="0c507-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="0c507-188">Se puede usar cualquier predicado de tipo `Func<HttpContext, bool>` para asignar solicitudes a nuevas ramas de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0c507-188">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="0c507-189">En el ejemplo siguiente se usa un predicado para detectar la presencia de una `branch` variable de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="0c507-189">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="0c507-190">En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior.</span><span class="sxs-lookup"><span data-stu-id="0c507-190">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="0c507-191">Solicitud</span><span class="sxs-lookup"><span data-stu-id="0c507-191">Request</span></span>                       | <span data-ttu-id="0c507-192">Respuesta</span><span class="sxs-lookup"><span data-stu-id="0c507-192">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="0c507-193">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="0c507-193">localhost:1234</span></span>                | <span data-ttu-id="0c507-194">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="0c507-194">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="0c507-195">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="0c507-195">localhost:1234/?branch=master</span></span> | <span data-ttu-id="0c507-196">Rama usada = master</span><span class="sxs-lookup"><span data-stu-id="0c507-196">Branch used = master</span></span>         |

<span data-ttu-id="0c507-197">`Map` admite la anidación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c507-197">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="0c507-198">`Map` puede hacer coincidir varios segmentos a la vez:</span><span class="sxs-lookup"><span data-stu-id="0c507-198">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="0c507-199">Middleware integrado</span><span class="sxs-lookup"><span data-stu-id="0c507-199">Built-in middleware</span></span>

<span data-ttu-id="0c507-200">ASP.NET Core incluye los componentes de software intermedio siguientes.</span><span class="sxs-lookup"><span data-stu-id="0c507-200">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="0c507-201">En la columna *Orden* se proporcionan notas sobre la ubicación del software intermedio en la solicitud de canalización, así como las condiciones con las que podría finalizar la solicitud y evitar que otro software intermedio procese una solicitud.</span><span class="sxs-lookup"><span data-stu-id="0c507-201">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="0c507-202">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="0c507-202">Middleware</span></span> | <span data-ttu-id="0c507-203">Descripción</span><span class="sxs-lookup"><span data-stu-id="0c507-203">Description</span></span> | <span data-ttu-id="0c507-204">Orden</span><span class="sxs-lookup"><span data-stu-id="0c507-204">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="0c507-205">Autenticación</span><span class="sxs-lookup"><span data-stu-id="0c507-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="0c507-206">Proporciona compatibilidad con autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c507-206">Provides authentication support.</span></span> | <span data-ttu-id="0c507-207">Antes de que se necesite `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="0c507-207">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="0c507-208">Terminal para devoluciones de llamadas OAuth.</span><span class="sxs-lookup"><span data-stu-id="0c507-208">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="0c507-209">CORS</span><span class="sxs-lookup"><span data-stu-id="0c507-209">CORS</span></span>](xref:security/cors) | <span data-ttu-id="0c507-210">Configura el uso compartido de recursos entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="0c507-210">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="0c507-211">Antes de los componentes que usan CORS.</span><span class="sxs-lookup"><span data-stu-id="0c507-211">Before components that use CORS.</span></span> |
| [<span data-ttu-id="0c507-212">Diagnóstico</span><span class="sxs-lookup"><span data-stu-id="0c507-212">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="0c507-213">Configura el diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="0c507-213">Configures diagnostics.</span></span> | <span data-ttu-id="0c507-214">Antes de los componentes que generan errores.</span><span class="sxs-lookup"><span data-stu-id="0c507-214">Before components that generate errors.</span></span> |
| [<span data-ttu-id="0c507-215">Encabezados reenviados</span><span class="sxs-lookup"><span data-stu-id="0c507-215">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="0c507-216">Reenvía encabezados con proxy a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="0c507-216">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="0c507-217">Antes de los componentes que consumen los campos actualizados (ejemplos: esquema, host, cliente, IP y método).</span><span class="sxs-lookup"><span data-stu-id="0c507-217">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="0c507-218">Invalidación del método HTTP</span><span class="sxs-lookup"><span data-stu-id="0c507-218">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="0c507-219">Permite que una solicitud POST entrante invalide el método.</span><span class="sxs-lookup"><span data-stu-id="0c507-219">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="0c507-220">Antes de los componentes que consumen el método actualizado.</span><span class="sxs-lookup"><span data-stu-id="0c507-220">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="0c507-221">Redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="0c507-221">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="0c507-222">Redireccione todas las solicitudes HTTP a HTTPS (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="0c507-222">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="0c507-223">Antes de los componentes que consumen la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0c507-223">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="0c507-224">Seguridad de transporte estricta de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0c507-224">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="0c507-225">Middleware de mejora de seguridad que agrega un encabezado de respuesta especial (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="0c507-225">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="0c507-226">Antes de que se envíen las respuestas y después de los componentes que modifican las solicitudes (por ejemplo, encabezados reenviados, reescritura de URL).</span><span class="sxs-lookup"><span data-stu-id="0c507-226">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="0c507-227">MVC</span><span class="sxs-lookup"><span data-stu-id="0c507-227">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="0c507-228">Procesa la solicitudes con MVC o Razor Pages (ASP.NET Core 2.0 o versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="0c507-228">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="0c507-229">Si hay una solicitud que coincida con una ruta, será final.</span><span class="sxs-lookup"><span data-stu-id="0c507-229">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="0c507-230">OWIN</span><span class="sxs-lookup"><span data-stu-id="0c507-230">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="0c507-231">Puede interoperar con aplicaciones, servidores y software intermedio basados en OWIN.</span><span class="sxs-lookup"><span data-stu-id="0c507-231">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="0c507-232">Si el software intermedio de OWIN procesa completamente la solicitud, será final.</span><span class="sxs-lookup"><span data-stu-id="0c507-232">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="0c507-233">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="0c507-233">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="0c507-234">Proporciona compatibilidad con la captura de respuestas.</span><span class="sxs-lookup"><span data-stu-id="0c507-234">Provides support for caching responses.</span></span> | <span data-ttu-id="0c507-235">Antes de los componentes que requieren el almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="0c507-235">Before components that require caching.</span></span> |
| [<span data-ttu-id="0c507-236">Compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="0c507-236">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="0c507-237">Proporciona compatibilidad con la compresión de respuestas.</span><span class="sxs-lookup"><span data-stu-id="0c507-237">Provides support for compressing responses.</span></span> | <span data-ttu-id="0c507-238">Antes de los componentes que requieren compresión.</span><span class="sxs-lookup"><span data-stu-id="0c507-238">Before components that require compression.</span></span> |
| [<span data-ttu-id="0c507-239">Localización de solicitudes</span><span class="sxs-lookup"><span data-stu-id="0c507-239">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="0c507-240">Proporciona compatibilidad con ubicación.</span><span class="sxs-lookup"><span data-stu-id="0c507-240">Provides localization support.</span></span> | <span data-ttu-id="0c507-241">Antes de los componentes que dependen de la ubicación.</span><span class="sxs-lookup"><span data-stu-id="0c507-241">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="0c507-242">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="0c507-242">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="0c507-243">Define y restringe las rutas de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0c507-243">Defines and constrains request routes.</span></span> | <span data-ttu-id="0c507-244">Terminal para rutas que coincidan.</span><span class="sxs-lookup"><span data-stu-id="0c507-244">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="0c507-245">Sesión</span><span class="sxs-lookup"><span data-stu-id="0c507-245">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="0c507-246">Proporciona compatibilidad con la administración de sesiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="0c507-246">Provides support for managing user sessions.</span></span> | <span data-ttu-id="0c507-247">Antes de los componentes que requieren Session.</span><span class="sxs-lookup"><span data-stu-id="0c507-247">Before components that require Session.</span></span> |
| [<span data-ttu-id="0c507-248">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="0c507-248">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="0c507-249">Proporciona compatibilidad con la proporción de archivos estáticos y la exploración de directorios.</span><span class="sxs-lookup"><span data-stu-id="0c507-249">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="0c507-250">Si hay una solicitud que coincida con un archivo, será final.</span><span class="sxs-lookup"><span data-stu-id="0c507-250">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="0c507-251">Reescritura de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="0c507-251">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="0c507-252">Proporciona compatibilidad con la reescritura de direcciones URL y la redirección de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="0c507-252">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="0c507-253">Antes de los componentes que consumen la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0c507-253">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="0c507-254">WebSockets</span><span class="sxs-lookup"><span data-stu-id="0c507-254">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="0c507-255">Habilita el protocolo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="0c507-255">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="0c507-256">Antes de los componentes necesarios para aceptar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0c507-256">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="0c507-257">Escritura de software intermedio</span><span class="sxs-lookup"><span data-stu-id="0c507-257">Write middleware</span></span>

<span data-ttu-id="0c507-258">El middleware normalmente está encapsulado en una clase y se expone con un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="0c507-258">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="0c507-259">Use el siguiente software intermedio a modo de ejemplo. En este se establece la referencia cultural de la solicitud actual a partir de la cadena de solicitud:</span><span class="sxs-lookup"><span data-stu-id="0c507-259">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="0c507-260">El código de ejemplo anterior se usa para mostrar la creación de un componente de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="0c507-260">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="0c507-261">Para obtener más información sobre la compatibilidad con la localización integrada de ASP.NET Core, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="0c507-261">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="0c507-262">Puede probar el middleware pasando la referencia cultural, por ejemplo, `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="0c507-262">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="0c507-263">El código siguiente mueve el delegado de middleware a una clase:</span><span class="sxs-lookup"><span data-stu-id="0c507-263">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0c507-264">El nombre del software intermedio del método `Task` debe ser `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="0c507-264">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="0c507-265">En ASP.NET Core 2.0 o posterior, el nombre puede ser `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c507-265">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="0c507-266">El método de extensión siguiente expone el software intermedio mediante <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="0c507-266">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="0c507-267">El código siguiente llama al middleware desde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0c507-267">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="0c507-268">El middleware debería seguir el [principio de dependencias explicitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) mediante la exposición de sus dependencias en el constructor.</span><span class="sxs-lookup"><span data-stu-id="0c507-268">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="0c507-269">El middleware se construye una vez por *duración de la aplicación*.</span><span class="sxs-lookup"><span data-stu-id="0c507-269">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="0c507-270">Si necesita compartir servicios con software intermedio en una solicitud, vea la sección [Dependencias bajo solicitud](#per-request-dependencies).</span><span class="sxs-lookup"><span data-stu-id="0c507-270">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="0c507-271">Los componentes de software intermedio pueden resolver sus dependencias de una [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) mediante parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="0c507-271">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="0c507-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) también puede aceptar parámetros adicionales directamente.</span><span class="sxs-lookup"><span data-stu-id="0c507-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="0c507-273">Dependencias bajo solicitud</span><span class="sxs-lookup"><span data-stu-id="0c507-273">Per-request dependencies</span></span>

<span data-ttu-id="0c507-274">Dado que el software intermedio se construye al inicio de la aplicación y no bajo solicitud, los servicios de duración *con ámbito* que usan los constructores de software intermedio no se comparten con otros tipos insertados mediante dependencias durante cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="0c507-274">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="0c507-275">Si debe compartir un servicio *con ámbito* entre su middleware y otros tipos, agregue esos servicios a la signatura del método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="0c507-275">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="0c507-276">El método `Invoke` puede aceptar parámetros adicionales que la inserción de dependencias propaga:</span><span class="sxs-lookup"><span data-stu-id="0c507-276">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="0c507-277">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0c507-277">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
