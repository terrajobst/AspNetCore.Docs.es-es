---
title: Exigir HTTPS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo requerir HTTPS/TLS en una aplicación web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a5359fe49e71ab59b47a8a5a39e7b806ad308235
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090996"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="a4a93-103">Exigir HTTPS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4a93-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="a4a93-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4a93-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4a93-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="a4a93-105">This document shows how to:</span></span>

* <span data-ttu-id="a4a93-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a4a93-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="a4a93-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="a4a93-108">Ninguna API puede evitar que a un cliente envía información confidencial en la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="a4a93-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="a4a93-109">Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial.</span><span class="sxs-lookup"><span data-stu-id="a4a93-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a4a93-110">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a4a93-111">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a4a93-112">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4a93-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a4a93-113">Las API Web deben:</span><span class="sxs-lookup"><span data-stu-id="a4a93-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="a4a93-114">No escucha en HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4a93-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="a4a93-115">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a4a93-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="a4a93-116">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="a4a93-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a4a93-117">Se recomienda que ASP.NET Core de producción llamada de aplicaciones web:</span><span class="sxs-lookup"><span data-stu-id="a4a93-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="a4a93-118">Middleware de redireccionamiento de HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) para redirigir las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="a4a93-119">Middleware HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) para enviar los encabezados de protocolo de seguridad de estricta transporte (HSTS) de HTTP a los clientes.</span><span class="sxs-lookup"><span data-stu-id="a4a93-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="a4a93-120">Las aplicaciones implementadas en una configuración de proxy inverso que el proxy controlar la seguridad de conexión (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a4a93-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="a4a93-121">Si el servidor proxy también controla la redirección de HTTPS, no hay ninguna necesidad de usar Middleware de redireccionamiento de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="a4a93-122">Si el servidor proxy también controla escribir encabezados HSTS (por ejemplo, [HSTS nativos se admite en IIS 10.0 (1709) o versiones posteriores](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), Middleware HSTS no es necesario por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4a93-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="a4a93-123">Para obtener más información, consulte [voluntaria de HTTPS/HSTS crea un proyecto](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="a4a93-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="a4a93-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="a4a93-124">UseHttpsRedirection</span></span>

