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
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652193"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="a94c9-103">Exigir HTTPS en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a94c9-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="a94c9-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a94c9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a94c9-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="a94c9-105">This document shows how to:</span></span>

* <span data-ttu-id="a94c9-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a94c9-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="a94c9-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="a94c9-108">Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que reciben información confidencial.</span><span class="sxs-lookup"><span data-stu-id="a94c9-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a94c9-109">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a94c9-110">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a94c9-111">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="a94c9-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a94c9-112">Las API Web deben realizar las tareas:</span><span class="sxs-lookup"><span data-stu-id="a94c9-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="a94c9-113">No escuchar en HTTP.</span><span class="sxs-lookup"><span data-stu-id="a94c9-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="a94c9-114">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a94c9-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="a94c9-115">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="a94c9-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a94c9-116">Se recomienda que todas las aplicaciones web de ASP.NET Core llamará Middleware de redirección de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="a94c9-117">El código siguiente llama `UseHttpsRedirection` en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="a94c9-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="a94c9-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="a94c9-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="a94c9-119">El código siguiente llama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:</span><span class="sxs-lookup"><span data-stu-id="a94c9-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="a94c9-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="a94c9-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="a94c9-121">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="a94c9-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="a94c9-122">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, que es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a94c9-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="a94c9-123">Deben llamar aplicaciones de producción [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="a94c9-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="a94c9-124">Establece el puerto HTTPS en 5001.</span><span class="sxs-lookup"><span data-stu-id="a94c9-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="a94c9-125">El valor predeterminado es 443.</span><span class="sxs-lookup"><span data-stu-id="a94c9-125">The default value is 443.</span></span>

<span data-ttu-id="a94c9-126">Los siguientes mecanismos establecen automáticamente el puerto:</span><span class="sxs-lookup"><span data-stu-id="a94c9-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="a94c9-127">El software intermedio puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="a94c9-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="a94c9-128">Se utiliza kestrel o HTTP.sys directamente con los puntos de conexión HTTPS (también se aplica a la aplicación se ejecuta con el depurador de código de Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="a94c9-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="a94c9-129">Solo **un puerto HTTPS** se utiliza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a94c9-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="a94c9-130">Se utiliza Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a94c9-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="a94c9-131">IIS Express tiene habilitados para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="a94c9-132">*launchSettings.json* establece el `sslPort` de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a94c9-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="a94c9-133">Cuando una aplicación se ejecute detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible.</span><span class="sxs-lookup"><span data-stu-id="a94c9-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="a94c9-134">El puerto debe configurarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="a94c9-134">The port must be manually configured.</span></span> <span data-ttu-id="a94c9-135">Cuando el puerto no está configurado, no se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a94c9-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="a94c9-136">El puerto se puede configurar estableciendo el:</span><span class="sxs-lookup"><span data-stu-id="a94c9-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="a94c9-137">La variable de entorno `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="a94c9-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="a94c9-138">`http_port` clave de configuración de host (por ejemplo, a través de *hostsettings.json* o un argumento de línea de comandos).</span><span class="sxs-lookup"><span data-stu-id="a94c9-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="a94c9-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="a94c9-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="a94c9-140">Vea el ejemplo anterior que se muestra cómo establecer el puerto a 5001.</span><span class="sxs-lookup"><span data-stu-id="a94c9-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="a94c9-141">El puerto puede configurarse indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="a94c9-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="a94c9-142">La variable de entorno configura el servidor y, a continuación, el middleware indirectamente detecta el puerto HTTPS a través de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="a94c9-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="a94c9-143">Si no se establece ningún puerto:</span><span class="sxs-lookup"><span data-stu-id="a94c9-143">If no port is set:</span></span>

* <span data-ttu-id="a94c9-144">No se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a94c9-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="a94c9-145">El middleware registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="a94c9-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="a94c9-146">Una alternativa al uso de Middleware de redirección de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="a94c9-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="a94c9-147">`AddRedirectToHttps` puede establecer el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="a94c9-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="a94c9-148">Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a94c9-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="a94c9-149">Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar HTTPS redirección Middleware (`UseHttpsRedirection`) se describe en este tema.</span><span class="sxs-lookup"><span data-stu-id="a94c9-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a94c9-150">El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="a94c9-151">`[RequireHttpsAttribute]` puede decorar controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="a94c9-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a94c9-152">Para aplicar el atributo global, agregue el código siguiente a `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a94c9-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="a94c9-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="a94c9-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="a94c9-154">El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="a94c9-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a94c9-155">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a94c9-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="a94c9-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="a94c9-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="a94c9-157">Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a94c9-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="a94c9-158">El middleware también permite a la aplicación para establecer el código de estado o el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="a94c9-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="a94c9-159">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="a94c9-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a94c9-160">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="a94c9-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a94c9-161">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a94c9-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="a94c9-162">Protocolo de seguridad de transporte estrictos de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a94c9-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="a94c9-163">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricta de HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) supone una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta especial.</span><span class="sxs-lookup"><span data-stu-id="a94c9-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="a94c9-164">Una vez que un explorador compatible recibe este encabezado ese explorador impide que todas las comunicaciones se envíen a través de HTTP para el dominio especificado y en su lugar, le enviará todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="a94c9-165">También evita que haga clic en HTTPS a través de solicitudes en exploradores.</span><span class="sxs-lookup"><span data-stu-id="a94c9-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="a94c9-166">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="a94c9-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="a94c9-167">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a94c9-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="a94c9-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="a94c9-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="a94c9-169">`UseHsts` no es recomendable en el desarrollo porque el encabezado HSTS es alta puede almacenar exploradores.</span><span class="sxs-lookup"><span data-stu-id="a94c9-169">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="a94c9-170">De forma predeterminada, UseHsts excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="a94c9-170">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="a94c9-171">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a94c9-171">The following code:</span></span>

