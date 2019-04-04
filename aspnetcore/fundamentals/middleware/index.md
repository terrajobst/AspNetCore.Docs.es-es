---
title: Middleware de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre el middleware de ASP.NET Core y la canalización de solicitudes.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665394"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="e7720-103">Middleware de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7720-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="e7720-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e7720-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e7720-105">El software intermedio es un software que se ensambla en una canalización de una aplicación para controlar las solicitudes y las respuestas.</span><span class="sxs-lookup"><span data-stu-id="e7720-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="e7720-106">Cada componente puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7720-106">Each component:</span></span>

* <span data-ttu-id="e7720-107">Elegir si se pasa la solicitud al siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="e7720-108">Realizar trabajos antes y después del siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="e7720-109">Los delegados de solicitudes se usan para crear la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e7720-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="e7720-110">Estos también controlan las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7720-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="e7720-111">Los delegados de solicitudes se configuran con los métodos de extensión <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> y <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="e7720-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="e7720-112">Un delegado de solicitudes se puede especificar en línea como un método anónimo (denominado middleware en línea) o se puede definir en una clase reutilizable.</span><span class="sxs-lookup"><span data-stu-id="e7720-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="e7720-113">Estas clases reutilizables y métodos anónimos en línea se conocen como *software intermedio* o *componentes de software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="e7720-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="e7720-114">Cada componente de software intermedio de la canalización de solicitudes es responsable de invocar el siguiente componente de la canalización o de cortocircuitar la canalización, en caso de ser necesario.</span><span class="sxs-lookup"><span data-stu-id="e7720-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="e7720-115">Cuando un middleware se cortocircuita, se llama *middleware de terminal* porque impide el procesamiento de la solicitud por parte de middleware adicional.</span><span class="sxs-lookup"><span data-stu-id="e7720-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="e7720-116">En <xref:migration/http-modules> se explica la diferencia entre las canalizaciones de solicitudes en ASP.NET Core y ASP.NET 4.x y se proporcionan más ejemplos de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="e7720-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="e7720-117">Creación de una canalización de software intermedio con IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="e7720-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="e7720-118">La canalización de solicitudes de ASP.NET Core consiste en una secuencia de delegados de solicitud a los que se llama de uno en uno.</span><span class="sxs-lookup"><span data-stu-id="e7720-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="e7720-119">En el siguiente diagrama se muestra este concepto.</span><span class="sxs-lookup"><span data-stu-id="e7720-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="e7720-120">El subproceso de ejecución sigue las flechas negras.</span><span class="sxs-lookup"><span data-stu-id="e7720-120">The thread of execution follows the black arrows.</span></span>

