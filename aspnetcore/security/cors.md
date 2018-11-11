---
title: Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo la CORS como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191326"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="8af62-103">Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8af62-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="8af62-104">Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8af62-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8af62-105">Seguridad del explorador impide que una página web que realizan solicitudes a un dominio diferente a la que proviene la página web.</span><span class="sxs-lookup"><span data-stu-id="8af62-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="8af62-106">Esta restricción se denomina el *directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="8af62-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="8af62-107">La directiva de mismo origen impide que un sitio malintencionado lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="8af62-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="8af62-108">En ocasiones, es posible que desea permitir que otros sitios realizan solicitudes entre orígenes en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af62-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="8af62-109">El [uso compartido de recursos entre orígenes](https://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite que un servidor modere la directiva de mismo origen.</span><span class="sxs-lookup"><span data-stu-id="8af62-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="8af62-110">Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras.</span><span class="sxs-lookup"><span data-stu-id="8af62-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="8af62-111">CORS es más seguro y más flexible que las técnicas anteriores, como [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="8af62-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="8af62-112">En este tema se muestra cómo se habilita la CORS en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8af62-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="8af62-113">Mismo origen</span><span class="sxs-lookup"><span data-stu-id="8af62-113">Same origin</span></span>

<span data-ttu-id="8af62-114">Dos direcciones URL tienen el mismo origen si tienen esquemas idénticos, los hosts y los puertos ([6454 RFC](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="8af62-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="8af62-115">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="8af62-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="8af62-116">Estas direcciones URL tienen orígenes diferentes que las dos direcciones URL anteriores:</span><span class="sxs-lookup"><span data-stu-id="8af62-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="8af62-117">`https://example.net` &ndash; Dominio diferente</span><span class="sxs-lookup"><span data-stu-id="8af62-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="8af62-118">`https://www.example.com/foo.html` &ndash; Subdominio distinto</span><span class="sxs-lookup"><span data-stu-id="8af62-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="8af62-119">`http://example.com/foo.html` &ndash; Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="8af62-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="8af62-120">`https://example.com:9000/foo.html` &ndash; Otro puerto</span><span class="sxs-lookup"><span data-stu-id="8af62-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="8af62-121">Internet Explorer no tiene en cuenta el puerto al comparar orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="8af62-122">Registrar servicios CORS</span><span class="sxs-lookup"><span data-stu-id="8af62-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8af62-123">Referencia de la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paquete.</span><span class="sxs-lookup"><span data-stu-id="8af62-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8af62-124">Referencia de la [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paquete.</span><span class="sxs-lookup"><span data-stu-id="8af62-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8af62-125">Agregue una referencia de paquete a la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paquete.</span><span class="sxs-lookup"><span data-stu-id="8af62-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="8af62-126">Llame a <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> en `Startup.ConfigureServices` para agregar servicios de la CORS al contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8af62-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="8af62-127">Habilitar la CORS</span><span class="sxs-lookup"><span data-stu-id="8af62-127">Enable CORS</span></span>

<span data-ttu-id="8af62-128">Después de registrar servicios CORS, use cualquiera de los métodos siguientes para habilitar la CORS en una aplicación ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8af62-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="8af62-129">[Middleware de CORS](#enable-cors-with-cors-middleware) &ndash; directivas CORS se aplican globalmente a la aplicación a través de middleware.</span><span class="sxs-lookup"><span data-stu-id="8af62-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="8af62-130">[La CORS en MVC](#enable-cors-in-mvc) &ndash; las directivas de aplicar la CORS por cada acción o por cada controlador.</span><span class="sxs-lookup"><span data-stu-id="8af62-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="8af62-131">No se usa el Middleware de CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="8af62-132">Habilitar la CORS con Middleware de CORS</span><span class="sxs-lookup"><span data-stu-id="8af62-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="8af62-133">Middleware de CORS controla las solicitudes de origen cruzado a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af62-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="8af62-134">Para habilitar el Middleware de CORS en la canalización de procesamiento de la solicitud, llame a la <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensión en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="8af62-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="8af62-135">Middleware de CORS debe preceder a cualquier punto de conexión definido en la aplicación que desea admitir las solicitudes entre orígenes (por ejemplo, antes de llamar a `UseMvc` de Middleware de páginas de Razor y MVC).</span><span class="sxs-lookup"><span data-stu-id="8af62-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="8af62-136">Un *directiva de origen cruzado* puede especificarse cuando se agrega el Middleware de CORS mediante el <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> clase.</span><span class="sxs-lookup"><span data-stu-id="8af62-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="8af62-137">Existen dos enfoques para definir una directiva CORS:</span><span class="sxs-lookup"><span data-stu-id="8af62-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="8af62-138">Llamar a `UseCors` con una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="8af62-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="8af62-139">La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8af62-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="8af62-140">[Las opciones de configuración](#cors-policy-options), tales como `WithOrigins`, se describen más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="8af62-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="8af62-141">En el ejemplo anterior, la directiva permite que las solicitudes entre orígenes de `https://example.com` y no hay otros orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="8af62-142">Debe especificarse la dirección URL sin una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="8af62-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="8af62-143">Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="8af62-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="8af62-144">`CorsPolicyBuilder` tiene una API fluida, por lo que se pueden encadenar llamadas de métodos:</span><span class="sxs-lookup"><span data-stu-id="8af62-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="8af62-145">Definir una o varias directivas CORS con nombre y seleccione la directiva por su nombre en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="8af62-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="8af62-146">En el ejemplo siguiente se agrega una directiva CORS definido por el usuario denominada *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="8af62-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="8af62-147">Para seleccionar la directiva, pase el nombre a `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="8af62-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="8af62-148">Habilitar la CORS en MVC</span><span class="sxs-lookup"><span data-stu-id="8af62-148">Enable CORS in MVC</span></span>

<span data-ttu-id="8af62-149">Como alternativa, puede usar MVC para aplicar directivas específicas de CORS por cada acción o por cada controlador.</span><span class="sxs-lookup"><span data-stu-id="8af62-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="8af62-150">Al usar MVC para habilitar la CORS, se usan los servicios registrados de la CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="8af62-151">No se usa el Middleware de CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="8af62-152">Por acción</span><span class="sxs-lookup"><span data-stu-id="8af62-152">Per action</span></span>

<span data-ttu-id="8af62-153">Para especificar una directiva CORS para una acción específica, agregue el [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo a la acción.</span><span class="sxs-lookup"><span data-stu-id="8af62-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="8af62-154">Especifique el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="8af62-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="8af62-155">Por cada controlador</span><span class="sxs-lookup"><span data-stu-id="8af62-155">Per controller</span></span>

<span data-ttu-id="8af62-156">Para especificar la directiva CORS de un controlador concreto, agregue el [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) a la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="8af62-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="8af62-157">Especifique el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="8af62-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="8af62-158">Es el orden de prioridad:</span><span class="sxs-lookup"><span data-stu-id="8af62-158">The precedence order is:</span></span>

1. <span data-ttu-id="8af62-159">predeterminada</span><span class="sxs-lookup"><span data-stu-id="8af62-159">action</span></span>
1. <span data-ttu-id="8af62-160">controlador</span><span class="sxs-lookup"><span data-stu-id="8af62-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="8af62-161">Deshabilitar la CORS</span><span class="sxs-lookup"><span data-stu-id="8af62-161">Disable CORS</span></span>

<span data-ttu-id="8af62-162">Para deshabilitar CORS para una acción o controlador, utilice la [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atributo:</span><span class="sxs-lookup"><span data-stu-id="8af62-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="8af62-163">Opciones de directiva CORS</span><span class="sxs-lookup"><span data-stu-id="8af62-163">CORS policy options</span></span>

<span data-ttu-id="8af62-164">Esta sección describen las distintas opciones que puede establecer en una directiva CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="8af62-165">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="8af62-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="8af62-166">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="8af62-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="8af62-167">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="8af62-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="8af62-168">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="8af62-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="8af62-169">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="8af62-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="8af62-170">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="8af62-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="8af62-171">Para algunas opciones, puede resultar útil leer la [cómo la CORS funciona](#how-cors-works) sección en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="8af62-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="8af62-172">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="8af62-172">Set the allowed origins</span></span>

<span data-ttu-id="8af62-173">El middleware de CORS en ASP.NET Core MVC tiene varias maneras de especificar los orígenes permitidos:</span><span class="sxs-lookup"><span data-stu-id="8af62-173">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="8af62-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Permite especificar una o más direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="8af62-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Allows specifying one or more URLs.</span></span> <span data-ttu-id="8af62-175">La dirección URL puede incluir el esquema, el nombre de host y el puerto sin información de ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="8af62-175">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="8af62-176">Por ejemplo: `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="8af62-176">For example, `https://example.com`.</span></span> <span data-ttu-id="8af62-177">Debe especificarse la dirección URL sin una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="8af62-177">The URL must be specified without a trailing slash (`/`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <span data-ttu-id="8af62-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Permite que las solicitudes CORS de todos los orígenes con cualquier esquema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="8af62-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="8af62-179">Considere detenidamente antes de permitir las solicitudes desde cualquier origen.</span><span class="sxs-lookup"><span data-stu-id="8af62-179">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="8af62-180">Permitir las solicitudes desde cualquier origen significa que *cualquier sitio Web* puede realizar solicitudes entre orígenes en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af62-180">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="8af62-181">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios.</span><span class="sxs-lookup"><span data-stu-id="8af62-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8af62-182">El servicio de la CORS devuelve una respuesta no válida de la CORS cuando una aplicación está configurada con los dos.</span><span class="sxs-lookup"><span data-stu-id="8af62-182">The CORS service returns an invalid CORS response when an app is configured with the two.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="8af62-183">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios.</span><span class="sxs-lookup"><span data-stu-id="8af62-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8af62-184">Considere la posibilidad de especificar una lista exacta de los orígenes, si el cliente necesita para autorizar el acceso a los recursos de servidor.</span><span class="sxs-lookup"><span data-stu-id="8af62-184">Consider specifying an exact list of origins if your client needs to authorize to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="8af62-185">Esta configuración afecta a [comprobaciones de las solicitudes y el encabezado de Access-Control-Allow-Origin](#preflight-requests) (descrita más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="8af62-185">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

* <span data-ttu-id="8af62-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> -Permite que las solicitudes CORS desde cualquier subdominio de un dominio determinado.</span><span class="sxs-lookup"><span data-stu-id="8af62-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> - Allows CORS requests from any sub-domain of a given domain.</span></span> <span data-ttu-id="8af62-187">El esquema no puede ser un carácter comodín.</span><span class="sxs-lookup"><span data-stu-id="8af62-187">The scheme cannot be a wildcard.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="8af62-188">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="8af62-188">Set the allowed HTTP methods</span></span>

<span data-ttu-id="8af62-189">Para permitir que todos los métodos HTTP, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-189">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="8af62-190">Esta configuración afecta a [comprobaciones de las solicitudes y el encabezado de Access-Control-Allow-Methods](#preflight-requests) (descrita más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="8af62-190">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="8af62-191">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="8af62-191">Set the allowed request headers</span></span>

<span data-ttu-id="8af62-192">Para permitir que los encabezados específicos que se enviarán en una solicitud de CORS, llamado *crear encabezados de solicitud*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:</span><span class="sxs-lookup"><span data-stu-id="8af62-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="8af62-193">Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="8af62-194">Esta configuración afecta a [comprobaciones de las solicitudes y el encabezado de Access-Control-Request-Headers](#preflight-requests) (descrita más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="8af62-194">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8af62-195">Una coincidencia de directiva de Middleware de CORS a encabezados específicos especificado por `WithHeaders` solo es posible cuando se envían los encabezados `Access-Control-Request-Headers` coincidir exactamente con los encabezados que se indica en `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="8af62-195">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="8af62-196">Por ejemplo, considere una aplicación configurada como sigue:</span><span class="sxs-lookup"><span data-stu-id="8af62-196">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8af62-197">Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="8af62-197">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="8af62-198">Devuelve la aplicación un *200 Aceptar* respuesta pero no envía de vuelta los encabezados CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-198">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8af62-199">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-199">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8af62-200">Middleware de CORS siempre permite cuatro encabezados en el `Access-Control-Request-Headers` para enviarse independientemente de los valores configurados en CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="8af62-200">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="8af62-201">Esta lista de encabezados incluye:</span><span class="sxs-lookup"><span data-stu-id="8af62-201">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="8af62-202">Por ejemplo, considere una aplicación configurada como sigue:</span><span class="sxs-lookup"><span data-stu-id="8af62-202">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8af62-203">Middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` siempre es la lista de permitidos:</span><span class="sxs-lookup"><span data-stu-id="8af62-203">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="8af62-204">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="8af62-204">Set the exposed response headers</span></span>

<span data-ttu-id="8af62-205">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af62-205">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="8af62-206">Para obtener más información, consulte [W3C Cross-Origin Resource Sharing (la terminología): encabezado de respuesta Simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="8af62-206">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="8af62-207">Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="8af62-207">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="8af62-208">La especificación de CORS llama a estos encabezados *encabezados de respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="8af62-208">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="8af62-209">Para que otros encabezados disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-209">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="8af62-210">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="8af62-210">Credentials in cross-origin requests</span></span>

<span data-ttu-id="8af62-211">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-211">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="8af62-212">De forma predeterminada, el explorador no envía las credenciales con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-212">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="8af62-213">Las credenciales incluyen las cookies y los esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="8af62-213">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="8af62-214">Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` a `true`.</span><span class="sxs-lookup"><span data-stu-id="8af62-214">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="8af62-215">Uso de `XMLHttpRequest` directamente:</span><span class="sxs-lookup"><span data-stu-id="8af62-215">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="8af62-216">En jQuery:</span><span class="sxs-lookup"><span data-stu-id="8af62-216">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="8af62-217">Además, el servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="8af62-217">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="8af62-218">Para permitir que las credenciales de origen cruzado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="8af62-219">La respuesta HTTP incluye un `Access-Control-Allow-Credentials` encabezado, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="8af62-220">Si el explorador envía credenciales, pero la respuesta no incluye válido `Access-Control-Allow-Credentials` encabezado, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="8af62-221">Tenga cuidado al permitir credenciales entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-221">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="8af62-222">Un sitio Web en otro dominio puede enviar las credenciales de un usuario con sesión iniciada de la aplicación en el nombre de usuario sin el conocimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="8af62-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="8af62-223">La especificación de CORS también indica ese valor orígenes a `"*"` (todos los orígenes) no es válido si el `Access-Control-Allow-Credentials` encabezado está presente.</span><span class="sxs-lookup"><span data-stu-id="8af62-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="8af62-224">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="8af62-224">Preflight requests</span></span>

<span data-ttu-id="8af62-225">Para algunas solicitudes CORS, el explorador envía una solicitud adicional antes de realizar la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="8af62-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="8af62-226">Se llama a esta solicitud una *solicitud preparatoria*.</span><span class="sxs-lookup"><span data-stu-id="8af62-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="8af62-227">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="8af62-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="8af62-228">El método de solicitud es GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="8af62-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="8af62-229">La aplicación, no establezca los encabezados de solicitud distinto `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="8af62-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="8af62-230">El `Content-Type` encabezado, si establece, tiene uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="8af62-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="8af62-231">La regla en los encabezados de solicitud establecido para la solicitud de cliente se aplica a los encabezados de la aplicación se establece mediante una llamada a `setRequestHeader` en el `XMLHttpRequest` objeto.</span><span class="sxs-lookup"><span data-stu-id="8af62-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="8af62-232">La especificación de CORS llama a estos encabezados *crear encabezados de solicitud*.</span><span class="sxs-lookup"><span data-stu-id="8af62-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="8af62-233">La regla no se aplica a los encabezados que se puede establecer el explorador, tales como `User-Agent`, `Host`, o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="8af62-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="8af62-234">Este es un ejemplo de una solicitud de preflight:</span><span class="sxs-lookup"><span data-stu-id="8af62-234">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="8af62-235">La solicitud preparatoria usa el método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="8af62-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="8af62-236">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="8af62-236">It includes two special headers:</span></span>

* <span data-ttu-id="8af62-237">`Access-Control-Request-Method`: El método HTTP que se usará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="8af62-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="8af62-238">`Access-Control-Request-Headers`: Una lista de encabezados de solicitud que la aplicación se establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="8af62-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="8af62-239">Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, como `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="8af62-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="8af62-240">Una solicitud preparatoria de CORS puede incluir un `Access-Control-Request-Headers` encabezado, que indica al servidor los encabezados que se envían con la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="8af62-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="8af62-241">Para permitir encabezados específicos, llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="8af62-242">Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="8af62-243">Los exploradores no están totalmente coherentes en cómo establezca `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="8af62-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="8af62-244">Si establece los encabezados en algo distinto `"*"` (o use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`, y `Origin`, además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="8af62-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="8af62-245">Este es un ejemplo de respuesta a la solicitud preparatoria (suponiendo que el servidor permite la solicitud):</span><span class="sxs-lookup"><span data-stu-id="8af62-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="8af62-246">La respuesta incluye un `Access-Control-Allow-Methods` encabezado que se enumera los métodos permitidos y, opcionalmente, un `Access-Control-Allow-Headers` encabezado, que se enumera los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="8af62-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="8af62-247">Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="8af62-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="8af62-248">Si se deniega la solicitud preparatoria, la aplicación devuelve un *200 Aceptar* respuesta pero no envía de vuelta los encabezados CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8af62-249">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="8af62-250">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="8af62-250">Set the preflight expiration time</span></span>

<span data-ttu-id="8af62-251">El `Access-Control-Max-Age` encabezado especifica cuánto tiempo puede almacenar en caché la respuesta a la solicitud preparatoria.</span><span class="sxs-lookup"><span data-stu-id="8af62-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="8af62-252">Para establecer este encabezado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="8af62-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="8af62-253">Cómo funciona la CORS</span><span class="sxs-lookup"><span data-stu-id="8af62-253">How CORS works</span></span>

<span data-ttu-id="8af62-254">Esta sección describe lo que ocurre en una solicitud de CORS en el nivel de los mensajes HTTP.</span><span class="sxs-lookup"><span data-stu-id="8af62-254">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="8af62-255">Es importante entender cómo funciona la CORS para que la directiva CORS puede ser configurada correctamente y depurando cuando se producen comportamientos inesperados.</span><span class="sxs-lookup"><span data-stu-id="8af62-255">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="8af62-256">La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-256">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="8af62-257">Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-257">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="8af62-258">No se necesita código JavaScript personalizado para habilitar CORS.</span><span class="sxs-lookup"><span data-stu-id="8af62-258">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="8af62-259">El siguiente es un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="8af62-259">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="8af62-260">El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:</span><span class="sxs-lookup"><span data-stu-id="8af62-260">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="8af62-261">Si el servidor permite la solicitud, Establece la `Access-Control-Allow-Origin` encabezado en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="8af62-261">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="8af62-262">El valor de este encabezado ya sea con la `Origin` encabezado de la solicitud o es el valor de carácter comodín `"*"`, lo que significa que se permite cualquier origen:</span><span class="sxs-lookup"><span data-stu-id="8af62-262">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="8af62-263">Si la respuesta no incluye el `Access-Control-Allow-Origin` encabezado, la solicitud de origen cruzado se produce un error.</span><span class="sxs-lookup"><span data-stu-id="8af62-263">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="8af62-264">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="8af62-264">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="8af62-265">Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="8af62-265">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8af62-266">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8af62-266">Additional resources</span></span>

* [<span data-ttu-id="8af62-267">Recursos entre orígenes (CORS) de uso compartido</span><span class="sxs-lookup"><span data-stu-id="8af62-267">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
