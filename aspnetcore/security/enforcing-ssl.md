---
title: Exigir HTTPS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo requerir HTTPS/TLS en una aplicación web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325606"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="2fcd6-103">Exigir HTTPS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fcd6-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="2fcd6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2fcd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2fcd6-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-105">This document shows how to:</span></span>

* <span data-ttu-id="2fcd6-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="2fcd6-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="2fcd6-108">Ninguna API puede evitar que a un cliente envía información confidencial en la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="2fcd6-109">Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="2fcd6-110">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="2fcd6-111">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="2fcd6-112">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="2fcd6-113">Las API Web deben:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="2fcd6-114">No escucha en HTTP.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="2fcd6-115">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="2fcd6-116">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="2fcd6-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2fcd6-117">Se recomienda toda la producción de ASP.NET Core llamada de aplicaciones web:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="2fcd6-118">El Middleware de redireccionamiento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="2fcd6-119">[UseHsts](#hsts), protocolo de seguridad de transporte estricto HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="2fcd6-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="2fcd6-120">UseHttpsRedirection</span></span>

<span data-ttu-id="2fcd6-121">El código siguiente llama `UseHttpsRedirection` en el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="2fcd6-122">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="2fcd6-123">Usa el valor predeterminado [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="2fcd6-124">Usa el valor predeterminado [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que se reemplaza por la `ASPNETCORE_HTTPS_PORT` variable de entorno o [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="2fcd6-125">Se recomienda usar redirecciones temporales en lugar de redirecciones permanentes, como el almacenamiento en caché de vínculo puede provocar un comportamiento inestable en entornos de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="2fcd6-126">Se recomienda usar [HSTS](#hsts) para indicar a los clientes que solo proteger el recurso se deben enviar solicitudes a la aplicación (solo en producción).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="2fcd6-127">Un puerto debe estar disponible para el middleware redirigir a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="2fcd6-128">Si el puerto no está disponible, no se produce la redirección a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="2fcd6-129">Se puede especificar el puerto HTTPS mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="2fcd6-130">Establecer `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="2fcd6-131">Establecer la variable de entorno `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="2fcd6-132">En el desarrollo, establezca una dirección URL HTTPS *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="2fcd6-133">Configurar un punto de conexión de dirección URL HTTPS para [Kestrel](xref:fundamentals/servers/kestrel) o [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="2fcd6-134">Cuando se usa Kestrel o HTTP.sys como un servidor perimetral de acceso público, Kestrel o HTTP.sys debe configurarse para que escuche en ambos:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="2fcd6-135">El puerto seguro donde se redirige el cliente (normalmente, 443 en 5001 en desarrollo y producción).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="2fcd6-136">El puerto no seguro (normalmente, 80 en producción) y 5000 en el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="2fcd6-137">El puerto no seguro debe ser accesible por el cliente en el orden de la aplicación para recibir una solicitud poco segura y redirigirlo a un puerto seguro.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="2fcd6-138">Ningún firewall entre el cliente y el servidor también debe tener los puertos abierto para el tráfico.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="2fcd6-139">Para obtener más información, consulte [configuración de punto de conexión Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="2fcd6-140">Los siguientes resaltan las llamadas de código [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="2fcd6-141">Una llamada a `AddHttpsRedirection` sólo es necesario cambiar los valores de `HttpsPort` o `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="2fcd6-142">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="2fcd6-143">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), que es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="2fcd6-144">Utilice los campos de la [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) clase para las asignaciones a `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="2fcd6-145">Establece el puerto HTTPS a 5001.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="2fcd6-146">El valor predeterminado es 443.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-146">The default value is 443.</span></span>

<span data-ttu-id="2fcd6-147">Los mecanismos siguientes establecen automáticamente el puerto:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="2fcd6-148">El middleware puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="2fcd6-149">Kestrel o HTTP.sys se usa directamente con los puntos de conexión HTTPS (también se aplica a la ejecución de la aplicación con el depurador de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="2fcd6-150">Solo **un puerto HTTPS** utilizado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="2fcd6-151">Se usa Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="2fcd6-152">IIS Express tiene habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="2fcd6-153">*launchSettings.json* establece el `sslPort` para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="2fcd6-154">Cuando se ejecuta una aplicación detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="2fcd6-155">El puerto debe configurarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-155">The port must be manually configured.</span></span> <span data-ttu-id="2fcd6-156">Cuando no se configura el puerto, no se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="2fcd6-157">El puerto puede configurarse estableciendo el [https_port de configuración de Host Web](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="2fcd6-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="2fcd6-158">**Clave**: https_port</span><span class="sxs-lookup"><span data-stu-id="2fcd6-158">**Key**: https_port</span></span>  
<span data-ttu-id="2fcd6-159">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="2fcd6-159">**Type**: *string*</span></span>  
<span data-ttu-id="2fcd6-160">**Valor predeterminado**: no se establece un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="2fcd6-161">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2fcd6-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2fcd6-162">**Variable de entorno**: `<PREFIX_>HTTPS_PORT` (el prefijo es `ASPNETCORE_` cuando se usa el Host de Web.)</span><span class="sxs-lookup"><span data-stu-id="2fcd6-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="2fcd6-163">Se puede configurar el puerto indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="2fcd6-164">La variable de entorno configura el servidor y, a continuación, el middleware detecta indirectamente el puerto HTTPS a través de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="2fcd6-165">Si no se establece ningún puerto:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-165">If no port is set:</span></span>

* <span data-ttu-id="2fcd6-166">No se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="2fcd6-167">El middleware registra la advertencia "No se pudo determinar el puerto https para la redirección."</span><span class="sxs-lookup"><span data-stu-id="2fcd6-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="2fcd6-168">Una alternativa al uso de Middleware de redireccionamiento de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="2fcd6-169">`AddRedirectToHttps` También se puede establecer el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="2fcd6-170">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="2fcd6-171">Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar software intermedio de redireccionamiento de HTTPS (`UseHttpsRedirection`) se describe en este tema.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="2fcd6-172">El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="2fcd6-173">`[RequireHttpsAttribute]` puede decorar los controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="2fcd6-174">Para aplicar el atributo global, agregue el código siguiente al `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="2fcd6-175">El código resaltado anterior requiere que todas las solicitudes usan `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="2fcd6-176">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="2fcd6-177">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="2fcd6-178">El software intermedio también permite a la aplicación para establecer el código de estado o el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="2fcd6-179">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="2fcd6-180">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no se considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="2fcd6-181">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="2fcd6-182">Protocolo de seguridad de transporte estricto HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="2fcd6-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="2fcd6-183">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricto (HSTS) de HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) es una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="2fcd6-184">Cuando un [explorador que admita HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recibe este encabezado:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="2fcd6-185">El explorador almacena la configuración para el dominio que impide el envío de cualquier comunicación a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="2fcd6-186">El explorador obliga a todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="2fcd6-187">El explorador impide que el usuario mediante certificados de confianza o no es válidos.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="2fcd6-188">El explorador deshabilita avisos para que un usuario que confíe en temporalmente dicho certificado.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="2fcd6-189">Porque se aplica HSTS por el cliente tiene algunas limitaciones:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="2fcd6-190">El cliente debe admitir HSTS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-190">The client must support HSTS.</span></span>
* <span data-ttu-id="2fcd6-191">HSTS requiere al menos una solicitud HTTPS correcta para establecer la directiva HSTS.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="2fcd6-192">La aplicación debe comprobar todas las solicitudes HTTP y redirigir o rechazar la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="2fcd6-193">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="2fcd6-194">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="2fcd6-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="2fcd6-195">`UseHsts` no se recomienda en el desarrollo porque la configuración de HSTS sea altamente almacenable en caché por los exploradores.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="2fcd6-196">De forma predeterminada, `UseHsts` excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="2fcd6-197">Para entornos de producción implementar HTTPS por primera vez, establezca el valor HSTS inicial en un valor pequeño.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="2fcd6-198">Establezca el valor de horas en no más de un solo día en caso de que necesite revertir la infraestructura HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="2fcd6-199">Una vez que esté seguro en la sostenibilidad de la configuración de HTTPS, aumente el valor de max-age HSTS; un valor frecuente es un año.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="2fcd6-200">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="2fcd6-201">Establece el parámetro precarga del encabezado Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="2fcd6-202">Precarga no forma parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en una instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="2fcd6-203">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="2fcd6-204">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS para hospedar subdominios.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="2fcd6-205">Establece explícitamente el parámetro de antigüedad máxima del encabezado Strict-Transport-Security en 60 días.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="2fcd6-206">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="2fcd6-207">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="2fcd6-208">Agrega `example.com` a la lista de hosts para excluir.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="2fcd6-209">`UseHsts` excluye los hosts de bucle invertido siguientes:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="2fcd6-210">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="2fcd6-211">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="2fcd6-212">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="2fcd6-213">El ejemplo anterior muestra cómo agregar hosts adicionales.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="2fcd6-214">Desactivación de HTTPS/HSTS crea un proyecto</span><span class="sxs-lookup"><span data-stu-id="2fcd6-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="2fcd6-215">En algunos escenarios de servicio de back-end donde se controla la seguridad de conexión en el perímetro orientados al público de la red, la configuración de seguridad de conexión en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="2fcd6-216">Web apps generados a partir de las plantillas en Visual Studio o desde la [dotnet nuevo](/dotnet/core/tools/dotnet-new) Habilitar comando [redirección HTTPS](#require) y [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="2fcd6-217">Para las implementaciones que no requieren estos escenarios, puede desactivar de HTTPS/HSTS cuando se crea la aplicación de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="2fcd6-218">Para participar en HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="2fcd6-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2fcd6-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fcd6-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="2fcd6-220">Desactive el **configurar HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2fcd6-222">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2fcd6-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="2fcd6-223">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="2fcd6-223">Use the `--no-https` option.</span></span> <span data-ttu-id="2fcd6-224">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="2fcd6-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="2fcd6-225">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="2fcd6-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="2fcd6-226">Consulte [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="2fcd6-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="2fcd6-227">Información adicional</span><span class="sxs-lookup"><span data-stu-id="2fcd6-227">Additional information</span></span>

* [<span data-ttu-id="2fcd6-228">Compatibilidad con exploradores de OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="2fcd6-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
