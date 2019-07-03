---
title: Implementación del servidor web HTTP.sys en ASP.NET Core
author: guardrex
description: Obtenga información sobre HTTP.sys, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo kernel de HTTP.sys, HTTP.sys es una alternativa a Kestrel que se puede usar para conectarse directamente a Internet sin IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/27/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 1b5e26171e5f807fdb918ccf8ae1ff1231ad5356
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048192"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="c4045-104">Implementación del servidor web HTTP.sys en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4045-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="c4045-105">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c4045-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c4045-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) es un [servidor web de ASP.NET Core](xref:fundamentals/servers/index) que solo se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="c4045-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="c4045-107">HTTP.sys supone una alternativa al servidor [Kestrel](xref:fundamentals/servers/kestrel) y ofrece algunas características que este no facilita.</span><span class="sxs-lookup"><span data-stu-id="c4045-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4045-108">HTTP.sys no es compatible con el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) y no se puede usar con IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c4045-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="c4045-109">HTTP.sys admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="c4045-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="c4045-110">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="c4045-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="c4045-111">Uso compartido de puertos</span><span class="sxs-lookup"><span data-stu-id="c4045-111">Port sharing</span></span>
* <span data-ttu-id="c4045-112">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="c4045-112">HTTPS with SNI</span></span>
* <span data-ttu-id="c4045-113">HTTP/2 a través de TLS (Windows 10 o posterior)</span><span class="sxs-lookup"><span data-stu-id="c4045-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="c4045-114">Transmisión directa de archivos</span><span class="sxs-lookup"><span data-stu-id="c4045-114">Direct file transmission</span></span>
* <span data-ttu-id="c4045-115">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="c4045-115">Response caching</span></span>
* <span data-ttu-id="c4045-116">WebSockets (Windows 8 o posterior)</span><span class="sxs-lookup"><span data-stu-id="c4045-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="c4045-117">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="c4045-117">Supported Windows versions:</span></span>

* <span data-ttu-id="c4045-118">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="c4045-118">Windows 7 or later</span></span>
* <span data-ttu-id="c4045-119">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="c4045-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="c4045-120">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c4045-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="c4045-121">Cuándo usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c4045-121">When to use HTTP.sys</span></span>