<span data-ttu-id="a94c9-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="a94c9-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="a94c9-173">Establece el parámetro precarga del encabezado de seguridad de transporte Strict.</span><span class="sxs-lookup"><span data-stu-id="a94c9-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="a94c9-174">Precarga no es parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="a94c9-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="a94c9-175">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="a94c9-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="a94c9-176">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS a subdominios de Host.</span><span class="sxs-lookup"><span data-stu-id="a94c9-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="a94c9-177">Establece explícitamente el parámetro de max-age del encabezado de seguridad de transporte Strict a 60 días.</span><span class="sxs-lookup"><span data-stu-id="a94c9-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="a94c9-178">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="a94c9-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="a94c9-179">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a94c9-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="a94c9-180">Agrega `example.com` a la lista de hosts que se van a excluir.</span><span class="sxs-lookup"><span data-stu-id="a94c9-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="a94c9-181">`UseHsts` excluye los siguientes hosts de bucle invertido:</span><span class="sxs-lookup"><span data-stu-id="a94c9-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="a94c9-182">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="a94c9-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a94c9-183">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="a94c9-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a94c9-184">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="a94c9-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="a94c9-185">En el ejemplo anterior se muestra cómo agregar hosts adicionales.</span><span class="sxs-lookup"><span data-stu-id="a94c9-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="a94c9-186">Desactivación de HTTPS en la creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="a94c9-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="a94c9-187">Habilitar las plantillas de aplicación de ASP.NET Core web 2.1 o posterior (en Visual Studio o la línea de comandos dotnet) [redirección HTTPS](#require) y [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="a94c9-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="a94c9-188">Para las implementaciones que no requieran HTTPS, puede cancelar voluntariamente la suscripción de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a94c9-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="a94c9-189">Por ejemplo, algunos servicios back-end donde HTTPS se está controlando externamente en el perímetro, mediante HTTPS en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="a94c9-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="a94c9-190">A la cancelación de HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a94c9-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a94c9-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a94c9-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a94c9-192">Desactive el **configurar para HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="a94c9-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a94c9-194">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="a94c9-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="a94c9-195">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="a94c9-195">Use the `--no-https` option.</span></span> <span data-ttu-id="a94c9-196">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="a94c9-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="a94c9-198">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="a94c9-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="a94c9-199">Vea [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="a94c9-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
