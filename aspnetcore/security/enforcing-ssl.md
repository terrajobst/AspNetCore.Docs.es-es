---
title: Exigir HTTPS en el núcleo de ASP.NET
author: rick-anderson
description: Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET de aplicación web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 24ab83192ded381b46fab337a986f51fb22b2227
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729508"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="b5fc8-103">Exigir HTTPS en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b5fc8-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="b5fc8-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b5fc8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5fc8-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-105">This document shows how to:</span></span>

* <span data-ttu-id="b5fc8-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="b5fc8-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="b5fc8-108">Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que reciben información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="b5fc8-109">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="b5fc8-110">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="b5fc8-111">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="b5fc8-112">Las API Web deben realizar las tareas:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="b5fc8-113">No escuchar en HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="b5fc8-114">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="b5fc8-115">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="b5fc8-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b5fc8-116">Se recomienda que todas las aplicaciones web de ASP.NET Core llamará Middleware de redirección de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="b5fc8-117">El código siguiente llama `UseHttpsRedirection` en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="b5fc8-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="b5fc8-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="b5fc8-119">El código siguiente llama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="b5fc8-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="b5fc8-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="b5fc8-121">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="b5fc8-122">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span></span>
* <span data-ttu-id="b5fc8-123">Establece el puerto HTTPS en 5001.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-123">Sets the HTTPS port to 5001.</span></span>

<span data-ttu-id="b5fc8-124">Los siguientes mecanismos establecen automáticamente el puerto:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="b5fc8-125">El software intermedio puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="b5fc8-126">Se utiliza kestrel o HTTP.sys directamente con los puntos de conexión HTTPS (también se aplica a la aplicación se ejecuta con el depurador de código de Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="b5fc8-127">Solo **un puerto HTTPS** se utiliza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="b5fc8-128">Se utiliza Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="b5fc8-129">IIS Express tiene habilitados para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="b5fc8-130">*launchSettings.json* establece el `sslPort` de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="b5fc8-131">Cuando una aplicación se ejecute detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="b5fc8-132">El puerto debe configurarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-132">The port must be manually configured.</span></span> <span data-ttu-id="b5fc8-133">Cuando el puerto no está configurado, no se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="b5fc8-134">El puerto se puede configurar estableciendo el:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="b5fc8-135">La variable de entorno `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="b5fc8-136">`http_port` clave de configuración de host (por ejemplo, a través de *hostsettings.json* o un argumento de línea de comandos).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="b5fc8-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="b5fc8-138">Vea el ejemplo anterior que se muestra cómo establecer el puerto a 5001.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="b5fc8-139">El puerto puede configurarse indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="b5fc8-140">La variable de entorno configura el servidor y, a continuación, el middleware indirectamente detecta el puerto HTTPS a través de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="b5fc8-141">Si no se establece ningún puerto:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-141">If no port is set:</span></span>

* <span data-ttu-id="b5fc8-142">No se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="b5fc8-143">El middleware registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-143">The middleware logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="b5fc8-144">El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-144">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="b5fc8-145">`[RequireHttpsAttribute]` puede decorar controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-145">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="b5fc8-146">Para aplicar el atributo global, agregue el código siguiente a `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-146">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="b5fc8-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="b5fc8-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="b5fc8-148">El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-148">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="b5fc8-149">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-149">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="b5fc8-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="b5fc8-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="b5fc8-151">Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-151">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="b5fc8-152">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-152">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="b5fc8-153">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-153">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="b5fc8-154">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-154">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="b5fc8-155">Protocolo de seguridad de transporte estrictos de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="b5fc8-155">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="b5fc8-156">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricta de HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) supone una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta especial.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-156">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="b5fc8-157">Una vez que un explorador compatible recibe este encabezado ese explorador impide que todas las comunicaciones se envíen a través de HTTP para el dominio especificado y en su lugar, le enviará todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-157">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="b5fc8-158">También evita que haga clic en HTTPS a través de solicitudes en exploradores.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-158">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="b5fc8-159">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-159">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="b5fc8-160">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="b5fc8-160">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="b5fc8-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="b5fc8-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="b5fc8-162">`UseHsts` no es recomendable en el desarrollo porque el encabezado HSTS es alta puede almacenar exploradores.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-162">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="b5fc8-163">De forma predeterminada, UseHsts excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-163">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="b5fc8-164">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-164">The following code:</span></span>

<span data-ttu-id="b5fc8-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="b5fc8-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="b5fc8-166">Establece el parámetro precarga del encabezado de seguridad de transporte Strict.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-166">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="b5fc8-167">Precarga no es parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-167">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="b5fc8-168">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-168">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="b5fc8-169">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS a subdominios de Host.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-169">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="b5fc8-170">Establece explícitamente el parámetro de max-age del encabezado de seguridad de transporte Strict a 60 días.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-170">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="b5fc8-171">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-171">If not set, defaults to 30 days.</span></span> <span data-ttu-id="b5fc8-172">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-172">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="b5fc8-173">Agrega `example.com` a la lista de hosts que se van a excluir.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-173">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="b5fc8-174">`UseHsts` excluye los siguientes hosts de bucle invertido:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-174">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="b5fc8-175">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-175">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b5fc8-176">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-176">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b5fc8-177">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-177">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="b5fc8-178">En el ejemplo anterior se muestra cómo agregar hosts adicionales.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-178">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="b5fc8-179">Desactivación de HTTPS en la creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="b5fc8-179">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="b5fc8-180">Habilitar las plantillas de aplicación de ASP.NET Core web 2.1 o posterior (en Visual Studio o la línea de comandos dotnet) [redirección HTTPS](#require) y [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-180">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="b5fc8-181">Para las implementaciones que no requieran HTTPS, puede cancelar voluntariamente la suscripción de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-181">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="b5fc8-182">Por ejemplo, algunos servicios back-end donde HTTPS se está controlando externamente en el perímetro, mediante HTTPS en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-182">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="b5fc8-183">A la cancelación de HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b5fc8-183">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5fc8-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5fc8-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b5fc8-185">Desactive el **configurar para HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-185">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b5fc8-187">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="b5fc8-187">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b5fc8-188">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="b5fc8-188">Use the `--no-https` option.</span></span> <span data-ttu-id="b5fc8-189">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="b5fc8-189">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="b5fc8-190">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="b5fc8-190">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="b5fc8-191">Vea [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="b5fc8-191">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
