---
title: "Implementación del servidor web HTTP.sys en ASP.NET Core"
author: rick-anderson
description: "Este artículo presenta HTTP.sys, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo kernel de Http.Sys, HTTP.sys es una alternativa a Kestrel que se puede usar para conectarse directamente a Internet sin IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="4d41b-104">Implementación del servidor web HTTP.sys en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d41b-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="4d41b-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="4d41b-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="4d41b-106">Este tema es válido únicamente para ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="4d41b-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="4d41b-107">En versiones anteriores de ASP.NET Core, HTTP.sys se denominaba [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="4d41b-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="4d41b-108">HTTP.sys es un [servidor web de ASP.NET Core](index.md) que solo se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="4d41b-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="4d41b-109">Está basado en el [controlador de modo kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d41b-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="4d41b-110">HTTP.sys supone una alternativa a [Kestrel](kestrel.md) que ofrece algunas características que este último no facilita.</span><span class="sxs-lookup"><span data-stu-id="4d41b-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="4d41b-111">**De hecho, HTTP.sys no se puede usar con IIS o IIS Express, ya que no es compatible con el [módulo ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="4d41b-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="4d41b-112">HTTP.sys admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="4d41b-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="4d41b-113">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="4d41b-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="4d41b-114">Uso compartido de puertos</span><span class="sxs-lookup"><span data-stu-id="4d41b-114">Port sharing</span></span>
- <span data-ttu-id="4d41b-115">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="4d41b-115">HTTPS with SNI</span></span>
- <span data-ttu-id="4d41b-116">HTTP/2 a través de TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="4d41b-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="4d41b-117">Transmisión directa de archivos</span><span class="sxs-lookup"><span data-stu-id="4d41b-117">Direct file transmission</span></span>
- <span data-ttu-id="4d41b-118">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="4d41b-118">Response caching</span></span>
- <span data-ttu-id="4d41b-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="4d41b-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="4d41b-120">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="4d41b-120">Supported Windows versions:</span></span>

- <span data-ttu-id="4d41b-121">Windows 7 y Windows Server 2008 R2 y posteriores</span><span class="sxs-lookup"><span data-stu-id="4d41b-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="4d41b-122">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d41b-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="4d41b-123">Cuándo usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d41b-123">When to use HTTP.sys</span></span>

<span data-ttu-id="4d41b-124">HTTP.sys es útil en implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="4d41b-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="4d41b-126">Al estar basado en Http.Sys, HTTP.sys no necesita un servidor proxy inverso para protegerse de ataques.</span><span class="sxs-lookup"><span data-stu-id="4d41b-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="4d41b-127">Http.Sys es una tecnología consolidada que protege contra muchos tipos de ataques y que proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características.</span><span class="sxs-lookup"><span data-stu-id="4d41b-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="4d41b-128">El propio IIS se ejecuta como una escucha HTTP sobre Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="4d41b-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="4d41b-129">HTTP.sys constituye una buena opción en las implementaciones internas cuando se necesita una característica que no está disponible en Kestrel, como la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="4d41b-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="4d41b-131">Cómo usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d41b-131">How to use HTTP.sys</span></span>

<span data-ttu-id="4d41b-132">Este es un resumen de las tareas de configuración del sistema operativo del host y la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d41b-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="4d41b-133">Configurar Windows Server</span><span class="sxs-lookup"><span data-stu-id="4d41b-133">Configure Windows Server</span></span>

* <span data-ttu-id="4d41b-134">Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="4d41b-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="4d41b-135">Registre previamente los prefijos de URL para enlazarlos a HTTP.sys y configure los certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="4d41b-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="4d41b-136">Si no registra previamente los prefijos de URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="4d41b-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="4d41b-137">La única excepción es si enlaza a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024; en ese caso, no se necesitan privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="4d41b-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="4d41b-138">Para más información, vea [Cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="4d41b-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="4d41b-139">Abra puertos del firewall para permitir que el tráfico llegue a HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="4d41b-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="4d41b-140">Puede usar *netsh.exe* o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="4d41b-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="4d41b-141">También hay [opciones de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="4d41b-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="4d41b-142">Configurar la aplicación ASP.NET Core para usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d41b-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="4d41b-143">No es necesario instalar ningún paquete si se usa el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="4d41b-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="4d41b-144">El paquete [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) está incluido en este metapaquete.</span><span class="sxs-lookup"><span data-stu-id="4d41b-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="4d41b-145">Llame al método de extensión `UseHttpSys` en `WebHostBuilder` en el método `Main` y especifique cualquier [opción de HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) que necesite, como se muestra en el siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4d41b-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="4d41b-146">Configurar las opciones de HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d41b-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="4d41b-147">Estas son algunas de las opciones y limitaciones de HTTP.sys que se pueden configurar.</span><span class="sxs-lookup"><span data-stu-id="4d41b-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="4d41b-148">**Conexiones de cliente máximas**</span><span class="sxs-lookup"><span data-stu-id="4d41b-148">**Maximum client connections**</span></span>

<span data-ttu-id="4d41b-149">Se puede establecer el número máximo de conexiones TCP abiertas simultáneas en toda la aplicación con este código en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d41b-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="4d41b-150">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="4d41b-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="4d41b-151">**Tamaño máximo del cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="4d41b-151">**Maximum request body size**</span></span>

<span data-ttu-id="4d41b-152">El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="4d41b-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="4d41b-153">El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="4d41b-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="4d41b-154">Este es un ejemplo que muestra cómo configurar la restricción en toda la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="4d41b-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="4d41b-155">Puede invalidar la configuración en una solicitud específica en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d41b-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="4d41b-156">Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="4d41b-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="4d41b-157">Hay una propiedad `IsReadOnly` que indica si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="4d41b-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="4d41b-158">Para más información sobre otras opciones de HTTP.sys, vea [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="4d41b-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="4d41b-159">Configurar puertos y direcciones URL de escucha</span><span class="sxs-lookup"><span data-stu-id="4d41b-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="4d41b-160">ASP.NET Core enlaza a `http://localhost:5000` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="4d41b-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="4d41b-161">Para configurar los puertos y los prefijos de dirección URL, puede usar el método de extensión `UseUrls`, el argumento de línea de comandos `urls`, la variable de entorno ASPNETCORE_URLS o la propiedad `UrlPrefixes` de [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="4d41b-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="4d41b-162">En el siguiente ejemplo se usa `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="4d41b-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="4d41b-163">Una ventaja de `UrlPrefixes` es que se recibe un mensaje de error inmediatamente si se intenta agregar un prefijo con el formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="4d41b-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="4d41b-164">Una ventaja de `UseUrls` (compartida con `urls` y ASPNETCORE_URLS) es que resulta más sencillo alternar entre Kestrel y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="4d41b-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="4d41b-165">Si usa tanto `UseUrls` (o `urls` o ASPNETCORE_URLS) como `UrlPrefixes`, la configuración de `UrlPrefixes` invalidará la de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="4d41b-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="4d41b-166">Para más información, vea [Hospedaje](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="4d41b-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="4d41b-167">HTTP.sys usa los [formatos de cadena UrlPrefix de la API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d41b-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="4d41b-168">Asegúrese de especificar en `UseUrls` o en `UrlPrefixes` las mismas cadenas de prefijo que registró previamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4d41b-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="4d41b-169">No usar IIS</span><span class="sxs-lookup"><span data-stu-id="4d41b-169">Don't use IIS</span></span>

<span data-ttu-id="4d41b-170">Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4d41b-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="4d41b-171">En Visual Studio, el perfil de inicio predeterminado corresponde a IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4d41b-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="4d41b-172">Para ejecutar el proyecto como una aplicación de consola, cambie manualmente el perfil seleccionado, como se muestra en esta captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="4d41b-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Seleccionar el perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="4d41b-174">Registrar previamente prefijos de dirección URL y configurar SSL</span><span class="sxs-lookup"><span data-stu-id="4d41b-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="4d41b-175">Tanto IIS como HTTP.sys se basan en el controlador de modo kernel de Http.Sys subyacente para escuchar las solicitudes y realizar el procesamiento inicial.</span><span class="sxs-lookup"><span data-stu-id="4d41b-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="4d41b-176">En IIS, la interfaz de usuario de administración ofrece una forma relativamente fácil de configurarlo todo.</span><span class="sxs-lookup"><span data-stu-id="4d41b-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="4d41b-177">Pero tendrá que configurar Http.Sys por su cuenta.</span><span class="sxs-lookup"><span data-stu-id="4d41b-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="4d41b-178">La herramienta integrada para llevar esto a cabo es *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="4d41b-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="4d41b-179">Con *netsh.exe* puede reservar prefijos de dirección URL y asignar certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="4d41b-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="4d41b-180">Esta herramienta requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="4d41b-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="4d41b-181">En este ejemplo se muestra lo mínimo que se necesita para reservar los prefijos de dirección URL de los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="4d41b-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="4d41b-182">En el siguiente ejemplo se muestra cómo asignar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="4d41b-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="4d41b-183">Documentación de referencia de *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="4d41b-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="4d41b-184">Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)</span><span class="sxs-lookup"><span data-stu-id="4d41b-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="4d41b-185">Cadenas de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="4d41b-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="4d41b-186">Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="4d41b-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="4d41b-187">Los artículos que hacen referencia a HttpListener son válidos también para HTTP.sys, puesto que ambos se basan en Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="4d41b-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="4d41b-188">Configuración de un puerto con un certificado SSL</span><span class="sxs-lookup"><span data-stu-id="4d41b-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="4d41b-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicación HTTPS: Certificación de cliente y hospedaje basado en HttpListener): blog de terceros bastante antiguo, pero que aún tiene información útil.</span><span class="sxs-lookup"><span data-stu-id="4d41b-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="4d41b-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedimientos con código no administrado de HttpListener o Http Server (C++) como un servidor simple SSL): también es un blog antiguo con información útil.</span><span class="sxs-lookup"><span data-stu-id="4d41b-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="4d41b-191">Estas son algunas herramientas de terceros que pueden ser más fáciles de usar que la línea de comandos *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="4d41b-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="4d41b-192">No están proporcionadas ni aprobadas por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4d41b-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="4d41b-193">Las herramientas se ejecutan como administrador de forma predeterminada, dado que *netsh.exe* requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="4d41b-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="4d41b-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario para mostrar una lista de certificados SSL y configurarlos, además de configurar otras opciones, reservas de prefijo y listas de certificados de confianza.</span><span class="sxs-lookup"><span data-stu-id="4d41b-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="4d41b-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar una lista de certificados SSL y prefijos de dirección URL, así como configurarlos.</span><span class="sxs-lookup"><span data-stu-id="4d41b-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="4d41b-196">La interfaz de usuario es más refinada que la de http.sys Manager e incluye algunas opciones de configuración más, aunque proporciona una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="4d41b-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="4d41b-197">No se puede crear una nueva lista de certificados de confianza (CTL), pero puede asignar las existentes.</span><span class="sxs-lookup"><span data-stu-id="4d41b-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="4d41b-198">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4d41b-198">Next steps</span></span>

<span data-ttu-id="4d41b-199">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="4d41b-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="4d41b-200">Aplicación de ejemplo de este artículo</span><span class="sxs-lookup"><span data-stu-id="4d41b-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="4d41b-201">Código fuente de HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d41b-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="4d41b-202">Hospedar aplicaciones de WPF</span><span class="sxs-lookup"><span data-stu-id="4d41b-202">Hosting</span></span>](xref:fundamentals/hosting)