<span data-ttu-id="a4a93-125">El código siguiente llama `UseHttpsRedirection` en el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="a4a93-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="a4a93-126">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="a4a93-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="a4a93-127">Usa el valor predeterminado [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="a4a93-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="a4a93-128">Usa el valor predeterminado [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que se reemplaza por la `ASPNETCORE_HTTPS_PORT` variable de entorno o [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="a4a93-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="a4a93-129">Se recomienda usar redirecciones temporales en lugar de redirecciones permanentes.</span><span class="sxs-lookup"><span data-stu-id="a4a93-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="a4a93-130">Almacenamiento en caché de vínculo, puede provocar un comportamiento inestable en entornos de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a4a93-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="a4a93-131">Si prefiere enviar un código de estado de redirección permanente cuando la aplicación está en un entorno de desarrollo no, consulte la [configurar redirecciones permanentes en producción](#configure-permanent-redirects-in-production) sección.</span><span class="sxs-lookup"><span data-stu-id="a4a93-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="a4a93-132">Se recomienda usar [HSTS](#http-strict-transport-security-protocol-hsts) para indicar a los clientes que solo proteger el recurso se deben enviar solicitudes a la aplicación (solo en producción).</span><span class="sxs-lookup"><span data-stu-id="a4a93-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="a4a93-133">Configuración del puerto</span><span class="sxs-lookup"><span data-stu-id="a4a93-133">Port configuration</span></span>

<span data-ttu-id="a4a93-134">Un puerto debe estar disponible para el middleware redirigir una solicitud poco segura a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="a4a93-135">Si el puerto no está disponible:</span><span class="sxs-lookup"><span data-stu-id="a4a93-135">If no port is available:</span></span>

* <span data-ttu-id="a4a93-136">No se produce la redirección a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="a4a93-137">El middleware registra la advertencia "No se pudo determinar el puerto https para la redirección."</span><span class="sxs-lookup"><span data-stu-id="a4a93-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="a4a93-138">Especifique el puerto HTTPS mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4a93-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="a4a93-139">Establecer [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="a4a93-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="a4a93-140">Establecer el `ASPNETCORE_HTTPS_PORT` variable de entorno o [https_port de configuración de Host Web](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="a4a93-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="a4a93-141">**Clave**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="a4a93-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="a4a93-142">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="a4a93-142">**Type**: *string*</span></span>  
  <span data-ttu-id="a4a93-143">**Valor predeterminado**: no se establece un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a4a93-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="a4a93-144">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a4a93-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="a4a93-145">**Variable de entorno**: `<PREFIX_>HTTPS_PORT` (el prefijo es `ASPNETCORE_` cuando se usa el [Web Host](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="a4a93-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="a4a93-146">Al configurar un <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> en `Program`:</span><span class="sxs-lookup"><span data-stu-id="a4a93-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="a4a93-147">Indique un puerto con el esquema seguro mediante el `ASPNETCORE_URLS` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="a4a93-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="a4a93-148">La variable de entorno configura el servidor.</span><span class="sxs-lookup"><span data-stu-id="a4a93-148">The environment variable configures the server.</span></span> <span data-ttu-id="a4a93-149">El middleware detecta indirectamente el puerto HTTPS a través de <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="a4a93-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="a4a93-150">(Does **no** funcionan en implementaciones de proxy inverso.)</span><span class="sxs-lookup"><span data-stu-id="a4a93-150">(Does **not** work in reverse proxy deployments.)</span></span>
* <span data-ttu-id="a4a93-151">En el desarrollo, establezca una dirección URL HTTPS *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a4a93-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="a4a93-152">Habilitar HTTPS cuando se usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a4a93-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="a4a93-153">Configurar un punto de conexión de dirección URL HTTPS para una implementación de edge orientados al público de [Kestrel](xref:fundamentals/servers/kestrel) o [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a4a93-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="a4a93-154">Solo **un puerto HTTPS** utilizado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4a93-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="a4a93-155">El middleware detecta el puerto a través de <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="a4a93-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="a4a93-156">Cuando se ejecuta una aplicación detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible.</span><span class="sxs-lookup"><span data-stu-id="a4a93-156">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="a4a93-157">El puerto debe configurarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="a4a93-157">The port must be manually configured.</span></span> <span data-ttu-id="a4a93-158">Cuando no se configura el puerto, no se redirigen las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a4a93-158">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="a4a93-159">Cuando se usa Kestrel o HTTP.sys como un servidor perimetral de acceso público, Kestrel o HTTP.sys debe configurarse para que escuche en ambos:</span><span class="sxs-lookup"><span data-stu-id="a4a93-159">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="a4a93-160">El puerto seguro donde se redirige el cliente (normalmente, 443 en 5001 en desarrollo y producción).</span><span class="sxs-lookup"><span data-stu-id="a4a93-160">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="a4a93-161">El puerto no seguro (normalmente, 80 en producción) y 5000 en el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a4a93-161">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="a4a93-162">El puerto no seguro debe ser accesible por el cliente en el orden de la aplicación para recibir una solicitud poco segura y redirigir al cliente a un puerto seguro.</span><span class="sxs-lookup"><span data-stu-id="a4a93-162">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="a4a93-163">Para obtener más información, consulte [configuración de punto de conexión Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="a4a93-163">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="a4a93-164">Escenarios de implementación</span><span class="sxs-lookup"><span data-stu-id="a4a93-164">Deployment scenarios</span></span>

<span data-ttu-id="a4a93-165">Ningún firewall entre el cliente y el servidor también debe tener los puertos de comunicación abierto para el tráfico.</span><span class="sxs-lookup"><span data-stu-id="a4a93-165">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="a4a93-166">Si las solicitudes se reenvían en una configuración de proxy inverso, use [software intermedio de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) antes de llamar al Middleware de redireccionamiento de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-166">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="a4a93-167">Reenvía las actualizaciones de software intermedio de encabezados el `Request.Scheme`, usando la `X-Forwarded-Proto` encabezado.</span><span class="sxs-lookup"><span data-stu-id="a4a93-167">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="a4a93-168">Los middleware que permite redirección los identificadores URI y otras directivas de seguridad para que funcione correctamente.</span><span class="sxs-lookup"><span data-stu-id="a4a93-168">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="a4a93-169">Cuando no se usa el software intermedio de encabezados reenviados, la aplicación de back-end no es posible que la combinación correcta de recepción y terminar en un bucle de redirección.</span><span class="sxs-lookup"><span data-stu-id="a4a93-169">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="a4a93-170">Un mensaje de error de usuario final común es que se han producido demasiadas redirecciones.</span><span class="sxs-lookup"><span data-stu-id="a4a93-170">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="a4a93-171">Al implementar en Azure App Service, siga las instrucciones de [Tutorial: enlazar un certificado SSL personalizado existente a Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="a4a93-171">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="a4a93-172">Opciones</span><span class="sxs-lookup"><span data-stu-id="a4a93-172">Options</span></span>

<span data-ttu-id="a4a93-173">Los siguientes resaltan las llamadas de código [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:</span><span class="sxs-lookup"><span data-stu-id="a4a93-173">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="a4a93-174">Una llamada a `AddHttpsRedirection` sólo es necesario cambiar los valores de `HttpsPort` o `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="a4a93-174">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="a4a93-175">El código resaltado anterior:</span><span class="sxs-lookup"><span data-stu-id="a4a93-175">The preceding highlighted code:</span></span>

* <span data-ttu-id="a4a93-176">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) a <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, que es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a4a93-176">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="a4a93-177">Utilice los campos de la <xref:Microsoft.AspNetCore.Http.StatusCodes> clase para las asignaciones a `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="a4a93-177">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="a4a93-178">Establece el puerto HTTPS a 5001.</span><span class="sxs-lookup"><span data-stu-id="a4a93-178">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="a4a93-179">El valor predeterminado es 443.</span><span class="sxs-lookup"><span data-stu-id="a4a93-179">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="a4a93-180">Configurar las redirecciones permanentes en producción</span><span class="sxs-lookup"><span data-stu-id="a4a93-180">Configure permanent redirects in production</span></span>

<span data-ttu-id="a4a93-181">El valor predeterminado es el software intermedio para enviar un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) con todos los redireccionamientos.</span><span class="sxs-lookup"><span data-stu-id="a4a93-181">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="a4a93-182">Si prefiere enviar un código de estado de redirección permanente cuando la aplicación está en un entorno de desarrollo que no sean, ajuste la configuración de las opciones de middleware en una comprobación condicional para un entorno de desarrollo no.</span><span class="sxs-lookup"><span data-stu-id="a4a93-182">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="a4a93-183">Al configurar un `IWebHostBuilder` en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a4a93-183">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="a4a93-184">Método alternativo de Middleware de redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="a4a93-184">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="a4a93-185">Una alternativa al uso de Middleware de redireccionamiento de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="a4a93-185">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="a4a93-186">`AddRedirectToHttps` También se puede establecer el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="a4a93-186">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="a4a93-187">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a4a93-187">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="a4a93-188">Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar software intermedio de redireccionamiento de HTTPS (`UseHttpsRedirection`) se describe en este tema.</span><span class="sxs-lookup"><span data-stu-id="a4a93-188">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a4a93-189">El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-189">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="a4a93-190">`[RequireHttpsAttribute]` puede decorar los controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="a4a93-190">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a4a93-191">Para aplicar el atributo global, agregue el código siguiente al `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a4a93-191">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="a4a93-192">El código resaltado anterior requiere que todas las solicitudes usan `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4a93-192">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a4a93-193">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a4a93-193">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="a4a93-194">Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a4a93-194">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="a4a93-195">El software intermedio también permite a la aplicación para establecer el código de estado o el código de estado y el puerto cuando se ejecuta la redirección.</span><span class="sxs-lookup"><span data-stu-id="a4a93-195">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="a4a93-196">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="a4a93-196">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a4a93-197">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no se considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="a4a93-197">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a4a93-198">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a4a93-198">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="a4a93-199">Protocolo de seguridad de transporte estricto HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a4a93-199">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="a4a93-200">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricto (HSTS) de HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) es una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a4a93-200">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="a4a93-201">Cuando un [explorador que admita HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recibe este encabezado:</span><span class="sxs-lookup"><span data-stu-id="a4a93-201">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="a4a93-202">El explorador almacena la configuración para el dominio que impide el envío de cualquier comunicación a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4a93-202">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="a4a93-203">El explorador obliga a todas las comunicaciones a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-203">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="a4a93-204">El explorador impide que el usuario mediante certificados de confianza o no es válidos.</span><span class="sxs-lookup"><span data-stu-id="a4a93-204">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="a4a93-205">El explorador deshabilita avisos para que un usuario que confíe en temporalmente dicho certificado.</span><span class="sxs-lookup"><span data-stu-id="a4a93-205">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="a4a93-206">Porque se aplica HSTS por el cliente tiene algunas limitaciones:</span><span class="sxs-lookup"><span data-stu-id="a4a93-206">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="a4a93-207">El cliente debe admitir HSTS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-207">The client must support HSTS.</span></span>
* <span data-ttu-id="a4a93-208">HSTS requiere al menos una solicitud HTTPS correcta para establecer la directiva HSTS.</span><span class="sxs-lookup"><span data-stu-id="a4a93-208">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="a4a93-209">La aplicación debe comprobar todas las solicitudes HTTP y redirigir o rechazar la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4a93-209">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="a4a93-210">ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="a4a93-210">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="a4a93-211">El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a4a93-211">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="a4a93-212">`UseHsts` no se recomienda en el desarrollo porque la configuración de HSTS sea altamente almacenable en caché por los exploradores.</span><span class="sxs-lookup"><span data-stu-id="a4a93-212">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="a4a93-213">De forma predeterminada, `UseHsts` excluye la dirección de bucle invertido local.</span><span class="sxs-lookup"><span data-stu-id="a4a93-213">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="a4a93-214">Para entornos de producción implementar HTTPS por primera vez, conjunto inicial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) en un valor pequeño mediante uno de los <xref:System.TimeSpan> métodos.</span><span class="sxs-lookup"><span data-stu-id="a4a93-214">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="a4a93-215">Establezca el valor de horas en no más de un solo día en caso de que necesite revertir la infraestructura HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4a93-215">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="a4a93-216">Una vez que esté seguro en la sostenibilidad de la configuración de HTTPS, aumente el valor de max-age HSTS; un valor frecuente es un año.</span><span class="sxs-lookup"><span data-stu-id="a4a93-216">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="a4a93-217">El código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4a93-217">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="a4a93-218">Establece el parámetro precarga del encabezado Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="a4a93-218">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="a4a93-219">Precarga no forma parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en una instalación nueva.</span><span class="sxs-lookup"><span data-stu-id="a4a93-219">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="a4a93-220">Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="a4a93-220">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="a4a93-221">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS para hospedar subdominios.</span><span class="sxs-lookup"><span data-stu-id="a4a93-221">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="a4a93-222">Establece explícitamente el parámetro de antigüedad máxima del encabezado Strict-Transport-Security en 60 días.</span><span class="sxs-lookup"><span data-stu-id="a4a93-222">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="a4a93-223">Si no se establece, el valor predeterminado es 30 días.</span><span class="sxs-lookup"><span data-stu-id="a4a93-223">If not set, defaults to 30 days.</span></span> <span data-ttu-id="a4a93-224">Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a4a93-224">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="a4a93-225">Agrega `example.com` a la lista de hosts para excluir.</span><span class="sxs-lookup"><span data-stu-id="a4a93-225">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="a4a93-226">`UseHsts` excluye los hosts de bucle invertido siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4a93-226">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="a4a93-227">`localhost` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="a4a93-227">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a4a93-228">`127.0.0.1` : La dirección de bucle invertido de IPv4.</span><span class="sxs-lookup"><span data-stu-id="a4a93-228">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a4a93-229">`[::1]` : La dirección de bucle invertido de IPv6.</span><span class="sxs-lookup"><span data-stu-id="a4a93-229">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="a4a93-230">Desactivación de HTTPS/HSTS crea un proyecto</span><span class="sxs-lookup"><span data-stu-id="a4a93-230">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="a4a93-231">En algunos escenarios de servicio de back-end donde se controla la seguridad de conexión en el perímetro orientados al público de la red, la configuración de seguridad de conexión en cada nodo no es necesario.</span><span class="sxs-lookup"><span data-stu-id="a4a93-231">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="a4a93-232">Web apps generados a partir de las plantillas en Visual Studio o desde la [dotnet nuevo](/dotnet/core/tools/dotnet-new) Habilitar comando [redirección HTTPS](#require-https) y [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="a4a93-232">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="a4a93-233">Para las implementaciones que no requieren estos escenarios, puede desactivar de HTTPS/HSTS cuando se crea la aplicación de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="a4a93-233">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="a4a93-234">Para participar en HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="a4a93-234">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4a93-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4a93-235">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a4a93-236">Desactive el **configurar HTTPS** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="a4a93-236">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Nuevo cuadro de diálogo de aplicación Web ASP.NET Core que muestra la configuración de casilla de verificación HTTPS no está seleccionada.](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4a93-238">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4a93-238">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="a4a93-239">Use la opción `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="a4a93-239">Use the `--no-https` option.</span></span> <span data-ttu-id="a4a93-240">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="a4a93-240">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="a4a93-241">Cómo configurar un certificado de desarrollador para Docker</span><span class="sxs-lookup"><span data-stu-id="a4a93-241">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="a4a93-242">Consulte [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="a4a93-242">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="a4a93-243">Información adicional</span><span class="sxs-lookup"><span data-stu-id="a4a93-243">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="a4a93-244">Hospedaje de ASP.NET Core en Linux con Apache: configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="a4a93-244">Host ASP.NET Core on Linux with Apache: SSL configuration</span></span>](xref:host-and-deploy/linux-apache#ssl-configuration)
* [<span data-ttu-id="a4a93-245">Hospedar ASP.NET Core en Linux con Nginx: configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="a4a93-245">Host ASP.NET Core on Linux with Nginx: SSL configuration</span></span>](xref:host-and-deploy/linux-nginx#configure-ssl)
* [<span data-ttu-id="a4a93-246">Cómo configurar SSL en IIS</span><span class="sxs-lookup"><span data-stu-id="a4a93-246">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="a4a93-247">Compatibilidad con exploradores de OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="a4a93-247">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
