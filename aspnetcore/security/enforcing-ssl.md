---
title: Exigir HTTPS en ASP.NET Core
author: rick-anderson
description: Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET web app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a4ab91ef23a798c919a23a44f5a050bd3c09d56a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356693"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="b90dd-103">Exigir HTTPS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b90dd-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="b90dd-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b90dd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b90dd-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="b90dd-105">This document shows how to:</span></span>

* <span data-ttu-id="b90dd-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b90dd-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="b90dd-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="b90dd-108">Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b90dd-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="b90dd-109">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="b90dd-110">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="b90dd-111">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="b90dd-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="b90dd-112">Las API Web deben:</span><span class="sxs-lookup"><span data-stu-id="b90dd-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="b90dd-113">No escucha en HTTP.</span><span class="sxs-lookup"><span data-stu-id="b90dd-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="b90dd-114">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b90dd-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="b90dd-115">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="b90dd-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b90dd-116">Se recomienda que todas las aplicaciones web de ASP.NET Core llamará Middleware de redireccionamiento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="b90dd-117">El código siguiente llama `UseHttpsRedirection` en el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="b90dd-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="b90dd-118">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="b90dd-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="b90dd-119">Usa el valor predeterminado [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="b90dd-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="b90dd-120">Deben llamar las aplicaciones de producción [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b90dd-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="b90dd-121">Usa el valor predeterminado [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="b90dd-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="b90dd-122">El código siguiente llama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:</span><span class="sxs-lookup"><span data-stu-id="b90dd-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="b90dd-123">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="b90dd-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="b90dd-124">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, que es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b90dd-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="b90dd-125">Deben llamar las aplicaciones de producción [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b90dd-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="b90dd-126">Establece el puerto HTTPS a 5001.</span><span class="sxs-lookup"><span data-stu-id="b90dd-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="b90dd-127">El valor predeterminado es 443.</span><span class="sxs-lookup"><span data-stu-id="b90dd-127">The default value is 443.</span></span>

<span data-ttu-id="b90dd-128">Los mecanismos siguientes establecen automáticamente el puerto:</span><span class="sxs-lookup"><span data-stu-id="b90dd-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="b90dd-129">El middleware puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b90dd-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="b90dd-130">Kestrel o HTTP.sys se usa directamente con los puntos de conexión HTTPS (también se aplica a la ejecución de la aplicación con el depurador de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="b90dd-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="b90dd-131">Solo **un puerto HTTPS** utilizado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b90dd-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="b90dd-132">Se usa Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b90dd-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="b90dd-133">IIS Express tiene habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="b90dd-134">*launchSettings.json* establece el `sslPort` para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b90dd-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="b90dd-135">Cuando se ejecuta una aplicación detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible.</span><span class="sxs-lookup"><span data-stu-id="b90dd-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="b90dd-136">El puerto debe configurarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="b90dd-136">The port must be manually configured.</span></span> <span data-ttu-id="b90dd-137">Cuando no se configura el puerto, no se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b90dd-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="b90dd-138">El puerto puede configurarse estableciendo el [https_port de configuración de Host Web](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="b90dd-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="b90dd-139">**Clave**: https_port **tipo**: *cadena*
**predeterminada**: no se establece un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b90dd-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="b90dd-140">**Establecer mediante**: `UseSetting` 
 **variable de entorno**: `<PREFIX_>HTTPS_PORT` (el prefijo es `ASPNETCORE_` cuando se usa el Host de Web.)</span><span class="sxs-lookup"><span data-stu-id="b90dd-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="b90dd-141">Se puede configurar el puerto indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="b90dd-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="b90dd-142">La variable de entorno configura el servidor y, a continuación, el middleware detecta indirectamente el puerto HTTPS a través de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="b90dd-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="b90dd-143">Si no se establece ningún puerto:</span><span class="sxs-lookup"><span data-stu-id="b90dd-143">If no port is set:</span></span>

* <span data-ttu-id="b90dd-144">No se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b90dd-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="b90dd-145">El middleware registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="b90dd-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="b90dd-146">Una alternativa al uso de Middleware de redireccionamiento de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="b90dd-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="b90dd-147">`AddRedirectToHttps` También se puede establecer el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="b90dd-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="b90dd-148">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b90dd-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="b90dd-149">Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar software intermedio de redireccionamiento de HTTPS (`UseHttpsRedirection`) se describe en este tema.</span><span class="sxs-lookup"><span data-stu-id="b90dd-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="b90dd-150">El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="b90dd-151">`[RequireHttpsAttribute]` puede decorar los controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="b90dd-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="b90dd-152">Para aplicar el atributo global, agregue el código siguiente al `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b90dd-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="b90dd-153">El código resaltado anterior requiere que todas las solicitudes usan `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b90dd-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="b90dd-154">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b90dd-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="b90dd-155">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b90dd-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="b90dd-156">El software intermedio también permite a la aplicación para establecer el código de estado o el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="b90dd-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="b90dd-157">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="b90dd-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="b90dd-158">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no se considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="b90dd-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="b90dd-159">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="b90dd-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="b90dd-160">Protocolo de seguridad de transporte estricto HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="b90dd-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="b90dd-161">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricto (HSTS) de HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) es una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta especial.</span><span class="sxs-lookup"><span data-stu-id="b90dd-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="b90dd-162">Una vez que un explorador compatible recibe este encabezado ese explorador impedirá cualquier comunicación que se envíen a través de HTTP para el dominio especificado y en su lugar, enviará todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-162">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="b90dd-163">También impide que haga clic en HTTPS a través de mensajes en los exploradores.</span><span class="sxs-lookup"><span data-stu-id="b90dd-163">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="b90dd-164">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="b90dd-164">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="b90dd-165">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="b90dd-165">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="b90dd-166">`UseHsts` no se recomienda en el desarrollo porque el encabezado HSTS es altamente almacenable en caché por los exploradores.</span><span class="sxs-lookup"><span data-stu-id="b90dd-166">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="b90dd-167">De forma predeterminada, `UseHsts` excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="b90dd-167">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="b90dd-168">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b90dd-168">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="b90dd-169">Establece el parámetro precarga del encabezado Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="b90dd-169">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="b90dd-170">Carga previa no es parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en una instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="b90dd-170">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="b90dd-171">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="b90dd-171">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="b90dd-172">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS para hospedar subdominios.</span><span class="sxs-lookup"><span data-stu-id="b90dd-172">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="b90dd-173">Establece explícitamente el parámetro de max-age del encabezado Strict-Transport-Security para 60 días.</span><span class="sxs-lookup"><span data-stu-id="b90dd-173">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="b90dd-174">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="b90dd-174">If not set, defaults to 30 days.</span></span> <span data-ttu-id="b90dd-175">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b90dd-175">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="b90dd-176">Agrega `example.com` a la lista de hosts para excluir.</span><span class="sxs-lookup"><span data-stu-id="b90dd-176">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="b90dd-177">`UseHsts` excluye los hosts de bucle invertido siguientes:</span><span class="sxs-lookup"><span data-stu-id="b90dd-177">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="b90dd-178">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="b90dd-178">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b90dd-179">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="b90dd-179">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b90dd-180">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="b90dd-180">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="b90dd-181">El ejemplo anterior muestra cómo agregar hosts adicionales.</span><span class="sxs-lookup"><span data-stu-id="b90dd-181">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="b90dd-182">Desactivación de HTTPS en la creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="b90dd-182">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="b90dd-183">Habilitan las plantillas de aplicación de ASP.NET Core web 2.1 o posterior (en Visual Studio o la línea de comandos de dotnet) [redirección HTTPS](#require) y [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b90dd-183">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="b90dd-184">Para las implementaciones que no requieran HTTPS, puede participar en HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b90dd-184">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="b90dd-185">Por ejemplo, algunos servicios de back-end donde HTTPS se está controlando externamente en el perímetro, mediante HTTPS en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="b90dd-185">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="b90dd-186">Para participar en HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b90dd-186">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b90dd-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b90dd-187">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b90dd-188">Desactive el **configurar HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="b90dd-188">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b90dd-190">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="b90dd-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b90dd-191">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="b90dd-191">Use the `--no-https` option.</span></span> <span data-ttu-id="b90dd-192">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="b90dd-192">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="b90dd-193">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="b90dd-193">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="b90dd-194">Consulte [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="b90dd-194">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
