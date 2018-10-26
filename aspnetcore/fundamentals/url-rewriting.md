---
title: Middleware de reescritura de URL en ASP.NET Core
author: guardrex
description: Aprenda a reescribir y redireccionar URL con el middleware de reescritura de URL en aplicaciones ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326074"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="85f35-103">Middleware de reescritura de URL en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85f35-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="85f35-104">Por [Luke Latham](https://github.com/guardrex) y [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="85f35-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="85f35-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="85f35-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="85f35-106">La reescritura de URL consiste en modificar varias URL de solicitud basadas en una o varias reglas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="85f35-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="85f35-107">La reescritura de URL crea una abstracción entre las ubicaciones de recursos y sus direcciones para que las ubicaciones y direcciones no estén estrechamente vinculadas.</span><span class="sxs-lookup"><span data-stu-id="85f35-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="85f35-108">Hay varias situaciones en las que la reescritura de URL es útil:</span><span class="sxs-lookup"><span data-stu-id="85f35-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="85f35-109">Mover o reemplazar recursos del servidor de forma temporal o permanente a la vez que se mantienen estables los localizadores para esos recursos.</span><span class="sxs-lookup"><span data-stu-id="85f35-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="85f35-110">Dividir el procesamiento de solicitudes entre distintas aplicaciones o entre áreas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="85f35-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="85f35-111">Quitar, agregar o reorganizar segmentos de URL en solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="85f35-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="85f35-112">Optimizar URL públicas para la optimización del motor de búsqueda (SEO).</span><span class="sxs-lookup"><span data-stu-id="85f35-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="85f35-113">Permitir el uso de URL públicas preparadas para ayudar a los usuarios a predecir el contenido que encontrarán siguiendo un vínculo.</span><span class="sxs-lookup"><span data-stu-id="85f35-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="85f35-114">Redirigir solicitudes no seguras para proteger puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="85f35-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="85f35-115">Evitar la creación de vínculos activos de imagen.</span><span class="sxs-lookup"><span data-stu-id="85f35-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="85f35-116">Puede definir reglas para cambiar la URL de varias maneras, incluidas expresiones regulares, reglas del módulo mod_rewrite de Apache, reglas del módulo de reescritura de IIS y lógica de regla personalizada.</span><span class="sxs-lookup"><span data-stu-id="85f35-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="85f35-117">En este documento se ofrece una introducción a la reescritura de URL e instrucciones sobre cómo usar el middleware de reescritura de URL en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="85f35-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="85f35-118">La reescritura de URL puede reducir el rendimiento de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="85f35-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="85f35-119">Cuando sea factible, debe limitar el número y la complejidad de las reglas.</span><span class="sxs-lookup"><span data-stu-id="85f35-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="85f35-120">Redireccionamiento y reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="85f35-121">La diferencia entre los términos *redirección de URL* y *reescritura de URL* en principio puede parecer sutil, pero tiene importantes implicaciones para proporcionar recursos a los clientes.</span><span class="sxs-lookup"><span data-stu-id="85f35-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="85f35-122">El middleware de reescritura de URL de ASP.NET Core es capaz de satisfacer las necesidades de ambos.</span><span class="sxs-lookup"><span data-stu-id="85f35-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="85f35-123">La *redirección de URL* es una operación del lado cliente, que da la instrucción al cliente de acceder a un recurso en otra dirección.</span><span class="sxs-lookup"><span data-stu-id="85f35-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="85f35-124">Esto requiere un recorrido de ida y vuelta al servidor.</span><span class="sxs-lookup"><span data-stu-id="85f35-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="85f35-125">La URL de redireccionamiento que se devuelve al cliente aparece en la barra de direcciones del explorador cuando el cliente realiza una nueva solicitud para el recurso.</span><span class="sxs-lookup"><span data-stu-id="85f35-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="85f35-126">Si `/resource` se *redirige* a `/different-resource`, el cliente solicita `/resource`.</span><span class="sxs-lookup"><span data-stu-id="85f35-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="85f35-127">El servidor responde que el cliente debe obtener el recurso en `/different-resource` con un código de estado que indica que la redirección es temporal o permanente.</span><span class="sxs-lookup"><span data-stu-id="85f35-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="85f35-128">El cliente ejecuta una nueva solicitud para el recurso en la URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="85f35-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Se cambió temporalmente un punto de conexión del servicio WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="85f35-134">Al redirigir las solicitudes a una URL diferente, se indica si la redirección es permanente o temporal.</span><span class="sxs-lookup"><span data-stu-id="85f35-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="85f35-135">El código de estado 301 (Movido definitivamente) se usa cuando el recurso tiene una nueva URL permanente y quiere indicar al cliente que todas las solicitudes futuras para el recurso deben usar la nueva URL.</span><span class="sxs-lookup"><span data-stu-id="85f35-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="85f35-136">*El cliente puede almacenar en caché la respuesta cuando se recibe un código de estado 301.*</span><span class="sxs-lookup"><span data-stu-id="85f35-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="85f35-137">El código de estado 302 (Encontrado) se usa cuando la redirección es temporal o normalmente está sujeta a cambios, de modo que el cliente no debe almacenar y reutilizar la URL de redireccionamiento en el futuro.</span><span class="sxs-lookup"><span data-stu-id="85f35-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="85f35-138">Para más información, vea [RFC 2616: definiciones de código de estado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="85f35-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="85f35-139">La *reescritura de URL* es una operación del lado servidor que proporciona un recurso desde una dirección de recursos distinta.</span><span class="sxs-lookup"><span data-stu-id="85f35-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="85f35-140">La reescritura de una URL no requiere un recorrido de ida y vuelta al servidor.</span><span class="sxs-lookup"><span data-stu-id="85f35-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="85f35-141">La dirección URL reescrita no se devuelve al cliente y no aparece en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="85f35-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="85f35-142">Cuando `/resource` se *reescribe* como `/different-resource`, el cliente solicita `/resource` y el servidor recupera *internamente* el recurso en `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="85f35-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="85f35-143">Aunque el cliente podría recuperar el recurso en la URL reescrita, el cliente no informará de que el recurso existe en la URL reescrita cuando realice su solicitud y reciba la respuesta.</span><span class="sxs-lookup"><span data-stu-id="85f35-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Se cambió un punto de conexión del servicio WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="85f35-148">Aplicación de ejemplo de reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-148">URL rewriting sample app</span></span>

<span data-ttu-id="85f35-149">Puede explorar las características del middleware de reescritura de URL con la [aplicación de ejemplo de reescritura de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="85f35-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="85f35-150">Esta aplicación aplica reglas de reescritura y redirección, además de mostrar la URL redirigida o reescrita.</span><span class="sxs-lookup"><span data-stu-id="85f35-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="85f35-151">Cuándo usar el middleware de reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="85f35-152">Use el middleware de reescritura de URL cuando no pueda usar el [módulo de reescritura de URL](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS en Windows Server, el [módulo mod_rewrite de Apache](https://httpd.apache.org/docs/2.4/rewrite/) en el servidor Apache, la [reescritura de URL en Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) o cuando la aplicación esté hospedada en el [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (antes denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="85f35-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="85f35-153">Las principales razones para usar las tecnologías de reescritura de URL basadas en servidor en IIS, Apache o Nginx son que el middleware no es compatible con todas las características de estos módulos y el rendimiento del middleware probablemente no coincida con el de los módulos.</span><span class="sxs-lookup"><span data-stu-id="85f35-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="85f35-154">Pero algunas características de los módulos de servidor no funcionan con proyectos de ASP.NET Core, como las restricciones `IsFile` y `IsDirectory` del módulo de reescritura de IIS.</span><span class="sxs-lookup"><span data-stu-id="85f35-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="85f35-155">En estos casos, es mejor usar el middleware.</span><span class="sxs-lookup"><span data-stu-id="85f35-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="85f35-156">Package</span><span class="sxs-lookup"><span data-stu-id="85f35-156">Package</span></span>

<span data-ttu-id="85f35-157">Para incluir el middleware en el proyecto, agregue una referencia al paquete [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/).</span><span class="sxs-lookup"><span data-stu-id="85f35-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="85f35-158">Esta característica está disponible para aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="85f35-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="85f35-159">Extensión y opciones</span><span class="sxs-lookup"><span data-stu-id="85f35-159">Extension and options</span></span>

<span data-ttu-id="85f35-160">Establezca las reglas de reescritura y redirección de URL mediante la creación de una instancia de la clase [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) con métodos de extensión para cada una de las reglas.</span><span class="sxs-lookup"><span data-stu-id="85f35-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="85f35-161">Encadene varias reglas en el orden que quiera procesarlas.</span><span class="sxs-lookup"><span data-stu-id="85f35-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="85f35-162">Las `RewriteOptions` se pasan al middleware de reescritura de URL cuando se agregan a la canalización de solicitudes con `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="85f35-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="85f35-163">Redirigir solicitudes que distintas de www a www</span><span class="sxs-lookup"><span data-stu-id="85f35-163">Redirect non-www to www</span></span>

<span data-ttu-id="85f35-164">Hay tres opciones que permiten a la aplicación redirigir solicitudes distintas de `www` a `www`:</span><span class="sxs-lookup"><span data-stu-id="85f35-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="85f35-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Redirige permanentemente la solicitud al subdominio `www` si la solicitud es distinta de `www`.</span><span class="sxs-lookup"><span data-stu-id="85f35-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="85f35-166">Se redirige con un código de estado [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect).</span><span class="sxs-lookup"><span data-stu-id="85f35-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="85f35-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirige la solicitud al subdominio `www` si la solicitud entrante es distinta de `www`.</span><span class="sxs-lookup"><span data-stu-id="85f35-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="85f35-168">Se redirige con un código de estado [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect).</span><span class="sxs-lookup"><span data-stu-id="85f35-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="85f35-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirige la solicitud al subdominio `www` si la solicitud entrante es distinta de `www`.</span><span class="sxs-lookup"><span data-stu-id="85f35-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="85f35-170">Permite proporcionar el código de estado para la respuesta.</span><span class="sxs-lookup"><span data-stu-id="85f35-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="85f35-171">Use los campos de la clase [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) para las asignaciones a `AddRedirectToWww`.</span><span class="sxs-lookup"><span data-stu-id="85f35-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="85f35-172">Redirección de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-172">URL redirect</span></span>

<span data-ttu-id="85f35-173">Use `AddRedirect` para redirigir las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="85f35-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="85f35-174">El primer parámetro contiene la expresión regular para hacer coincidir la ruta de acceso de la URL entrante.</span><span class="sxs-lookup"><span data-stu-id="85f35-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="85f35-175">El segundo parámetro es la cadena de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="85f35-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="85f35-176">El tercer parámetro, si está presente, especifica el código de estado.</span><span class="sxs-lookup"><span data-stu-id="85f35-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="85f35-177">Si no se especifica el código de estado, el valor predeterminado es 302 (Encontrado), lo que indica que el recurso se ha movido o reemplazado temporalmente.</span><span class="sxs-lookup"><span data-stu-id="85f35-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="85f35-178">En un explorador con herramientas de desarrollo habilitadas, realice una solicitud a la aplicación de ejemplo con la ruta de acceso `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="85f35-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="85f35-179">La expresión regular coincide con la ruta de acceso de la solicitud en `redirect-rule/(.*)` y la ruta de acceso se reemplaza con `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="85f35-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="85f35-180">La URL de redireccionamiento se devuelve al cliente con un código de estado 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="85f35-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="85f35-181">El explorador realiza una solicitud nueva en la URL de redireccionamiento, que aparece en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="85f35-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="85f35-182">Puesto que no hay ninguna regla en la aplicación de ejemplo que coincida con la URL de redireccionamiento, la segunda solicitud recibe una respuesta 200 (Correcto) de la aplicación y el cuerpo de la respuesta muestra la URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="85f35-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="85f35-183">Se realiza un recorrido de ida y vuelta al servidor cuando se *redirige* una URL.</span><span class="sxs-lookup"><span data-stu-id="85f35-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="85f35-184">Tenga cuidado al establecer las reglas de redirección.</span><span class="sxs-lookup"><span data-stu-id="85f35-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="85f35-185">Las reglas de redirección se evalúan en cada solicitud enviada a la aplicación, incluso después de una redirección.</span><span class="sxs-lookup"><span data-stu-id="85f35-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="85f35-186">Es fácil crear por error un bucle de redirecciones infinitas.</span><span class="sxs-lookup"><span data-stu-id="85f35-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="85f35-187">Solicitud original: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="85f35-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="85f35-189">La parte de la expresión incluida entre paréntesis se denomina *grupo de capturas*.</span><span class="sxs-lookup"><span data-stu-id="85f35-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="85f35-190">El punto (`.`) de la expresión significa *buscar cualquier carácter*.</span><span class="sxs-lookup"><span data-stu-id="85f35-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="85f35-191">El asterisco (`*`) indica *buscar el carácter precedente cero o más veces*.</span><span class="sxs-lookup"><span data-stu-id="85f35-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="85f35-192">Por tanto, el grupo de capturas `(.*)` busca los dos últimos segmentos de la ruta de acceso de la URL, `1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="85f35-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="85f35-193">Cualquier valor que se proporcione en la URL de la solicitud después de `redirect-rule/` es capturado por este grupo de capturas único.</span><span class="sxs-lookup"><span data-stu-id="85f35-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="85f35-194">En la cadena de reemplazo, se insertan los grupos capturados en la cadena con el signo de dólar (`$`) seguido del número de secuencia de la captura.</span><span class="sxs-lookup"><span data-stu-id="85f35-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="85f35-195">Se obtiene el valor del primer grupo de capturas con `$1`, el segundo con `$2`, y así siguen en secuencia para los grupos de capturas de la expresión regular.</span><span class="sxs-lookup"><span data-stu-id="85f35-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="85f35-196">Solo hay un grupo capturado en la expresión regular de la regla de redirección de la aplicación de ejemplo, por lo que solo hay un grupo insertado en la cadena de reemplazo, que es `$1`.</span><span class="sxs-lookup"><span data-stu-id="85f35-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="85f35-197">Cuando se aplica la regla, la URL se convierte en `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="85f35-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="85f35-198">Redirección de URL a un punto de conexión segura</span><span class="sxs-lookup"><span data-stu-id="85f35-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="85f35-199">Use `AddRedirectToHttps` para redirigir solicitudes HTTP al mismo host y ruta con HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="85f35-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="85f35-200">Si no se proporciona el código de estado, el middleware muestra de forma predeterminada 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="85f35-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="85f35-201">Si no se proporciona el puerto, el middleware muestra de forma predeterminada `null`, lo que significa que el protocolo cambia a `https://` y el cliente accede al recurso en el puerto 443.</span><span class="sxs-lookup"><span data-stu-id="85f35-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="85f35-202">En el ejemplo se muestra cómo establecer el código de estado en 301 (Movido definitivamente) y cambiar el puerto a 5001.</span><span class="sxs-lookup"><span data-stu-id="85f35-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="85f35-203">Use `AddRedirectToHttpsPermanent` para redirigir las solicitudes poco seguras al mismo host y ruta de acceso mediante el protocolo HTTPS seguro (`https://` en el puerto 443).</span><span class="sxs-lookup"><span data-stu-id="85f35-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="85f35-204">El middleware establece el código de estado en 301 (Movido definitivamente).</span><span class="sxs-lookup"><span data-stu-id="85f35-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="85f35-205">Al redirigir a HTTPS sin la necesidad de disponer de reglas de redirección adicionales, se recomienda usar el Middleware de redirección de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85f35-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="85f35-206">Para más información, vea el tema [Aplicación de HTTPS](xref:security/enforcing-ssl#require-https).</span><span class="sxs-lookup"><span data-stu-id="85f35-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="85f35-207">La aplicación de ejemplo es capaz de mostrar cómo usar `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="85f35-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="85f35-208">Agregue el método de extensión a `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="85f35-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="85f35-209">Realice una solicitud poco segura a la aplicación en cualquier URL.</span><span class="sxs-lookup"><span data-stu-id="85f35-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="85f35-210">Descarte la advertencia de seguridad del explorador que indica que el certificado autofirmado no es de confianza o cree una excepción para confiar en el certificado en cuestión.</span><span class="sxs-lookup"><span data-stu-id="85f35-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="85f35-211">Solicitud original mediante `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="85f35-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="85f35-213">Solicitud original mediante `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="85f35-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="85f35-215">Reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-215">URL rewrite</span></span>

<span data-ttu-id="85f35-216">Use `AddRewrite` para crear una regla para reescribir URL.</span><span class="sxs-lookup"><span data-stu-id="85f35-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="85f35-217">El primer parámetro contiene la expresión regular para buscar coincidencias en la ruta de dirección de URL entrante.</span><span class="sxs-lookup"><span data-stu-id="85f35-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="85f35-218">El segundo parámetro es la cadena de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="85f35-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="85f35-219">El tercer parámetro, `skipRemainingRules: {true|false}`, indica al middleware si al aplicar la regla actual tiene que omitir o no alguna regla de redirección adicional.</span><span class="sxs-lookup"><span data-stu-id="85f35-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="85f35-220">Solicitud original: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="85f35-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="85f35-222">Lo primero que se observa en la expresión regular es el acento circunflejo (`^`) al principio de la expresión.</span><span class="sxs-lookup"><span data-stu-id="85f35-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="85f35-223">Esto significa que la búsqueda de coincidencias empieza al principio de la ruta de dirección de URL.</span><span class="sxs-lookup"><span data-stu-id="85f35-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="85f35-224">En el ejemplo anterior con la regla de redirección, `redirect-rule/(.*)`, no hay ningún acento circunflejo al principio de la expresión regular; por tanto, cualquier carácter puede preceder a `redirect-rule/` en la ruta de acceso para una coincidencia correcta.</span><span class="sxs-lookup"><span data-stu-id="85f35-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="85f35-225">Ruta de acceso</span><span class="sxs-lookup"><span data-stu-id="85f35-225">Path</span></span>                               | <span data-ttu-id="85f35-226">Coincidir con</span><span class="sxs-lookup"><span data-stu-id="85f35-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="85f35-227">Sí</span><span class="sxs-lookup"><span data-stu-id="85f35-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="85f35-228">Sí</span><span class="sxs-lookup"><span data-stu-id="85f35-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="85f35-229">Sí</span><span class="sxs-lookup"><span data-stu-id="85f35-229">Yes</span></span>   |

<span data-ttu-id="85f35-230">La regla de reescritura, `^rewrite-rule/(\d+)/(\d+)`, solo encuentra rutas de acceso que empiezan con `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="85f35-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="85f35-231">Observe la diferencia de coincidencia entre la siguiente regla de reescritura y la regla de redirección anterior.</span><span class="sxs-lookup"><span data-stu-id="85f35-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="85f35-232">Ruta de acceso</span><span class="sxs-lookup"><span data-stu-id="85f35-232">Path</span></span>                              | <span data-ttu-id="85f35-233">Coincidir con</span><span class="sxs-lookup"><span data-stu-id="85f35-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="85f35-234">Sí</span><span class="sxs-lookup"><span data-stu-id="85f35-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="85f35-235">No</span><span class="sxs-lookup"><span data-stu-id="85f35-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="85f35-236">No</span><span class="sxs-lookup"><span data-stu-id="85f35-236">No</span></span>    |

<span data-ttu-id="85f35-237">Después de la parte `^rewrite-rule/` de la expresión, hay dos grupos de captura, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="85f35-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="85f35-238">`\d` significa *buscar un dígito (número)*.</span><span class="sxs-lookup"><span data-stu-id="85f35-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="85f35-239">El signo más (`+`) significa *buscar uno o más de los caracteres anteriores*.</span><span class="sxs-lookup"><span data-stu-id="85f35-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="85f35-240">Por tanto, la URL debe contener un número seguido de una barra diagonal, seguida de otro número.</span><span class="sxs-lookup"><span data-stu-id="85f35-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="85f35-241">Estos grupos de capturas se insertan en la URL de reescritura como `$1` y `$2`.</span><span class="sxs-lookup"><span data-stu-id="85f35-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="85f35-242">La cadena de reemplazo de la regla de reescritura coloca los grupos capturados en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="85f35-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="85f35-243">La ruta de acceso solicitada de `/rewrite-rule/1234/5678` se reescribe para obtener el recurso en `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="85f35-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="85f35-244">Si una cadena de consulta está presente en la solicitud original, se conserva cuando se reescribe la URL.</span><span class="sxs-lookup"><span data-stu-id="85f35-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="85f35-245">No hay ningún recorrido de ida y vuelta al servidor para obtener el recurso.</span><span class="sxs-lookup"><span data-stu-id="85f35-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="85f35-246">Si el recurso existe, se captura y se devuelve al cliente con un código de estado 200 (Correcto).</span><span class="sxs-lookup"><span data-stu-id="85f35-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="85f35-247">Como el cliente no se redirige, la URL no cambia en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="85f35-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="85f35-248">En lo que al cliente se refiere, la operación de reescritura de URL nunca se produjo.</span><span class="sxs-lookup"><span data-stu-id="85f35-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="85f35-249">Use `skipRemainingRules: true` siempre que sea posible, ya que las reglas de coincidencia son un proceso costoso y reducen el tiempo de respuesta de aplicación.</span><span class="sxs-lookup"><span data-stu-id="85f35-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="85f35-250">Para obtener la respuesta más rápida de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="85f35-250">For the fastest app response:</span></span>
> * <span data-ttu-id="85f35-251">Ordene las reglas de reescritura desde la que coincida con más frecuencia a la que coincida con menos frecuencia.</span><span class="sxs-lookup"><span data-stu-id="85f35-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="85f35-252">Omita el procesamiento de las reglas restantes cuando se produzca una coincidencia; no es necesario ningún procesamiento de reglas adicional.</span><span class="sxs-lookup"><span data-stu-id="85f35-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="85f35-253">mod_rewrite de Apache</span><span class="sxs-lookup"><span data-stu-id="85f35-253">Apache mod_rewrite</span></span>

<span data-ttu-id="85f35-254">Aplique reglas mod_rewrite de Apache con `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="85f35-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="85f35-255">Asegúrese de que el archivo de reglas se implementa con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="85f35-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="85f35-256">Para obtener más información y ejemplos de reglas mod_rewrite, vea [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) (mod_rewrite de Apache).</span><span class="sxs-lookup"><span data-stu-id="85f35-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="85f35-257">Se usa `StreamReader` para leer las reglas del archivo de reglas *ApacheModRewrite.txt*.</span><span class="sxs-lookup"><span data-stu-id="85f35-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="85f35-258">El primer parámetro toma un `IFileProvider`, que se proporciona mediante una [inserción de dependencias](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="85f35-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="85f35-259">Se inserta `IHostingEnvironment` para proporcionar `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="85f35-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="85f35-260">El segundo parámetro es la ruta de acceso al archivo de reglas, que es *ApacheModRewrite.txt* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="85f35-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="85f35-261">La aplicación de ejemplo redirige las solicitudes de `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="85f35-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="85f35-262">El código de estado de la respuesta es 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="85f35-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="85f35-263">Solicitud original: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="85f35-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="85f35-265">El middleware admite las siguientes variables de servidor mod_rewrite de Apache:</span><span class="sxs-lookup"><span data-stu-id="85f35-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="85f35-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="85f35-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="85f35-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="85f35-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="85f35-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="85f35-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="85f35-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="85f35-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="85f35-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="85f35-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="85f35-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="85f35-271">HTTP_HOST</span></span>
* <span data-ttu-id="85f35-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="85f35-272">HTTP_REFERER</span></span>
* <span data-ttu-id="85f35-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="85f35-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="85f35-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="85f35-274">HTTPS</span></span>
* <span data-ttu-id="85f35-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="85f35-275">IPV6</span></span>
* <span data-ttu-id="85f35-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="85f35-276">QUERY_STRING</span></span>
* <span data-ttu-id="85f35-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="85f35-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="85f35-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="85f35-278">REMOTE_PORT</span></span>
* <span data-ttu-id="85f35-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="85f35-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="85f35-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="85f35-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="85f35-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="85f35-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="85f35-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="85f35-282">REQUEST_URI</span></span>
* <span data-ttu-id="85f35-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="85f35-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="85f35-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="85f35-284">SERVER_ADDR</span></span>
* <span data-ttu-id="85f35-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="85f35-285">SERVER_PORT</span></span>
* <span data-ttu-id="85f35-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="85f35-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="85f35-287">TIME</span><span class="sxs-lookup"><span data-stu-id="85f35-287">TIME</span></span>
* <span data-ttu-id="85f35-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="85f35-288">TIME_DAY</span></span>
* <span data-ttu-id="85f35-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="85f35-289">TIME_HOUR</span></span>
* <span data-ttu-id="85f35-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="85f35-290">TIME_MIN</span></span>
* <span data-ttu-id="85f35-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="85f35-291">TIME_MON</span></span>
* <span data-ttu-id="85f35-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="85f35-292">TIME_SEC</span></span>
* <span data-ttu-id="85f35-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="85f35-293">TIME_WDAY</span></span>
* <span data-ttu-id="85f35-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="85f35-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="85f35-295">Reglas del Módulo URL Rewrite para IIS</span><span class="sxs-lookup"><span data-stu-id="85f35-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="85f35-296">Para usar reglas que se apliquen al Módulo URL Rewrite para IIS, use `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="85f35-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="85f35-297">Asegúrese de que el archivo de reglas se implementa con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="85f35-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="85f35-298">No dirija el middleware para que use el archivo *web.config* cuando se ejecute en Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="85f35-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="85f35-299">Con IIS, estas reglas deben almacenarse fuera de *web.config* para evitar conflictos con el Módulo URL Rewrite para IIS.</span><span class="sxs-lookup"><span data-stu-id="85f35-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="85f35-300">Para obtener más información y ejemplos de reglas del Módulo URL Rewrite para IIS, vea [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso del Módulo URL Rewrite 2.0) y [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Referencia de configuración del Módulo URL Rewrite).</span><span class="sxs-lookup"><span data-stu-id="85f35-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="85f35-301">Se usa `StreamReader` para leer las reglas del archivo de reglas *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="85f35-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="85f35-302">El primer parámetro toma `IFileProvider`, mientras que el segundo parámetro es la ruta de acceso al archivo de reglas XML, que en la aplicación de ejemplo es *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="85f35-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="85f35-303">La aplicación de ejemplo reescribe las solicitudes de `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="85f35-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="85f35-304">La respuesta se envía al cliente con un código de estado 200 (Correcto).</span><span class="sxs-lookup"><span data-stu-id="85f35-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="85f35-305">Solicitud original: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="85f35-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="85f35-307">Si tiene un Módulo URL Rewrite para IIS activo con reglas configuradas en el nivel de servidor que podrían afectar a la aplicación de manera no deseada, puede deshabilitar el Módulo URL Rewrite para IIS para una aplicación.</span><span class="sxs-lookup"><span data-stu-id="85f35-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="85f35-308">Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).</span><span class="sxs-lookup"><span data-stu-id="85f35-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="85f35-309">Características no admitidas</span><span class="sxs-lookup"><span data-stu-id="85f35-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="85f35-310">El middleware publicado con ASP.NET Core 2.x no admite las siguientes características de Módulo URL Rewrite para IIS:</span><span class="sxs-lookup"><span data-stu-id="85f35-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="85f35-311">Reglas de salida</span><span class="sxs-lookup"><span data-stu-id="85f35-311">Outbound Rules</span></span>
* <span data-ttu-id="85f35-312">Variables de servidor personalizadas</span><span class="sxs-lookup"><span data-stu-id="85f35-312">Custom Server Variables</span></span>
* <span data-ttu-id="85f35-313">Caracteres comodín</span><span class="sxs-lookup"><span data-stu-id="85f35-313">Wildcards</span></span>
* <span data-ttu-id="85f35-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="85f35-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="85f35-315">El middleware publicado con ASP.NET Core 1.x no admite las siguientes características de Módulo URL Rewrite para IIS:</span><span class="sxs-lookup"><span data-stu-id="85f35-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="85f35-316">Reglas globales</span><span class="sxs-lookup"><span data-stu-id="85f35-316">Global Rules</span></span>
* <span data-ttu-id="85f35-317">Reglas de salida</span><span class="sxs-lookup"><span data-stu-id="85f35-317">Outbound Rules</span></span>
* <span data-ttu-id="85f35-318">Mapas de reescritura</span><span class="sxs-lookup"><span data-stu-id="85f35-318">Rewrite Maps</span></span>
* <span data-ttu-id="85f35-319">Acción CustomResponse</span><span class="sxs-lookup"><span data-stu-id="85f35-319">CustomResponse Action</span></span>
* <span data-ttu-id="85f35-320">Variables de servidor personalizadas</span><span class="sxs-lookup"><span data-stu-id="85f35-320">Custom Server Variables</span></span>
* <span data-ttu-id="85f35-321">Caracteres comodín</span><span class="sxs-lookup"><span data-stu-id="85f35-321">Wildcards</span></span>
* <span data-ttu-id="85f35-322">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="85f35-322">Action:CustomResponse</span></span>
* <span data-ttu-id="85f35-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="85f35-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="85f35-324">Variables de servidor compatibles</span><span class="sxs-lookup"><span data-stu-id="85f35-324">Supported server variables</span></span>

<span data-ttu-id="85f35-325">El middleware admite las siguientes variables de servidor del Módulo URL Rewrite para IIS:</span><span class="sxs-lookup"><span data-stu-id="85f35-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="85f35-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="85f35-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="85f35-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="85f35-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="85f35-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="85f35-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="85f35-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="85f35-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="85f35-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="85f35-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="85f35-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="85f35-331">HTTP_HOST</span></span>
* <span data-ttu-id="85f35-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="85f35-332">HTTP_REFERER</span></span>
* <span data-ttu-id="85f35-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="85f35-333">HTTP_URL</span></span>
* <span data-ttu-id="85f35-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="85f35-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="85f35-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="85f35-335">HTTPS</span></span>
* <span data-ttu-id="85f35-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="85f35-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="85f35-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="85f35-337">QUERY_STRING</span></span>
* <span data-ttu-id="85f35-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="85f35-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="85f35-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="85f35-339">REMOTE_PORT</span></span>
* <span data-ttu-id="85f35-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="85f35-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="85f35-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="85f35-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="85f35-342">También puede obtener `IFileProvider` a través de `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="85f35-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="85f35-343">Con este enfoque logrará mayor flexibilidad para la ubicación de los archivos de reglas de reescritura.</span><span class="sxs-lookup"><span data-stu-id="85f35-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="85f35-344">Asegúrese de que los archivos de reglas de reescritura se implementan en el servidor en la ruta de acceso que proporcione.</span><span class="sxs-lookup"><span data-stu-id="85f35-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="85f35-345">Regla basada en métodos</span><span class="sxs-lookup"><span data-stu-id="85f35-345">Method-based rule</span></span>

<span data-ttu-id="85f35-346">Use `Add(Action<RewriteContext> applyRule)` para implementar su propia lógica de la regla en un método.</span><span class="sxs-lookup"><span data-stu-id="85f35-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="85f35-347">`RewriteContext` expone el `HttpContext` para usarlo en el método.</span><span class="sxs-lookup"><span data-stu-id="85f35-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="85f35-348">`context.Result` determina cómo se administra el procesamiento adicional en la canalización.</span><span class="sxs-lookup"><span data-stu-id="85f35-348">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="85f35-349">context.Result</span><span class="sxs-lookup"><span data-stu-id="85f35-349">context.Result</span></span>                       | <span data-ttu-id="85f35-350">Acción</span><span class="sxs-lookup"><span data-stu-id="85f35-350">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="85f35-351">`RuleResult.ContinueRules` (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="85f35-351">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="85f35-352">Continuar aplicando reglas</span><span class="sxs-lookup"><span data-stu-id="85f35-352">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="85f35-353">Dejar de aplicar reglas y enviar la respuesta</span><span class="sxs-lookup"><span data-stu-id="85f35-353">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="85f35-354">Dejar de aplicar reglas y enviar el contexto al siguiente middleware</span><span class="sxs-lookup"><span data-stu-id="85f35-354">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="85f35-355">La aplicación de ejemplo muestra un método que redirige las solicitudes para las rutas de acceso que terminen con *.xml*.</span><span class="sxs-lookup"><span data-stu-id="85f35-355">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="85f35-356">Si realiza una solicitud para `/file.xml`, se redirige a `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="85f35-356">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="85f35-357">El código de estado se establece en 301 (Movido definitivamente).</span><span class="sxs-lookup"><span data-stu-id="85f35-357">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="85f35-358">Para una redirección, debe establecer explícitamente el código de estado de la respuesta; en caso contrario, se devuelve un código de estado 200 (Correcto) y no se produce la redirección en el cliente.</span><span class="sxs-lookup"><span data-stu-id="85f35-358">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="85f35-359">Solicitud original: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="85f35-359">Original Request: `/file.xml`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="85f35-361">Regla basada en IRule</span><span class="sxs-lookup"><span data-stu-id="85f35-361">IRule-based rule</span></span>

<span data-ttu-id="85f35-362">Use `Add(IRule)` para implementar su propia lógica de la regla en una clase que deriva de `IRule`.</span><span class="sxs-lookup"><span data-stu-id="85f35-362">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="85f35-363">Al usar `IRule`, se logra mayor flexibilidad que si se usa el enfoque de reglas basadas en métodos.</span><span class="sxs-lookup"><span data-stu-id="85f35-363">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="85f35-364">La clase derivada puede incluir un constructor, donde puede pasar parámetros para el método `ApplyRule`.</span><span class="sxs-lookup"><span data-stu-id="85f35-364">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="85f35-365">Se comprueba que los valores de los parámetros en la aplicación de ejemplo para `extension` y `newPath` cumplen ciertas condiciones.</span><span class="sxs-lookup"><span data-stu-id="85f35-365">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="85f35-366">`extension` debe contener un valor, que debe ser *.png*, *.jpg* o *.gif*.</span><span class="sxs-lookup"><span data-stu-id="85f35-366">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="85f35-367">Si `newPath` no es válido, se genera `ArgumentException` .</span><span class="sxs-lookup"><span data-stu-id="85f35-367">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="85f35-368">Si se realiza una solicitud para *image.png*, se redirige a `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="85f35-368">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="85f35-369">Si se realiza una solicitud para *image.jpg*, se redirige a `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="85f35-369">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="85f35-370">El código de estado se establece en 301 (Movido definitivamente) y se establece `context.Result` para detener el procesamiento de reglas y enviar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="85f35-370">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="85f35-371">Solicitud original: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="85f35-371">Original Request: `/image.png`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="85f35-373">Solicitud original: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="85f35-373">Original Request: `/image.jpg`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="85f35-375">Ejemplos de expresiones regulares</span><span class="sxs-lookup"><span data-stu-id="85f35-375">Regex examples</span></span>

| <span data-ttu-id="85f35-376">Objetivo</span><span class="sxs-lookup"><span data-stu-id="85f35-376">Goal</span></span> | <span data-ttu-id="85f35-377">Cadena de expresión regular &</span><span class="sxs-lookup"><span data-stu-id="85f35-377">Regex String &</span></span><br><span data-ttu-id="85f35-378">Ejemplo de coincidencia</span><span class="sxs-lookup"><span data-stu-id="85f35-378">Match Example</span></span> | <span data-ttu-id="85f35-379">Cadena de reemplazo &</span><span class="sxs-lookup"><span data-stu-id="85f35-379">Replacement String &</span></span><br><span data-ttu-id="85f35-380">Ejemplo de resultado</span><span class="sxs-lookup"><span data-stu-id="85f35-380">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="85f35-381">Ruta de acceso de reescritura en la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="85f35-381">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="85f35-382">Quitar barra diagonal final</span><span class="sxs-lookup"><span data-stu-id="85f35-382">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="85f35-383">Exigir barra diagonal final</span><span class="sxs-lookup"><span data-stu-id="85f35-383">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="85f35-384">Evitar reescritura de solicitudes específicas</span><span class="sxs-lookup"><span data-stu-id="85f35-384">Avoid rewriting specific requests</span></span> | <span data-ttu-id="85f35-385">`^(.*)(?<!\.axd)$` o `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="85f35-385">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="85f35-386">Sí: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="85f35-386">Yes: `/resource.htm`</span></span><br><span data-ttu-id="85f35-387">No: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="85f35-387">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="85f35-388">Reorganizar los segmentos de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-388">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="85f35-389">Reemplazar un segmento de URL</span><span class="sxs-lookup"><span data-stu-id="85f35-389">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="85f35-390">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="85f35-390">Additional resources</span></span>

* [<span data-ttu-id="85f35-391">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="85f35-391">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="85f35-392">Middleware</span><span class="sxs-lookup"><span data-stu-id="85f35-392">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="85f35-393">Expresiones regulares en .NET</span><span class="sxs-lookup"><span data-stu-id="85f35-393">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="85f35-394">Lenguaje de expresiones regulares: referencia rápida</span><span class="sxs-lookup"><span data-stu-id="85f35-394">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* <span data-ttu-id="85f35-395">[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) (mod_rewrite de Apache)</span><span class="sxs-lookup"><span data-stu-id="85f35-395">[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)</span></span>
* <span data-ttu-id="85f35-396">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso del Módulo URL Rewrite 2.0 para IIS)</span><span class="sxs-lookup"><span data-stu-id="85f35-396">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="85f35-397">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Referencia de configuración del Módulo URL Rewrite)</span><span class="sxs-lookup"><span data-stu-id="85f35-397">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* <span data-ttu-id="85f35-398">[IIS URL Rewrite Module Forum](https://forums.iis.net/1152.aspx) (Foro del Módulo URL Rewrite para IIS)</span><span class="sxs-lookup"><span data-stu-id="85f35-398">[IIS URL Rewrite Module Forum](https://forums.iis.net/1152.aspx)</span></span>
* [<span data-ttu-id="85f35-399">Cómo simplificar la estructura de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="85f35-399">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* <span data-ttu-id="85f35-400">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 trucos y consejos para reescritura de URL)</span><span class="sxs-lookup"><span data-stu-id="85f35-400">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="85f35-401">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (Usar la barra diagonal o no)</span><span class="sxs-lookup"><span data-stu-id="85f35-401">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
