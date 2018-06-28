---
title: Middleware de reescritura de URL en ASP.NET Core
author: guardrex
description: Aprenda a reescribir y redireccionar URL con el middleware de reescritura de URL en aplicaciones ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: a4ffa512825fedafdc58ade9929097e255593fa9
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652219"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="5ae1b-103">Middleware de reescritura de URL en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ae1b-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="5ae1b-104">Por [Luke Latham](https://github.com/guardrex) y [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="5ae1b-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5ae1b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5ae1b-106">La reescritura de URL consiste en modificar varias URL de solicitud basadas en una o varias reglas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="5ae1b-107">La reescritura de URL crea una abstracción entre las ubicaciones de recursos y sus direcciones para que las ubicaciones y direcciones no estén estrechamente vinculadas.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="5ae1b-108">Hay varias situaciones en las que la reescritura de URL es útil:</span><span class="sxs-lookup"><span data-stu-id="5ae1b-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="5ae1b-109">Mover o reemplazar recursos del servidor de forma temporal o permanente a la vez que se mantienen estables los localizadores para esos recursos.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="5ae1b-110">Dividir el procesamiento de solicitudes entre distintas aplicaciones o entre áreas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="5ae1b-111">Quitar, agregar o reorganizar segmentos de URL en solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="5ae1b-112">Optimizar URL públicas para la optimización del motor de búsqueda (SEO).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="5ae1b-113">Permitir el uso de URL públicas preparadas para ayudar a los usuarios a predecir el contenido que encontrarán siguiendo un vínculo.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="5ae1b-114">Redirigir solicitudes no seguras para proteger puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="5ae1b-115">Evitar la creación de vínculos activos de imagen.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="5ae1b-116">Puede definir reglas para cambiar la URL de varias maneras, incluidas expresiones regulares, reglas del módulo mod_rewrite de Apache, reglas del módulo de reescritura de IIS y lógica de regla personalizada.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="5ae1b-117">En este documento se ofrece una introducción a la reescritura de URL e instrucciones sobre cómo usar el middleware de reescritura de URL en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="5ae1b-118">La reescritura de URL puede reducir el rendimiento de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="5ae1b-119">Cuando sea factible, debe limitar el número y la complejidad de las reglas.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="5ae1b-120">Redireccionamiento y reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="5ae1b-121">La diferencia entre los términos *redirección de URL* y *reescritura de URL* en principio puede parecer sutil, pero tiene importantes implicaciones para proporcionar recursos a los clientes.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="5ae1b-122">El middleware de reescritura de URL de ASP.NET Core es capaz de satisfacer las necesidades de ambos.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="5ae1b-123">La *redirección de URL* es una operación del lado cliente, que da la instrucción al cliente de acceder a un recurso en otra dirección.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="5ae1b-124">Esto requiere un recorrido de ida y vuelta al servidor.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="5ae1b-125">La URL de redireccionamiento que se devuelve al cliente aparece en la barra de direcciones del explorador cuando el cliente realiza una nueva solicitud para el recurso.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="5ae1b-126">Si `/resource` se *redirige* a `/different-resource`, el cliente solicita `/resource`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="5ae1b-127">El servidor responde que el cliente debe obtener el recurso en `/different-resource` con un código de estado que indica que la redirección es temporal o permanente.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="5ae1b-128">El cliente ejecuta una nueva solicitud para el recurso en la URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Se cambió temporalmente un punto de conexión del servicio WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="5ae1b-134">Al redirigir las solicitudes a una URL diferente, se indica si la redirección es permanente o temporal.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="5ae1b-135">El código de estado 301 (Movido definitivamente) se usa cuando el recurso tiene una nueva URL permanente y quiere indicar al cliente que todas las solicitudes futuras para el recurso deben usar la nueva URL.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="5ae1b-136">*El cliente puede almacenar en caché la respuesta cuando se recibe un código de estado 301.*</span><span class="sxs-lookup"><span data-stu-id="5ae1b-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="5ae1b-137">El código de estado 302 (Encontrado) se usa cuando la redirección es temporal o normalmente está sujeta a cambios, de modo que el cliente no debe almacenar y reutilizar la URL de redireccionamiento en el futuro.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="5ae1b-138">Para más información, vea [RFC 2616: definiciones de código de estado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="5ae1b-139">La *reescritura de URL* es una operación del lado servidor que proporciona un recurso desde una dirección de recursos distinta.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="5ae1b-140">La reescritura de una URL no requiere un recorrido de ida y vuelta al servidor.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="5ae1b-141">La dirección URL reescrita no se devuelve al cliente y no aparece en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="5ae1b-142">Cuando `/resource` se *reescribe* como `/different-resource`, el cliente solicita `/resource` y el servidor recupera *internamente* el recurso en `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="5ae1b-143">Aunque el cliente podría recuperar el recurso en la URL reescrita, el cliente no informará de que el recurso existe en la URL reescrita cuando realice su solicitud y reciba la respuesta.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Se cambió un punto de conexión del servicio WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="5ae1b-148">Aplicación de ejemplo de reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-148">URL rewriting sample app</span></span>

