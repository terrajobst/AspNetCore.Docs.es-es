---
title: Exigir HTTPS en un núcleo de ASP.NET
author: rick-anderson
description: Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET de aplicación web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0433ddb3bf1ef0074c683903ad4553cd6a0b4741
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687823"
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="bf37e-103">Exigir HTTPS en un núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf37e-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="bf37e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf37e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bf37e-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="bf37e-105">This document shows how to:</span></span>

- <span data-ttu-id="bf37e-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="bf37e-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="bf37e-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="bf37e-108">Hacer **no** usar `RequireHttpsAttribute` en las API Web que reciben información confidencial.</span><span class="sxs-lookup"><span data-stu-id="bf37e-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="bf37e-109">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="bf37e-110">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="bf37e-111">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf37e-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="bf37e-112">Las API Web deben realizar las tareas:</span><span class="sxs-lookup"><span data-stu-id="bf37e-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="bf37e-113">No escuchar en HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf37e-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="bf37e-114">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bf37e-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="bf37e-115">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="bf37e-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="bf37e-116">Se recomienda que todos los núcleos de ASP.NET web aplicaciones de la llamada `UseHttpsRedirection` para redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="bf37e-117">Si `UseHsts` se llama en la aplicación, debe llamarse antes de `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="bf37e-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="bf37e-118">El código siguiente llama `UseHttpsRedirection` en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="bf37e-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="bf37e-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="bf37e-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="bf37e-120">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf37e-120">The following code:</span></span>

<span data-ttu-id="bf37e-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="bf37e-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="bf37e-122">Conjuntos de `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="bf37e-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="bf37e-123">Establece el puerto HTTPS en 5001.</span><span class="sxs-lookup"><span data-stu-id="bf37e-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="bf37e-124">El [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="bf37e-125">`[RequireHttpsAttribute]` puede decorar controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="bf37e-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="bf37e-126">Para aplicar el atributo global, agregue el código siguiente a `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bf37e-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="bf37e-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="bf37e-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="bf37e-128">El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf37e-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="bf37e-129">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bf37e-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="bf37e-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="bf37e-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="bf37e-131">Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="bf37e-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="bf37e-132">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="bf37e-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="bf37e-133">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="bf37e-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="bf37e-134">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="bf37e-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="bf37e-135">Protocolo de seguridad de transporte estrictos de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="bf37e-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="bf37e-136">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricta de HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) supone una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta especial.</span><span class="sxs-lookup"><span data-stu-id="bf37e-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="bf37e-137">Una vez que un explorador compatible recibe este encabezado ese explorador impide que todas las comunicaciones se envíen a través de HTTP para el dominio especificado y en su lugar, le enviará todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="bf37e-138">También evita que haga clic en HTTPS a través de solicitudes en exploradores.</span><span class="sxs-lookup"><span data-stu-id="bf37e-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="bf37e-139">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="bf37e-139">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="bf37e-140">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="bf37e-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="bf37e-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="bf37e-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="bf37e-142">`UseHsts` no es recomendable en el desarrollo porque el encabezado HSTS es alta puede almacenar exploradores.</span><span class="sxs-lookup"><span data-stu-id="bf37e-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="bf37e-143">De forma predeterminada, UseHsts excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="bf37e-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="bf37e-144">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf37e-144">The following code:</span></span>

<span data-ttu-id="bf37e-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="bf37e-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="bf37e-146">Establece el parámetro precarga del encabezado de seguridad de transporte Strict.</span><span class="sxs-lookup"><span data-stu-id="bf37e-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="bf37e-147">Precarga no es parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="bf37e-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="bf37e-148">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="bf37e-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="bf37e-149">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS a subdominios de Host.</span><span class="sxs-lookup"><span data-stu-id="bf37e-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="bf37e-150">Establece explícitamente el parámetro de max-age del encabezado de seguridad de transporte Strict a 60 días.</span><span class="sxs-lookup"><span data-stu-id="bf37e-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="bf37e-151">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="bf37e-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="bf37e-152">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bf37e-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="bf37e-153">Agrega `example.com` a la lista de hosts que se van a excluir.</span><span class="sxs-lookup"><span data-stu-id="bf37e-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="bf37e-154">`UseHsts` excluye los siguientes hosts de bucle invertido:</span><span class="sxs-lookup"><span data-stu-id="bf37e-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="bf37e-155">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="bf37e-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bf37e-156">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="bf37e-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bf37e-157">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="bf37e-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="bf37e-158">En el ejemplo anterior se muestra cómo agregar hosts adicionales.</span><span class="sxs-lookup"><span data-stu-id="bf37e-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="bf37e-159">Desactivación de HTTPS en la creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="bf37e-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="bf37e-160">Habilitan el ASP.NET Core 2.1 y posteriores plantillas de aplicación web (desde Visual Studio o la línea de comandos dotnet) [redirección HTTPS](#require) y [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="bf37e-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="bf37e-161">Para las implementaciones que no requieran HTTPS, puede cancelar voluntariamente la suscripción de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf37e-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="bf37e-162">Por ejemplo, algunos servicios back-end donde HTTPS se está controlando externamente en el perímetro, mediante HTTPS en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="bf37e-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="bf37e-163">A la cancelación de HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bf37e-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf37e-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf37e-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bf37e-165">Desactive el **configurar para HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="bf37e-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bf37e-167">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="bf37e-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="bf37e-168">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="bf37e-168">Use the `--no-https` option.</span></span> <span data-ttu-id="bf37e-169">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="bf37e-169">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="bf37e-170">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="bf37e-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="bf37e-171">Vea [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="bf37e-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
