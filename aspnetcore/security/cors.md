---
title: Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo CORS como estándar para permitir o rechazar solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 57098be73164c71d1b0d1fe2f3aee7ec41a32346
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727312"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="d0272-103">Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0272-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="d0272-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0272-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0272-105">En este artículo se muestra cómo habilitar CORS en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0272-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="d0272-106">La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web.</span><span class="sxs-lookup"><span data-stu-id="d0272-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="d0272-107">Esta restricción se llama *Directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="d0272-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="d0272-108">La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="d0272-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="d0272-109">En ocasiones, es posible que desee permitir que otros sitios realicen solicitudes entre orígenes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0272-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="d0272-110">Para obtener más información, consulte el [artículo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="d0272-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="d0272-111">[Uso compartido de recursos entre orígenes](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="d0272-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="d0272-112">Es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.</span><span class="sxs-lookup"><span data-stu-id="d0272-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="d0272-113">**No** es una característica de seguridad, CORS reduce la seguridad.</span><span class="sxs-lookup"><span data-stu-id="d0272-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="d0272-114">Una API no es más segura al permitir CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="d0272-115">Para obtener más información, vea [Cómo funciona CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="d0272-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="d0272-116">Permite que un servidor permita explícitamente algunas solicitudes entre orígenes mientras se rechazan otras.</span><span class="sxs-lookup"><span data-stu-id="d0272-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="d0272-117">Es más seguro y más flexible que las técnicas anteriores, como [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="d0272-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="d0272-118">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d0272-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="d0272-119">Mismo origen</span><span class="sxs-lookup"><span data-stu-id="d0272-119">Same origin</span></span>

<span data-ttu-id="d0272-120">Dos direcciones URL tienen el mismo origen si tienen esquemas, hosts y puertos idénticos ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="d0272-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="d0272-121">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="d0272-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="d0272-122">Estas direcciones URL tienen distintos orígenes que las dos direcciones URL anteriores:</span><span class="sxs-lookup"><span data-stu-id="d0272-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="d0272-123">`https://example.net` &ndash; dominio diferente</span><span class="sxs-lookup"><span data-stu-id="d0272-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="d0272-124">`https://www.example.com/foo.html` &ndash; subdominio diferente</span><span class="sxs-lookup"><span data-stu-id="d0272-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="d0272-125">`http://example.com/foo.html` &ndash; esquema diferente</span><span class="sxs-lookup"><span data-stu-id="d0272-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="d0272-126">`https://example.com:9000/foo.html` &ndash; puerto diferente</span><span class="sxs-lookup"><span data-stu-id="d0272-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="d0272-127">Internet Explorer no tiene en cuenta el puerto al comparar orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="d0272-128">CORS con Directiva con nombre y middleware</span><span class="sxs-lookup"><span data-stu-id="d0272-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="d0272-129">Middleware de CORS controla las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="d0272-130">El código siguiente habilita CORS para toda la aplicación con el origen especificado:</span><span class="sxs-lookup"><span data-stu-id="d0272-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="d0272-131">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="d0272-131">The preceding code:</span></span>

* <span data-ttu-id="d0272-132">Establece el nombre de la Directiva en "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="d0272-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="d0272-133">El nombre de la Directiva es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="d0272-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="d0272-134">Llama al método de extensión <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, que habilita CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="d0272-135">Llama a <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="d0272-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d0272-136">La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d0272-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="d0272-137">[Las opciones de configuración](#cors-policy-options), como `WithOrigins`, se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="d0272-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="d0272-138">La llamada al método <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> agrega los servicios CORS al contenedor de servicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d0272-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="d0272-139">Para obtener más información, vea [Opciones de directiva de CORS](#cpo) en este documento.</span><span class="sxs-lookup"><span data-stu-id="d0272-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="d0272-140">El método <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> puede encadenar métodos, tal y como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0272-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="d0272-141">Nota: la dirección URL **no** debe contener una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="d0272-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="d0272-142">Si la dirección URL finaliza con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="d0272-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="d0272-143">Aplicar directivas CORS a todos los puntos de conexión</span><span class="sxs-lookup"><span data-stu-id="d0272-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="d0272-144">El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="d0272-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="d0272-145">Con el enrutamiento de punto de conexión, el middleware de CORS debe estar configurado para ejecutarse entre las llamadas a `UseRouting` y `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="d0272-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="d0272-146">Una configuración incorrecta hará que el middleware deje de funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="d0272-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="d0272-147">El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="d0272-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="d0272-148">Nota: se debe llamar a `UseCors` antes de `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="d0272-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="d0272-149">Consulte [habilitación de CORS en Razor pages, controladores y métodos de acción](#ecors) para aplicar la Directiva CORS en el nivel de página/controlador/acción.</span><span class="sxs-lookup"><span data-stu-id="d0272-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="d0272-150">Consulte [prueba de CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.</span><span class="sxs-lookup"><span data-stu-id="d0272-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="d0272-151">Habilitación de CORS con enrutamiento de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="d0272-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="d0272-152">Con el enrutamiento de punto de conexión, CORS se puede habilitar en cada punto de conexión mediante el `RequireCors` conjunto de métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="d0272-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="d0272-153">Del mismo modo, CORS también puede habilitarse para todos los controladores:</span><span class="sxs-lookup"><span data-stu-id="d0272-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="d0272-154">Habilitación de CORS con atributos</span><span class="sxs-lookup"><span data-stu-id="d0272-154">Enable CORS with attributes</span></span>

<span data-ttu-id="d0272-155">El atributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) proporciona una alternativa a la aplicación de CORS globalmente.</span><span class="sxs-lookup"><span data-stu-id="d0272-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="d0272-156">El atributo `[EnableCors]` habilita CORS para los puntos de conexión seleccionados, en lugar de todos los extremos.</span><span class="sxs-lookup"><span data-stu-id="d0272-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="d0272-157">Use `[EnableCors]` para especificar la directiva predeterminada y `[EnableCors("{Policy String}")]` para especificar una directiva.</span><span class="sxs-lookup"><span data-stu-id="d0272-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="d0272-158">El atributo `[EnableCors]` se puede aplicar a:</span><span class="sxs-lookup"><span data-stu-id="d0272-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="d0272-159">`PageModel` de página de Razor</span><span class="sxs-lookup"><span data-stu-id="d0272-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="d0272-160">Controlador</span><span class="sxs-lookup"><span data-stu-id="d0272-160">Controller</span></span>
* <span data-ttu-id="d0272-161">Método de acción del controlador</span><span class="sxs-lookup"><span data-stu-id="d0272-161">Controller action method</span></span>

<span data-ttu-id="d0272-162">Puede aplicar diferentes directivas al controlador/página-modelo/acción con el atributo `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="d0272-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="d0272-163">Cuando el atributo `[EnableCors]` se aplica a un método Controllers/Page-Model/Action y CORS está habilitado en middleware, se aplican ambas directivas.</span><span class="sxs-lookup"><span data-stu-id="d0272-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="d0272-164">Se recomienda no combinar directivas.</span><span class="sxs-lookup"><span data-stu-id="d0272-164">We recommend against combining policies.</span></span> <span data-ttu-id="d0272-165">Use el `[EnableCors]` atributo o middleware, no ambos en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0272-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="d0272-166">En el código siguiente se aplica una directiva diferente a cada método:</span><span class="sxs-lookup"><span data-stu-id="d0272-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="d0272-167">En el código siguiente se crea una directiva predeterminada de CORS y una directiva denominada `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="d0272-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="d0272-168">Deshabilitar CORS</span><span class="sxs-lookup"><span data-stu-id="d0272-168">Disable CORS</span></span>

<span data-ttu-id="d0272-169">El atributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) deshabilita CORS para el controlador/página-modelo/acción.</span><span class="sxs-lookup"><span data-stu-id="d0272-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="d0272-170">Opciones de directiva de CORS</span><span class="sxs-lookup"><span data-stu-id="d0272-170">CORS policy options</span></span>

<span data-ttu-id="d0272-171">En esta sección se describen las distintas opciones que se pueden establecer en una directiva de CORS:</span><span class="sxs-lookup"><span data-stu-id="d0272-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="d0272-172">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="d0272-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="d0272-173">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="d0272-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="d0272-174">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="d0272-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="d0272-175">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="d0272-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="d0272-176">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="d0272-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="d0272-177">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="d0272-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="d0272-178">se llama a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d0272-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d0272-179">Para algunas opciones, puede resultar útil leer la sección [Cómo funciona CORS](#how-cors) en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="d0272-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="d0272-180">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="d0272-180">Set the allowed origins</span></span>

<span data-ttu-id="d0272-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; permite solicitudes de CORS desde todos los orígenes con cualquier esquema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="d0272-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="d0272-182">`AllowAnyOrigin` es inseguro porque *cualquier sitio web* puede realizar solicitudes entre orígenes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0272-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="d0272-183">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios.</span><span class="sxs-lookup"><span data-stu-id="d0272-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="d0272-184">El servicio CORS devuelve una respuesta de CORS no válida cuando se configura una aplicación con ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="d0272-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="d0272-185">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios.</span><span class="sxs-lookup"><span data-stu-id="d0272-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="d0272-186">En el caso de una aplicación segura, especifique una lista exacta de orígenes si el cliente debe autorizarse para obtener acceso a los recursos del servidor.</span><span class="sxs-lookup"><span data-stu-id="d0272-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="d0272-187">`AllowAnyOrigin` afecta a las solicitudes preparatorias y al encabezado `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="d0272-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="d0272-188">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="d0272-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d0272-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; establece la propiedad <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> de la Directiva para que sea una función que permita que los orígenes coincidan con un dominio comodín configurado al evaluar si se permite el origen.</span><span class="sxs-lookup"><span data-stu-id="d0272-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="d0272-190">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="d0272-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="d0272-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="d0272-192">Permite cualquier método HTTP:</span><span class="sxs-lookup"><span data-stu-id="d0272-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="d0272-193">Afecta a las solicitudes preparatorias y al encabezado `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="d0272-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="d0272-194">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="d0272-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="d0272-195">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="d0272-195">Set the allowed request headers</span></span>

<span data-ttu-id="d0272-196">Para permitir el envío de encabezados específicos en una solicitud de CORS, denominados *encabezados de solicitud de autor*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:</span><span class="sxs-lookup"><span data-stu-id="d0272-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="d0272-197">Para permitir todos los encabezados de solicitud de autor, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="d0272-198">Esta configuración afecta a las solicitudes preparatorias y al encabezado `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="d0272-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="d0272-199">Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="d0272-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d0272-200">Una coincidencia de directiva de middleware de CORS con encabezados específicos especificados por `WithHeaders` solo es posible cuando los encabezados enviados en `Access-Control-Request-Headers` coinciden exactamente con los encabezados indicados en `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="d0272-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="d0272-201">Por ejemplo, considere una aplicación configurada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="d0272-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="d0272-202">Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="d0272-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="d0272-203">La aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="d0272-204">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d0272-205">El middleware de CORS siempre permite el envío de cuatro encabezados en el `Access-Control-Request-Headers`, independientemente de los valores configurados en CorsPolicy. Headers.</span><span class="sxs-lookup"><span data-stu-id="d0272-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="d0272-206">Esta lista de encabezados incluye:</span><span class="sxs-lookup"><span data-stu-id="d0272-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="d0272-207">Por ejemplo, considere una aplicación configurada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="d0272-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="d0272-208">Middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` siempre está en la lista de permitidos:</span><span class="sxs-lookup"><span data-stu-id="d0272-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="d0272-209">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="d0272-209">Set the exposed response headers</span></span>

<span data-ttu-id="d0272-210">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0272-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="d0272-211">Para obtener más información, consulte [uso compartido de recursos entre orígenes (terminología) de W3C: encabezado de respuesta simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="d0272-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="d0272-212">Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="d0272-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="d0272-213">La especificación CORS llama a estos encabezados de *respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="d0272-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="d0272-214">Para que otros encabezados estén disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="d0272-215">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="d0272-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="d0272-216">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="d0272-217">De forma predeterminada, el explorador no envía credenciales con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="d0272-218">Las credenciales incluyen cookies y esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0272-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="d0272-219">Para enviar credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` en `true`.</span><span class="sxs-lookup"><span data-stu-id="d0272-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="d0272-220">Usar `XMLHttpRequest` directamente:</span><span class="sxs-lookup"><span data-stu-id="d0272-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="d0272-221">Usar jQuery:</span><span class="sxs-lookup"><span data-stu-id="d0272-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="d0272-222">Uso de la [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="d0272-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="d0272-223">El servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="d0272-223">The server must allow the credentials.</span></span> <span data-ttu-id="d0272-224">Para permitir las credenciales entre orígenes, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="d0272-225">La respuesta HTTP incluye un encabezado `Access-Control-Allow-Credentials`, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="d0272-226">Si el explorador envía credenciales pero la respuesta no incluye un encabezado de `Access-Control-Allow-Credentials` válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="d0272-227">Permitir las credenciales entre orígenes es un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="d0272-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="d0272-228">Un sitio web en otro dominio puede enviar las credenciales del usuario que ha iniciado sesión a la aplicación en nombre del usuario sin el conocimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="d0272-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="d0272-229">La especificación CORS también indica que el establecimiento de orígenes en `"*"` (todos los orígenes) no es válido si el encabezado `Access-Control-Allow-Credentials` está presente.</span><span class="sxs-lookup"><span data-stu-id="d0272-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="d0272-230">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="d0272-230">Preflight requests</span></span>

<span data-ttu-id="d0272-231">En algunas solicitudes de CORS, el explorador envía una solicitud adicional antes de efectuar la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="d0272-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="d0272-232">Esta solicitud se denomina *solicitud preparatoria*.</span><span class="sxs-lookup"><span data-stu-id="d0272-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="d0272-233">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d0272-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="d0272-234">El método de solicitud es GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="d0272-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="d0272-235">La aplicación no establece encabezados de solicitud distintos de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="d0272-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="d0272-236">El encabezado `Content-Type`, si se establece, tiene uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="d0272-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="d0272-237">La regla de los encabezados de solicitud establecidos para la solicitud de cliente se aplica a los encabezados que establece la aplicación mediante una llamada a `setRequestHeader` en el objeto `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="d0272-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="d0272-238">La especificación CORS llama a estos encabezados de *solicitud Author*.</span><span class="sxs-lookup"><span data-stu-id="d0272-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="d0272-239">La regla no se aplica a los encabezados que el explorador puede establecer, como `User-Agent`, `Host`o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="d0272-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="d0272-240">El siguiente es un ejemplo de una solicitud preparatoria:</span><span class="sxs-lookup"><span data-stu-id="d0272-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="d0272-241">La solicitud previa al vuelo usa el método HTTP OPTIONs.</span><span class="sxs-lookup"><span data-stu-id="d0272-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="d0272-242">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="d0272-242">It includes two special headers:</span></span>

* <span data-ttu-id="d0272-243">`Access-Control-Request-Method`: método HTTP que se utilizará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="d0272-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="d0272-244">`Access-Control-Request-Headers`: una lista de encabezados de solicitud que la aplicación establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="d0272-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="d0272-245">Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, como `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="d0272-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="d0272-246">Una solicitud preparatoria de CORS podría incluir un encabezado `Access-Control-Request-Headers`, que indica al servidor los encabezados que se envían con la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="d0272-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="d0272-247">Para permitir encabezados específicos, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="d0272-248">Para permitir todos los encabezados de solicitud de autor, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="d0272-249">Los exploradores no son totalmente coherentes en el modo en que establecen `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="d0272-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="d0272-250">Si establece encabezados en algo distinto de `"*"` (o usa <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`y `Origin`, además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="d0272-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="d0272-251">A continuación se ofrece un ejemplo de respuesta a la solicitud preparatoria (siempre que el servidor permita la solicitud):</span><span class="sxs-lookup"><span data-stu-id="d0272-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="d0272-252">La respuesta incluye un encabezado `Access-Control-Allow-Methods` que enumera los métodos permitidos y, opcionalmente, un encabezado `Access-Control-Allow-Headers`, que enumera los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="d0272-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="d0272-253">Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="d0272-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="d0272-254">Si se deniega la solicitud preparatoria, la aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="d0272-255">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="d0272-256">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="d0272-256">Set the preflight expiration time</span></span>

<span data-ttu-id="d0272-257">El encabezado `Access-Control-Max-Age` especifica cuánto tiempo se puede almacenar en caché la respuesta a la solicitud preparatoria.</span><span class="sxs-lookup"><span data-stu-id="d0272-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="d0272-258">Para establecer este encabezado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="d0272-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="d0272-259">Cómo funciona CORS</span><span class="sxs-lookup"><span data-stu-id="d0272-259">How CORS works</span></span>

<span data-ttu-id="d0272-260">En esta sección se describe lo que sucede en una solicitud de [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) en el nivel de los mensajes http.</span><span class="sxs-lookup"><span data-stu-id="d0272-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="d0272-261">CORS **no** es una característica de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d0272-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="d0272-262">CORS es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.</span><span class="sxs-lookup"><span data-stu-id="d0272-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="d0272-263">Por ejemplo, un actor malintencionado podría usar [impedir el scripting entre sitios (XSS)](xref:security/cross-site-scripting) en el sitio y ejecutar una solicitud entre sitios a su sitio habilitado para CORS para robar información.</span><span class="sxs-lookup"><span data-stu-id="d0272-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="d0272-264">La API no es más segura al permitir CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="d0272-265">Es el cliente (explorador) el que debe aplicar CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="d0272-266">El servidor ejecuta la solicitud y devuelve la respuesta, es el cliente el que devuelve un error y bloquea la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d0272-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="d0272-267">Por ejemplo, cualquiera de las siguientes herramientas mostrará la respuesta del servidor:</span><span class="sxs-lookup"><span data-stu-id="d0272-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="d0272-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="d0272-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="d0272-269">Postman</span><span class="sxs-lookup"><span data-stu-id="d0272-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="d0272-270">HttpClient de .NET</span><span class="sxs-lookup"><span data-stu-id="d0272-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="d0272-271">Un explorador web escribiendo la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="d0272-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="d0272-272">Es una forma de que un servidor permita que los exploradores ejecuten una solicitud de API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) o [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) entre orígenes que, de lo contrario, se prohibirá.</span><span class="sxs-lookup"><span data-stu-id="d0272-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="d0272-273">Los exploradores (sin CORS) no pueden realizar solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="d0272-274">Antes de CORS, se usaba [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) para eludir esta restricción.</span><span class="sxs-lookup"><span data-stu-id="d0272-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="d0272-275">JSONP no usa XHR, usa la etiqueta `<script>` para recibir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d0272-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="d0272-276">Los scripts pueden cargarse entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="d0272-277">La [especificación CORS](https://www.w3.org/TR/cors/) presentó varios encabezados HTTP nuevos que permiten solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="d0272-278">Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="d0272-279">No se necesita código JavaScript personalizado para habilitar CORS.</span><span class="sxs-lookup"><span data-stu-id="d0272-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="d0272-280">A continuación se presenta un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="d0272-281">El encabezado `Origin` proporciona el dominio del sitio que realiza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d0272-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="d0272-282">El encabezado `Origin` es obligatorio y debe ser diferente del host.</span><span class="sxs-lookup"><span data-stu-id="d0272-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="d0272-283">Si el servidor permite la solicitud, establece el encabezado `Access-Control-Allow-Origin` en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d0272-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="d0272-284">El valor de este encabezado coincide con el encabezado `Origin` de la solicitud o es el valor de carácter comodín `"*"`, lo que significa que se permite cualquier origen:</span><span class="sxs-lookup"><span data-stu-id="d0272-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="d0272-285">Si la respuesta no incluye el encabezado `Access-Control-Allow-Origin`, se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="d0272-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="d0272-286">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d0272-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="d0272-287">Aunque el servidor devuelva una respuesta correcta, el explorador no pone la respuesta a disposición de la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="d0272-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="d0272-288">Prueba de CORS</span><span class="sxs-lookup"><span data-stu-id="d0272-288">Test CORS</span></span>

<span data-ttu-id="d0272-289">Para probar CORS:</span><span class="sxs-lookup"><span data-stu-id="d0272-289">To test CORS:</span></span>

1. <span data-ttu-id="d0272-290">[Cree un proyecto de API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="d0272-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="d0272-291">Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="d0272-291">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="d0272-292">Habilite CORS con uno de los enfoques de este documento.</span><span class="sxs-lookup"><span data-stu-id="d0272-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="d0272-293">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d0272-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="d0272-294">`WithOrigins("https://localhost:<port>");` solo se debe usar para probar una aplicación de ejemplo similar a la [descarga del código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="d0272-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="d0272-295">Cree un proyecto de aplicación web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="d0272-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="d0272-296">En el ejemplo se utiliza Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d0272-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="d0272-297">Puede crear la aplicación web en la misma solución que el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="d0272-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="d0272-298">Agregue el siguiente código resaltado al archivo *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d0272-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="d0272-299">En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` por la dirección URL a la aplicación implementada.</span><span class="sxs-lookup"><span data-stu-id="d0272-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="d0272-300">Implemente el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="d0272-300">Deploy the API project.</span></span> <span data-ttu-id="d0272-301">Por ejemplo, [implemente en Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="d0272-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="d0272-302">Ejecute la aplicación Razor Pages o MVC desde el escritorio y haga clic en el botón **probar** .</span><span class="sxs-lookup"><span data-stu-id="d0272-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="d0272-303">Use las herramientas F12 para revisar los mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="d0272-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="d0272-304">Quitar el origen localhost de `WithOrigins` e implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0272-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="d0272-305">Como alternativa, ejecute la aplicación cliente con un puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="d0272-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="d0272-306">Por ejemplo, ejecute desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0272-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="d0272-307">Pruebe con la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="d0272-307">Test with the client app.</span></span> <span data-ttu-id="d0272-308">Los errores de CORS devuelven un error, pero el mensaje de error no está disponible para JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d0272-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="d0272-309">Use la pestaña consola de las herramientas F12 para ver el error.</span><span class="sxs-lookup"><span data-stu-id="d0272-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="d0272-310">Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0272-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="d0272-311">Uso de Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="d0272-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="d0272-312">**SEC7120: [CORS] el origen `https://localhost:44375` no encontró `https://localhost:44375` en el encabezado de respuesta Access-Control-Allow-Origin para el recurso de origen cruzado en `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="d0272-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="d0272-313">Usar Chrome:</span><span class="sxs-lookup"><span data-stu-id="d0272-313">Using Chrome:</span></span>

     <span data-ttu-id="d0272-314">**La Directiva CORS ha bloqueado el acceso a XMLHttpRequest en `https://webapi.azurewebsites.net/api/values/1` desde el origen `https://localhost:44375`: no hay ningún encabezado "Access-Control-Allow-Origin" en el recurso solicitado.**</span><span class="sxs-lookup"><span data-stu-id="d0272-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="d0272-315">Los puntos de conexión habilitados para CORS se pueden probar con una herramienta, como [Fiddler](https://www.telerik.com/fiddler) o [Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="d0272-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="d0272-316">Cuando se usa una herramienta, el origen de la solicitud especificada por el encabezado `Origin` debe ser diferente del host que recibe la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d0272-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="d0272-317">Si la solicitud no es de *origen cruzado* según el valor del encabezado `Origin`:</span><span class="sxs-lookup"><span data-stu-id="d0272-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="d0272-318">No es necesario que el middleware de CORS procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d0272-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="d0272-319">Los encabezados CORS no se devuelven en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d0272-319">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="d0272-320">CORS en IIS</span><span class="sxs-lookup"><span data-stu-id="d0272-320">CORS in IIS</span></span>

<span data-ttu-id="d0272-321">Al implementar en IIS, CORS debe ejecutarse antes de la autenticación de Windows si el servidor no está configurado para permitir el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="d0272-321">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="d0272-322">Para admitir este escenario, es necesario instalar y configurar el [módulo IIS CORS](https://www.iis.net/downloads/microsoft/iis-cors-module) para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0272-322">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0272-323">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d0272-323">Additional resources</span></span>

* [<span data-ttu-id="d0272-324">Uso compartido de recursos entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="d0272-324">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="d0272-325">Introducción al módulo IIS CORS</span><span class="sxs-lookup"><span data-stu-id="d0272-325">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
