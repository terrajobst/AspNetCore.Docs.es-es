---
title: "Habilitación de solicitudes entre orígenes (CORS)"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 0d1c34f5c82fe1a28a405617ea77084db899139b
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="1f07a-103">Habilitación de solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="1f07a-103">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="1f07a-104">Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1f07a-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="1f07a-105">Seguridad del explorador impide que una página web que realiza las solicitudes AJAX a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="1f07a-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="1f07a-106">Esta restricción se denomina la *directiva de mismo origen*y se impide que un sitio malintencionado pueda leer los datos confidenciales desde otro sitio.</span><span class="sxs-lookup"><span data-stu-id="1f07a-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="1f07a-107">Sin embargo, en ocasiones, puede permitir que otros sitios realizar solicitudes entre orígenes en la API web.</span><span class="sxs-lookup"><span data-stu-id="1f07a-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="1f07a-108">[Entre el uso compartido de recursos de origen](http://www.w3.org/TR/cors/) (CORS) es un estándar de W3C que permite que un servidor relajar la directiva de mismo origen.</span><span class="sxs-lookup"><span data-stu-id="1f07a-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="1f07a-109">Con CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar los otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="1f07a-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="1f07a-110">CORS es más seguro y más flexible que técnicas anteriores como [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="1f07a-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="1f07a-111">En este tema se muestra cómo habilitar CORS en una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f07a-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="1f07a-112">¿Qué es el "mismo origen"?</span><span class="sxs-lookup"><span data-stu-id="1f07a-112">What is "same origin"?</span></span>

<span data-ttu-id="1f07a-113">Dos direcciones URL tienen el mismo origen si tienen puertos, los hosts y los esquemas idénticos.</span><span class="sxs-lookup"><span data-stu-id="1f07a-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="1f07a-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="1f07a-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="1f07a-115">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="1f07a-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="1f07a-116">Estas direcciones URL tengan diferentes orígenes que el anterior dos:</span><span class="sxs-lookup"><span data-stu-id="1f07a-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="1f07a-117">`http://example.net`-Dominio diferente</span><span class="sxs-lookup"><span data-stu-id="1f07a-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="1f07a-118">`http://www.example.com/foo.html`-Otro subdominio</span><span class="sxs-lookup"><span data-stu-id="1f07a-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="1f07a-119">`https://example.com/foo.html`-Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="1f07a-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="1f07a-120">`http://example.com:9000/foo.html`-Otro puerto</span><span class="sxs-lookup"><span data-stu-id="1f07a-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="1f07a-121">Internet Explorer no tiene en cuenta el puerto al comparar elementos Origin.</span><span class="sxs-lookup"><span data-stu-id="1f07a-121">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="1f07a-122">Configuración de CORS</span><span class="sxs-lookup"><span data-stu-id="1f07a-122">Setting up CORS</span></span>

