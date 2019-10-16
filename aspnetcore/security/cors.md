---
title: Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo CORS como estándar para permitir o rechazar solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/13/2019
uid: security/cors
ms.openlocfilehash: 3a51d365626c858ad48298a1108e37eba9050fe7
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391292"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="fe791-103">Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe791-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="fe791-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe791-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fe791-105">En este artículo se muestra cómo habilitar CORS en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe791-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="fe791-106">La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web.</span><span class="sxs-lookup"><span data-stu-id="fe791-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="fe791-107">Esta restricción se llama *Directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="fe791-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="fe791-108">La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="fe791-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="fe791-109">En ocasiones, es posible que desee permitir que otros sitios realicen solicitudes entre orígenes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fe791-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="fe791-110">Para obtener más información, consulte el [artículo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="fe791-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="fe791-111">[Uso compartido de recursos entre orígenes](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="fe791-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="fe791-112">Es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.</span><span class="sxs-lookup"><span data-stu-id="fe791-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="fe791-113">**No** es una característica de seguridad, CORS reduce la seguridad.</span><span class="sxs-lookup"><span data-stu-id="fe791-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="fe791-114">Una API no es más segura al permitir CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="fe791-115">Para obtener más información, vea [Cómo funciona CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="fe791-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="fe791-116">Permite que un servidor permita explícitamente algunas solicitudes entre orígenes mientras se rechazan otras.</span><span class="sxs-lookup"><span data-stu-id="fe791-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="fe791-117">Es más seguro y más flexible que las técnicas anteriores, como [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="fe791-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="fe791-118">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe791-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="fe791-119">Mismo origen</span><span class="sxs-lookup"><span data-stu-id="fe791-119">Same origin</span></span>

<span data-ttu-id="fe791-120">Dos direcciones URL tienen el mismo origen si tienen esquemas, hosts y puertos idénticos ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="fe791-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="fe791-121">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="fe791-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="fe791-122">Estas direcciones URL tienen distintos orígenes que las dos direcciones URL anteriores:</span><span class="sxs-lookup"><span data-stu-id="fe791-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="fe791-123">dominio diferente `https://example.net` &ndash;</span><span class="sxs-lookup"><span data-stu-id="fe791-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="fe791-124">`https://www.example.com/foo.html` &ndash; subdominio diferente</span><span class="sxs-lookup"><span data-stu-id="fe791-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="fe791-125">`http://example.com/foo.html` &ndash; esquema diferente</span><span class="sxs-lookup"><span data-stu-id="fe791-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="fe791-126">`https://example.com:9000/foo.html` &ndash; puerto diferente</span><span class="sxs-lookup"><span data-stu-id="fe791-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="fe791-127">Internet Explorer no tiene en cuenta el puerto al comparar los orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="fe791-128">CORS con Directiva con nombre y middleware</span><span class="sxs-lookup"><span data-stu-id="fe791-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="fe791-129">Middleware de CORS controla las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="fe791-130">El código siguiente habilita CORS para toda la aplicación con el origen especificado:</span><span class="sxs-lookup"><span data-stu-id="fe791-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="fe791-131">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="fe791-131">The preceding code:</span></span>

* <span data-ttu-id="fe791-132">Establece el nombre de la Directiva en "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="fe791-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="fe791-133">El nombre de la Directiva es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="fe791-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="fe791-134">Llama al método de extensión <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, que habilita CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="fe791-135">Llama a <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="fe791-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="fe791-136">La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fe791-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="fe791-137">[Las opciones de configuración](#cors-policy-options), como `WithOrigins`, se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="fe791-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="fe791-138">La llamada al método <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> agrega los servicios CORS al contenedor de servicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fe791-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="fe791-139">Para obtener más información, vea [Opciones de directiva de CORS](#cpo) en este documento.</span><span class="sxs-lookup"><span data-stu-id="fe791-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="fe791-140">El método <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> puede encadenar métodos, tal y como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="fe791-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="fe791-141">Nota: la dirección URL **no** debe contener una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="fe791-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="fe791-142">Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="fe791-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="fe791-143">Aplicar directivas CORS a todos los puntos de conexión</span><span class="sxs-lookup"><span data-stu-id="fe791-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="fe791-144">El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="fe791-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="fe791-145">Con el enrutamiento de punto de conexión, el middleware de CORS debe estar configurado para ejecutarse entre las llamadas a `UseRouting` y `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="fe791-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="fe791-146">Una configuración incorrecta hará que el middleware deje de funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="fe791-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="fe791-147">El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="fe791-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```
<span data-ttu-id="fe791-148">Nota: se debe llamar a `UseCors` antes de `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="fe791-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="fe791-149">Consulte [habilitación de CORS en Razor pages, controladores y métodos de acción](#ecors) para aplicar la Directiva CORS en el nivel de página/controlador/acción.</span><span class="sxs-lookup"><span data-stu-id="fe791-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="fe791-150">Consulte [prueba de CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.</span><span class="sxs-lookup"><span data-stu-id="fe791-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="fe791-151">Habilitación de CORS con enrutamiento de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="fe791-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="fe791-152">Con el enrutamiento de punto de conexión, CORS se puede habilitar en cada punto de conexión mediante el conjunto `RequireCors` de métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="fe791-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="fe791-153">Del mismo modo, CORS también puede habilitarse para todos los controladores:</span><span class="sxs-lookup"><span data-stu-id="fe791-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="fe791-154">Habilitación de CORS con atributos</span><span class="sxs-lookup"><span data-stu-id="fe791-154">Enable CORS with attributes</span></span>

<span data-ttu-id="fe791-155">El atributo [&lbrack;EnableCors @ no__t-2](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) proporciona una alternativa a la aplicación de CORS globalmente.</span><span class="sxs-lookup"><span data-stu-id="fe791-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="fe791-156">El atributo `[EnableCors]` habilita CORS para los puntos de conexión seleccionados, en lugar de todos los extremos.</span><span class="sxs-lookup"><span data-stu-id="fe791-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="fe791-157">Use `[EnableCors]` para especificar la directiva predeterminada y `[EnableCors("{Policy String}")]` para especificar una directiva.</span><span class="sxs-lookup"><span data-stu-id="fe791-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="fe791-158">El atributo `[EnableCors]` se puede aplicar a:</span><span class="sxs-lookup"><span data-stu-id="fe791-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="fe791-159">Página de Razor `PageModel`</span><span class="sxs-lookup"><span data-stu-id="fe791-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="fe791-160">Controlador</span><span class="sxs-lookup"><span data-stu-id="fe791-160">Controller</span></span>
* <span data-ttu-id="fe791-161">Método de acción del controlador</span><span class="sxs-lookup"><span data-stu-id="fe791-161">Controller action method</span></span>

<span data-ttu-id="fe791-162">Puede aplicar diferentes directivas al controlador/página-modelo/acción con el atributo `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="fe791-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="fe791-163">Cuando se aplica el atributo `[EnableCors]` a un método Controllers/Page-Model/Action y CORS está habilitado en middleware, se aplican ambas directivas.</span><span class="sxs-lookup"><span data-stu-id="fe791-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="fe791-164">Se recomienda no combinar directivas.</span><span class="sxs-lookup"><span data-stu-id="fe791-164">We recommend against combining policies.</span></span> <span data-ttu-id="fe791-165">Use el atributo `[EnableCors]` o middleware, no ambos en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="fe791-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="fe791-166">En el código siguiente se aplica una directiva diferente a cada método:</span><span class="sxs-lookup"><span data-stu-id="fe791-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="fe791-167">En el código siguiente se crea una directiva predeterminada de CORS y una directiva denominada `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="fe791-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="fe791-168">Deshabilitar CORS</span><span class="sxs-lookup"><span data-stu-id="fe791-168">Disable CORS</span></span>

<span data-ttu-id="fe791-169">El atributo [&lbrack;DisableCors @ no__t-2](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) deshabilita CORS para el controlador/página-modelo/acción.</span><span class="sxs-lookup"><span data-stu-id="fe791-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="fe791-170">Opciones de directiva de CORS</span><span class="sxs-lookup"><span data-stu-id="fe791-170">CORS policy options</span></span>

<span data-ttu-id="fe791-171">En esta sección se describen las distintas opciones que se pueden establecer en una directiva de CORS:</span><span class="sxs-lookup"><span data-stu-id="fe791-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="fe791-172">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="fe791-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="fe791-173">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="fe791-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="fe791-174">Establecer los encabezados de solicitud permitidos</span><span class="sxs-lookup"><span data-stu-id="fe791-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="fe791-175">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="fe791-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="fe791-176">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="fe791-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="fe791-177">Establecer la hora de expiración de la comprobación previa</span><span class="sxs-lookup"><span data-stu-id="fe791-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="fe791-178">se llama a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fe791-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fe791-179">Para algunas opciones, puede resultar útil leer la sección [Cómo funciona CORS](#how-cors) en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="fe791-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="fe791-180">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="fe791-180">Set the allowed origins</span></span>

<span data-ttu-id="fe791-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; permite solicitudes de CORS de todos los orígenes con cualquier esquema (@no__t 2 o `https`).</span><span class="sxs-lookup"><span data-stu-id="fe791-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="fe791-182">`AllowAnyOrigin` es inseguro porque *cualquier sitio web* puede realizar solicitudes entre orígenes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fe791-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="fe791-183">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios.</span><span class="sxs-lookup"><span data-stu-id="fe791-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fe791-184">El servicio CORS devuelve una respuesta de CORS no válida cuando se configura una aplicación con ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="fe791-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="fe791-185">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios.</span><span class="sxs-lookup"><span data-stu-id="fe791-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fe791-186">En el caso de una aplicación segura, especifique una lista exacta de orígenes si el cliente debe autorizarse para obtener acceso a los recursos del servidor.</span><span class="sxs-lookup"><span data-stu-id="fe791-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="fe791-187">`AllowAnyOrigin` afecta a las solicitudes preparatorias y al encabezado `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="fe791-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="fe791-188">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fe791-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fe791-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; establece la propiedad <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> de la Directiva para que sea una función que permita que los orígenes coincidan con un dominio comodín configurado al evaluar si se permite el origen.</span><span class="sxs-lookup"><span data-stu-id="fe791-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="fe791-190">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="fe791-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="fe791-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="fe791-192">Permite cualquier método HTTP:</span><span class="sxs-lookup"><span data-stu-id="fe791-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="fe791-193">Afecta a las solicitudes preparatorias y al encabezado `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="fe791-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="fe791-194">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fe791-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="fe791-195">Establecer los encabezados de solicitud permitidos</span><span class="sxs-lookup"><span data-stu-id="fe791-195">Set the allowed request headers</span></span>

<span data-ttu-id="fe791-196">Para permitir el envío de encabezados específicos en una solicitud de CORS, denominados *encabezados de solicitud de autor*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:</span><span class="sxs-lookup"><span data-stu-id="fe791-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fe791-197">Para permitir todos los encabezados de solicitud de autor, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fe791-198">Esta configuración afecta a las solicitudes preparatorias y al encabezado `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fe791-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="fe791-199">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="fe791-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fe791-200">Una coincidencia de directiva de middleware de CORS con encabezados específicos especificados por `WithHeaders` solo es posible cuando los encabezados enviados en `Access-Control-Request-Headers` coinciden exactamente con los encabezados indicados en `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="fe791-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="fe791-201">Por ejemplo, considere una aplicación configurada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="fe791-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fe791-202">Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="fe791-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="fe791-203">La aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fe791-204">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fe791-205">El middleware de CORS siempre permite el envío de cuatro encabezados en el `Access-Control-Request-Headers`, independientemente de los valores configurados en CorsPolicy. Headers.</span><span class="sxs-lookup"><span data-stu-id="fe791-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="fe791-206">Esta lista de encabezados incluye:</span><span class="sxs-lookup"><span data-stu-id="fe791-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="fe791-207">Por ejemplo, considere una aplicación configurada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="fe791-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fe791-208">El middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` siempre está en la lista de permitidos:</span><span class="sxs-lookup"><span data-stu-id="fe791-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="fe791-209">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="fe791-209">Set the exposed response headers</span></span>

<span data-ttu-id="fe791-210">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fe791-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="fe791-211">Para obtener más información, consulte [uso compartido de recursos entre orígenes (terminología) de W3C: encabezado de respuesta simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="fe791-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="fe791-212">Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="fe791-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="fe791-213">La especificación CORS llama a estos encabezados de *respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="fe791-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="fe791-214">Para que otros encabezados estén disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="fe791-215">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="fe791-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="fe791-216">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="fe791-217">De forma predeterminada, el explorador no envía credenciales con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="fe791-218">Las credenciales incluyen cookies y esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe791-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="fe791-219">Para enviar credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` en `true`.</span><span class="sxs-lookup"><span data-stu-id="fe791-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="fe791-220">Usar `XMLHttpRequest` directamente:</span><span class="sxs-lookup"><span data-stu-id="fe791-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="fe791-221">Usar jQuery:</span><span class="sxs-lookup"><span data-stu-id="fe791-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="fe791-222">Uso de la [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="fe791-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="fe791-223">El servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="fe791-223">The server must allow the credentials.</span></span> <span data-ttu-id="fe791-224">Para permitir las credenciales entre orígenes, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="fe791-225">La respuesta HTTP incluye un encabezado `Access-Control-Allow-Credentials`, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="fe791-226">Si el explorador envía credenciales pero la respuesta no incluye un encabezado `Access-Control-Allow-Credentials` válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="fe791-227">Permitir las credenciales entre orígenes es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="fe791-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="fe791-228">Un sitio web en otro dominio puede enviar las credenciales del usuario que ha iniciado sesión a la aplicación en nombre del usuario sin el conocimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="fe791-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="fe791-229">La especificación CORS también indica que el establecimiento de orígenes en `"*"` (todos los orígenes) no es válido si el encabezado `Access-Control-Allow-Credentials` está presente.</span><span class="sxs-lookup"><span data-stu-id="fe791-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="fe791-230">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="fe791-230">Preflight requests</span></span>

<span data-ttu-id="fe791-231">En algunas solicitudes de CORS, el explorador envía una solicitud adicional antes de efectuar la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="fe791-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="fe791-232">Esta solicitud se denomina *solicitud preparatoria*.</span><span class="sxs-lookup"><span data-stu-id="fe791-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="fe791-233">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe791-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="fe791-234">El método de solicitud es GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="fe791-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="fe791-235">La aplicación no establece encabezados de solicitud distintos de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="fe791-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="fe791-236">El encabezado `Content-Type`, si está establecido, tiene uno de los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe791-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="fe791-237">La regla de los encabezados de solicitud establecidos para la solicitud de cliente se aplica a los encabezados que establece la aplicación mediante una llamada a `setRequestHeader` en el objeto `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="fe791-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="fe791-238">La especificación CORS llama a estos encabezados de *solicitud Author*.</span><span class="sxs-lookup"><span data-stu-id="fe791-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="fe791-239">La regla no se aplica a los encabezados que el explorador puede establecer, como `User-Agent`, `Host` o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="fe791-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="fe791-240">El siguiente es un ejemplo de una solicitud preparatoria:</span><span class="sxs-lookup"><span data-stu-id="fe791-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="fe791-241">La solicitud previa al vuelo usa el método HTTP OPTIONs.</span><span class="sxs-lookup"><span data-stu-id="fe791-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="fe791-242">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="fe791-242">It includes two special headers:</span></span>

* <span data-ttu-id="fe791-243">`Access-Control-Request-Method`: método HTTP que se utilizará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="fe791-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="fe791-244">`Access-Control-Request-Headers`: una lista de encabezados de solicitud que la aplicación establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="fe791-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="fe791-245">Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, como `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="fe791-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="fe791-246">Una solicitud preparatoria de CORS podría incluir un encabezado `Access-Control-Request-Headers`, que indica al servidor los encabezados que se envían con la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="fe791-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="fe791-247">Para permitir encabezados específicos, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fe791-248">Para permitir todos los encabezados de solicitud de autor, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fe791-249">Los exploradores no son totalmente coherentes en el modo en que establecen `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fe791-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="fe791-250">Si establece encabezados en un valor distinto de `"*"` (o usa <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type` y `Origin`, además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="fe791-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="fe791-251">A continuación se ofrece un ejemplo de respuesta a la solicitud preparatoria (siempre que el servidor permita la solicitud):</span><span class="sxs-lookup"><span data-stu-id="fe791-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="fe791-252">La respuesta incluye un encabezado `Access-Control-Allow-Methods` que enumera los métodos permitidos y, opcionalmente, un encabezado `Access-Control-Allow-Headers`, que enumera los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="fe791-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="fe791-253">Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="fe791-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="fe791-254">Si se deniega la solicitud preparatoria, la aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fe791-255">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="fe791-256">Establecer la hora de expiración de la comprobación previa</span><span class="sxs-lookup"><span data-stu-id="fe791-256">Set the preflight expiration time</span></span>

<span data-ttu-id="fe791-257">El encabezado `Access-Control-Max-Age` especifica cuánto tiempo se puede almacenar en caché la respuesta a la solicitud preparatoria.</span><span class="sxs-lookup"><span data-stu-id="fe791-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="fe791-258">Para establecer este encabezado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="fe791-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="fe791-259">Cómo funciona CORS</span><span class="sxs-lookup"><span data-stu-id="fe791-259">How CORS works</span></span>

<span data-ttu-id="fe791-260">En esta sección se describe lo que sucede en una solicitud de [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) en el nivel de los mensajes http.</span><span class="sxs-lookup"><span data-stu-id="fe791-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="fe791-261">CORS **no** es una característica de seguridad.</span><span class="sxs-lookup"><span data-stu-id="fe791-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="fe791-262">CORS es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.</span><span class="sxs-lookup"><span data-stu-id="fe791-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="fe791-263">Por ejemplo, un actor malintencionado podría usar [impedir el scripting entre sitios (XSS)](xref:security/cross-site-scripting) en el sitio y ejecutar una solicitud entre sitios a su sitio habilitado para CORS para robar información.</span><span class="sxs-lookup"><span data-stu-id="fe791-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="fe791-264">La API no es más segura al permitir CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="fe791-265">Es el cliente (explorador) el que debe aplicar CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="fe791-266">El servidor ejecuta la solicitud y devuelve la respuesta, es el cliente el que devuelve un error y bloquea la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fe791-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="fe791-267">Por ejemplo, cualquiera de las siguientes herramientas mostrará la respuesta del servidor:</span><span class="sxs-lookup"><span data-stu-id="fe791-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="fe791-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="fe791-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="fe791-269">Postman</span><span class="sxs-lookup"><span data-stu-id="fe791-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="fe791-270">HttpClient de .NET</span><span class="sxs-lookup"><span data-stu-id="fe791-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="fe791-271">Un explorador web escribiendo la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="fe791-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="fe791-272">Es una forma de que un servidor permita que los exploradores ejecuten una solicitud de API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) o [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) entre orígenes que, de lo contrario, se prohibirá.</span><span class="sxs-lookup"><span data-stu-id="fe791-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="fe791-273">Los exploradores (sin CORS) no pueden realizar solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="fe791-274">Antes de CORS, se usaba [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) para eludir esta restricción.</span><span class="sxs-lookup"><span data-stu-id="fe791-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="fe791-275">JSONP no usa XHR, usa la etiqueta `<script>` para recibir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fe791-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="fe791-276">Los scripts pueden cargarse entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="fe791-277">La [especificación CORS](https://www.w3.org/TR/cors/) presentó varios encabezados HTTP nuevos que permiten solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="fe791-278">Si un explorador admite CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="fe791-279">No es necesario el código personalizado de JavaScript para habilitar CORS.</span><span class="sxs-lookup"><span data-stu-id="fe791-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="fe791-280">A continuación se presenta un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="fe791-281">El encabezado `Origin` proporciona el dominio del sitio que realiza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fe791-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="fe791-282">El encabezado `Origin` es obligatorio y debe ser diferente del host.</span><span class="sxs-lookup"><span data-stu-id="fe791-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="fe791-283">Si el servidor permite la solicitud, establece el encabezado `Access-Control-Allow-Origin` en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fe791-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="fe791-284">El valor de este encabezado coincide con el encabezado `Origin` de la solicitud o es el valor de carácter comodín `"*"`, lo que significa que se permite cualquier origen:</span><span class="sxs-lookup"><span data-stu-id="fe791-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="fe791-285">Si la respuesta no incluye el encabezado `Access-Control-Allow-Origin`, se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="fe791-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="fe791-286">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fe791-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="fe791-287">Aunque el servidor devuelva una respuesta correcta, el explorador no pone la respuesta a disposición de la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="fe791-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="fe791-288">Prueba de CORS</span><span class="sxs-lookup"><span data-stu-id="fe791-288">Test CORS</span></span>

<span data-ttu-id="fe791-289">Para probar CORS:</span><span class="sxs-lookup"><span data-stu-id="fe791-289">To test CORS:</span></span>

1. <span data-ttu-id="fe791-290">[Cree un proyecto de API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="fe791-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="fe791-291">Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="fe791-291">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="fe791-292">Habilite CORS con uno de los enfoques de este documento.</span><span class="sxs-lookup"><span data-stu-id="fe791-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="fe791-293">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fe791-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="fe791-294">`WithOrigins("https://localhost:<port>");` solo se debe usar para probar una aplicación de ejemplo similar a la [descarga del código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="fe791-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="fe791-295">Cree un proyecto de aplicación web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="fe791-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="fe791-296">En el ejemplo se utiliza Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fe791-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="fe791-297">Puede crear la aplicación web en la misma solución que el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="fe791-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="fe791-298">Agregue el siguiente código resaltado al archivo *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="fe791-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="fe791-299">En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` por la dirección URL a la aplicación implementada.</span><span class="sxs-lookup"><span data-stu-id="fe791-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="fe791-300">Implemente el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="fe791-300">Deploy the API project.</span></span> <span data-ttu-id="fe791-301">Por ejemplo, [implemente en Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="fe791-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="fe791-302">Ejecute la aplicación Razor Pages o MVC desde el escritorio y haga clic en el botón **probar** .</span><span class="sxs-lookup"><span data-stu-id="fe791-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="fe791-303">Use las herramientas F12 para revisar los mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="fe791-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="fe791-304">Quite el origen localhost de `WithOrigins` e implemente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fe791-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="fe791-305">Como alternativa, ejecute la aplicación cliente con un puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="fe791-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="fe791-306">Por ejemplo, ejecute desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe791-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="fe791-307">Pruebe con la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="fe791-307">Test with the client app.</span></span> <span data-ttu-id="fe791-308">Los errores de CORS devuelven un error, pero el mensaje de error no está disponible para JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fe791-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="fe791-309">Use la pestaña consola de las herramientas F12 para ver el error.</span><span class="sxs-lookup"><span data-stu-id="fe791-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="fe791-310">Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="fe791-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="fe791-311">Uso de Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="fe791-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="fe791-312">**SEC7120: [CORS] el origen `https://localhost:44375` no encontró `https://localhost:44375` en el encabezado de respuesta Access-Control-Allow-Origin para el recurso de origen cruzado en `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="fe791-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="fe791-313">Usar Chrome:</span><span class="sxs-lookup"><span data-stu-id="fe791-313">Using Chrome:</span></span>

     <span data-ttu-id="fe791-314">**La Directiva CORS ha bloqueado el acceso a XMLHttpRequest en `https://webapi.azurewebsites.net/api/values/1` desde el origen `https://localhost:44375`: no hay ningún encabezado "Access-Control-Allow-Origin" presente en el recurso solicitado.**</span><span class="sxs-lookup"><span data-stu-id="fe791-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="fe791-315">Los puntos de conexión habilitados para CORS se pueden probar con una herramienta, como [Fiddler](https://www.telerik.com/fiddler) o [Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="fe791-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="fe791-316">Cuando se usa una herramienta, el origen de la solicitud especificada por el encabezado `Origin` debe ser diferente del host que recibe la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fe791-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="fe791-317">Si la solicitud no es de *origen cruzado* según el valor del encabezado `Origin`:</span><span class="sxs-lookup"><span data-stu-id="fe791-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="fe791-318">No es necesario que el middleware de CORS procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fe791-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="fe791-319">Los encabezados CORS no se devuelven en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fe791-319">CORS headers aren't returned in the response.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe791-320">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fe791-320">Additional resources</span></span>

* [<span data-ttu-id="fe791-321">Uso compartido de recursos entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="fe791-321">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
