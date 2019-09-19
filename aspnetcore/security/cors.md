---
title: Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo CORS como estándar para permitir o rechazar solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a34b77ad799a00707048c923b82b48774ce91682
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108075"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="9775e-103">Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9775e-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="9775e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9775e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9775e-105">En este artículo se muestra cómo habilitar CORS en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9775e-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="9775e-106">La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web.</span><span class="sxs-lookup"><span data-stu-id="9775e-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="9775e-107">Esta restricción se llama *Directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="9775e-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="9775e-108">La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="9775e-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="9775e-109">En ocasiones, es posible que desee permitir que otros sitios realicen solicitudes entre orígenes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9775e-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="9775e-110">Para obtener más información, consulte el [artículo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="9775e-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="9775e-111">[Uso compartido de recursos entre orígenes](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="9775e-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="9775e-112">Es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.</span><span class="sxs-lookup"><span data-stu-id="9775e-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="9775e-113">**No** es una característica de seguridad, CORS reduce la seguridad.</span><span class="sxs-lookup"><span data-stu-id="9775e-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="9775e-114">Una API no es más segura al permitir CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="9775e-115">Para obtener más información, vea [Cómo funciona CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="9775e-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="9775e-116">Permite que un servidor permita explícitamente algunas solicitudes entre orígenes mientras se rechazan otras.</span><span class="sxs-lookup"><span data-stu-id="9775e-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="9775e-117">Es más seguro y más flexible que las técnicas anteriores, como [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="9775e-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="9775e-118">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9775e-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="9775e-119">Mismo origen</span><span class="sxs-lookup"><span data-stu-id="9775e-119">Same origin</span></span>

<span data-ttu-id="9775e-120">Dos direcciones URL tienen el mismo origen si tienen esquemas, hosts y puertos idénticos ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="9775e-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="9775e-121">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="9775e-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="9775e-122">Estas direcciones URL tienen distintos orígenes que las dos direcciones URL anteriores:</span><span class="sxs-lookup"><span data-stu-id="9775e-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="9775e-123">`https://example.net`&ndash; Dominio diferente</span><span class="sxs-lookup"><span data-stu-id="9775e-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="9775e-124">`https://www.example.com/foo.html`&ndash; Subdominio diferente</span><span class="sxs-lookup"><span data-stu-id="9775e-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="9775e-125">`http://example.com/foo.html`&ndash; Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="9775e-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="9775e-126">`https://example.com:9000/foo.html`&ndash; Puerto diferente</span><span class="sxs-lookup"><span data-stu-id="9775e-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="9775e-127">Internet Explorer no tiene en cuenta el puerto al comparar orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="9775e-128">CORS con Directiva con nombre y middleware</span><span class="sxs-lookup"><span data-stu-id="9775e-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="9775e-129">Middleware de CORS controla las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="9775e-130">El código siguiente habilita CORS para toda la aplicación con el origen especificado:</span><span class="sxs-lookup"><span data-stu-id="9775e-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="9775e-131">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="9775e-131">The preceding code:</span></span>

* <span data-ttu-id="9775e-132">Establece el nombre de la directiva\_en "myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="9775e-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="9775e-133">El nombre de la Directiva es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="9775e-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="9775e-134">Llama al <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensión, que habilita CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="9775e-135">Llama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> a con una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="9775e-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="9775e-136">La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="9775e-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="9775e-137">[Las opciones de configuración](#cors-policy-options), `WithOrigins`como, se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="9775e-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="9775e-138">La <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> llamada al método agrega los servicios CORS al contenedor de servicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9775e-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="9775e-139">Para obtener más información, vea [Opciones de directiva de CORS](#cpo) en este documento.</span><span class="sxs-lookup"><span data-stu-id="9775e-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="9775e-140">El <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> método puede encadenar métodos, tal y como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9775e-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="9775e-141">Nota: La dirección URL **no** debe contener una barra diagonal final`/`().</span><span class="sxs-lookup"><span data-stu-id="9775e-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="9775e-142">Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="9775e-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9775e-143">El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="9775e-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="9775e-144">Con el enrutamiento de punto de conexión, el middleware de CORS debe estar configurado para `UseRouting` ejecutarse entre las llamadas a y. `UseEndpoints`</span><span class="sxs-lookup"><span data-stu-id="9775e-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="9775e-145">Una configuración incorrecta hará que el middleware deje de funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="9775e-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="9775e-146">El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="9775e-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="9775e-147">Nota: `UseCors` se debe llamar a `UseMvc`antes de.</span><span class="sxs-lookup"><span data-stu-id="9775e-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="9775e-148">Consulte [habilitación de CORS en Razor pages, controladores y métodos de acción](#ecors) para aplicar la Directiva CORS en el nivel de página/controlador/acción.</span><span class="sxs-lookup"><span data-stu-id="9775e-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="9775e-149">Consulte [prueba de CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.</span><span class="sxs-lookup"><span data-stu-id="9775e-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="9775e-150">Habilitación de CORS con enrutamiento de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="9775e-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="9775e-151">Con el enrutamiento de punto de conexión, CORS se puede habilitar en cada punto de `RequireCors` conexión mediante el conjunto de métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="9775e-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="9775e-152">Del mismo modo, CORS también puede habilitarse para todos los controladores:</span><span class="sxs-lookup"><span data-stu-id="9775e-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="9775e-153">Habilitación de CORS con atributos</span><span class="sxs-lookup"><span data-stu-id="9775e-153">Enable CORS with attributes</span></span>

<span data-ttu-id="9775e-154">El [ &lbrack;atributoEnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) proporciona una alternativa a la aplicación de CORS globalmente.</span><span class="sxs-lookup"><span data-stu-id="9775e-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="9775e-155">El `[EnableCors]` atributo habilita CORS para los puntos de conexión seleccionados, en lugar de todos los extremos.</span><span class="sxs-lookup"><span data-stu-id="9775e-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="9775e-156">Se `[EnableCors]` usa para especificar la directiva predeterminada `[EnableCors("{Policy String}")]` y para especificar una directiva.</span><span class="sxs-lookup"><span data-stu-id="9775e-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="9775e-157">El `[EnableCors]` atributo se puede aplicar a:</span><span class="sxs-lookup"><span data-stu-id="9775e-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="9775e-158">Página de Razor`PageModel`</span><span class="sxs-lookup"><span data-stu-id="9775e-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="9775e-159">Responsable del tratamiento de datos</span><span class="sxs-lookup"><span data-stu-id="9775e-159">Controller</span></span>
* <span data-ttu-id="9775e-160">Método de acción del controlador</span><span class="sxs-lookup"><span data-stu-id="9775e-160">Controller action method</span></span>

<span data-ttu-id="9775e-161">Puede aplicar diferentes directivas al controlador/página-modelo o acción con el `[EnableCors]` atributo.</span><span class="sxs-lookup"><span data-stu-id="9775e-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="9775e-162">Cuando el `[EnableCors]` atributo se aplica a un método Controllers/Page-Model/Action y CORS está habilitado en middleware, se aplican ambas directivas.</span><span class="sxs-lookup"><span data-stu-id="9775e-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="9775e-163">Se recomienda no combinar directivas.</span><span class="sxs-lookup"><span data-stu-id="9775e-163">We recommend against combining policies.</span></span> <span data-ttu-id="9775e-164">Use el `[EnableCors]` atributo o middleware, no ambos en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="9775e-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="9775e-165">En el código siguiente se aplica una directiva diferente a cada método:</span><span class="sxs-lookup"><span data-stu-id="9775e-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="9775e-166">En el código siguiente se crea una directiva predeterminada de CORS y `"AnotherPolicy"`una directiva denominada:</span><span class="sxs-lookup"><span data-stu-id="9775e-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="9775e-167">Deshabilitar CORS</span><span class="sxs-lookup"><span data-stu-id="9775e-167">Disable CORS</span></span>

<span data-ttu-id="9775e-168">[ El&lbrack;atributoDisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) deshabilita CORS para el controlador/página-modelo/acción.</span><span class="sxs-lookup"><span data-stu-id="9775e-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="9775e-169">Opciones de directiva de CORS</span><span class="sxs-lookup"><span data-stu-id="9775e-169">CORS policy options</span></span>

<span data-ttu-id="9775e-170">En esta sección se describen las distintas opciones que se pueden establecer en una directiva de CORS:</span><span class="sxs-lookup"><span data-stu-id="9775e-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="9775e-171">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="9775e-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="9775e-172">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="9775e-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="9775e-173">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="9775e-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="9775e-174">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="9775e-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="9775e-175">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="9775e-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="9775e-176">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="9775e-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="9775e-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>se llama a `Startup.ConfigureServices`en.</span><span class="sxs-lookup"><span data-stu-id="9775e-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9775e-178">Para algunas opciones, puede resultar útil leer la sección [Cómo funciona CORS](#how-cors) en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="9775e-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="9775e-179">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="9775e-179">Set the allowed origins</span></span>

<span data-ttu-id="9775e-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Permite solicitudes de CORS de todos los orígenes con cualquier`http` esquema `https`(o). &ndash;</span><span class="sxs-lookup"><span data-stu-id="9775e-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="9775e-181">`AllowAnyOrigin`no es seguro porque *cualquier sitio web* puede realizar solicitudes entre orígenes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9775e-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="9775e-182">`AllowAnyOrigin` Especificar y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios.</span><span class="sxs-lookup"><span data-stu-id="9775e-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="9775e-183">El servicio CORS devuelve una respuesta de CORS no válida cuando se configura una aplicación con ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="9775e-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="9775e-184">`AllowAnyOrigin` Especificar y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios.</span><span class="sxs-lookup"><span data-stu-id="9775e-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="9775e-185">En el caso de una aplicación segura, especifique una lista exacta de orígenes si el cliente debe autorizarse para obtener acceso a los recursos del servidor.</span><span class="sxs-lookup"><span data-stu-id="9775e-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="9775e-186">`AllowAnyOrigin`afecta a las solicitudes preparatorias `Access-Control-Allow-Origin` y al encabezado.</span><span class="sxs-lookup"><span data-stu-id="9775e-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="9775e-187">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="9775e-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9775e-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Establece la<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propiedad de la Directiva para que sea una función que permita que los orígenes coincidan con un dominio comodín configurado al evaluar si se permite el origen.</span><span class="sxs-lookup"><span data-stu-id="9775e-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="9775e-189">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="9775e-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="9775e-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="9775e-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="9775e-191">Permite cualquier método HTTP:</span><span class="sxs-lookup"><span data-stu-id="9775e-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="9775e-192">Afecta a las solicitudes preparatorias `Access-Control-Allow-Methods` y al encabezado.</span><span class="sxs-lookup"><span data-stu-id="9775e-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="9775e-193">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="9775e-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="9775e-194">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="9775e-194">Set the allowed request headers</span></span>

<span data-ttu-id="9775e-195">Para permitir el envío de encabezados específicos en una solicitud de CORS, denominados *encabezados de solicitud*de autor <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> , llame a y especifique los encabezados permitidos:</span><span class="sxs-lookup"><span data-stu-id="9775e-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="9775e-196">Para permitir todos los encabezados de solicitud de autor <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>, llame a:</span><span class="sxs-lookup"><span data-stu-id="9775e-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="9775e-197">Esta configuración afecta a las solicitudes preparatorias `Access-Control-Request-Headers` y al encabezado.</span><span class="sxs-lookup"><span data-stu-id="9775e-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="9775e-198">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="9775e-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9775e-199">Una coincidencia de directiva de middleware de CORS con encabezados específicos `WithHeaders` especificados por solo es posible cuando los encabezados `Access-Control-Request-Headers` enviados coinciden exactamente con los encabezados `WithHeaders`indicados en.</span><span class="sxs-lookup"><span data-stu-id="9775e-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="9775e-200">Por ejemplo, considere una aplicación configurada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="9775e-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="9775e-201">Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="9775e-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="9775e-202">La aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="9775e-203">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="9775e-204">El middleware de CORS siempre permite que se envíen cuatro encabezados en el `Access-Control-Request-Headers` , independientemente de los valores configurados en CorsPolicy. Headers.</span><span class="sxs-lookup"><span data-stu-id="9775e-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="9775e-205">Esta lista de encabezados incluye:</span><span class="sxs-lookup"><span data-stu-id="9775e-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="9775e-206">Por ejemplo, considere una aplicación configurada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="9775e-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="9775e-207">El middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud `Content-Language` porque siempre está en la lista de permitidos:</span><span class="sxs-lookup"><span data-stu-id="9775e-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="9775e-208">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="9775e-208">Set the exposed response headers</span></span>

<span data-ttu-id="9775e-209">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9775e-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="9775e-210">Para obtener más información, [consulte uso compartido de recursos entre orígenes de W3C (terminología): Encabezado](https://www.w3.org/TR/cors/#simple-response-header)de respuesta simple.</span><span class="sxs-lookup"><span data-stu-id="9775e-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="9775e-211">Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="9775e-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="9775e-212">La especificación CORS llama a estos encabezados de *respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="9775e-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="9775e-213">Para que otros encabezados estén disponibles para la aplicación, llame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>a:</span><span class="sxs-lookup"><span data-stu-id="9775e-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="9775e-214">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="9775e-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="9775e-215">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="9775e-216">De forma predeterminada, el explorador no envía credenciales con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="9775e-217">Las credenciales incluyen cookies y esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="9775e-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="9775e-218">Para enviar credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` en `true`.</span><span class="sxs-lookup"><span data-stu-id="9775e-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="9775e-219">Usar `XMLHttpRequest` directamente:</span><span class="sxs-lookup"><span data-stu-id="9775e-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="9775e-220">Usar jQuery:</span><span class="sxs-lookup"><span data-stu-id="9775e-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="9775e-221">Uso de la [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="9775e-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="9775e-222">El servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="9775e-222">The server must allow the credentials.</span></span> <span data-ttu-id="9775e-223">Para permitir las credenciales entre orígenes, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>llame a:</span><span class="sxs-lookup"><span data-stu-id="9775e-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="9775e-224">La respuesta http incluye un `Access-Control-Allow-Credentials` encabezado, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="9775e-225">Si el explorador envía credenciales pero la respuesta no incluye un `Access-Control-Allow-Credentials` encabezado válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="9775e-226">Permitir las credenciales entre orígenes es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="9775e-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="9775e-227">Un sitio web en otro dominio puede enviar las credenciales del usuario que ha iniciado sesión a la aplicación en nombre del usuario sin el conocimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="9775e-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="9775e-228">La especificación CORS también indica que el establecimiento de `"*"` orígenes en (todos los orígenes) no `Access-Control-Allow-Credentials` es válido si el encabezado está presente.</span><span class="sxs-lookup"><span data-stu-id="9775e-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="9775e-229">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="9775e-229">Preflight requests</span></span>

<span data-ttu-id="9775e-230">En algunas solicitudes de CORS, el explorador envía una solicitud adicional antes de efectuar la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="9775e-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="9775e-231">Esta solicitud se denomina *solicitud preparatoria*.</span><span class="sxs-lookup"><span data-stu-id="9775e-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="9775e-232">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="9775e-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="9775e-233">El método de solicitud es GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="9775e-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="9775e-234">La aplicación no establece encabezados de solicitud distintos de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="9775e-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="9775e-235">El `Content-Type` encabezado, si está establecido, tiene uno de los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="9775e-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="9775e-236">La regla de los encabezados de solicitud establecidos para la solicitud de cliente se aplica a los encabezados que establece la `setRequestHeader` aplicación mediante `XMLHttpRequest` una llamada a en el objeto.</span><span class="sxs-lookup"><span data-stu-id="9775e-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="9775e-237">La especificación CORS llama a estos encabezados de *solicitud Author*.</span><span class="sxs-lookup"><span data-stu-id="9775e-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="9775e-238">La regla no se aplica a los encabezados que el explorador puede establecer, `User-Agent`como `Host`, o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="9775e-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="9775e-239">El siguiente es un ejemplo de una solicitud preparatoria:</span><span class="sxs-lookup"><span data-stu-id="9775e-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="9775e-240">La solicitud previa al vuelo usa el método HTTP OPTIONs.</span><span class="sxs-lookup"><span data-stu-id="9775e-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="9775e-241">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="9775e-241">It includes two special headers:</span></span>

* <span data-ttu-id="9775e-242">`Access-Control-Request-Method`: Método HTTP que se utilizará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="9775e-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="9775e-243">`Access-Control-Request-Headers`: Una lista de encabezados de solicitud que la aplicación establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="9775e-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="9775e-244">Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, `User-Agent`como.</span><span class="sxs-lookup"><span data-stu-id="9775e-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="9775e-245">Una solicitud preparatoria de CORS podría incluir un `Access-Control-Request-Headers` encabezado, que indica al servidor los encabezados que se envían con la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="9775e-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="9775e-246">Para permitir encabezados específicos, llame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>a:</span><span class="sxs-lookup"><span data-stu-id="9775e-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="9775e-247">Para permitir todos los encabezados de solicitud de autor <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>, llame a:</span><span class="sxs-lookup"><span data-stu-id="9775e-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="9775e-248">Los exploradores no son totalmente coherentes en `Access-Control-Request-Headers`el modo en que se establecen.</span><span class="sxs-lookup"><span data-stu-id="9775e-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="9775e-249">Si establece encabezados en un valor `"*"` distinto de (o use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`y `Origin`, además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="9775e-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="9775e-250">A continuación se ofrece un ejemplo de respuesta a la solicitud preparatoria (siempre que el servidor permita la solicitud):</span><span class="sxs-lookup"><span data-stu-id="9775e-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="9775e-251">La respuesta incluye un `Access-Control-Allow-Methods` encabezado que enumera los métodos permitidos y, opcionalmente, un `Access-Control-Allow-Headers` encabezado, que enumera los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="9775e-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="9775e-252">Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="9775e-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="9775e-253">Si se deniega la solicitud preparatoria, la aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="9775e-254">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="9775e-255">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="9775e-255">Set the preflight expiration time</span></span>

<span data-ttu-id="9775e-256">El `Access-Control-Max-Age` encabezado especifica cuánto tiempo se puede almacenar en caché la respuesta a la solicitud preparatoria.</span><span class="sxs-lookup"><span data-stu-id="9775e-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="9775e-257">Para establecer este encabezado, llame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>a:</span><span class="sxs-lookup"><span data-stu-id="9775e-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="9775e-258">Cómo funciona CORS</span><span class="sxs-lookup"><span data-stu-id="9775e-258">How CORS works</span></span>

<span data-ttu-id="9775e-259">En esta sección se describe lo que sucede en una solicitud de [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) en el nivel de los mensajes http.</span><span class="sxs-lookup"><span data-stu-id="9775e-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="9775e-260">CORS **no** es una característica de seguridad.</span><span class="sxs-lookup"><span data-stu-id="9775e-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="9775e-261">CORS es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.</span><span class="sxs-lookup"><span data-stu-id="9775e-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="9775e-262">Por ejemplo, un actor malintencionado podría usar [impedir el scripting entre sitios (XSS)](xref:security/cross-site-scripting) en el sitio y ejecutar una solicitud entre sitios a su sitio habilitado para CORS para robar información.</span><span class="sxs-lookup"><span data-stu-id="9775e-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="9775e-263">La API no es más segura al permitir CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="9775e-264">Es el cliente (explorador) el que debe aplicar CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="9775e-265">El servidor ejecuta la solicitud y devuelve la respuesta, es el cliente el que devuelve un error y bloquea la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9775e-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="9775e-266">Por ejemplo, cualquiera de las siguientes herramientas mostrará la respuesta del servidor:</span><span class="sxs-lookup"><span data-stu-id="9775e-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="9775e-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="9775e-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="9775e-268">Postman</span><span class="sxs-lookup"><span data-stu-id="9775e-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="9775e-269">HttpClient de .NET</span><span class="sxs-lookup"><span data-stu-id="9775e-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="9775e-270">Un explorador web escribiendo la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="9775e-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="9775e-271">Es una forma de que un servidor permita que los exploradores ejecuten una solicitud de API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) o [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) entre orígenes que, de lo contrario, se prohibirá.</span><span class="sxs-lookup"><span data-stu-id="9775e-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="9775e-272">Los exploradores (sin CORS) no pueden realizar solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="9775e-273">Antes de CORS, se usaba [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) para eludir esta restricción.</span><span class="sxs-lookup"><span data-stu-id="9775e-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="9775e-274">JSONP no usa XHR, usa la `<script>` etiqueta para recibir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9775e-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="9775e-275">Los scripts pueden cargarse entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="9775e-276">La [especificación CORS](https://www.w3.org/TR/cors/) presentó varios encabezados HTTP nuevos que permiten solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="9775e-277">Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="9775e-278">No se necesita código JavaScript personalizado para habilitar CORS.</span><span class="sxs-lookup"><span data-stu-id="9775e-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="9775e-279">A continuación se presenta un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="9775e-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="9775e-280">El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:</span><span class="sxs-lookup"><span data-stu-id="9775e-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="9775e-281">Si el servidor permite la solicitud, establece el `Access-Control-Allow-Origin` encabezado en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9775e-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="9775e-282">El valor de este encabezado coincide con el `Origin` encabezado de la solicitud o con el valor `"*"`comodín, lo que significa que se permite cualquier origen:</span><span class="sxs-lookup"><span data-stu-id="9775e-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="9775e-283">Si la respuesta no incluye el `Access-Control-Allow-Origin` encabezado, se produce un error en la solicitud de origen cruzado.</span><span class="sxs-lookup"><span data-stu-id="9775e-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="9775e-284">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9775e-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="9775e-285">Aunque el servidor devuelva una respuesta correcta, el explorador no pone la respuesta a disposición de la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="9775e-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="9775e-286">Prueba de CORS</span><span class="sxs-lookup"><span data-stu-id="9775e-286">Test CORS</span></span>

<span data-ttu-id="9775e-287">Para probar CORS:</span><span class="sxs-lookup"><span data-stu-id="9775e-287">To test CORS:</span></span>

1. <span data-ttu-id="9775e-288">[Cree un proyecto de API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="9775e-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="9775e-289">Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="9775e-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="9775e-290">Habilite CORS con uno de los enfoques de este documento.</span><span class="sxs-lookup"><span data-stu-id="9775e-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="9775e-291">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9775e-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="9775e-292">`WithOrigins("https://localhost:<port>");`solo se debe usar para probar una aplicación de ejemplo similar a la [descarga del código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="9775e-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="9775e-293">Cree un proyecto de aplicación web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="9775e-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="9775e-294">En el ejemplo se utiliza Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9775e-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="9775e-295">Puede crear la aplicación web en la misma solución que el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="9775e-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="9775e-296">Agregue el siguiente código resaltado al archivo *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9775e-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="9775e-297">En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` por la dirección URL a la aplicación implementada.</span><span class="sxs-lookup"><span data-stu-id="9775e-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="9775e-298">Implemente el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="9775e-298">Deploy the API project.</span></span> <span data-ttu-id="9775e-299">Por ejemplo, [implemente en Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="9775e-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="9775e-300">Ejecute la aplicación Razor Pages o MVC desde el escritorio y haga clic en el botón **probar** .</span><span class="sxs-lookup"><span data-stu-id="9775e-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="9775e-301">Use las herramientas F12 para revisar los mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="9775e-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="9775e-302">Quite el origen localhost de `WithOrigins` e implemente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9775e-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="9775e-303">Como alternativa, ejecute la aplicación cliente con un puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="9775e-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="9775e-304">Por ejemplo, ejecute desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9775e-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="9775e-305">Pruebe con la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="9775e-305">Test with the client app.</span></span> <span data-ttu-id="9775e-306">Los errores de CORS devuelven un error, pero el mensaje de error no está disponible para JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9775e-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="9775e-307">Use la pestaña consola de las herramientas F12 para ver el error.</span><span class="sxs-lookup"><span data-stu-id="9775e-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="9775e-308">Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9775e-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="9775e-309">Uso de Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="9775e-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="9775e-310">**SEC7120: [CORS] el origen `https://localhost:44375` no se encontró `https://localhost:44375` en el encabezado de respuesta Access-Control-Allow-Origin para el recurso entre orígenes en`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="9775e-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="9775e-311">Usar Chrome:</span><span class="sxs-lookup"><span data-stu-id="9775e-311">Using Chrome:</span></span>

     <span data-ttu-id="9775e-312">**La Directiva CORS ha `https://webapi.azurewebsites.net/api/values/1` bloqueado el `https://localhost:44375` acceso a XMLHttpRequest desde el origen. No existe ningún encabezado "Access-Control-Allow-Origin" en el recurso solicitado.**</span><span class="sxs-lookup"><span data-stu-id="9775e-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9775e-313">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9775e-313">Additional resources</span></span>

* [<span data-ttu-id="9775e-314">Uso compartido de recursos entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="9775e-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
