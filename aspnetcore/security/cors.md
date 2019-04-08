---
title: Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo la CORS como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: fe5b750c44e5fad9ba80efb2cc8116d0a64b1a17
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068302"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="58f9b-103">Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58f9b-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="58f9b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="58f9b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="58f9b-105">En este artículo se muestra cómo se habilita la CORS en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58f9b-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="58f9b-106">Seguridad del explorador impide que una página web que realizan solicitudes a un dominio diferente a la que proviene la página web.</span><span class="sxs-lookup"><span data-stu-id="58f9b-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="58f9b-107">Esta restricción se denomina el *directiva del mismo origen*.</span><span class="sxs-lookup"><span data-stu-id="58f9b-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="58f9b-108">La directiva de mismo origen impide que un sitio malintencionado lea datos confidenciales de otro sitio.</span><span class="sxs-lookup"><span data-stu-id="58f9b-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="58f9b-109">En ocasiones, es posible que desea permitir que otros sitios realizan solicitudes entre orígenes en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="58f9b-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="58f9b-110">Para obtener más información, consulte el [artículo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="58f9b-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="58f9b-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="58f9b-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="58f9b-112">Es un W3C estándar que permite que un servidor a ser menos exigentes con la directiva de mismo origen.</span><span class="sxs-lookup"><span data-stu-id="58f9b-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="58f9b-113">Es **no** una característica de seguridad, la CORS relaja la seguridad.</span><span class="sxs-lookup"><span data-stu-id="58f9b-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="58f9b-114">Una API no es más segura, ya que permite la CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="58f9b-115">Para obtener más información, consulte [cómo la CORS funciona](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="58f9b-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="58f9b-116">Permite que un servidor permitir explícitamente algunas solicitudes entre orígenes y rechaza otras.</span><span class="sxs-lookup"><span data-stu-id="58f9b-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="58f9b-117">Es más seguro y más flexible que las técnicas anteriores, como [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="58f9b-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="58f9b-118">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="58f9b-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="58f9b-119">Mismo origen</span><span class="sxs-lookup"><span data-stu-id="58f9b-119">Same origin</span></span>

<span data-ttu-id="58f9b-120">Dos direcciones URL tienen el mismo origen si tienen esquemas idénticos, los hosts y los puertos ([6454 RFC](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="58f9b-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="58f9b-121">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="58f9b-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="58f9b-122">Estas direcciones URL tienen orígenes diferentes que las dos direcciones URL anteriores:</span><span class="sxs-lookup"><span data-stu-id="58f9b-122">These URLs have different origins than the previous two URLs:</span></span>

* `https://example.net` <span data-ttu-id="58f9b-123">&ndash; Dominio diferente</span><span class="sxs-lookup"><span data-stu-id="58f9b-123">&ndash; Different domain</span></span>
* `https://www.example.com/foo.html` <span data-ttu-id="58f9b-124">&ndash; Subdominio distinto</span><span class="sxs-lookup"><span data-stu-id="58f9b-124">&ndash; Different subdomain</span></span>
* `http://example.com/foo.html` <span data-ttu-id="58f9b-125">&ndash; Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="58f9b-125">&ndash; Different scheme</span></span>
* `https://example.com:9000/foo.html` <span data-ttu-id="58f9b-126">&ndash; Otro puerto</span><span class="sxs-lookup"><span data-stu-id="58f9b-126">&ndash; Different port</span></span>

<span data-ttu-id="58f9b-127">Internet Explorer no tiene en cuenta el puerto al comparar orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="58f9b-128">CORS con middleware y directivas con nombre</span><span class="sxs-lookup"><span data-stu-id="58f9b-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="58f9b-129">Middleware de CORS controla las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="58f9b-130">El código siguiente permite la CORS para toda la aplicación con el origen especificado:</span><span class="sxs-lookup"><span data-stu-id="58f9b-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="58f9b-131">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="58f9b-131">The preceding code:</span></span>

* <span data-ttu-id="58f9b-132">El nombre de la directiva se establece en "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="58f9b-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="58f9b-133">El nombre de la directiva es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="58f9b-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="58f9b-134">Las llamadas del <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensión, lo que permite la CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="58f9b-135">Las llamadas <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="58f9b-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="58f9b-136">La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="58f9b-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="58f9b-137">[Las opciones de configuración](#cors-policy-options), tales como `WithOrigins`, se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="58f9b-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="58f9b-138">El <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> llamada al método agrega los servicios de la CORS al contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="58f9b-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="58f9b-139">Para obtener más información, consulte [las opciones de directiva CORS](#cpo) en este documento.</span><span class="sxs-lookup"><span data-stu-id="58f9b-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="58f9b-140">El <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> método puede encadenar métodos, como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="58f9b-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="58f9b-141">El siguiente código resaltado aplica a las directivas de la CORS a todos los extremos de las aplicaciones a través de Middleware de CORS:</span><span class="sxs-lookup"><span data-stu-id="58f9b-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

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

<span data-ttu-id="58f9b-142">Consulte [habilitar CORS en las páginas de Razor, controladores y métodos de acción](#ecors) para aplicar la directiva CORS en el nivel de página o controlador/acción.</span><span class="sxs-lookup"><span data-stu-id="58f9b-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="58f9b-143">Nota:</span><span class="sxs-lookup"><span data-stu-id="58f9b-143">Note:</span></span>

* `UseCors` <span data-ttu-id="58f9b-144">debe llamarse antes `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-144">must be called before `UseMvc`.</span></span>
* <span data-ttu-id="58f9b-145">La dirección URL debe **no** contienen una barra diagonal final (`/`).</span><span class="sxs-lookup"><span data-stu-id="58f9b-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="58f9b-146">Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.</span><span class="sxs-lookup"><span data-stu-id="58f9b-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="58f9b-147">Consulte [prueba CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.</span><span class="sxs-lookup"><span data-stu-id="58f9b-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="58f9b-148">Habilitar la CORS con atributos</span><span class="sxs-lookup"><span data-stu-id="58f9b-148">Enable CORS with attributes</span></span>

<span data-ttu-id="58f9b-149">El [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo proporciona una alternativa a la aplicación la CORS globalmente.</span><span class="sxs-lookup"><span data-stu-id="58f9b-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="58f9b-150">El `[EnableCors]` atributo habilita la CORS para los puntos de conexión seleccionados, en lugar de todos los puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="58f9b-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="58f9b-151">Use `[EnableCors]` para especificar la directiva predeterminada y `[EnableCors("{Policy String}")]` para especificar una directiva.</span><span class="sxs-lookup"><span data-stu-id="58f9b-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="58f9b-152">El `[EnableCors]` atributo puede aplicarse a:</span><span class="sxs-lookup"><span data-stu-id="58f9b-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="58f9b-153">Página de Razor</span><span class="sxs-lookup"><span data-stu-id="58f9b-153">Razor Page</span></span> `PageModel`
* <span data-ttu-id="58f9b-154">Controlador</span><span class="sxs-lookup"><span data-stu-id="58f9b-154">Controller</span></span>
* <span data-ttu-id="58f9b-155">Método de acción de controlador</span><span class="sxs-lookup"><span data-stu-id="58f9b-155">Controller action method</span></span>

<span data-ttu-id="58f9b-156">Puede aplicar diferentes directivas a/página-modelo o acción de controlador con el `[EnableCors]` atributo.</span><span class="sxs-lookup"><span data-stu-id="58f9b-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="58f9b-157">Cuando el `[EnableCors]` atributo se aplica a un método de acción o controladores/página-modelo y se ha habilitado CORS en middleware, se aplicarán ambas directivas.</span><span class="sxs-lookup"><span data-stu-id="58f9b-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="58f9b-158">No se recomienda combinar las directivas.</span><span class="sxs-lookup"><span data-stu-id="58f9b-158">We recommend against combining policies.</span></span> <span data-ttu-id="58f9b-159">Use el `[EnableCors]` atributo o un software intermedio, no a ambos en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="58f9b-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="58f9b-160">El código siguiente aplica una directiva diferente a cada método:</span><span class="sxs-lookup"><span data-stu-id="58f9b-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="58f9b-161">El código siguiente crea una directiva predeterminada CORS y una directiva denominada `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="58f9b-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="58f9b-162">Deshabilitar la CORS</span><span class="sxs-lookup"><span data-stu-id="58f9b-162">Disable CORS</span></span>

<span data-ttu-id="58f9b-163">El [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atributo deshabilita la CORS para la acción o controlador/página-modelo.</span><span class="sxs-lookup"><span data-stu-id="58f9b-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="58f9b-164">Opciones de directiva CORS</span><span class="sxs-lookup"><span data-stu-id="58f9b-164">CORS policy options</span></span>

<span data-ttu-id="58f9b-165">En esta sección se describe las distintas opciones que se pueden establecer en una directiva CORS:</span><span class="sxs-lookup"><span data-stu-id="58f9b-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="58f9b-166">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="58f9b-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="58f9b-167">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="58f9b-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="58f9b-168">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="58f9b-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="58f9b-169">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="58f9b-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="58f9b-170">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="58f9b-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="58f9b-171">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="58f9b-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> <span data-ttu-id="58f9b-172">se llama en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-172">is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="58f9b-173">Para algunas opciones, puede resultar útil leer la [cómo la CORS funciona](#how-cors) sección en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="58f9b-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="58f9b-174">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="58f9b-174">Set the allowed origins</span></span>

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> <span data-ttu-id="58f9b-175">&ndash; Permite que las solicitudes CORS de todos los orígenes con cualquier esquema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="58f9b-175">&ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> `AllowAnyOrigin` <span data-ttu-id="58f9b-176">no es segura porque *cualquier sitio Web* puede realizar solicitudes entre orígenes en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="58f9b-176">is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="58f9b-177">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios.</span><span class="sxs-lookup"><span data-stu-id="58f9b-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="58f9b-178">El servicio de la CORS devuelve una respuesta no válida de la CORS cuando una aplicación está configurada con ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="58f9b-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="58f9b-179">Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios.</span><span class="sxs-lookup"><span data-stu-id="58f9b-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="58f9b-180">Para una aplicación segura, especifique una lista exacta de los orígenes, si el cliente debe autorizar a sí mismo para tener acceso a los recursos del servidor.</span><span class="sxs-lookup"><span data-stu-id="58f9b-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

`AllowAnyOrigin` <span data-ttu-id="58f9b-181">Afecta a las solicitudes de comprobaciones y `Access-Control-Allow-Origin` encabezado.</span><span class="sxs-lookup"><span data-stu-id="58f9b-181">affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="58f9b-182">Para obtener más información, consulte el [las solicitudes de comprobaciones](#preflight-requests) sección.</span><span class="sxs-lookup"><span data-stu-id="58f9b-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="58f9b-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Establece el <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propiedad de la directiva a una función que permite orígenes para que coincida con un dominio con comodín configurado al evaluar si se permite el origen.</span><span class="sxs-lookup"><span data-stu-id="58f9b-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="58f9b-184">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="58f9b-184">Set the allowed HTTP methods</span></span>

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*><span data-ttu-id="58f9b-185">:</span><span class="sxs-lookup"><span data-stu-id="58f9b-185">:</span></span>

* <span data-ttu-id="58f9b-186">Permite que cualquier método HTTP:</span><span class="sxs-lookup"><span data-stu-id="58f9b-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="58f9b-187">Afecta a las solicitudes de comprobaciones y `Access-Control-Allow-Methods` encabezado.</span><span class="sxs-lookup"><span data-stu-id="58f9b-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="58f9b-188">Para obtener más información, consulte el [las solicitudes de comprobaciones](#preflight-requests) sección.</span><span class="sxs-lookup"><span data-stu-id="58f9b-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="58f9b-189">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="58f9b-189">Set the allowed request headers</span></span>

<span data-ttu-id="58f9b-190">Para permitir que los encabezados específicos que se enviarán en una solicitud de CORS, llamado *crear encabezados de solicitud*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:</span><span class="sxs-lookup"><span data-stu-id="58f9b-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="58f9b-191">Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="58f9b-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="58f9b-192">Esta configuración afecta a las solicitudes de preflight y `Access-Control-Request-Headers` encabezado.</span><span class="sxs-lookup"><span data-stu-id="58f9b-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="58f9b-193">Para obtener más información, consulte el [las solicitudes de comprobaciones](#preflight-requests) sección.</span><span class="sxs-lookup"><span data-stu-id="58f9b-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="58f9b-194">Una coincidencia de directiva de Middleware de CORS a encabezados específicos especificado por `WithHeaders` solo es posible cuando se envían los encabezados `Access-Control-Request-Headers` coincidir exactamente con los encabezados que se indica en `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="58f9b-195">Por ejemplo, considere una aplicación configurada como sigue:</span><span class="sxs-lookup"><span data-stu-id="58f9b-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="58f9b-196">Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="58f9b-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="58f9b-197">Devuelve la aplicación un *200 Aceptar* respuesta pero no envía de vuelta los encabezados CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="58f9b-198">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="58f9b-199">Middleware de CORS siempre permite cuatro encabezados en el `Access-Control-Request-Headers` para enviarse independientemente de los valores configurados en CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="58f9b-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="58f9b-200">Esta lista de encabezados incluye:</span><span class="sxs-lookup"><span data-stu-id="58f9b-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="58f9b-201">Por ejemplo, considere una aplicación configurada como sigue:</span><span class="sxs-lookup"><span data-stu-id="58f9b-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="58f9b-202">Middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` siempre es la lista de permitidos:</span><span class="sxs-lookup"><span data-stu-id="58f9b-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="58f9b-203">Establecer los encabezados de respuesta expuestos</span><span class="sxs-lookup"><span data-stu-id="58f9b-203">Set the exposed response headers</span></span>

<span data-ttu-id="58f9b-204">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="58f9b-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="58f9b-205">Para obtener más información, consulte [W3C Cross-Origin Resource Sharing (la terminología): Encabezado de respuesta simple](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="58f9b-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="58f9b-206">Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="58f9b-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="58f9b-207">La especificación de CORS llama a estos encabezados *encabezados de respuesta simple*.</span><span class="sxs-lookup"><span data-stu-id="58f9b-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="58f9b-208">Para que otros encabezados disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="58f9b-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="58f9b-209">Credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="58f9b-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="58f9b-210">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="58f9b-211">De forma predeterminada, el explorador no envía las credenciales con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="58f9b-212">Las credenciales incluyen las cookies y los esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="58f9b-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="58f9b-213">Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` a `true`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="58f9b-214">Uso de `XMLHttpRequest` directamente:</span><span class="sxs-lookup"><span data-stu-id="58f9b-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="58f9b-215">Uso de jQuery:</span><span class="sxs-lookup"><span data-stu-id="58f9b-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="58f9b-216">Mediante el [capturar API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="58f9b-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="58f9b-217">El servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="58f9b-217">The server must allow the credentials.</span></span> <span data-ttu-id="58f9b-218">Para permitir que las credenciales de origen cruzado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="58f9b-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="58f9b-219">La respuesta HTTP incluye un `Access-Control-Allow-Credentials` encabezado, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="58f9b-220">Si el explorador envía credenciales, pero la respuesta no incluye válido `Access-Control-Allow-Credentials` encabezado, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="58f9b-221">Permitir las credenciales de origen cruzado es un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="58f9b-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="58f9b-222">Un sitio Web en otro dominio puede enviar las credenciales de un usuario con sesión iniciada de la aplicación en el nombre de usuario sin el conocimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="58f9b-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="58f9b-223">La especificación de CORS también indica ese valor orígenes a `"*"` (todos los orígenes) no es válido si el `Access-Control-Allow-Credentials` encabezado está presente.</span><span class="sxs-lookup"><span data-stu-id="58f9b-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="58f9b-224">Solicitudes preparatorias</span><span class="sxs-lookup"><span data-stu-id="58f9b-224">Preflight requests</span></span>

<span data-ttu-id="58f9b-225">Para algunas solicitudes CORS, el explorador envía una solicitud adicional antes de realizar la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="58f9b-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="58f9b-226">Se llama a esta solicitud una *solicitud preparatoria*.</span><span class="sxs-lookup"><span data-stu-id="58f9b-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="58f9b-227">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="58f9b-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="58f9b-228">El método de solicitud es GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="58f9b-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="58f9b-229">La aplicación, no establezca los encabezados de solicitud distinto `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="58f9b-230">El `Content-Type` encabezado, si establece, tiene uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="58f9b-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="58f9b-231">La regla en los encabezados de solicitud establecido para la solicitud de cliente se aplica a los encabezados de la aplicación se establece mediante una llamada a `setRequestHeader` en el `XMLHttpRequest` objeto.</span><span class="sxs-lookup"><span data-stu-id="58f9b-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="58f9b-232">La especificación de CORS llama a estos encabezados *crear encabezados de solicitud*.</span><span class="sxs-lookup"><span data-stu-id="58f9b-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="58f9b-233">La regla no se aplica a los encabezados que se puede establecer el explorador, tales como `User-Agent`, `Host`, o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="58f9b-234">Este es un ejemplo de una solicitud de preflight:</span><span class="sxs-lookup"><span data-stu-id="58f9b-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="58f9b-235">La solicitud preparatoria usa el método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="58f9b-236">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="58f9b-236">It includes two special headers:</span></span>

* `Access-Control-Request-Method`<span data-ttu-id="58f9b-237">: El método HTTP que se usará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="58f9b-237">: The HTTP method that will be used for the actual request.</span></span>
* `Access-Control-Request-Headers`<span data-ttu-id="58f9b-238">: Una lista de encabezados de solicitud que la aplicación se establece en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="58f9b-238">: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="58f9b-239">Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, como `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="58f9b-240">Una solicitud preparatoria de CORS puede incluir un `Access-Control-Request-Headers` encabezado, que indica al servidor los encabezados que se envían con la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="58f9b-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="58f9b-241">Para permitir encabezados específicos, llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="58f9b-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="58f9b-242">Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="58f9b-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="58f9b-243">Los exploradores no están totalmente coherentes en cómo establezca `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="58f9b-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="58f9b-244">Si establece los encabezados en algo distinto `"*"` (o use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`, y `Origin`, además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="58f9b-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="58f9b-245">Este es un ejemplo de respuesta a la solicitud preparatoria (suponiendo que el servidor permite la solicitud):</span><span class="sxs-lookup"><span data-stu-id="58f9b-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="58f9b-246">La respuesta incluye un `Access-Control-Allow-Methods` encabezado que se enumera los métodos permitidos y, opcionalmente, un `Access-Control-Allow-Headers` encabezado, que se enumera los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="58f9b-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="58f9b-247">Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="58f9b-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="58f9b-248">Si se deniega la solicitud preparatoria, la aplicación devuelve un *200 Aceptar* respuesta pero no envía de vuelta los encabezados CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="58f9b-249">Por lo tanto, el explorador no intenta la solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="58f9b-250">Establecer el tiempo de expiración de las comprobaciones preparatorias</span><span class="sxs-lookup"><span data-stu-id="58f9b-250">Set the preflight expiration time</span></span>

<span data-ttu-id="58f9b-251">El `Access-Control-Max-Age` encabezado especifica cuánto tiempo puede almacenar en caché la respuesta a la solicitud preparatoria.</span><span class="sxs-lookup"><span data-stu-id="58f9b-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="58f9b-252">Para establecer este encabezado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="58f9b-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="58f9b-253">Cómo funciona la CORS</span><span class="sxs-lookup"><span data-stu-id="58f9b-253">How CORS works</span></span>

<span data-ttu-id="58f9b-254">Esta sección describe lo que ocurre en un [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) solicitud en el nivel de los mensajes HTTP.</span><span class="sxs-lookup"><span data-stu-id="58f9b-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="58f9b-255">CORS es **no** una característica de seguridad.</span><span class="sxs-lookup"><span data-stu-id="58f9b-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="58f9b-256">CORS es un estándar de W3C que permite que un servidor a ser menos exigentes con la directiva de mismo origen.</span><span class="sxs-lookup"><span data-stu-id="58f9b-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="58f9b-257">Por ejemplo, podría utilizar un individuo malintencionado [evitar Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) en su sitio y ejecutar una solicitud entre sitios a su sitio habilitado para CORS para robar información.</span><span class="sxs-lookup"><span data-stu-id="58f9b-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="58f9b-258">La API no es más segura, ya que permite la CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="58f9b-259">Depende del cliente (explorador) exigir la CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="58f9b-260">El servidor ejecuta la solicitud y devuelve la respuesta, es el cliente que devuelve un error y bloques de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="58f9b-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="58f9b-261">Por ejemplo, cualquiera de las siguientes herramientas mostrará la respuesta del servidor:</span><span class="sxs-lookup"><span data-stu-id="58f9b-261">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="58f9b-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="58f9b-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="58f9b-263">Postman</span><span class="sxs-lookup"><span data-stu-id="58f9b-263">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="58f9b-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="58f9b-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="58f9b-265">Un explorador web escribiendo la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="58f9b-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="58f9b-266">Es una manera de ejecutar un origen cruzado para un servidor permitir que los exploradores [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) o [API capturar](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) solicitud que en caso contrario, podría estar prohibido.</span><span class="sxs-lookup"><span data-stu-id="58f9b-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="58f9b-267">Los exploradores (sin la CORS) no pueden realizar solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="58f9b-268">Antes de la CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) se usó para eludir esta restricción.</span><span class="sxs-lookup"><span data-stu-id="58f9b-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="58f9b-269">JSONP no usa XHR, usa el `<script>` etiqueta para recibir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="58f9b-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="58f9b-270">Las secuencias de comandos pueden estar cargado entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="58f9b-271">El [especificación CORS](https://www.w3.org/TR/cors/) introdujo varios encabezados HTTP nuevo que permiten a las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-271">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="58f9b-272">Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="58f9b-273">No se necesita código JavaScript personalizado para habilitar CORS.</span><span class="sxs-lookup"><span data-stu-id="58f9b-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="58f9b-274">El siguiente es un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="58f9b-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="58f9b-275">El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:</span><span class="sxs-lookup"><span data-stu-id="58f9b-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="58f9b-276">Si el servidor permite la solicitud, Establece la `Access-Control-Allow-Origin` encabezado en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="58f9b-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="58f9b-277">El valor de este encabezado ya sea con la `Origin` encabezado de la solicitud o es el valor de carácter comodín `"*"`, lo que significa que se permite cualquier origen:</span><span class="sxs-lookup"><span data-stu-id="58f9b-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="58f9b-278">Si la respuesta no incluye el `Access-Control-Allow-Origin` encabezado, la solicitud de origen cruzado se produce un error.</span><span class="sxs-lookup"><span data-stu-id="58f9b-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="58f9b-279">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="58f9b-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="58f9b-280">Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="58f9b-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="58f9b-281">Prueba de CORS</span><span class="sxs-lookup"><span data-stu-id="58f9b-281">Test CORS</span></span>

<span data-ttu-id="58f9b-282">Para probar la CORS:</span><span class="sxs-lookup"><span data-stu-id="58f9b-282">To test CORS:</span></span>

1. <span data-ttu-id="58f9b-283">[Crear un proyecto de API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="58f9b-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="58f9b-284">Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="58f9b-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="58f9b-285">Habilitar la CORS mediante uno de los enfoques de este documento.</span><span class="sxs-lookup"><span data-stu-id="58f9b-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="58f9b-286">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="58f9b-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` <span data-ttu-id="58f9b-287">solo debe usarse para probar una aplicación de ejemplo similar a la [descargar código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="58f9b-287">should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="58f9b-288">Cree un proyecto de aplicación web (páginas de Razor o MVC).</span><span class="sxs-lookup"><span data-stu-id="58f9b-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="58f9b-289">El ejemplo utiliza las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="58f9b-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="58f9b-290">Puede crear la aplicación web en la misma solución que el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="58f9b-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="58f9b-291">Agregue el código resaltado siguiente a la *Index.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="58f9b-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="58f9b-292">En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` con la dirección URL a la aplicación implementada.</span><span class="sxs-lookup"><span data-stu-id="58f9b-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="58f9b-293">Implementar el proyecto de API.</span><span class="sxs-lookup"><span data-stu-id="58f9b-293">Deploy the API project.</span></span> <span data-ttu-id="58f9b-294">Por ejemplo, [implementar en Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="58f9b-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="58f9b-295">Ejecute la aplicación de las páginas de Razor o MVC desde el escritorio y haga clic en el **prueba** botón.</span><span class="sxs-lookup"><span data-stu-id="58f9b-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="58f9b-296">Use las herramientas de F12 para revisar los mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="58f9b-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="58f9b-297">Quitar el origen de localhost de `WithOrigins` e implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="58f9b-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="58f9b-298">Como alternativa, puede ejecutar la aplicación cliente con un puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="58f9b-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="58f9b-299">Por ejemplo, ejecutar desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58f9b-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="58f9b-300">Prueba con la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="58f9b-300">Test with the client app.</span></span> <span data-ttu-id="58f9b-301">Errores de la CORS devuelven un error, pero el mensaje de error no está disponible en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="58f9b-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="58f9b-302">Utilice la pestaña de la consola en las herramientas de F12 para ver el error.</span><span class="sxs-lookup"><span data-stu-id="58f9b-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="58f9b-303">Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="58f9b-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="58f9b-304">Con Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="58f9b-304">Using Microsoft Edge:</span></span>

     **<span data-ttu-id="58f9b-305">SEC7120: [CORS] origen `https://localhost:44375` no se encontró `https://localhost:44375` en el encabezado de respuesta de Access-Control-Allow-Origin para recursos entre orígenes en</span><span class="sxs-lookup"><span data-stu-id="58f9b-305">SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at</span></span> `https://webapi.azurewebsites.net/api/values/1`**

   * <span data-ttu-id="58f9b-306">Uso de Chrome:</span><span class="sxs-lookup"><span data-stu-id="58f9b-306">Using Chrome:</span></span>

     **<span data-ttu-id="58f9b-307">Acceso a XMLHttpRequest en `https://webapi.azurewebsites.net/api/values/1` desde origen `https://localhost:44375` ha sido bloqueada por la directiva CORS: Ningún encabezado "Access-Control-Allow-Origin" está presente en el recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="58f9b-307">Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span>**

## <a name="additional-resources"></a><span data-ttu-id="58f9b-308">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="58f9b-308">Additional resources</span></span>

* [<span data-ttu-id="58f9b-309">Recursos entre orígenes (CORS) de uso compartido</span><span class="sxs-lookup"><span data-stu-id="58f9b-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
