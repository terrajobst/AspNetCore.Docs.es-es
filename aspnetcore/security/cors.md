---
title: Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo la CORS como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "41836155"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="71a89-103">Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71a89-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="71a89-104">Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="71a89-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="71a89-105">La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="71a89-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="71a89-106">Esta restricción se denomina *"directiva de mismo origen"* e impide que un sitio malintencionado lea los datos confidenciales desde otro sitio.</span><span class="sxs-lookup"><span data-stu-id="71a89-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="71a89-107">En cambio, a veces, es posible que quiera permitir que otros sitios realicen solicitudes entre orígenes en su API web.</span><span class="sxs-lookup"><span data-stu-id="71a89-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="71a89-108">El [uso compartido de recursos entre orígenes](http://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite que un servidor modere la directiva de mismo origen.</span><span class="sxs-lookup"><span data-stu-id="71a89-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="71a89-109">Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras.</span><span class="sxs-lookup"><span data-stu-id="71a89-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="71a89-110">CORS es más seguro y flexible que técnicas anteriores como [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="71a89-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="71a89-111">En este tema, se muestra cómo habilitar CORS en una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71a89-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="71a89-112">¿Qué es el "mismo origen"?</span><span class="sxs-lookup"><span data-stu-id="71a89-112">What is "same origin"?</span></span>

<span data-ttu-id="71a89-113">Dos direcciones URL tienen el mismo origen si tienen puertos, hosts y esquemas idénticos.</span><span class="sxs-lookup"><span data-stu-id="71a89-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="71a89-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="71a89-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="71a89-115">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="71a89-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="71a89-116">Estas direcciones URL tienen orígenes diferentes de los dos anteriores:</span><span class="sxs-lookup"><span data-stu-id="71a89-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="71a89-117">`http://example.net`: dominio diferente</span><span class="sxs-lookup"><span data-stu-id="71a89-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="71a89-118">`http://www.example.com/foo.html`: subdominio diferente</span><span class="sxs-lookup"><span data-stu-id="71a89-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="71a89-119">`https://example.com/foo.html`: esquema diferente</span><span class="sxs-lookup"><span data-stu-id="71a89-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="71a89-120">`http://example.com:9000/foo.html`: puerto diferente</span><span class="sxs-lookup"><span data-stu-id="71a89-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="71a89-121">Internet Explorer no tiene en cuenta el puerto al comparar orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="71a89-122">Habilitar la CORS</span><span class="sxs-lookup"><span data-stu-id="71a89-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="71a89-123">Para configurar CORS en su aplicación, agregue el paquete `Microsoft.AspNetCore.Cors` al proyecto.</span><span class="sxs-lookup"><span data-stu-id="71a89-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="71a89-124">Llame a [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="71a89-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="71a89-125">Habilitación de CORS con middleware</span><span class="sxs-lookup"><span data-stu-id="71a89-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="71a89-126">Para habilitar la CORS, agregue el middleware CORS a la canalización de solicitudes mediante el `UseCors` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="71a89-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="71a89-127">El middleware de CORS debe preceder a cualquier punto de conexión definido en la aplicación que desea admitir las solicitudes entre orígenes (por ejemplo, antes de cualquier llamada a `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="71a89-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="71a89-128">Una directiva de origen cruzado se puede especificar cuando se agrega el middleware CORS mediante el [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) clase.</span><span class="sxs-lookup"><span data-stu-id="71a89-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="71a89-129">Hay dos formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="71a89-129">There are two ways to do this.</span></span> <span data-ttu-id="71a89-130">La primera consiste en llamar a `UseCors` con una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="71a89-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="71a89-131">**Nota:** Debe especificarse la dirección URL sin una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="71a89-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="71a89-132">Si la dirección URL finaliza con `/`, la comparación devolverá `false` y no se devolverá ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="71a89-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="71a89-133">La expresión lambda toma un objeto `CorsPolicyBuilder`.</span><span class="sxs-lookup"><span data-stu-id="71a89-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="71a89-134">Encontrará una lista de las [opciones de configuración](#cors-policy-options) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="71a89-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="71a89-135">En este ejemplo, la directiva permite las solicitudes entre orígenes de `http://example.com` y no de otros orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="71a89-136">CorsPolicyBuilder tiene una API fluida, por lo que puede encadenar llamadas de métodos:</span><span class="sxs-lookup"><span data-stu-id="71a89-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="71a89-137">El segundo enfoque es definir una o varias directivas CORS con nombre y, después, seleccionar la directiva por su nombre en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="71a89-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="71a89-138">Este ejemplo agrega una directiva CORS denominada "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="71a89-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="71a89-139">Para seleccionar la directiva, pase el nombre a `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="71a89-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="71a89-140">Habilitación de CORS en MVC</span><span class="sxs-lookup"><span data-stu-id="71a89-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="71a89-141">Como alternativa, puede usar MVC para aplicar CORS específico por acción, por controlador o globalmente para todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="71a89-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="71a89-142">Al utilizar MVC para habilitar la CORS se utilizan los mismos servicios CORS, pero no es el middleware de CORS.</span><span class="sxs-lookup"><span data-stu-id="71a89-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="71a89-143">Por acción</span><span class="sxs-lookup"><span data-stu-id="71a89-143">Per action</span></span>

<span data-ttu-id="71a89-144">Para especificar una directiva CORS para una acción específica, agregue el atributo `[EnableCors]`a la acción.</span><span class="sxs-lookup"><span data-stu-id="71a89-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="71a89-145">Especifique el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="71a89-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="71a89-146">Por cada controlador</span><span class="sxs-lookup"><span data-stu-id="71a89-146">Per controller</span></span>

<span data-ttu-id="71a89-147">Para especificar la directiva CORS para un controlador específico, agregue el atributo `[EnableCors]` a la clase controller.</span><span class="sxs-lookup"><span data-stu-id="71a89-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="71a89-148">Especifique el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="71a89-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="71a89-149">Globalmente</span><span class="sxs-lookup"><span data-stu-id="71a89-149">Globally</span></span>

<span data-ttu-id="71a89-150">Puede habilitar CORS globalmente para todos los controladores si agrega el filtro `CorsAuthorizationFilterFactory`a la colección de filtros globales:</span><span class="sxs-lookup"><span data-stu-id="71a89-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="71a89-151">El orden de prioridad es: acción, controlador, global.</span><span class="sxs-lookup"><span data-stu-id="71a89-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="71a89-152">Las directivas de nivel de acción tienen prioridad sobre las directivas de nivel de controlador y estas últimas tienen prioridad sobre las directivas globales.</span><span class="sxs-lookup"><span data-stu-id="71a89-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="71a89-153">Deshabilitar la CORS</span><span class="sxs-lookup"><span data-stu-id="71a89-153">Disable CORS</span></span>

<span data-ttu-id="71a89-154">Para deshabilitar CORS para una acción o un controlador, use el atributo `[DisableCors]`.</span><span class="sxs-lookup"><span data-stu-id="71a89-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="71a89-155">Opciones de directiva CORS</span><span class="sxs-lookup"><span data-stu-id="71a89-155">CORS policy options</span></span>

<span data-ttu-id="71a89-156">Esta sección describen las distintas opciones que puede establecer en una directiva CORS.</span><span class="sxs-lookup"><span data-stu-id="71a89-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="71a89-157">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="71a89-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="71a89-158">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="71a89-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="71a89-159">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="71a89-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="71a89-160">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="71a89-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="71a89-161">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="71a89-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="71a89-162">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="71a89-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="71a89-163">Para algunas opciones, puede resultar útil leer [cómo la CORS funciona](#how-cors-works) primero.</span><span class="sxs-lookup"><span data-stu-id="71a89-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="71a89-164">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="71a89-164">Set the allowed origins</span></span>

<span data-ttu-id="71a89-165">Para permitir uno o más orígenes específicos:</span><span class="sxs-lookup"><span data-stu-id="71a89-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="71a89-166">Para permitir todos los orígenes:</span><span class="sxs-lookup"><span data-stu-id="71a89-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="71a89-167">Considere detenidamente antes de permitir las solicitudes desde cualquier origen.</span><span class="sxs-lookup"><span data-stu-id="71a89-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="71a89-168">Significa que literalmente cualquier sitio Web puede realizar llamadas de AJAX a la API.</span><span class="sxs-lookup"><span data-stu-id="71a89-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="71a89-169">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="71a89-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="71a89-170">Para permitir todos los métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="71a89-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="71a89-171">Esto afecta a las solicitudes de comprobaciones preparatorias y al encabezado Access-Control-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="71a89-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="71a89-172">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="71a89-172">Set the allowed request headers</span></span>

<span data-ttu-id="71a89-173">Una solicitud de comprobaciones preparatorias de CORS puede incluir un encabezado Access-Control-Request-Headers, en el que aparezcan los encabezados HTTP que ha establecido la aplicación (los denominados "encabezados de solicitud de autor").</span><span class="sxs-lookup"><span data-stu-id="71a89-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="71a89-174">Para agregar encabezados específicos a la lista de permitidos:</span><span class="sxs-lookup"><span data-stu-id="71a89-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="71a89-175">Para permitir todos los encabezados de solicitud de autor:</span><span class="sxs-lookup"><span data-stu-id="71a89-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="71a89-176">Los exploradores no son completamente coherentes en la forma en que establecen Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="71a89-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="71a89-177">Si establece encabezados en un valor que no sea "\*", debe incluir al menos "accept", "content-type" y "origin", además de los encabezados personalizados que quiera admitir.</span><span class="sxs-lookup"><span data-stu-id="71a89-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="71a89-178">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="71a89-178">Set the exposed response headers</span></span>

<span data-ttu-id="71a89-179">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71a89-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="71a89-180">(Consulte [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="71a89-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="71a89-181">Control de caché</span><span class="sxs-lookup"><span data-stu-id="71a89-181">Cache-Control</span></span>

* <span data-ttu-id="71a89-182">Idioma del contenido</span><span class="sxs-lookup"><span data-stu-id="71a89-182">Content-Language</span></span>

* <span data-ttu-id="71a89-183">Content-Type</span><span class="sxs-lookup"><span data-stu-id="71a89-183">Content-Type</span></span>

* <span data-ttu-id="71a89-184">Expires</span><span class="sxs-lookup"><span data-stu-id="71a89-184">Expires</span></span>

* <span data-ttu-id="71a89-185">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="71a89-185">Last-Modified</span></span>

* <span data-ttu-id="71a89-186">pragma</span><span class="sxs-lookup"><span data-stu-id="71a89-186">Pragma</span></span>

<span data-ttu-id="71a89-187">La especificación CORS llama a estos *encabezados de respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="71a89-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="71a89-188">Para que otros encabezados estén disponibles para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="71a89-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="71a89-189">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="71a89-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="71a89-190">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="71a89-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="71a89-191">De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="71a89-192">Las credenciales incluyen las cookies, así como los esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a89-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="71a89-193">Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer XMLHttpRequest.withCredentials en true.</span><span class="sxs-lookup"><span data-stu-id="71a89-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="71a89-194">Al usar directamente el objeto XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="71a89-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="71a89-195">En jQuery:</span><span class="sxs-lookup"><span data-stu-id="71a89-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="71a89-196">Además, el servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="71a89-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="71a89-197">Para permitir las credenciales entre orígenes:</span><span class="sxs-lookup"><span data-stu-id="71a89-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="71a89-198">Ahora, la respuesta HTTP incluirá un encabezado Access-Control-Allow-Credentials, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="71a89-199">Si el explorador envía las credenciales, pero la respuesta no incluye un encabezado Access-Control-Allow-Credentials válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud de AJAX.</span><span class="sxs-lookup"><span data-stu-id="71a89-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="71a89-200">Tenga cuidado al permitir credenciales entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="71a89-201">Un sitio web en otro dominio puede enviar las credenciales de un usuario que ha iniciado sesión a la aplicación en nombre del usuario sin su conocimiento.</span><span class="sxs-lookup"><span data-stu-id="71a89-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="71a89-202">La especificación de CORS también indica ese valor orígenes a `"*"` (todos los orígenes) no es válido si el `Access-Control-Allow-Credentials` encabezado está presente.</span><span class="sxs-lookup"><span data-stu-id="71a89-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="71a89-203">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="71a89-203">Set the preflight expiration time</span></span>

<span data-ttu-id="71a89-204">El encabezado Access-Control-Max-Age especifica durante cuánto tiempo puede almacenarse en caché la respuesta a la solicitud de comprobaciones preparatorias.</span><span class="sxs-lookup"><span data-stu-id="71a89-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="71a89-205">Para establecer este encabezado:</span><span class="sxs-lookup"><span data-stu-id="71a89-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="71a89-206">Cómo funciona la CORS</span><span class="sxs-lookup"><span data-stu-id="71a89-206">How CORS works</span></span>

<span data-ttu-id="71a89-207">Esta sección describe lo que ocurre en una solicitud de CORS en el nivel de los mensajes HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a89-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="71a89-208">Es importante entender cómo funciona la CORS para que la directiva CORS puede ser configurada correctamente y depurando cuando se producen comportamientos inesperados.</span><span class="sxs-lookup"><span data-stu-id="71a89-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="71a89-209">La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="71a89-210">Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="71a89-211">No se necesita código JavaScript personalizado para habilitar CORS.</span><span class="sxs-lookup"><span data-stu-id="71a89-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="71a89-212">Este es un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="71a89-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="71a89-213">El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:</span><span class="sxs-lookup"><span data-stu-id="71a89-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="71a89-214">Si el servidor permite la solicitud, establece el encabezado Access-Control-Allow-Origin en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="71a89-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="71a89-215">El valor de este encabezado coincide con el encabezado de origen de la solicitud, o es el valor de carácter comodín "\*", lo que significa que se permite cualquier origen:</span><span class="sxs-lookup"><span data-stu-id="71a89-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="71a89-216">Si la respuesta no incluye el encabezado Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX.</span><span class="sxs-lookup"><span data-stu-id="71a89-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="71a89-217">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="71a89-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="71a89-218">Incluso si el servidor devuelve una respuesta correcta, el explorador no expone la respuesta a la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="71a89-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="71a89-219">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="71a89-219">Preflight Requests</span></span>

<span data-ttu-id="71a89-220">Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud para el recurso real.</span><span class="sxs-lookup"><span data-stu-id="71a89-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="71a89-221">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="71a89-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="71a89-222">El método de solicitud es GET, HEAD o POST, y</span><span class="sxs-lookup"><span data-stu-id="71a89-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="71a89-223">La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o Last-Event-ID, y</span><span class="sxs-lookup"><span data-stu-id="71a89-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="71a89-224">El encabezado Content-Type (si se establece) es uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="71a89-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="71a89-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="71a89-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="71a89-226">varias partes/datos de formulario</span><span class="sxs-lookup"><span data-stu-id="71a89-226">multipart/form-data</span></span>

  * <span data-ttu-id="71a89-227">text/plain</span><span class="sxs-lookup"><span data-stu-id="71a89-227">text/plain</span></span>

<span data-ttu-id="71a89-228">La regla sobre los encabezados de solicitud se aplica a los encabezados que la aplicación establece mediante una llamada a setRequestHeader en el objeto XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="71a89-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="71a89-229">(La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados que puede establecer el explorador, como User-Agent, Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="71a89-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="71a89-230">Este es un ejemplo de una solicitud de preflight:</span><span class="sxs-lookup"><span data-stu-id="71a89-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="71a89-231">La solicitud preparatoria usa el método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="71a89-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="71a89-232">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="71a89-232">It includes two special headers:</span></span>

* <span data-ttu-id="71a89-233">Access-Control-Request-Method: el método HTTP que se usará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="71a89-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="71a89-234">Access-Control-Request-Headers: una lista de encabezados de solicitud que la aplicación establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="71a89-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="71a89-235">(De nuevo, esto no incluye los encabezados que establece el explorador).</span><span class="sxs-lookup"><span data-stu-id="71a89-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="71a89-236">Este es un ejemplo de respuesta, suponiendo que el servidor permite la solicitud:</span><span class="sxs-lookup"><span data-stu-id="71a89-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="71a89-237">La respuesta incluye un encabezado Access-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-Allow-Headers, que muestra los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="71a89-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="71a89-238">Si la solicitud de comprobaciones preparatorias se realiza correctamente, el explorador envía la solicitud real, como se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="71a89-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