<span data-ttu-id="5ae1b-149">Puede explorar las características del middleware de reescritura de URL con la [aplicación de ejemplo de reescritura de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="5ae1b-150">Esta aplicación aplica reglas de reescritura y redirección, además de mostrar la URL redirigida o reescrita.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="5ae1b-151">Cuándo usar el middleware de reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="5ae1b-152">Use el middleware de reescritura de URL cuando no pueda usar el [módulo de reescritura de URL](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS en Windows Server, el [módulo mod_rewrite de Apache](https://httpd.apache.org/docs/2.4/rewrite/) en el servidor Apache, la [reescritura de URL en Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) o cuando la aplicación esté hospedada en el [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (antes denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="5ae1b-153">Las principales razones para usar las tecnologías de reescritura de URL basadas en servidor en IIS, Apache o Nginx son que el middleware no es compatible con todas las características de estos módulos y el rendimiento del middleware probablemente no coincida con el de los módulos.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="5ae1b-154">Pero algunas características de los módulos de servidor no funcionan con proyectos de ASP.NET Core, como las restricciones `IsFile` y `IsDirectory` del módulo de reescritura de IIS.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="5ae1b-155">En estos casos, es mejor usar el middleware.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="5ae1b-156">Package</span><span class="sxs-lookup"><span data-stu-id="5ae1b-156">Package</span></span>

<span data-ttu-id="5ae1b-157">Para incluir el middleware en el proyecto, agregue una referencia al paquete [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="5ae1b-158">Esta característica está disponible para aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="5ae1b-159">Extensión y opciones</span><span class="sxs-lookup"><span data-stu-id="5ae1b-159">Extension and options</span></span>

<span data-ttu-id="5ae1b-160">Establezca las reglas de reescritura y redirección de URL mediante la creación de una instancia de la clase `RewriteOptions` con métodos de extensión para cada una de las reglas.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="5ae1b-161">Encadene varias reglas en el orden que quiera procesarlas.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="5ae1b-162">Las `RewriteOptions` se pasan al middleware de reescritura de URL cuando se agregan a la canalización de solicitudes con `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

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

---

### <a name="url-redirect"></a><span data-ttu-id="5ae1b-165">Redirección de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-165">URL redirect</span></span>

<span data-ttu-id="5ae1b-166">Use `AddRedirect` para redirigir las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="5ae1b-167">El primer parámetro contiene la expresión regular para hacer coincidir la ruta de acceso de la URL entrante.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="5ae1b-168">El segundo parámetro es la cadena de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="5ae1b-169">El tercer parámetro, si está presente, especifica el código de estado.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="5ae1b-170">Si no se especifica el código de estado, el valor predeterminado es 302 (Encontrado), lo que indica que el recurso se ha movido o reemplazado temporalmente.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="5ae1b-173">En un explorador con herramientas de desarrollo habilitadas, realice una solicitud a la aplicación de ejemplo con la ruta de acceso `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="5ae1b-174">La expresión regular coincide con la ruta de acceso de la solicitud en `redirect-rule/(.*)` y la ruta de acceso se reemplaza con `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="5ae1b-175">La URL de redireccionamiento se devuelve al cliente con un código de estado 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="5ae1b-176">El explorador realiza una solicitud nueva en la URL de redireccionamiento, que aparece en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="5ae1b-177">Puesto que no hay ninguna regla en la aplicación de ejemplo que coincida con la URL de redireccionamiento, la segunda solicitud recibe una respuesta 200 (Correcto) de la aplicación y el cuerpo de la respuesta muestra la URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="5ae1b-178">Se realiza un recorrido de ida y vuelta al servidor cuando se *redirige* una URL.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="5ae1b-179">Tenga cuidado al establecer las reglas de redirección.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="5ae1b-180">Las reglas de redirección se evalúan en cada solicitud enviada a la aplicación, incluso después de una redirección.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="5ae1b-181">Es fácil crear por error un bucle de redirecciones infinitas.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="5ae1b-182">Solicitud original: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="5ae1b-184">La parte de la expresión incluida entre paréntesis se denomina *grupo de capturas*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="5ae1b-185">El punto (`.`) de la expresión significa *buscar cualquier carácter*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="5ae1b-186">El asterisco (`*`) indica *buscar el carácter precedente cero o más veces*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="5ae1b-187">Por tanto, el grupo de capturas `(.*)` busca los dos últimos segmentos de la ruta de acceso de la URL, `1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="5ae1b-188">Cualquier valor que se proporcione en la URL de la solicitud después de `redirect-rule/` es capturado por este grupo de capturas único.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="5ae1b-189">En la cadena de reemplazo, se insertan los grupos capturados en la cadena con el signo de dólar (`$`) seguido del número de secuencia de la captura.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="5ae1b-190">Se obtiene el valor del primer grupo de capturas con `$1`, el segundo con `$2`, y así siguen en secuencia para los grupos de capturas de la expresión regular.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="5ae1b-191">Solo hay un grupo capturado en la expresión regular de la regla de redirección de la aplicación de ejemplo, por lo que solo hay un grupo insertado en la cadena de reemplazo, que es `$1`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="5ae1b-192">Cuando se aplica la regla, la URL se convierte en `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="5ae1b-193">Redirección de URL a un punto de conexión segura</span><span class="sxs-lookup"><span data-stu-id="5ae1b-193">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="5ae1b-194">Use `AddRedirectToHttps` para redirigir solicitudes HTTP al mismo host y ruta con HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="5ae1b-195">Si no se proporciona el código de estado, el middleware muestra de forma predeterminada 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="5ae1b-196">Si no se proporciona el puerto, el middleware muestra de forma predeterminada `null`, lo que significa que el protocolo cambia a `https://` y el cliente accede al recurso en el puerto 443.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="5ae1b-197">En el ejemplo se muestra cómo establecer el código de estado en 301 (Movido definitivamente) y cambiar el puerto a 5001.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="5ae1b-198">Use `AddRedirectToHttpsPermanent` para redirigir las solicitudes poco seguras al mismo host y ruta de acceso mediante el protocolo HTTPS seguro (`https://` en el puerto 443).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="5ae1b-199">El middleware establece el código de estado en 301 (Movido definitivamente).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="5ae1b-200">Al redirigir a HTTPS sin la necesidad de disponer de reglas de redirección adicionales, se recomienda usar el Middleware de redirección de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-200">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="5ae1b-201">Para más información, vea el tema [Aplicación de HTTPS](xref:security/enforcing-ssl#require-https).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-201">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="5ae1b-202">La aplicación de ejemplo es capaz de mostrar cómo usar `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-202">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="5ae1b-203">Agregue el método de extensión a `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-203">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="5ae1b-204">Realice una solicitud poco segura a la aplicación en cualquier URL.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-204">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="5ae1b-205">Descarte la advertencia de seguridad del explorador que indica que el certificado autofirmado no es de confianza o cree una excepción para confiar en el certificado en cuestión.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-205">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="5ae1b-206">Solicitud original mediante `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-206">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="5ae1b-208">Solicitud original mediante `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-208">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="5ae1b-210">Reescritura de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-210">URL rewrite</span></span>

<span data-ttu-id="5ae1b-211">Use `AddRewrite` para crear una regla para reescribir URL.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-211">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="5ae1b-212">El primer parámetro contiene la expresión regular para buscar coincidencias en la ruta de dirección de URL entrante.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-212">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="5ae1b-213">El segundo parámetro es la cadena de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-213">The second parameter is the replacement string.</span></span> <span data-ttu-id="5ae1b-214">El tercer parámetro, `skipRemainingRules: {true|false}`, indica al middleware si al aplicar la regla actual tiene que omitir o no alguna regla de redirección adicional.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-214">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="5ae1b-217">Solicitud original: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-217">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="5ae1b-219">Lo primero que se observa en la expresión regular es el acento circunflejo (`^`) al principio de la expresión.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-219">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="5ae1b-220">Esto significa que la búsqueda de coincidencias empieza al principio de la ruta de dirección de URL.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-220">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="5ae1b-221">En el ejemplo anterior con la regla de redirección, `redirect-rule/(.*)`, no hay ningún acento circunflejo al principio de la expresión regular; por tanto, cualquier carácter puede preceder a `redirect-rule/` en la ruta de acceso para una coincidencia correcta.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-221">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="5ae1b-222">Ruta de acceso</span><span class="sxs-lookup"><span data-stu-id="5ae1b-222">Path</span></span>                               | <span data-ttu-id="5ae1b-223">Coincidir con</span><span class="sxs-lookup"><span data-stu-id="5ae1b-223">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="5ae1b-224">Sí</span><span class="sxs-lookup"><span data-stu-id="5ae1b-224">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="5ae1b-225">Sí</span><span class="sxs-lookup"><span data-stu-id="5ae1b-225">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="5ae1b-226">Sí</span><span class="sxs-lookup"><span data-stu-id="5ae1b-226">Yes</span></span>   |

<span data-ttu-id="5ae1b-227">La regla de reescritura, `^rewrite-rule/(\d+)/(\d+)`, solo encuentra rutas de acceso que empiezan con `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-227">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="5ae1b-228">Observe la diferencia de coincidencia entre la siguiente regla de reescritura y la regla de redirección anterior.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-228">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="5ae1b-229">Ruta de acceso</span><span class="sxs-lookup"><span data-stu-id="5ae1b-229">Path</span></span>                              | <span data-ttu-id="5ae1b-230">Coincidir con</span><span class="sxs-lookup"><span data-stu-id="5ae1b-230">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="5ae1b-231">Sí</span><span class="sxs-lookup"><span data-stu-id="5ae1b-231">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="5ae1b-232">No</span><span class="sxs-lookup"><span data-stu-id="5ae1b-232">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="5ae1b-233">No</span><span class="sxs-lookup"><span data-stu-id="5ae1b-233">No</span></span>    |

<span data-ttu-id="5ae1b-234">Después de la parte `^rewrite-rule/` de la expresión, hay dos grupos de captura, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-234">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="5ae1b-235">`\d` significa *buscar un dígito (número)*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-235">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="5ae1b-236">El signo más (`+`) significa *buscar uno o más de los caracteres anteriores*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-236">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="5ae1b-237">Por tanto, la URL debe contener un número seguido de una barra diagonal, seguida de otro número.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-237">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="5ae1b-238">Estos grupos de capturas se insertan en la URL de reescritura como `$1` y `$2`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-238">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="5ae1b-239">La cadena de reemplazo de la regla de reescritura coloca los grupos capturados en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-239">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="5ae1b-240">La ruta de acceso solicitada de `/rewrite-rule/1234/5678` se reescribe para obtener el recurso en `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-240">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="5ae1b-241">Si una cadena de consulta está presente en la solicitud original, se conserva cuando se reescribe la URL.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-241">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="5ae1b-242">No hay ningún recorrido de ida y vuelta al servidor para obtener el recurso.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-242">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="5ae1b-243">Si el recurso existe, se captura y se devuelve al cliente con un código de estado 200 (Correcto).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-243">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="5ae1b-244">Como el cliente no se redirige, la URL no cambia en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-244">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="5ae1b-245">En lo que al cliente se refiere, la operación de reescritura de URL nunca se produjo.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-245">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="5ae1b-246">Use `skipRemainingRules: true` siempre que sea posible, ya que las reglas de coincidencia son un proceso costoso y reducen el tiempo de respuesta de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-246">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="5ae1b-247">Para obtener la respuesta más rápida de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5ae1b-247">For the fastest app response:</span></span>
> * <span data-ttu-id="5ae1b-248">Ordene las reglas de reescritura desde la que coincida con más frecuencia a la que coincida con menos frecuencia.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-248">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="5ae1b-249">Omita el procesamiento de las reglas restantes cuando se produzca una coincidencia; no es necesario ningún procesamiento de reglas adicional.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-249">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="5ae1b-250">mod_rewrite de Apache</span><span class="sxs-lookup"><span data-stu-id="5ae1b-250">Apache mod_rewrite</span></span>

<span data-ttu-id="5ae1b-251">Aplique reglas mod_rewrite de Apache con `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-251">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="5ae1b-252">Asegúrese de que el archivo de reglas se implementa con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-252">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="5ae1b-253">Para obtener más información y ejemplos de reglas mod_rewrite, vea [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) (mod_rewrite de Apache).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-253">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="5ae1b-255">Se usa `StreamReader` para leer las reglas del archivo de reglas *ApacheModRewrite.txt*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-255">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-256">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-256">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5ae1b-257">El primer parámetro toma un `IFileProvider`, que se proporciona mediante una [inserción de dependencias](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-257">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="5ae1b-258">Se inserta `IHostingEnvironment` para proporcionar `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-258">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="5ae1b-259">El segundo parámetro es la ruta de acceso al archivo de reglas, que es *ApacheModRewrite.txt* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-259">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="5ae1b-260">La aplicación de ejemplo redirige las solicitudes de `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-260">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="5ae1b-261">El código de estado de la respuesta es 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-261">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="5ae1b-262">Solicitud original: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-262">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="5ae1b-264">Variables de servidor compatibles</span><span class="sxs-lookup"><span data-stu-id="5ae1b-264">Supported server variables</span></span>

<span data-ttu-id="5ae1b-265">El middleware admite las siguientes variables de servidor mod_rewrite de Apache:</span><span class="sxs-lookup"><span data-stu-id="5ae1b-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="5ae1b-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="5ae1b-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="5ae1b-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="5ae1b-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="5ae1b-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="5ae1b-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="5ae1b-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="5ae1b-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="5ae1b-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="5ae1b-271">HTTP_HOST</span></span>
* <span data-ttu-id="5ae1b-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="5ae1b-272">HTTP_REFERER</span></span>
* <span data-ttu-id="5ae1b-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="5ae1b-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="5ae1b-274">HTTPS</span></span>
* <span data-ttu-id="5ae1b-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="5ae1b-275">IPV6</span></span>
* <span data-ttu-id="5ae1b-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="5ae1b-276">QUERY_STRING</span></span>
* <span data-ttu-id="5ae1b-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="5ae1b-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-278">REMOTE_PORT</span></span>
* <span data-ttu-id="5ae1b-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="5ae1b-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="5ae1b-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="5ae1b-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="5ae1b-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="5ae1b-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="5ae1b-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="5ae1b-282">REQUEST_URI</span></span>
* <span data-ttu-id="5ae1b-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="5ae1b-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="5ae1b-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-284">SERVER_ADDR</span></span>
* <span data-ttu-id="5ae1b-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-285">SERVER_PORT</span></span>
* <span data-ttu-id="5ae1b-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="5ae1b-287">TIME</span><span class="sxs-lookup"><span data-stu-id="5ae1b-287">TIME</span></span>
* <span data-ttu-id="5ae1b-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="5ae1b-288">TIME_DAY</span></span>
* <span data-ttu-id="5ae1b-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-289">TIME_HOUR</span></span>
* <span data-ttu-id="5ae1b-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="5ae1b-290">TIME_MIN</span></span>
* <span data-ttu-id="5ae1b-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="5ae1b-291">TIME_MON</span></span>
* <span data-ttu-id="5ae1b-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="5ae1b-292">TIME_SEC</span></span>
* <span data-ttu-id="5ae1b-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="5ae1b-293">TIME_WDAY</span></span>
* <span data-ttu-id="5ae1b-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="5ae1b-295">Reglas del Módulo URL Rewrite para IIS</span><span class="sxs-lookup"><span data-stu-id="5ae1b-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="5ae1b-296">Para usar reglas que se apliquen al Módulo URL Rewrite para IIS, use `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="5ae1b-297">Asegúrese de que el archivo de reglas se implementa con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="5ae1b-298">No dirija el middleware para que use el archivo *web.config* cuando se ejecute en Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="5ae1b-299">Con IIS, estas reglas deben almacenarse fuera de *web.config* para evitar conflictos con el Módulo URL Rewrite para IIS.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="5ae1b-300">Para obtener más información y ejemplos de reglas del Módulo URL Rewrite para IIS, vea [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso del Módulo URL Rewrite 2.0) y [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Referencia de configuración del Módulo URL Rewrite).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="5ae1b-302">Se usa `StreamReader` para leer las reglas del archivo de reglas *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-302">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5ae1b-304">El primer parámetro toma `IFileProvider`, mientras que el segundo parámetro es la ruta de acceso al archivo de reglas XML, que en la aplicación de ejemplo es *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-304">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="5ae1b-305">La aplicación de ejemplo reescribe las solicitudes de `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-305">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="5ae1b-306">La respuesta se envía al cliente con un código de estado 200 (Correcto).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-306">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="5ae1b-307">Solicitud original: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-307">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="5ae1b-309">Si tiene un Módulo URL Rewrite para IIS activo con reglas configuradas en el nivel de servidor que podrían afectar a la aplicación de manera no deseada, puede deshabilitar el Módulo URL Rewrite para IIS para una aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-309">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="5ae1b-310">Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-310">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="5ae1b-311">Características no admitidas</span><span class="sxs-lookup"><span data-stu-id="5ae1b-311">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5ae1b-313">El middleware publicado con ASP.NET Core 2.x no admite las siguientes características de Módulo URL Rewrite para IIS:</span><span class="sxs-lookup"><span data-stu-id="5ae1b-313">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="5ae1b-314">Reglas de salida</span><span class="sxs-lookup"><span data-stu-id="5ae1b-314">Outbound Rules</span></span>
* <span data-ttu-id="5ae1b-315">Variables de servidor personalizadas</span><span class="sxs-lookup"><span data-stu-id="5ae1b-315">Custom Server Variables</span></span>
* <span data-ttu-id="5ae1b-316">Caracteres comodín</span><span class="sxs-lookup"><span data-stu-id="5ae1b-316">Wildcards</span></span>
* <span data-ttu-id="5ae1b-317">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="5ae1b-317">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5ae1b-319">El middleware publicado con ASP.NET Core 1.x no admite las siguientes características de Módulo URL Rewrite para IIS:</span><span class="sxs-lookup"><span data-stu-id="5ae1b-319">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="5ae1b-320">Reglas globales</span><span class="sxs-lookup"><span data-stu-id="5ae1b-320">Global Rules</span></span>
* <span data-ttu-id="5ae1b-321">Reglas de salida</span><span class="sxs-lookup"><span data-stu-id="5ae1b-321">Outbound Rules</span></span>
* <span data-ttu-id="5ae1b-322">Mapas de reescritura</span><span class="sxs-lookup"><span data-stu-id="5ae1b-322">Rewrite Maps</span></span>
* <span data-ttu-id="5ae1b-323">Acción CustomResponse</span><span class="sxs-lookup"><span data-stu-id="5ae1b-323">CustomResponse Action</span></span>
* <span data-ttu-id="5ae1b-324">Variables de servidor personalizadas</span><span class="sxs-lookup"><span data-stu-id="5ae1b-324">Custom Server Variables</span></span>
* <span data-ttu-id="5ae1b-325">Caracteres comodín</span><span class="sxs-lookup"><span data-stu-id="5ae1b-325">Wildcards</span></span>
* <span data-ttu-id="5ae1b-326">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="5ae1b-326">Action:CustomResponse</span></span>
* <span data-ttu-id="5ae1b-327">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="5ae1b-327">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="5ae1b-328">Variables de servidor compatibles</span><span class="sxs-lookup"><span data-stu-id="5ae1b-328">Supported server variables</span></span>

<span data-ttu-id="5ae1b-329">El middleware admite las siguientes variables de servidor del Módulo URL Rewrite para IIS:</span><span class="sxs-lookup"><span data-stu-id="5ae1b-329">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="5ae1b-330">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="5ae1b-330">CONTENT_LENGTH</span></span>
* <span data-ttu-id="5ae1b-331">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="5ae1b-331">CONTENT_TYPE</span></span>
* <span data-ttu-id="5ae1b-332">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-332">HTTP_ACCEPT</span></span>
* <span data-ttu-id="5ae1b-333">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="5ae1b-333">HTTP_CONNECTION</span></span>
* <span data-ttu-id="5ae1b-334">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="5ae1b-334">HTTP_COOKIE</span></span>
* <span data-ttu-id="5ae1b-335">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="5ae1b-335">HTTP_HOST</span></span>
* <span data-ttu-id="5ae1b-336">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="5ae1b-336">HTTP_REFERER</span></span>
* <span data-ttu-id="5ae1b-337">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-337">HTTP_URL</span></span>
* <span data-ttu-id="5ae1b-338">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-338">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="5ae1b-339">HTTPS</span><span class="sxs-lookup"><span data-stu-id="5ae1b-339">HTTPS</span></span>
* <span data-ttu-id="5ae1b-340">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-340">LOCAL_ADDR</span></span>
* <span data-ttu-id="5ae1b-341">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="5ae1b-341">QUERY_STRING</span></span>
* <span data-ttu-id="5ae1b-342">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="5ae1b-342">REMOTE_ADDR</span></span>
* <span data-ttu-id="5ae1b-343">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="5ae1b-343">REMOTE_PORT</span></span>
* <span data-ttu-id="5ae1b-344">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="5ae1b-344">REQUEST_FILENAME</span></span>
* <span data-ttu-id="5ae1b-345">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="5ae1b-345">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="5ae1b-346">También puede obtener `IFileProvider` a través de `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-346">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="5ae1b-347">Con este enfoque logrará mayor flexibilidad para la ubicación de los archivos de reglas de reescritura.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-347">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="5ae1b-348">Asegúrese de que los archivos de reglas de reescritura se implementan en el servidor en la ruta de acceso que proporcione.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-348">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="5ae1b-349">Regla basada en métodos</span><span class="sxs-lookup"><span data-stu-id="5ae1b-349">Method-based rule</span></span>

<span data-ttu-id="5ae1b-350">Use `Add(Action<RewriteContext> applyRule)` para implementar su propia lógica de la regla en un método.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-350">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="5ae1b-351">`RewriteContext` expone el `HttpContext` para usarlo en el método.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-351">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="5ae1b-352">`context.Result` determina cómo se administra el procesamiento adicional en la canalización.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-352">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="5ae1b-353">context.Result</span><span class="sxs-lookup"><span data-stu-id="5ae1b-353">context.Result</span></span>                       | <span data-ttu-id="5ae1b-354">Acción</span><span class="sxs-lookup"><span data-stu-id="5ae1b-354">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="5ae1b-355">`RuleResult.ContinueRules` (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-355">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="5ae1b-356">Continuar aplicando reglas</span><span class="sxs-lookup"><span data-stu-id="5ae1b-356">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="5ae1b-357">Dejar de aplicar reglas y enviar la respuesta</span><span class="sxs-lookup"><span data-stu-id="5ae1b-357">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="5ae1b-358">Dejar de aplicar reglas y enviar el contexto al siguiente middleware</span><span class="sxs-lookup"><span data-stu-id="5ae1b-358">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-360">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-360">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="5ae1b-361">La aplicación de ejemplo muestra un método que redirige las solicitudes para las rutas de acceso que terminen con *.xml*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-361">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="5ae1b-362">Si realiza una solicitud para `/file.xml`, se redirige a `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-362">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="5ae1b-363">El código de estado se establece en 301 (Movido definitivamente).</span><span class="sxs-lookup"><span data-stu-id="5ae1b-363">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="5ae1b-364">Para una redirección, debe establecer explícitamente el código de estado de la respuesta; en caso contrario, se devuelve un código de estado 200 (Correcto) y no se produce la redirección en el cliente.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-364">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="5ae1b-365">Solicitud original: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-365">Original Request: `/file.xml`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="5ae1b-367">Regla basada en IRule</span><span class="sxs-lookup"><span data-stu-id="5ae1b-367">IRule-based rule</span></span>

<span data-ttu-id="5ae1b-368">Use `Add(IRule)` para implementar su propia lógica de la regla en una clase que deriva de `IRule`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-368">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="5ae1b-369">Al usar `IRule`, se logra mayor flexibilidad que si se usa el enfoque de reglas basadas en métodos.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-369">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="5ae1b-370">La clase derivada puede incluir un constructor, donde puede pasar parámetros para el método `ApplyRule`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-370">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae1b-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae1b-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae1b-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="5ae1b-373">Se comprueba que los valores de los parámetros en la aplicación de ejemplo para `extension` y `newPath` cumplen ciertas condiciones.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-373">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="5ae1b-374">`extension` debe contener un valor, que debe ser *.png*, *.jpg* o *.gif*.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-374">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="5ae1b-375">Si `newPath` no es válido, se genera `ArgumentException` .</span><span class="sxs-lookup"><span data-stu-id="5ae1b-375">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="5ae1b-376">Si se realiza una solicitud para *image.png*, se redirige a `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-376">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="5ae1b-377">Si se realiza una solicitud para *image.jpg*, se redirige a `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-377">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="5ae1b-378">El código de estado se establece en 301 (Movido definitivamente) y se establece `context.Result` para detener el procesamiento de reglas y enviar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="5ae1b-378">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="5ae1b-379">Solicitud original: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-379">Original Request: `/image.png`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="5ae1b-381">Solicitud original: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-381">Original Request: `/image.jpg`</span></span>

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="5ae1b-383">Ejemplos de expresiones regulares</span><span class="sxs-lookup"><span data-stu-id="5ae1b-383">Regex examples</span></span>

| <span data-ttu-id="5ae1b-384">Objetivo</span><span class="sxs-lookup"><span data-stu-id="5ae1b-384">Goal</span></span> | <span data-ttu-id="5ae1b-385">Cadena de expresión regular &</span><span class="sxs-lookup"><span data-stu-id="5ae1b-385">Regex String &</span></span><br><span data-ttu-id="5ae1b-386">Ejemplo de coincidencia</span><span class="sxs-lookup"><span data-stu-id="5ae1b-386">Match Example</span></span> | <span data-ttu-id="5ae1b-387">Cadena de reemplazo &</span><span class="sxs-lookup"><span data-stu-id="5ae1b-387">Replacement String &</span></span><br><span data-ttu-id="5ae1b-388">Ejemplo de resultado</span><span class="sxs-lookup"><span data-stu-id="5ae1b-388">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="5ae1b-389">Ruta de acceso de reescritura en la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="5ae1b-389">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="5ae1b-390">Quitar barra diagonal final</span><span class="sxs-lookup"><span data-stu-id="5ae1b-390">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="5ae1b-391">Exigir barra diagonal final</span><span class="sxs-lookup"><span data-stu-id="5ae1b-391">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="5ae1b-392">Evitar reescritura de solicitudes específicas</span><span class="sxs-lookup"><span data-stu-id="5ae1b-392">Avoid rewriting specific requests</span></span> | <span data-ttu-id="5ae1b-393">`^(.*)(?<!\.axd)$` o `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-393">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="5ae1b-394">Sí: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="5ae1b-395">No: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="5ae1b-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="5ae1b-396">Reorganizar los segmentos de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="5ae1b-397">Reemplazar un segmento de URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="5ae1b-398">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5ae1b-398">Additional resources</span></span>

* [<span data-ttu-id="5ae1b-399">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="5ae1b-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="5ae1b-400">Middleware</span><span class="sxs-lookup"><span data-stu-id="5ae1b-400">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="5ae1b-401">Expresiones regulares en .NET</span><span class="sxs-lookup"><span data-stu-id="5ae1b-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="5ae1b-402">Lenguaje de expresiones regulares: referencia rápida</span><span class="sxs-lookup"><span data-stu-id="5ae1b-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* <span data-ttu-id="5ae1b-403">[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) (mod_rewrite de Apache)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-403">[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)</span></span>
* <span data-ttu-id="5ae1b-404">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso del Módulo URL Rewrite 2.0 para IIS)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-404">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="5ae1b-405">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Referencia de configuración del Módulo URL Rewrite)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-405">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* <span data-ttu-id="5ae1b-406">[IIS URL Rewrite Module Forum](https://forums.iis.net/1152.aspx) (Foro del Módulo URL Rewrite para IIS)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-406">[IIS URL Rewrite Module Forum](https://forums.iis.net/1152.aspx)</span></span>
* [<span data-ttu-id="5ae1b-407">Cómo simplificar la estructura de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="5ae1b-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* <span data-ttu-id="5ae1b-408">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 trucos y consejos para reescritura de URL)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-408">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="5ae1b-409">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (Usar la barra diagonal o no)</span><span class="sxs-lookup"><span data-stu-id="5ae1b-409">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