<span data-ttu-id="c4045-122">HTTP.sys resulta útil para implementaciones en las que:</span><span class="sxs-lookup"><span data-stu-id="c4045-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="c4045-123">Es necesario exponer el servidor directamente a Internet sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="c4045-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="c4045-125">Una implementación interna requiere una característica que no está disponible en Kestrel, como [Autenticación de Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="c4045-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="c4045-127">HTTP.sys es una tecnología consolidada que protege contra muchos tipos de ataques y que proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características.</span><span class="sxs-lookup"><span data-stu-id="c4045-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="c4045-128">El propio IIS se ejecuta como agente de escucha de HTTP sobre HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c4045-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="c4045-129">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c4045-129">HTTP/2 support</span></span>

<span data-ttu-id="c4045-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) está habilitado para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:</span><span class="sxs-lookup"><span data-stu-id="c4045-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="c4045-131">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="c4045-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="c4045-132">Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="c4045-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="c4045-133">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="c4045-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c4045-134">Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="c4045-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c4045-135">Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="c4045-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="c4045-136">HTTP/2 está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c4045-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="c4045-137">Si no se establece una conexión HTTP/2, la conexión vuelve a HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="c4045-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="c4045-138">En una futura versión de Windows, las marcas de configuración de HTTP/2 estarán disponibles, incluida la capacidad para deshabilitar HTTP/2 con HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c4045-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="c4045-139">Autenticación de modo kernel con Kerberos</span><span class="sxs-lookup"><span data-stu-id="c4045-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="c4045-140">HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="c4045-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="c4045-141">La autenticación de modo usuario no se admite con Kerberos y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c4045-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="c4045-142">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="c4045-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="c4045-143">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4045-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="c4045-144">Cómo usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c4045-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="c4045-145">Configuración de la aplicación de ASP.NET Core para usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c4045-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="c4045-146">No se requiere una referencia de paquete en el archivo de proyecto cuando se usa el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="c4045-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="c4045-147">Si no usa el metapaquete `Microsoft.AspNetCore.App`, agregue una referencia de paquete a [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span><span class="sxs-lookup"><span data-stu-id="c4045-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="c4045-148">Llame al método de extensión <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> al compilar el host y especifique las <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions> necesarias:</span><span class="sxs-lookup"><span data-stu-id="c4045-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="c4045-149">La configuración adicional de HTTP.sys se controla a través de [Configuración del Registro](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span><span class="sxs-lookup"><span data-stu-id="c4045-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="c4045-150">**Opciones de HTTP.sys**</span><span class="sxs-lookup"><span data-stu-id="c4045-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="c4045-151">Propiedad.</span><span class="sxs-lookup"><span data-stu-id="c4045-151">Property</span></span> | <span data-ttu-id="c4045-152">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c4045-152">Description</span></span> | <span data-ttu-id="c4045-153">Default</span><span class="sxs-lookup"><span data-stu-id="c4045-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="c4045-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="c4045-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="c4045-155">Controlar si se permite la entrada/salida sincrónica de los objetos `HttpContext.Request.Body` y `HttpContext.Response.Body`.</span><span class="sxs-lookup"><span data-stu-id="c4045-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="c4045-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="c4045-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="c4045-157">Permitir solicitudes anónimas.</span><span class="sxs-lookup"><span data-stu-id="c4045-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="c4045-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="c4045-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="c4045-159">Especificar los esquemas de autenticación permitidos.</span><span class="sxs-lookup"><span data-stu-id="c4045-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="c4045-160">Puede modificarse en cualquier momento antes de eliminar el agente de escucha.</span><span class="sxs-lookup"><span data-stu-id="c4045-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="c4045-161">Los valores se proporcionan con la [enumeración AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None` y `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="c4045-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="c4045-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="c4045-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="c4045-163">Intentar el almacenamiento en memoria caché en [modo kernel](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) de las respuestas con encabezados elegibles.</span><span class="sxs-lookup"><span data-stu-id="c4045-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="c4045-164">Es posible que la respuesta no incluya encabezados `Set-Cookie`, `Vary` o `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="c4045-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="c4045-165">Debe incluir un encabezado `Cache-Control` que sea `public` y un valor `shared-max-age` o `max-age`, o un encabezado `Expires`.</span><span class="sxs-lookup"><span data-stu-id="c4045-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="c4045-166">Número máximo de aceptaciones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="c4045-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="c4045-167">Cinco &times; [entorno.<br> ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="c4045-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="c4045-168">Establecer el número máximo de conexiones simultáneas que se aceptan.</span><span class="sxs-lookup"><span data-stu-id="c4045-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="c4045-169">Use `-1` para infinito.</span><span class="sxs-lookup"><span data-stu-id="c4045-169">Use `-1` for infinite.</span></span> <span data-ttu-id="c4045-170">Use `null` para usar la configuración de la máquina del Registro.</span><span class="sxs-lookup"><span data-stu-id="c4045-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="c4045-171">(ilimitado)</span><span class="sxs-lookup"><span data-stu-id="c4045-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="c4045-172">Vea la sección <a href="#maxrequestbodysize">MaxRequestBodySize</a>.</span><span class="sxs-lookup"><span data-stu-id="c4045-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="c4045-173">30 000 000 bytes</span><span class="sxs-lookup"><span data-stu-id="c4045-173">30000000 bytes</span></span><br><span data-ttu-id="c4045-174">(~28,6 MB)</span><span class="sxs-lookup"><span data-stu-id="c4045-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="c4045-175">Número máximo de solicitudes que se pueden poner en cola.</span><span class="sxs-lookup"><span data-stu-id="c4045-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="c4045-176">1000</span><span class="sxs-lookup"><span data-stu-id="c4045-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="c4045-177">Indicar si las escrituras del cuerpo de respuesta que no se producen debido a desconexiones del cliente deben iniciar excepciones o finalizar con normalidad.</span><span class="sxs-lookup"><span data-stu-id="c4045-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="c4045-178">(finalizar con normalidad)</span><span class="sxs-lookup"><span data-stu-id="c4045-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="c4045-179">Exponer la configuración de <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> de HTTP.sys, que también puede configurarse en el Registro.</span><span class="sxs-lookup"><span data-stu-id="c4045-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="c4045-180">Siga los vínculos de API para obtener más información sobre cada configuración, incluidos los valores predeterminados:</span><span class="sxs-lookup"><span data-stu-id="c4045-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="c4045-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Tiempo permitido para que la API HTTP Server purgue el cuerpo de la entidad en una conexión persistente.</span><span class="sxs-lookup"><span data-stu-id="c4045-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="c4045-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Tiempo permitido para que llegue el cuerpo de la entidad de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c4045-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="c4045-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Tiempo permitido para que la API HTTP Server analice el encabezado de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c4045-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="c4045-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Tiempo permitido para una conexión inactiva.</span><span class="sxs-lookup"><span data-stu-id="c4045-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="c4045-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; Tasa mínima de envío de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="c4045-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="c4045-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Tiempo permitido para que la solicitud permanezca en la cola de solicitudes antes de que la aplicación la recoja.</span><span class="sxs-lookup"><span data-stu-id="c4045-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="c4045-187">Especifique <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> para registrarse con HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c4045-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="c4045-188">El más útil es [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), que se usa para agregar un prefijo a la colección.</span><span class="sxs-lookup"><span data-stu-id="c4045-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="c4045-189">Pueden modificarse en cualquier momento antes de eliminar el agente de escucha.</span><span class="sxs-lookup"><span data-stu-id="c4045-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="c4045-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="c4045-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="c4045-191">Tamaño máximo permitido de cualquier cuerpo de solicitud en bytes.</span><span class="sxs-lookup"><span data-stu-id="c4045-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="c4045-192">Cuando se establece en `null`, el tamaño máximo del cuerpo de solicitud es ilimitado.</span><span class="sxs-lookup"><span data-stu-id="c4045-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="c4045-193">Este límite no tiene ningún efecto en las conexiones actualizadas, que siempre son ilimitadas.</span><span class="sxs-lookup"><span data-stu-id="c4045-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="c4045-194">El método recomendado para reemplazar el límite de una aplicación de ASP.NET Core MVC para un solo objeto `IActionResult` consiste en usar el atributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="c4045-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="c4045-195">Se inicia una excepción si la aplicación intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="c4045-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="c4045-196">Puede usarse una propiedad `IsReadOnly` que indique si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="c4045-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="c4045-197">Si la aplicación debe reemplazar <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> por solicitud, use <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span><span class="sxs-lookup"><span data-stu-id="c4045-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="c4045-198">If usa Visual Studio, asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c4045-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="c4045-199">En Visual Studio, el perfil de inicio predeterminado es para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c4045-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="c4045-200">Para ejecutar el proyecto como aplicación de consola, cambie manualmente el perfil seleccionado, tal y como se muestra en la siguiente captura de pantalla:</span><span class="sxs-lookup"><span data-stu-id="c4045-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![Seleccionar perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="c4045-202">Configurar Windows Server</span><span class="sxs-lookup"><span data-stu-id="c4045-202">Configure Windows Server</span></span>

1. <span data-ttu-id="c4045-203">Determine los puertos que se van a abrir para la aplicación y use el [Firewall de Windows](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) o el cmdlet [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) de PowerShell para abrir los puertos del firewall y permitir que el tráfico llegue a HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c4045-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="c4045-204">En los comandos y la configuración de la aplicación siguientes, se usa el puerto 443.</span><span class="sxs-lookup"><span data-stu-id="c4045-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="c4045-205">Cuando realice una implementación en una máquina virtual de Azure, abra los puertos del [grupo de seguridad de red](/azure/virtual-machines/windows/nsg-quickstart-portal).</span><span class="sxs-lookup"><span data-stu-id="c4045-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="c4045-206">En los comandos y la configuración de la aplicación siguientes, se usa el puerto 443.</span><span class="sxs-lookup"><span data-stu-id="c4045-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="c4045-207">Si es necesario, obtenga e instale los certificados X.509.</span><span class="sxs-lookup"><span data-stu-id="c4045-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="c4045-208">En Windows, puede crear certificados autofirmados con el [cmdlet New-SelfSignedCertificate de PowerShell](/powershell/module/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="c4045-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="c4045-209">Para obtener un ejemplo no compatible, vea [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="c4045-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="c4045-210">Instale certificados autofirmados o firmados por CA en el almacén **personal del equipo** > **local** del servidor.</span><span class="sxs-lookup"><span data-stu-id="c4045-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="c4045-211">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd), instale .NET Core, .NET Framework o ambos (si se trata de una aplicación de .NET Core que tiene como destino .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="c4045-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="c4045-212">**.NET Core**: si la aplicación requiere .NET Core, obtenga y ejecute el instalador del **entorno de ejecución de .NET Core** desde las [descargas de .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c4045-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="c4045-213">No instale el SDK completo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c4045-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="c4045-214">**.NET Framework**: si la aplicación requiere .NET Framework, consulte la [guía de instalación de .NET Framework.](/dotnet/framework/install/)</span><span class="sxs-lookup"><span data-stu-id="c4045-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="c4045-215">Instale la versión necesaria de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c4045-215">Install the required .NET Framework.</span></span> <span data-ttu-id="c4045-216">El instalador de la versión más reciente de .NET Framework está disponible en la página de [descargas de .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c4045-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="c4045-217">Si la aplicación se basa en la [implementación autocontenida](/dotnet/core/deploying/#framework-dependent-deployments-scd), incluirá el entorno de ejecución en la implementación.</span><span class="sxs-lookup"><span data-stu-id="c4045-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="c4045-218">No se requiere la instalación de ningún marco en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c4045-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="c4045-219">Configure los puertos y las direcciones URL en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4045-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="c4045-220">ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c4045-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="c4045-221">Para configurar los puertos y los prefijos de dirección URL, las opciones incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4045-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="c4045-222">El argumento de la línea de comandos `urls`</span><span class="sxs-lookup"><span data-stu-id="c4045-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="c4045-223">La variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="c4045-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="c4045-224">En el código siguiente, se muestra cómo usar <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>con la dirección IP local del servidor, `10.0.0.4`, en el puerto 443:</span><span class="sxs-lookup"><span data-stu-id="c4045-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="c4045-225">Una ventaja de `UrlPrefixes` es que se genera inmediatamente un mensaje de error para prefijos con formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="c4045-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="c4045-226">La configuración de `UrlPrefixes` invalida la configuración de `UseUrls`/`urls`/`ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="c4045-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="c4045-227">Por lo tanto, la ventaja de `UseUrls`, `urls` y la variable de entorno `ASPNETCORE_URLS` es que resulta más fácil de cambiar entre Kestrel y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c4045-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="c4045-228">HTTP.sys usa los [formatos de cadena UrlPrefix de la API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4045-228">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="c4045-229">Los enlaces de carácter comodín de nivel superior (`http://*:80/` y `http://+:80`) **no** se deben usar.</span><span class="sxs-lookup"><span data-stu-id="c4045-229">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="c4045-230">Los enlaces de carácter comodín de nivel superior generan vulnerabilidades de seguridad en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4045-230">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="c4045-231">Esto se aplica tanto a los caracteres comodín fuertes como a los débiles.</span><span class="sxs-lookup"><span data-stu-id="c4045-231">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="c4045-232">Use nombres de host explícitos o direcciones IP en lugar de caracteres comodín.</span><span class="sxs-lookup"><span data-stu-id="c4045-232">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="c4045-233">Los enlaces de carácter comodín de subdominio (por ejemplo, `*.mysub.com`) no suponen ningún riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable).</span><span class="sxs-lookup"><span data-stu-id="c4045-233">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="c4045-234">Para obtener más información, vea [RFC 7230: Sección 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="c4045-234">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="c4045-235">Registre previamente los prefijos de URL en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c4045-235">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="c4045-236">La herramienta integrada para configurar HTTP.sys es *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="c4045-236">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="c4045-237">*netsh.exe* se usa para reservar prefijos de dirección URL y asignar certificados X.509.</span><span class="sxs-lookup"><span data-stu-id="c4045-237">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="c4045-238">Esta herramienta requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="c4045-238">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="c4045-239">Use la herramienta *netsh.exe* para registrar URL de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c4045-239">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="c4045-240">`<URL>`: localizador uniforme de recursos (URL) completo especificado.</span><span class="sxs-lookup"><span data-stu-id="c4045-240">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="c4045-241">No use ningún enlace de carácter comodín.</span><span class="sxs-lookup"><span data-stu-id="c4045-241">Don't use a wildcard binding.</span></span> <span data-ttu-id="c4045-242">Use un nombre de host válido o la dirección IP local.</span><span class="sxs-lookup"><span data-stu-id="c4045-242">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="c4045-243">*La dirección URL debe incluir una barra diagonal final.*</span><span class="sxs-lookup"><span data-stu-id="c4045-243">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="c4045-244">`<USER>`: especifica el nombre de usuario o de grupo de usuarios.</span><span class="sxs-lookup"><span data-stu-id="c4045-244">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="c4045-245">En el ejemplo siguiente, la dirección IP local del servidor es `10.0.0.4`:</span><span class="sxs-lookup"><span data-stu-id="c4045-245">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="c4045-246">Cuando se registra una dirección URL, la herramienta responde con `URL reservation successfully added`.</span><span class="sxs-lookup"><span data-stu-id="c4045-246">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="c4045-247">Para eliminar una dirección URL registrada, use el comando `delete urlacl`:</span><span class="sxs-lookup"><span data-stu-id="c4045-247">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="c4045-248">Registre certificados X.509 en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c4045-248">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="c4045-249">Use la herramienta *netsh.exe* para registrar certificados de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c4045-249">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="c4045-250">`<IP>`: especifica la dirección IP local para el enlace.</span><span class="sxs-lookup"><span data-stu-id="c4045-250">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="c4045-251">No use ningún enlace de carácter comodín.</span><span class="sxs-lookup"><span data-stu-id="c4045-251">Don't use a wildcard binding.</span></span> <span data-ttu-id="c4045-252">Use una dirección IP válida.</span><span class="sxs-lookup"><span data-stu-id="c4045-252">Use a valid IP address.</span></span>
   * <span data-ttu-id="c4045-253">`<PORT>`: especifica el puerto para el enlace.</span><span class="sxs-lookup"><span data-stu-id="c4045-253">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="c4045-254">`<THUMBPRINT>`: huella digital del certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="c4045-254">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="c4045-255">`<GUID>`: GUID generado por el desarrollador para representar la aplicación con fines informativos.</span><span class="sxs-lookup"><span data-stu-id="c4045-255">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="c4045-256">A modo de referencia, almacene el GUID en la aplicación como etiqueta de paquete:</span><span class="sxs-lookup"><span data-stu-id="c4045-256">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="c4045-257">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c4045-257">In Visual Studio:</span></span>
     * <span data-ttu-id="c4045-258">Abra las propiedades del proyecto de la aplicación. Para ello, haga clic con el botón derecho en la aplicación, en el **Explorador de soluciones**, y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="c4045-258">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="c4045-259">Seleccione la pestaña **Paquete**.</span><span class="sxs-lookup"><span data-stu-id="c4045-259">Select the **Package** tab.</span></span>
     * <span data-ttu-id="c4045-260">Escriba el GUID que creó en el campo **Etiquetas**.</span><span class="sxs-lookup"><span data-stu-id="c4045-260">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="c4045-261">Si no usa Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c4045-261">When not using Visual Studio:</span></span>
     * <span data-ttu-id="c4045-262">Abra el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4045-262">Open the app's project file.</span></span>
     * <span data-ttu-id="c4045-263">Agregue una propiedad `<PackageTags>` a un elemento `<PropertyGroup>` nuevo o existente con el GUID que creó:</span><span class="sxs-lookup"><span data-stu-id="c4045-263">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="c4045-264">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c4045-264">In the following example:</span></span>

   * <span data-ttu-id="c4045-265">La dirección IP local del servidor es `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="c4045-265">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="c4045-266">Un generador de GUID aleatorios en línea proporciona el valor `appid`.</span><span class="sxs-lookup"><span data-stu-id="c4045-266">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="c4045-267">Cuando se registra un certificado, la herramienta responde con `SSL Certificate successfully added`.</span><span class="sxs-lookup"><span data-stu-id="c4045-267">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="c4045-268">Para eliminar un registro de certificados, use el comando `delete sslcert`:</span><span class="sxs-lookup"><span data-stu-id="c4045-268">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="c4045-269">Documentación de referencia de *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="c4045-269">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="c4045-270">Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)</span><span class="sxs-lookup"><span data-stu-id="c4045-270">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="c4045-271">Cadenas de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="c4045-271">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="c4045-272">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4045-272">Run the app.</span></span>

   <span data-ttu-id="c4045-273">No se necesitan privilegios de administrador para ejecutar la aplicación al enlazar a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024.</span><span class="sxs-lookup"><span data-stu-id="c4045-273">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="c4045-274">Para otras configuraciones (por ejemplo, usar una dirección IP local o enlazar al puerto 443), ejecute la aplicación con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="c4045-274">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="c4045-275">La aplicación responde a la dirección IP pública del servidor.</span><span class="sxs-lookup"><span data-stu-id="c4045-275">The app responds at the server's public IP address.</span></span> <span data-ttu-id="c4045-276">En este ejemplo, el servidor está disponible en Internet mediante su dirección IP pública, `104.214.79.47`.</span><span class="sxs-lookup"><span data-stu-id="c4045-276">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="c4045-277">En este ejemplo, se usa un certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c4045-277">A development certificate is used in this example.</span></span> <span data-ttu-id="c4045-278">La página se carga de forma segura tras omitir la advertencia de que el certificado no es de confianza para el explorador.</span><span class="sxs-lookup"><span data-stu-id="c4045-278">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Ventana del explorador en la que se muestra cargada la página de índice de la aplicación.](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="c4045-280">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="c4045-280">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="c4045-281">En el caso de las aplicaciones hospedadas por HTTP.sys que interactúan con las solicitudes de Internet o de una red corporativa, puede que sea necesario configurar más elementos si esas aplicaciones se hospedan detrás de servidores proxy y equilibradores de carga.</span><span class="sxs-lookup"><span data-stu-id="c4045-281">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="c4045-282">Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="c4045-282">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c4045-283">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c4045-283">Additional resources</span></span>

* [<span data-ttu-id="c4045-284">Habilitación de la autenticación de Windows con HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c4045-284">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#httpsys)
* [<span data-ttu-id="c4045-285">API HTTP Server</span><span class="sxs-lookup"><span data-stu-id="c4045-285">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="c4045-286">Repositorio aspnet/HttpSysServer de GitHub (código fuente)</span><span class="sxs-lookup"><span data-stu-id="c4045-286">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="c4045-287">El host</span><span class="sxs-lookup"><span data-stu-id="c4045-287">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