![En el patrón de procesamiento de solicitudes se muestra una solicitud entrante, el procesamiento a través de tres softwares intermedios y la respuesta saliente de la aplicación.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="e7720-124">Cada delegado puede realizar operaciones antes y después del siguiente.</span><span class="sxs-lookup"><span data-stu-id="e7720-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="e7720-125">Los delegados que controlan excepciones deben llamarse al principio de la canalización para que puedan capturar las excepciones que se producen en las fases siguientes de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="e7720-126">La aplicación ASP.NET Core más sencilla posible configura un solo delegado de solicitudes que controla todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e7720-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="e7720-127">En este caso no se incluye una canalización de solicitudes real.</span><span class="sxs-lookup"><span data-stu-id="e7720-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="e7720-128">En su lugar, solo se llama a una única función anónima en respuesta a todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7720-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="e7720-129">El primer delegado de <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> finaliza la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="e7720-130">Encadene varios delegados de solicitudes con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="e7720-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="e7720-131">El parámetro `next` representa el siguiente delegado de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="e7720-132">Si *no* llama al *siguiente* parámetro, puede cortocircuitar la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="e7720-133">Normalmente, puede realizar acciones antes y después del siguiente delegado, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7720-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

<span data-ttu-id="e7720-134">Cuando un delegado no pasa una solicitud al siguiente delegado, se denomina *cortocircuitar la canalización de solicitudes*.</span><span class="sxs-lookup"><span data-stu-id="e7720-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="e7720-135">Este proceso es necesario muchas veces, ya que previene la realización de trabajo innecesario.</span><span class="sxs-lookup"><span data-stu-id="e7720-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="e7720-136">Por ejemplo, el [middleware de archivos estáticos](xref:fundamentals/static-files) puede actuar como *middleware de terminal* procesando una solicitud para un archivo estático y cortocircuitando el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="e7720-137">El middleware agregado a la canalización antes del middleware que finaliza el procesamiento sigue procesando código después de sus instrucciones `next.Invoke`.</span><span class="sxs-lookup"><span data-stu-id="e7720-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="e7720-138">Sin embargo, consulte la siguiente advertencia sobre intentar escribir en una respuesta que ya se ha enviado.</span><span class="sxs-lookup"><span data-stu-id="e7720-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="e7720-139">No llame a `next.Invoke` después de haber enviado la respuesta al cliente.</span><span class="sxs-lookup"><span data-stu-id="e7720-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="e7720-140">Si se modifica <xref:Microsoft.AspNetCore.Http.HttpResponse> después de haber iniciado la respuesta, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="e7720-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="e7720-141">Por ejemplo, se producirá una excepción al realizar cambios tales como el establecimiento de encabezados o el código.</span><span class="sxs-lookup"><span data-stu-id="e7720-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="e7720-142">Si escribe en el cuerpo de la respuesta después de llamar a `next`:</span><span class="sxs-lookup"><span data-stu-id="e7720-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="e7720-143">Puede provocar una infracción del protocolo.</span><span class="sxs-lookup"><span data-stu-id="e7720-143">May cause a protocol violation.</span></span> <span data-ttu-id="e7720-144">Por ejemplo, si escribe más de la longitud `Content-Length` establecida.</span><span class="sxs-lookup"><span data-stu-id="e7720-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="e7720-145">Puede dañar el formato del cuerpo.</span><span class="sxs-lookup"><span data-stu-id="e7720-145">May corrupt the body format.</span></span> <span data-ttu-id="e7720-146">Por ejemplo, si escribe un pie de página en HTML en un archivo CSS.</span><span class="sxs-lookup"><span data-stu-id="e7720-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="e7720-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> es una sugerencia útil para indicar si se han enviado los encabezados o se han realizado escrituras en el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="e7720-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="e7720-148">Ordenar</span><span class="sxs-lookup"><span data-stu-id="e7720-148">Order</span></span>

<span data-ttu-id="e7720-149">El orden en el que se agregan los componentes de software intermedio en el método `Startup.Configure` define el orden en el que se invocarán los componentes de software intermedio en las solicitudes y el orden inverso de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e7720-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="e7720-150">Por motivos de seguridad, rendimiento y funcionalidad, el orden es básico.</span><span class="sxs-lookup"><span data-stu-id="e7720-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="e7720-151">El siguiente método `Startup.Configure` agrega los componentes de middleware para escenarios de aplicaciones comunes:</span><span class="sxs-lookup"><span data-stu-id="e7720-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="e7720-152">Control de errores y excepciones</span><span class="sxs-lookup"><span data-stu-id="e7720-152">Exception/error handling</span></span>
1. <span data-ttu-id="e7720-153">Protocolo de seguridad de transporte estricta de HTTP</span><span class="sxs-lookup"><span data-stu-id="e7720-153">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="e7720-154">Redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="e7720-154">HTTPS redirection</span></span>
1. <span data-ttu-id="e7720-155">Servidor de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="e7720-155">Static file server</span></span>
1. <span data-ttu-id="e7720-156">Cumplimiento de directivas de cookies</span><span class="sxs-lookup"><span data-stu-id="e7720-156">Cookie policy enforcement</span></span>
1. <span data-ttu-id="e7720-157">Autenticación</span><span class="sxs-lookup"><span data-stu-id="e7720-157">Authentication</span></span>
1. <span data-ttu-id="e7720-158">Sesión</span><span class="sxs-lookup"><span data-stu-id="e7720-158">Session</span></span>
1. <span data-ttu-id="e7720-159">MVC</span><span class="sxs-lookup"><span data-stu-id="e7720-159">MVC</span></span>

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
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="e7720-160">Control de errores y excepciones</span><span class="sxs-lookup"><span data-stu-id="e7720-160">Exception/error handling</span></span>
1. <span data-ttu-id="e7720-161">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="e7720-161">Static files</span></span>
1. <span data-ttu-id="e7720-162">Autenticación</span><span class="sxs-lookup"><span data-stu-id="e7720-162">Authentication</span></span>
1. <span data-ttu-id="e7720-163">Sesión</span><span class="sxs-lookup"><span data-stu-id="e7720-163">Session</span></span>
1. <span data-ttu-id="e7720-164">MVC</span><span class="sxs-lookup"><span data-stu-id="e7720-164">MVC</span></span>

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

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="e7720-165">En el código de ejemplo anterior, cada método de extensión de software intermedio se expone en <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> a través del espacio de nombres de <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="e7720-165">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="e7720-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> es el primer componente de software intermedio que se agrega a la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="e7720-167">Por lo tanto, el software intermedio del controlador de excepciones detectará las excepciones que se produzcan en las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="e7720-167">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="e7720-168">El software intermedio de archivos estáticos se llama al principio de la canalización para que pueda controlar solicitudes y realizar cortocircuitos sin pasar por los componentes restantes.</span><span class="sxs-lookup"><span data-stu-id="e7720-168">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="e7720-169">Este middleware **no** proporciona comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="e7720-169">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="e7720-170">Los archivos que proporciona, incluidos los de *wwwroot*, están disponibles de forma pública.</span><span class="sxs-lookup"><span data-stu-id="e7720-170">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="e7720-171">Para obtener más información sobre cómo proteger este tipo de archivos, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="e7720-171">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e7720-172">Si el software intermedio de archivos estáticos no controla la solicitud, se pasará al software intermedio de autenticación (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), que realizará la autenticación.</span><span class="sxs-lookup"><span data-stu-id="e7720-172">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="e7720-173">Este software intermedio no cortocircuita las solicitudes sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="e7720-173">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="e7720-174">Aunque autentique solicitudes, la autorización (y también el rechazo) se producirán después de que MVC seleccione una página de Razor o un control y una acción de MVC concretos.</span><span class="sxs-lookup"><span data-stu-id="e7720-174">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e7720-175">Si el software intermedio de archivos estáticos no controla la solicitud, se pasará al software intermedio de identidad (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), que realizará la autenticación.</span><span class="sxs-lookup"><span data-stu-id="e7720-175">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="e7720-176">Este middleware no cortocircuita las solicitudes sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="e7720-176">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="e7720-177">Aunque autentique solicitudes, la autorización (y también el rechazo) se producirán después de que MVC seleccione un control y una acción concretos.</span><span class="sxs-lookup"><span data-stu-id="e7720-177">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="e7720-178">En el ejemplo siguiente se muestra un orden de software intermedio en el que el software intermedio de archivos estáticos controla las solicitudes de archivos estáticos antes del software intermedio de compresión de respuestas.</span><span class="sxs-lookup"><span data-stu-id="e7720-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="e7720-179">Los archivos estáticos no se comprimen en este orden de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="e7720-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="e7720-180">Las respuestas de MVC de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> se pueden comprimir.</span><span class="sxs-lookup"><span data-stu-id="e7720-180">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="e7720-181">Use, Run y Map</span><span class="sxs-lookup"><span data-stu-id="e7720-181">Use, Run, and Map</span></span>

<span data-ttu-id="e7720-182">Puede configurar la canalización HTTP con `Use`, `Run` y `Map`.</span><span class="sxs-lookup"><span data-stu-id="e7720-182">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="e7720-183">El método `Use` puede cortocircuitar la canalización (solo si no llama a un delegado de solicitudes `next`).</span><span class="sxs-lookup"><span data-stu-id="e7720-183">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="e7720-184">`Run` es una convención y es posible que algunos componentes de middleware expongan métodos `Run[Middleware]` que se ejecutan al final de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-184">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="e7720-185">Las extensiones <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> se usan como convenciones para la creación de ramas en la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="e7720-186">`Map*` crea una rama de la canalización de solicitudes según las coincidencias de la ruta de solicitud proporcionada.</span><span class="sxs-lookup"><span data-stu-id="e7720-186">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="e7720-187">Si la ruta de solicitud comienza con la ruta proporcionada, se ejecuta la creación de la rama.</span><span class="sxs-lookup"><span data-stu-id="e7720-187">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="e7720-188">En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior.</span><span class="sxs-lookup"><span data-stu-id="e7720-188">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="e7720-189">Request</span><span class="sxs-lookup"><span data-stu-id="e7720-189">Request</span></span>             | <span data-ttu-id="e7720-190">Respuesta</span><span class="sxs-lookup"><span data-stu-id="e7720-190">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="e7720-191">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="e7720-191">localhost:1234</span></span>      | <span data-ttu-id="e7720-192">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="e7720-192">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="e7720-193">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="e7720-193">localhost:1234/map1</span></span> | <span data-ttu-id="e7720-194">Prueba 1 de Map</span><span class="sxs-lookup"><span data-stu-id="e7720-194">Map Test 1</span></span>                   |
| <span data-ttu-id="e7720-195">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="e7720-195">localhost:1234/map2</span></span> | <span data-ttu-id="e7720-196">Prueba 2 de Map</span><span class="sxs-lookup"><span data-stu-id="e7720-196">Map Test 2</span></span>                   |
| <span data-ttu-id="e7720-197">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="e7720-197">localhost:1234/map3</span></span> | <span data-ttu-id="e7720-198">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="e7720-198">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="e7720-199">Cuando se usa `Map`, los segmentos de ruta que coincidan se eliminan de `HttpRequest.Path` y se anexan a `HttpRequest.PathBase` por cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="e7720-199">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="e7720-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea una rama de la canalización de solicitudes según el resultado del predicado proporcionado.</span><span class="sxs-lookup"><span data-stu-id="e7720-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="e7720-201">Se puede usar cualquier predicado de tipo `Func<HttpContext, bool>` para asignar solicitudes a nuevas ramas de la canalización.</span><span class="sxs-lookup"><span data-stu-id="e7720-201">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="e7720-202">En el ejemplo siguiente se usa un predicado para detectar la presencia de una `branch` variable de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="e7720-202">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="e7720-203">En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior.</span><span class="sxs-lookup"><span data-stu-id="e7720-203">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="e7720-204">Request</span><span class="sxs-lookup"><span data-stu-id="e7720-204">Request</span></span>                       | <span data-ttu-id="e7720-205">Respuesta</span><span class="sxs-lookup"><span data-stu-id="e7720-205">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="e7720-206">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="e7720-206">localhost:1234</span></span>                | <span data-ttu-id="e7720-207">Saludos del delegado sin Map.</span><span class="sxs-lookup"><span data-stu-id="e7720-207">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="e7720-208">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="e7720-208">localhost:1234/?branch=master</span></span> | <span data-ttu-id="e7720-209">Rama usada = master</span><span class="sxs-lookup"><span data-stu-id="e7720-209">Branch used = master</span></span>         |

<span data-ttu-id="e7720-210">`Map` admite la anidación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e7720-210">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="e7720-211">`Map` puede hacer coincidir varios segmentos a la vez:</span><span class="sxs-lookup"><span data-stu-id="e7720-211">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="e7720-212">Middleware integrado</span><span class="sxs-lookup"><span data-stu-id="e7720-212">Built-in middleware</span></span>

<span data-ttu-id="e7720-213">ASP.NET Core incluye los componentes de software intermedio siguientes.</span><span class="sxs-lookup"><span data-stu-id="e7720-213">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="e7720-214">En la columna *Orden* se proporcionan notas sobre la ubicación del middleware en la canalización de procesamiento de solicitudes, así como las condiciones con las que podría finalizar el procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e7720-214">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="e7720-215">Cuando un middleware cortocircuita la canalización de procesamiento de solicitudes e impide el procesamiento de una solicitud por parte de middleware descendente adicional, se llama *middleware de terminal*.</span><span class="sxs-lookup"><span data-stu-id="e7720-215">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="e7720-216">Para más información sobre cómo cortocircuitar, consulte la sección [Creación de una canalización de middleware con IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="e7720-216">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="e7720-217">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="e7720-217">Middleware</span></span> | <span data-ttu-id="e7720-218">Descripción</span><span class="sxs-lookup"><span data-stu-id="e7720-218">Description</span></span> | <span data-ttu-id="e7720-219">Ordenar</span><span class="sxs-lookup"><span data-stu-id="e7720-219">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="e7720-220">Autenticación</span><span class="sxs-lookup"><span data-stu-id="e7720-220">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="e7720-221">Proporciona compatibilidad con autenticación.</span><span class="sxs-lookup"><span data-stu-id="e7720-221">Provides authentication support.</span></span> | <span data-ttu-id="e7720-222">Antes de que se necesite `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="e7720-222">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="e7720-223">Terminal para devoluciones de llamadas OAuth.</span><span class="sxs-lookup"><span data-stu-id="e7720-223">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="e7720-224">Directiva de cookies</span><span class="sxs-lookup"><span data-stu-id="e7720-224">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="e7720-225">Realiza un seguimiento del consentimiento de los usuarios para almacenar información personal y aplica los estándares mínimos para los campos de las cookies, como `secure` y `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="e7720-225">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="e7720-226">Antes del middleware que emite las cookies.</span><span class="sxs-lookup"><span data-stu-id="e7720-226">Before middleware that issues cookies.</span></span> <span data-ttu-id="e7720-227">Ejemplos: autenticación, sesión y MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="e7720-227">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="e7720-228">CORS</span><span class="sxs-lookup"><span data-stu-id="e7720-228">CORS</span></span>](xref:security/cors) | <span data-ttu-id="e7720-229">Configura el uso compartido de recursos entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="e7720-229">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="e7720-230">Antes de los componentes que usan CORS.</span><span class="sxs-lookup"><span data-stu-id="e7720-230">Before components that use CORS.</span></span> |
| [<span data-ttu-id="e7720-231">Control de excepciones</span><span class="sxs-lookup"><span data-stu-id="e7720-231">Exception Handling</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="e7720-232">Controla las excepciones.</span><span class="sxs-lookup"><span data-stu-id="e7720-232">Handles exceptions.</span></span> | <span data-ttu-id="e7720-233">Antes de los componentes que generan errores.</span><span class="sxs-lookup"><span data-stu-id="e7720-233">Before components that generate errors.</span></span> |
| [<span data-ttu-id="e7720-234">Encabezados reenviados</span><span class="sxs-lookup"><span data-stu-id="e7720-234">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="e7720-235">Reenvía encabezados con proxy a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="e7720-235">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="e7720-236">Antes de los componentes que consumen los campos actualizados.</span><span class="sxs-lookup"><span data-stu-id="e7720-236">Before components that consume the updated fields.</span></span> <span data-ttu-id="e7720-237">Ejemplos: esquema, host, IP de cliente y método.</span><span class="sxs-lookup"><span data-stu-id="e7720-237">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="e7720-238">Comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="e7720-238">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="e7720-239">Comprueba el estado de una aplicación ASP.NET Core y sus dependencias, como la comprobación de disponibilidad de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e7720-239">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="e7720-240">Terminal si una solicitud coincide con un punto de conexión de comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="e7720-240">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="e7720-241">Invalidación del método HTTP</span><span class="sxs-lookup"><span data-stu-id="e7720-241">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="e7720-242">Permite que una solicitud POST entrante invalide el método.</span><span class="sxs-lookup"><span data-stu-id="e7720-242">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="e7720-243">Antes de los componentes que consumen el método actualizado.</span><span class="sxs-lookup"><span data-stu-id="e7720-243">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="e7720-244">Redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="e7720-244">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="e7720-245">Redireccione todas las solicitudes HTTP a HTTPS (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="e7720-245">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="e7720-246">Antes de los componentes que consumen la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="e7720-246">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="e7720-247">Seguridad de transporte estricta de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="e7720-247">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="e7720-248">Middleware de mejora de seguridad que agrega un encabezado de respuesta especial (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="e7720-248">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="e7720-249">Antes de que se envíen las respuestas y después de los componentes que modifican las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e7720-249">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="e7720-250">Ejemplos: encabezados reenviados y reescritura de URL.</span><span class="sxs-lookup"><span data-stu-id="e7720-250">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="e7720-251">MVC</span><span class="sxs-lookup"><span data-stu-id="e7720-251">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="e7720-252">Procesa la solicitudes con MVC o Razor Pages (ASP.NET Core 2.0 o versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="e7720-252">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="e7720-253">Si hay una solicitud que coincida con una ruta, será final.</span><span class="sxs-lookup"><span data-stu-id="e7720-253">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="e7720-254">OWIN</span><span class="sxs-lookup"><span data-stu-id="e7720-254">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="e7720-255">Puede interoperar con aplicaciones, servidores y software intermedio basados en OWIN.</span><span class="sxs-lookup"><span data-stu-id="e7720-255">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="e7720-256">Si el software intermedio de OWIN procesa completamente la solicitud, será final.</span><span class="sxs-lookup"><span data-stu-id="e7720-256">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="e7720-257">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="e7720-257">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="e7720-258">Proporciona compatibilidad con la captura de respuestas.</span><span class="sxs-lookup"><span data-stu-id="e7720-258">Provides support for caching responses.</span></span> | <span data-ttu-id="e7720-259">Antes de los componentes que requieren el almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="e7720-259">Before components that require caching.</span></span> |
| [<span data-ttu-id="e7720-260">Compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="e7720-260">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="e7720-261">Proporciona compatibilidad con la compresión de respuestas.</span><span class="sxs-lookup"><span data-stu-id="e7720-261">Provides support for compressing responses.</span></span> | <span data-ttu-id="e7720-262">Antes de los componentes que requieren compresión.</span><span class="sxs-lookup"><span data-stu-id="e7720-262">Before components that require compression.</span></span> |
| [<span data-ttu-id="e7720-263">Localización de solicitudes</span><span class="sxs-lookup"><span data-stu-id="e7720-263">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="e7720-264">Proporciona compatibilidad con ubicación.</span><span class="sxs-lookup"><span data-stu-id="e7720-264">Provides localization support.</span></span> | <span data-ttu-id="e7720-265">Antes de los componentes que dependen de la ubicación.</span><span class="sxs-lookup"><span data-stu-id="e7720-265">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="e7720-266">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="e7720-266">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="e7720-267">Define y restringe las rutas de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e7720-267">Defines and constrains request routes.</span></span> | <span data-ttu-id="e7720-268">Terminal para rutas que coincidan.</span><span class="sxs-lookup"><span data-stu-id="e7720-268">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="e7720-269">Sesión</span><span class="sxs-lookup"><span data-stu-id="e7720-269">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="e7720-270">Proporciona compatibilidad con la administración de sesiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="e7720-270">Provides support for managing user sessions.</span></span> | <span data-ttu-id="e7720-271">Antes de los componentes que requieren Session.</span><span class="sxs-lookup"><span data-stu-id="e7720-271">Before components that require Session.</span></span> |
| [<span data-ttu-id="e7720-272">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="e7720-272">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="e7720-273">Proporciona compatibilidad con la proporción de archivos estáticos y la exploración de directorios.</span><span class="sxs-lookup"><span data-stu-id="e7720-273">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="e7720-274">Si hay una solicitud que coincida con un archivo, será final.</span><span class="sxs-lookup"><span data-stu-id="e7720-274">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="e7720-275">Reescritura de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="e7720-275">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="e7720-276">Proporciona compatibilidad con la reescritura de direcciones URL y la redirección de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e7720-276">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="e7720-277">Antes de los componentes que consumen la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="e7720-277">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="e7720-278">WebSockets</span><span class="sxs-lookup"><span data-stu-id="e7720-278">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="e7720-279">Habilita el protocolo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="e7720-279">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="e7720-280">Antes de los componentes necesarios para aceptar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e7720-280">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="e7720-281">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e7720-281">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