<span data-ttu-id="1f07a-123">Para configurar agregar CORS para su aplicación el `Microsoft.AspNetCore.Cors` paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="1f07a-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="1f07a-124">Agregue los servicios CORS de Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="1f07a-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="1f07a-125">Habilitación de CORS con middleware</span><span class="sxs-lookup"><span data-stu-id="1f07a-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="1f07a-126">Para habilitar CORS para toda la aplicación agregar el middleware CORS a la canalización de solicitud usando el `UseCors` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="1f07a-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="1f07a-127">Tenga en cuenta que el middleware CORS debe preceder a cualquier extremo definido en la aplicación que desea admitir las solicitudes entre orígenes (p. ej.</span><span class="sxs-lookup"><span data-stu-id="1f07a-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="1f07a-128">antes de cualquier llamada a `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="1f07a-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="1f07a-129">Puede especificar una directiva de entre orígenes cuando se agrega el middleware CORS mediante la `CorsPolicyBuilder` clase.</span><span class="sxs-lookup"><span data-stu-id="1f07a-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="1f07a-130">Existen dos formas de lograr esto.</span><span class="sxs-lookup"><span data-stu-id="1f07a-130">There are two ways to do this.</span></span> <span data-ttu-id="1f07a-131">La primera consiste en llamar a UseCors con una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="1f07a-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="1f07a-132">**Nota:** debe especificarse la dirección URL sin una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="1f07a-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="1f07a-133">Si la dirección URL finaliza con `/`, devolverá la comparación `false` y no se devolverá ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="1f07a-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="1f07a-134">La expresión lambda toma un `CorsPolicyBuilder` objeto.</span><span class="sxs-lookup"><span data-stu-id="1f07a-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="1f07a-135">Encontrará una lista de los [opciones de configuración](#cors-policy-options) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="1f07a-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="1f07a-136">En este ejemplo, la directiva permite que las solicitudes entre orígenes de `http://example.com` y no hay otros orígenes.</span><span class="sxs-lookup"><span data-stu-id="1f07a-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="1f07a-137">Tenga en cuenta que CorsPolicyBuilder tiene una API fluida, por lo que se pueden encadenar llamadas de método:</span><span class="sxs-lookup"><span data-stu-id="1f07a-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="1f07a-138">El segundo enfoque es definir una o varias directivas CORS con nombre y, a continuación, seleccione la directiva por su nombre en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="1f07a-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="1f07a-139">Este ejemplo agrega una directiva CORS denominada "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="1f07a-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="1f07a-140">Para seleccionar la directiva, pase el nombre a `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="1f07a-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="1f07a-141">Habilitación de CORS en MVC</span><span class="sxs-lookup"><span data-stu-id="1f07a-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="1f07a-142">Como alternativa, puede usar MVC para aplicar CORS específico por cada acción, por cada controlador o globalmente para todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="1f07a-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="1f07a-143">Al utilizar MVC para habilitar CORS se utilizan los mismos servicios CORS, pero el middleware CORS no lo es.</span><span class="sxs-lookup"><span data-stu-id="1f07a-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="1f07a-144">Por cada acción</span><span class="sxs-lookup"><span data-stu-id="1f07a-144">Per action</span></span>

<span data-ttu-id="1f07a-145">Para especificar una directiva CORS para una acción específica agregar el `[EnableCors]` atribuir a la acción.</span><span class="sxs-lookup"><span data-stu-id="1f07a-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="1f07a-146">Especifique el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="1f07a-146">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="1f07a-147">Por cada controlador</span><span class="sxs-lookup"><span data-stu-id="1f07a-147">Per controller</span></span>

<span data-ttu-id="1f07a-148">Para especificar la directiva CORS de un controlador específico agregar el `[EnableCors]` atributo a la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="1f07a-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="1f07a-149">Especifique el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="1f07a-149">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="1f07a-150">Global</span><span class="sxs-lookup"><span data-stu-id="1f07a-150">Globally</span></span>

<span data-ttu-id="1f07a-151">Puede habilitar CORS globalmente para todos los controladores mediante la adición de la `CorsAuthorizationFilterFactory` filtro a la colección de filtros globales:</span><span class="sxs-lookup"><span data-stu-id="1f07a-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="1f07a-152">Es el orden de prioridad: acción, controlador, de forma global.</span><span class="sxs-lookup"><span data-stu-id="1f07a-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="1f07a-153">Las directivas de nivel de acción tienen prioridad sobre las directivas de nivel de controlador y las directivas de nivel de controlador tienen prioridad sobre las directivas globales.</span><span class="sxs-lookup"><span data-stu-id="1f07a-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="1f07a-154">Deshabilitar CORS</span><span class="sxs-lookup"><span data-stu-id="1f07a-154">Disable CORS</span></span>

<span data-ttu-id="1f07a-155">Para deshabilitar CORS para una acción o controlador, utilice la `[DisableCors]` atributo.</span><span class="sxs-lookup"><span data-stu-id="1f07a-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="1f07a-156">Opciones de directiva CORS</span><span class="sxs-lookup"><span data-stu-id="1f07a-156">CORS policy options</span></span>

<span data-ttu-id="1f07a-157">Esta sección describe las distintas opciones que se pueden establecer en una directiva CORS.</span><span class="sxs-lookup"><span data-stu-id="1f07a-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="1f07a-158">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="1f07a-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="1f07a-159">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="1f07a-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="1f07a-160">Establecer los encabezados de solicitudes permitido</span><span class="sxs-lookup"><span data-stu-id="1f07a-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="1f07a-161">Establecer los encabezados de respuesta expuesto</span><span class="sxs-lookup"><span data-stu-id="1f07a-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="1f07a-162">Credenciales en solicitudes cross-origin</span><span class="sxs-lookup"><span data-stu-id="1f07a-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="1f07a-163">Establecer el tiempo de expiración preparatoria</span><span class="sxs-lookup"><span data-stu-id="1f07a-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="1f07a-164">Para algunas opciones pueden serle de ayuda leer [CORS cómo funciona](#how-cors-works) primero.</span><span class="sxs-lookup"><span data-stu-id="1f07a-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="1f07a-165">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="1f07a-165">Set the allowed origins</span></span>

<span data-ttu-id="1f07a-166">Para permitir que uno o más orígenes específicos:</span><span class="sxs-lookup"><span data-stu-id="1f07a-166">To allow one or more specific origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="1f07a-167">Para permitir que todos los orígenes:</span><span class="sxs-lookup"><span data-stu-id="1f07a-167">To allow all origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="1f07a-168">Debe considerar detenidamente antes de permitir que las solicitudes de cualquier origen.</span><span class="sxs-lookup"><span data-stu-id="1f07a-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="1f07a-169">Significa que literalmente cualquier sitio Web puede realizar las llamadas de AJAX a su API.</span><span class="sxs-lookup"><span data-stu-id="1f07a-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="1f07a-170">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="1f07a-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="1f07a-171">Para permitir que todos los métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="1f07a-171">To allow all HTTP methods:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="1f07a-172">Esto afecta a solicitudes preparatorias y encabezado de acceso-Control-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="1f07a-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="1f07a-173">Establecer los encabezados de solicitudes permitido</span><span class="sxs-lookup"><span data-stu-id="1f07a-173">Set the allowed request headers</span></span>

<span data-ttu-id="1f07a-174">Una solicitud preparatoria de CORS puede incluir un encabezado de acceso-Access-Control-Request-Headers, enumerar los encabezados HTTP establecidos por la aplicación (la "crear encabezados de solicitud").</span><span class="sxs-lookup"><span data-stu-id="1f07a-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="1f07a-175">A encabezados específicos de la lista blanca de direcciones:</span><span class="sxs-lookup"><span data-stu-id="1f07a-175">To whitelist specific headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="1f07a-176">Para permitir que todos crear encabezados de solicitud:</span><span class="sxs-lookup"><span data-stu-id="1f07a-176">To allow all author request headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="1f07a-177">Los exploradores no son completamente coherentes en forma de conjunto de acceso-Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="1f07a-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="1f07a-178">Si establece encabezados en ningún otro valor distinto de "*", debe incluir al menos "Aceptar", "content-type" y "origen", además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="1f07a-178">If you set headers to anything other than "*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="1f07a-179">Establecer los encabezados de respuesta expuesto</span><span class="sxs-lookup"><span data-stu-id="1f07a-179">Set the exposed response headers</span></span>

<span data-ttu-id="1f07a-180">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f07a-180">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="1f07a-181">(Consulte [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="1f07a-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="1f07a-182">Control de caché</span><span class="sxs-lookup"><span data-stu-id="1f07a-182">Cache-Control</span></span>

* <span data-ttu-id="1f07a-183">Content-Language</span><span class="sxs-lookup"><span data-stu-id="1f07a-183">Content-Language</span></span>

* <span data-ttu-id="1f07a-184">Tipo de contenido</span><span class="sxs-lookup"><span data-stu-id="1f07a-184">Content-Type</span></span>

* <span data-ttu-id="1f07a-185">Expira</span><span class="sxs-lookup"><span data-stu-id="1f07a-185">Expires</span></span>

* <span data-ttu-id="1f07a-186">Última modificación</span><span class="sxs-lookup"><span data-stu-id="1f07a-186">Last-Modified</span></span>

* <span data-ttu-id="1f07a-187">Pragma</span><span class="sxs-lookup"><span data-stu-id="1f07a-187">Pragma</span></span>

<span data-ttu-id="1f07a-188">La especificación CORS llama a estos *encabezados de respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="1f07a-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="1f07a-189">Para que otros encabezados estén disponibles para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="1f07a-189">To make other headers available to the application:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="1f07a-190">Credenciales en solicitudes cross-origin</span><span class="sxs-lookup"><span data-stu-id="1f07a-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="1f07a-191">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="1f07a-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="1f07a-192">De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="1f07a-192">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="1f07a-193">Las credenciales son las cookies, así como esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f07a-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="1f07a-194">Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer XMLHttpRequest.withCredentials en true.</span><span class="sxs-lookup"><span data-stu-id="1f07a-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="1f07a-195">Utilizar directamente el objeto XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="1f07a-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="1f07a-196">En jQuery:</span><span class="sxs-lookup"><span data-stu-id="1f07a-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="1f07a-197">Además, el servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="1f07a-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="1f07a-198">Para permitir que las credenciales entre orígenes:</span><span class="sxs-lookup"><span data-stu-id="1f07a-198">To allow cross-origin credentials:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="1f07a-199">Ahora, la respuesta HTTP incluirá un encabezado de acceso-Control-Allow-Credentials, que indica al explorador que el servidor permite que las credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="1f07a-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="1f07a-200">Si el explorador envía las credenciales, pero la respuesta no incluye un encabezado de acceso-Control-Allow-Credentials válido, el explorador no expondrá la respuesta a la aplicación y se produce un error en la solicitud de AJAX.</span><span class="sxs-lookup"><span data-stu-id="1f07a-200">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="1f07a-201">Tenga mucho cuidado acerca de cómo permitir credenciales entre orígenes, ya que significa que un sitio Web en otro dominio puede enviar credenciales ha iniciado la sesión de un usuario a la aplicación en nombre del usuario, sin el conocimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="1f07a-201">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="1f07a-202">El CORS spec también los Estados que orígenes de configuración que "*" (todos los orígenes) no es válido si el encabezado de acceso-Control-Allow-Credentials está presente.</span><span class="sxs-lookup"><span data-stu-id="1f07a-202">The CORS spec also states that setting origins to "*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="1f07a-203">Establecer el tiempo de expiración preparatoria</span><span class="sxs-lookup"><span data-stu-id="1f07a-203">Set the preflight expiration time</span></span>

<span data-ttu-id="1f07a-204">El encabezado de acceso-Control: Max-Age especifica cuánto tiempo puede almacenarse en caché la respuesta a la solicitud preparatoria.</span><span class="sxs-lookup"><span data-stu-id="1f07a-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="1f07a-205">Para establecer este encabezado:</span><span class="sxs-lookup"><span data-stu-id="1f07a-205">To set this header:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="1f07a-206">Funcionamiento de CORS</span><span class="sxs-lookup"><span data-stu-id="1f07a-206">How CORS works</span></span>

<span data-ttu-id="1f07a-207">En esta sección se describe lo que sucede en una solicitud CORS, en el nivel de los mensajes HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f07a-207">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="1f07a-208">Es importante comprender el funcionamiento de CORS, por lo que puede configurar la directiva CORS correctamente y solucionar los problemas si lo no funciona como esperaba.</span><span class="sxs-lookup"><span data-stu-id="1f07a-208">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="1f07a-209">La especificación de CORS presenta varios encabezados HTTP nuevo que permiten las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="1f07a-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="1f07a-210">Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes; no es necesario hacer nada especial en el código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1f07a-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="1f07a-211">Este es un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="1f07a-211">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="1f07a-212">El encabezado de "Origen" proporciona el dominio del sitio que está realizando la solicitud:</span><span class="sxs-lookup"><span data-stu-id="1f07a-212">The "Origin" header gives the domain of the site that is making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="1f07a-213">Si el servidor permite la solicitud, Establece el encabezado de acceso Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="1f07a-213">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="1f07a-214">El valor de este encabezado coincide con el encabezado de origen, o es el valor de carácter comodín "*", lo que significa que se permite cualquier origen.:</span><span class="sxs-lookup"><span data-stu-id="1f07a-214">The value of this header either matches the Origin header, or is the wildcard value "*", meaning that any origin is allowed.:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="1f07a-215">Si la respuesta no incluye el encabezado de acceso Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX.</span><span class="sxs-lookup"><span data-stu-id="1f07a-215">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="1f07a-216">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1f07a-216">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="1f07a-217">Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="1f07a-217">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="1f07a-218">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="1f07a-218">Preflight Requests</span></span>

<span data-ttu-id="1f07a-219">Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud real para el recurso.</span><span class="sxs-lookup"><span data-stu-id="1f07a-219">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="1f07a-220">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="1f07a-220">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="1f07a-221">El método de solicitud es GET, HEAD o POST, y</span><span class="sxs-lookup"><span data-stu-id="1f07a-221">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="1f07a-222">La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o último--Id. de evento, y</span><span class="sxs-lookup"><span data-stu-id="1f07a-222">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="1f07a-223">El encabezado Content-Type (si establece) es uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="1f07a-223">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="1f07a-224">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="1f07a-224">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="1f07a-225">varias partes/de datos de formulario</span><span class="sxs-lookup"><span data-stu-id="1f07a-225">multipart/form-data</span></span>

  * <span data-ttu-id="1f07a-226">texto/sin formato</span><span class="sxs-lookup"><span data-stu-id="1f07a-226">text/plain</span></span>

<span data-ttu-id="1f07a-227">La regla acerca de los encabezados de solicitud se aplica a los encabezados de la aplicación se establece mediante una llamada a setRequestHeader en el objeto XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="1f07a-227">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="1f07a-228">(La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados que se puede establecer el explorador, como agente de usuario, el Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="1f07a-228">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="1f07a-229">Este es un ejemplo de una solicitud preparatoria:</span><span class="sxs-lookup"><span data-stu-id="1f07a-229">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="1f07a-230">La solicitud preparatoria utiliza el método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="1f07a-230">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="1f07a-231">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="1f07a-231">It includes two special headers:</span></span>

* <span data-ttu-id="1f07a-232">Acceso Access-Control-Request-Method: El método HTTP que se usará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="1f07a-232">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="1f07a-233">Acceso Access-Control-Request-Headers: Una lista de encabezados de solicitud que la aplicación se establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="1f07a-233">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="1f07a-234">(De nuevo, esto no incluye encabezados que establece el explorador.)</span><span class="sxs-lookup"><span data-stu-id="1f07a-234">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="1f07a-235">Esta es una respuesta de ejemplo, suponiendo que el servidor permite que la solicitud:</span><span class="sxs-lookup"><span data-stu-id="1f07a-235">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="1f07a-236">La respuesta incluye un encabezado de acceso-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-permitir-Headers, que se muestra los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="1f07a-236">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="1f07a-237">Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real, como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1f07a-237">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
