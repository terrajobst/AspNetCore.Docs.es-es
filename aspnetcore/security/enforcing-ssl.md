---
title: Exigir HTTPS en ASP.NET Core
author: rick-anderson
description: Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET web app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523150"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="d5108-103">Exigir HTTPS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5108-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="d5108-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5108-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d5108-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="d5108-105">This document shows how to:</span></span>

* <span data-ttu-id="d5108-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d5108-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="d5108-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="d5108-108">Ninguna API puede evitar que a un cliente envía información confidencial en la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="d5108-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="d5108-109">Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial.</span><span class="sxs-lookup"><span data-stu-id="d5108-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="d5108-110">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="d5108-111">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="d5108-112">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5108-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="d5108-113">Las API Web deben:</span><span class="sxs-lookup"><span data-stu-id="d5108-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="d5108-114">No escucha en HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5108-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="d5108-115">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d5108-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="d5108-116">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="d5108-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d5108-117">Se recomienda toda la producción de ASP.NET Core llamada de aplicaciones web:</span><span class="sxs-lookup"><span data-stu-id="d5108-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="d5108-118">El Middleware de redireccionamiento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="d5108-119">[UseHsts](#hsts), protocolo de seguridad de transporte estricto HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="d5108-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="d5108-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="d5108-120">UseHttpsRedirection</span></span>

<span data-ttu-id="d5108-121">El código siguiente llama `UseHttpsRedirection` en el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="d5108-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="d5108-122">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="d5108-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="d5108-123">Usa el valor predeterminado [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="d5108-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="d5108-124">Usa el valor predeterminado [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que se reemplaza por la `ASPNETCORE_HTTPS_PORT` variable de entorno o [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="d5108-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="d5108-125">Un puerto debe estar disponible para el middleware redirigir a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="d5108-126">Si el puerto no está disponible, no se realizará la redirección a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="d5108-127">El puerto HTTPS se puede especificar cualquiera de los siguientes ajustes:</span><span class="sxs-lookup"><span data-stu-id="d5108-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="d5108-128">El `ASPNETCORE_HTTPS_PORT` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="d5108-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="d5108-129">En el desarrollo, una dirección url HTTPS en *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d5108-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="d5108-130">Una dirección url HTTPS configurada directamente en Kestrel o HttpSys.</span><span class="sxs-lookup"><span data-stu-id="d5108-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="d5108-131">Los siguientes resaltan las llamadas de código [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:</span><span class="sxs-lookup"><span data-stu-id="d5108-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="d5108-132">Una llamada a `AddHttpsRedirection` sólo es necesario cambiar los valores de ` HttpsPort` o ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="d5108-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="d5108-133">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="d5108-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="d5108-134">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, que es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d5108-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="d5108-135">Establece el puerto HTTPS a 5001.</span><span class="sxs-lookup"><span data-stu-id="d5108-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="d5108-136">El valor predeterminado es 443.</span><span class="sxs-lookup"><span data-stu-id="d5108-136">The default value is 443.</span></span>

<span data-ttu-id="d5108-137">Los mecanismos siguientes establecen automáticamente el puerto:</span><span class="sxs-lookup"><span data-stu-id="d5108-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="d5108-138">El middleware puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d5108-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="d5108-139">Kestrel o HTTP.sys se usa directamente con los puntos de conexión HTTPS (también se aplica a la ejecución de la aplicación con el depurador de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="d5108-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="d5108-140">Solo **un puerto HTTPS** utilizado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5108-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="d5108-141">Se usa Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d5108-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="d5108-142">IIS Express tiene habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="d5108-143">*launchSettings.json* establece el `sslPort` para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d5108-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="d5108-144">Cuando se ejecuta una aplicación detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible.</span><span class="sxs-lookup"><span data-stu-id="d5108-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="d5108-145">El puerto debe configurarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="d5108-145">The port must be manually configured.</span></span> <span data-ttu-id="d5108-146">Cuando no se configura el puerto, no se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d5108-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="d5108-147">El puerto puede configurarse estableciendo el [https_port de configuración de Host Web](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="d5108-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="d5108-148">**Clave**: https_port</span><span class="sxs-lookup"><span data-stu-id="d5108-148">**Key**: https_port</span></span>  
<span data-ttu-id="d5108-149">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d5108-149">**Type**: *string*</span></span>  
<span data-ttu-id="d5108-150">**Valor predeterminado**: no se establece un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d5108-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="d5108-151">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5108-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d5108-152">**Variable de entorno**: `<PREFIX_>HTTPS_PORT` (el prefijo es `ASPNETCORE_` cuando se usa el Host de Web.)</span><span class="sxs-lookup"><span data-stu-id="d5108-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="d5108-153">Se puede configurar el puerto indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="d5108-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="d5108-154">La variable de entorno configura el servidor y, a continuación, el middleware detecta indirectamente el puerto HTTPS a través de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="d5108-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="d5108-155">Si no se establece ningún puerto:</span><span class="sxs-lookup"><span data-stu-id="d5108-155">If no port is set:</span></span>

* <span data-ttu-id="d5108-156">No se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d5108-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="d5108-157">El middleware registra la advertencia "No se pudo determinar el puerto https para la redirección."</span><span class="sxs-lookup"><span data-stu-id="d5108-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="d5108-158">Una alternativa al uso de Middleware de redireccionamiento de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="d5108-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="d5108-159">`AddRedirectToHttps` También se puede establecer el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="d5108-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="d5108-160">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="d5108-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="d5108-161">Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar software intermedio de redireccionamiento de HTTPS (`UseHttpsRedirection`) se describe en este tema.</span><span class="sxs-lookup"><span data-stu-id="d5108-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="d5108-162">El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="d5108-163">`[RequireHttpsAttribute]` puede decorar los controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="d5108-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="d5108-164">Para aplicar el atributo global, agregue el código siguiente al `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="d5108-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="d5108-165">El código resaltado anterior requiere que todas las solicitudes usan `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5108-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="d5108-166">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d5108-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="d5108-167">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="d5108-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="d5108-168">El software intermedio también permite a la aplicación para establecer el código de estado o el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="d5108-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="d5108-169">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d5108-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="d5108-170">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no se considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="d5108-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="d5108-171">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d5108-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="d5108-172">Protocolo de seguridad de transporte estricto HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="d5108-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="d5108-173">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricto (HSTS) de HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) es una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d5108-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="d5108-174">Cuando un [explorador que admita HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recibe este encabezado:</span><span class="sxs-lookup"><span data-stu-id="d5108-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="d5108-175">El explorador almacena la configuración para el dominio que impide el envío de cualquier comunicación a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5108-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="d5108-176">El explorador obliga a todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="d5108-177">El explorador impide que el usuario mediante certificados de confianza o no es válidos.</span><span class="sxs-lookup"><span data-stu-id="d5108-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="d5108-178">El explorador deshabilita avisos para que un usuario que confíe en temporalmente dicho certificado.</span><span class="sxs-lookup"><span data-stu-id="d5108-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="d5108-179">Porque se aplica HSTS por el cliente tiene algunas limitaciones:</span><span class="sxs-lookup"><span data-stu-id="d5108-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="d5108-180">El cliente debe admitir HSTS.</span><span class="sxs-lookup"><span data-stu-id="d5108-180">The client must support HSTS.</span></span>
* <span data-ttu-id="d5108-181">HSTS requiere al menos una solicitud HTTPS correcta para establecer la directiva HSTS.</span><span class="sxs-lookup"><span data-stu-id="d5108-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="d5108-182">La aplicación debe comprobar todas las solicitudes HTTP y redirigir o rechazar la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5108-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="d5108-183">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="d5108-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="d5108-184">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="d5108-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="d5108-185">`UseHsts` no se recomienda en el desarrollo porque la configuración de HSTS sea altamente almacenable en caché por los exploradores.</span><span class="sxs-lookup"><span data-stu-id="d5108-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="d5108-186">De forma predeterminada, `UseHsts` excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="d5108-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="d5108-187">Para entornos de producción implementar HTTPS por primera vez, establezca el valor HSTS inicial en un valor pequeño.</span><span class="sxs-lookup"><span data-stu-id="d5108-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="d5108-188">Establezca el valor de horas en no más de un solo día en caso de que necesite revertir la infraestructura HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5108-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="d5108-189">Una vez que esté seguro en la sostenibilidad de la configuración de HTTPS, aumente el valor de max-age HSTS; un valor frecuente es un año.</span><span class="sxs-lookup"><span data-stu-id="d5108-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="d5108-190">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d5108-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="d5108-191">Establece el parámetro precarga del encabezado Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="d5108-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="d5108-192">Precarga no forma parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en una instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="d5108-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="d5108-193">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="d5108-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="d5108-194">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS para hospedar subdominios.</span><span class="sxs-lookup"><span data-stu-id="d5108-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="d5108-195">Establece explícitamente el parámetro de antigüedad máxima del encabezado Strict-Transport-Security en 60 días.</span><span class="sxs-lookup"><span data-stu-id="d5108-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="d5108-196">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="d5108-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="d5108-197">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="d5108-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="d5108-198">Agrega `example.com` a la lista de hosts para excluir.</span><span class="sxs-lookup"><span data-stu-id="d5108-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="d5108-199">`UseHsts` excluye los hosts de bucle invertido siguientes:</span><span class="sxs-lookup"><span data-stu-id="d5108-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="d5108-200">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="d5108-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="d5108-201">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="d5108-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="d5108-202">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="d5108-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="d5108-203">El ejemplo anterior muestra cómo agregar hosts adicionales.</span><span class="sxs-lookup"><span data-stu-id="d5108-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="d5108-204">Desactivación de HTTPS en la creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="d5108-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="d5108-205">Habilitan las plantillas de aplicación de ASP.NET Core web 2.1 o posterior (en Visual Studio o la línea de comandos de dotnet) [redirección HTTPS](#require) y [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="d5108-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="d5108-206">Para las implementaciones que no requieran HTTPS, puede participar en HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5108-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="d5108-207">Por ejemplo, algunos servicios de back-end donde HTTPS se está controlando externamente en el perímetro, mediante HTTPS en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="d5108-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="d5108-208">Para participar en HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d5108-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5108-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5108-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d5108-210">Desactive el **configurar HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="d5108-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d5108-212">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="d5108-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="d5108-213">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="d5108-213">Use the `--no-https` option.</span></span> <span data-ttu-id="d5108-214">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="d5108-214">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="d5108-215">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="d5108-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="d5108-216">Consulte [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="d5108-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="d5108-217">Información adicional</span><span class="sxs-lookup"><span data-stu-id="d5108-217">Additional information</span></span>

* [<span data-ttu-id="d5108-218">Compatibilidad con exploradores de OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="d5108-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
