---
title: "Implementación de servidor web de HTTP.sys en ASP.NET Core"
author: rick-anderson
description: "Presenta HTTP.sys, un servidor web para ASP.NET Core en Windows. Basado en el controlador Http.Sys en modo kernel, HTTP.sys es una alternativa a Kestrel que puede usarse para una conexión directa a Internet sin IIS."
keywords: Prefijos Core,HttpSys,HTTP.sys,HttpListener,url de ASP.NET, SSL
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d035e-105">Implementación de servidor web de HTTP.sys en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d035e-105">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d035e-106">Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d035e-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="d035e-107">En este tema se aplica solo a ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="d035e-107">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="d035e-108">En versiones anteriores de ASP.NET Core, se denomina HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="d035e-108">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="d035e-109">HTTP.sys es un [servidor web de ASP.NET Core](index.md) que sólo se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="d035e-109">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="d035e-110">Se basa en el [controlador de modo de núcleo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="d035e-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d035e-111">HTTP.sys es una alternativa a [Kestrel](kestrel.md) que ofrece algunas características que no Kestel.</span><span class="sxs-lookup"><span data-stu-id="d035e-111">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="d035e-112">**HTTP.sys no puede utilizarse con IIS o IIS Express, ya que no es compatible con la [módulo principal de ASP.NET](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="d035e-112">**HTTP.sys can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="d035e-113">HTTP.sys es compatible con las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="d035e-113">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="d035e-114">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="d035e-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="d035e-115">Uso compartido de puertos</span><span class="sxs-lookup"><span data-stu-id="d035e-115">Port sharing</span></span>
- <span data-ttu-id="d035e-116">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="d035e-116">HTTPS with SNI</span></span>
- <span data-ttu-id="d035e-117">HTTP/2 a través de TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="d035e-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="d035e-118">Transmisión de archivos directa</span><span class="sxs-lookup"><span data-stu-id="d035e-118">Direct file transmission</span></span>
- <span data-ttu-id="d035e-119">Las respuestas en caché</span><span class="sxs-lookup"><span data-stu-id="d035e-119">Response caching</span></span>
- <span data-ttu-id="d035e-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="d035e-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="d035e-121">Versiones admitidas de Windows:</span><span class="sxs-lookup"><span data-stu-id="d035e-121">Supported Windows versions:</span></span>

- <span data-ttu-id="d035e-122">Windows 7 y Windows Server 2008 R2 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d035e-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="d035e-123">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d035e-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="d035e-124">Cuándo utilizar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d035e-124">When to use HTTP.sys</span></span>

<span data-ttu-id="d035e-125">HTTP.sys es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="d035e-125">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="d035e-127">Dado que se basa en Http.Sys, HTTP.sys no requiere un servidor proxy inverso para la protección frente a ataques.</span><span class="sxs-lookup"><span data-stu-id="d035e-127">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="d035e-128">Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características.</span><span class="sxs-lookup"><span data-stu-id="d035e-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="d035e-129">El propio IIS se ejecuta como un agente de escucha HTTP sobre Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="d035e-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="d035e-130">HTTP.sys es una buena elección para implementaciones internas cuando necesite una característica no está disponible en Kestrel, como la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d035e-130">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="d035e-132">Cómo usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d035e-132">How to use HTTP.sys</span></span>

<span data-ttu-id="d035e-133">Este es un resumen de tareas de instalación para el sistema operativo del host y la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d035e-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="d035e-134">Configurar Windows Server</span><span class="sxs-lookup"><span data-stu-id="d035e-134">Configure Windows Server</span></span>

* <span data-ttu-id="d035e-135">Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="d035e-135">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="d035e-136">Registrar previamente los prefijos de dirección URL para enlazar con HTTP.sys y configurar los certificados de SSL</span><span class="sxs-lookup"><span data-stu-id="d035e-136">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="d035e-137">Si no registrar previamente los prefijos de dirección URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d035e-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="d035e-138">La única excepción es si enlazará a localhost mediante HTTP (HTTPS no) con un número de puerto mayor que 1024; en ese caso, no se requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d035e-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="d035e-139">Para obtener más información, consulte [cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="d035e-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="d035e-140">Abrir puertos del firewall para permitir el tráfico llegar a HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d035e-140">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="d035e-141">Puede usar *netsh.exe* o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="d035e-141">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="d035e-142">También hay [configuración del registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="d035e-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="d035e-143">Configurar la aplicación de ASP.NET Core para usar HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d035e-143">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="d035e-144">No hay ninguna instalación de paquete es necesaria si usa el [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="d035e-144">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="d035e-145">El [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) paquete se incluye en el metapackage.</span><span class="sxs-lookup"><span data-stu-id="d035e-145">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="d035e-146">Llame a la `UseHttpSys` método de extensión `WebHostBuilder` en su `Main` método especifica cualquier [HTTP.sys opciones](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) que necesite, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d035e-146">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="d035e-147">Configurar las opciones de HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d035e-147">Configure HTTP.sys options</span></span>

<span data-ttu-id="d035e-148">Estos son algunos de los límites que se pueden configurar y configuración de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d035e-148">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="d035e-149">**Conexiones de cliente máximo**</span><span class="sxs-lookup"><span data-stu-id="d035e-149">**Maximum client connections**</span></span>

<span data-ttu-id="d035e-150">Se puede establecer el número máximo de conexiones simultáneas de TCP abiertos en toda la aplicación con el siguiente código en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d035e-150">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="d035e-151">El número máximo de conexiones es un número ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="d035e-151">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="d035e-152">**Tamaño del cuerpo de solicitud máximo**</span><span class="sxs-lookup"><span data-stu-id="d035e-152">**Maximum request body size**</span></span>

<span data-ttu-id="d035e-153">El tamaño predeterminado del cuerpo de solicitud máximo es: 30.000.000 bytes, que es aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="d035e-153">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="d035e-154">La manera recomendada para invalidar el límite de una aplicación de MVC de ASP.NET Core es usar el [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="d035e-154">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d035e-155">Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación, todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="d035e-155">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="d035e-156">Puede invalidar la configuración en una solicitud específica en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d035e-156">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="d035e-157">Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación se ha empezado la lectura de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d035e-157">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="d035e-158">Hay un `IsReadOnly` propiedad que indica si la `MaxRequestBodySize` propiedad está en estado de solo lectura, lo que significa es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="d035e-158">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d035e-159">Para obtener información sobre otras opciones de HTTP.sys, vea [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d035e-159">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="d035e-160">Configurar los puertos y las direcciones URL para escuchar en</span><span class="sxs-lookup"><span data-stu-id="d035e-160">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="d035e-161">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d035e-161">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d035e-162">Para configurar los puertos y los prefijos de dirección URL, puede usar el `UseUrls` método de extensión, el `urls` argumento de línea de comandos, la variable de entorno ASPNETCORE_URLS, o la `UrlPrefixes` propiedad [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d035e-162">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="d035e-163">El siguiente ejemplo de código usa `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="d035e-163">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="d035e-164">Una ventaja de `UrlPrefixes` es que se obtiene un mensaje de error inmediatamente si se intenta agregar un prefijo con el formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="d035e-164">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="d035e-165">Una ventaja de `UseUrls` (compartida con `urls` y ASPNETCORE_URLS) es que resulta más sencillo alternar entre Kestrel y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d035e-165">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="d035e-166">Si utiliza ambos `UseUrls` (o `urls` o ASPNETCORE_URLS) y `UrlPrefixes`, la configuración de `UrlPrefixes` invalidar las de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d035e-166">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="d035e-167">Para obtener más información, consulte [hospedaje](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="d035e-167">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="d035e-168">HTTP.sys usa el [formatos de cadena HTTP Server API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="d035e-168">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d035e-169">Asegúrese de especificar las mismas cadenas de prefijo en `UseUrls` o `UrlPrefixes` que registrar previamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d035e-169">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="d035e-170">No use IIS</span><span class="sxs-lookup"><span data-stu-id="d035e-170">Don't use IIS</span></span>

<span data-ttu-id="d035e-171">Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d035e-171">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="d035e-172">En Visual Studio, el perfil de inicio predeterminado es de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d035e-172">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="d035e-173">Para ejecutar el proyecto como una aplicación de consola, cambie manualmente el perfil seleccionado, como se muestra en la siguiente captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="d035e-173">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Seleccione el perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="d035e-175">Registrar previamente prefijos de dirección URL y la configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="d035e-175">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="d035e-176">IIS y HTTP.sys se basan en el controlador de modo kernel de subyacente Http.Sys para escuchar las solicitudes e inicial del procesamiento.</span><span class="sxs-lookup"><span data-stu-id="d035e-176">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="d035e-177">En IIS, la interfaz de usuario de administración ofrece una forma es relativamente fácil de configurar todos los elementos.</span><span class="sxs-lookup"><span data-stu-id="d035e-177">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="d035e-178">Sin embargo, debe configurar Http.Sys usted mismo.</span><span class="sxs-lookup"><span data-stu-id="d035e-178">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="d035e-179">La herramienta integrada para hacer que es *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="d035e-179">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="d035e-180">Con *netsh.exe* puede reservar prefijos de dirección URL y asignar los certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="d035e-180">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="d035e-181">La herramienta requiere privilegios administrativos.</span><span class="sxs-lookup"><span data-stu-id="d035e-181">The tool requires administrative privileges.</span></span>

<span data-ttu-id="d035e-182">En el ejemplo siguiente se muestra el mínimo necesario para reservar los prefijos de dirección URL para los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="d035e-182">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="d035e-183">En el ejemplo siguiente se muestra cómo asignar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="d035e-183">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="d035e-184">Aquí está la documentación de referencia de *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="d035e-184">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="d035e-185">Comandos Netsh para hipertexto transferir protocolo (HTTP)</span><span class="sxs-lookup"><span data-stu-id="d035e-185">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="d035e-186">Cadenas de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="d035e-186">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="d035e-187">Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="d035e-187">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="d035e-188">Artículos que hacen referencia a HttpListener se aplican por igual a HTTP.sys, tal y como se basan en Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="d035e-188">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="d035e-189">Cómo: configurar un puerto con un certificado SSL</span><span class="sxs-lookup"><span data-stu-id="d035e-189">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="d035e-190">[Comunicación HTTPS - HttpListener en función de hospedaje y la certificación de cliente](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) esto es un blog de terceros y es bastante antiguo pero aún tiene información útil.</span><span class="sxs-lookup"><span data-stu-id="d035e-190">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="d035e-191">[Cómo: Tutorial utilizando HttpListener o servidor Http de código no administrado (C++) como un servidor Simple SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Esto también es un blog anterior con información útil.</span><span class="sxs-lookup"><span data-stu-id="d035e-191">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="d035e-192">Estas son algunas herramientas de terceros que pueden ser más fáciles de usar que los *netsh.exe* línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d035e-192">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="d035e-193">Estos no son proporcionados por o están aprobados por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d035e-193">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="d035e-194">Las herramientas se ejecutan como administrador de forma predeterminada, ya que *netsh.exe* propio requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d035e-194">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="d035e-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario de la lista y configurar certificados SSL y opciones, las reservas de prefijo y listas de confianza de certificados.</span><span class="sxs-lookup"><span data-stu-id="d035e-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="d035e-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar o configurar certificados SSL y los prefijos de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d035e-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="d035e-197">La interfaz de usuario es más refinada que http.sys Manager y expone unas cuantas más opciones de configuración, pero en caso contrario, proporciona una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="d035e-197">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="d035e-198">No se puede crear una nueva lista de confianza de certificados (CTL), pero puede asignar los existentes.</span><span class="sxs-lookup"><span data-stu-id="d035e-198">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="d035e-199">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d035e-199">Next steps</span></span>

<span data-ttu-id="d035e-200">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="d035e-200">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d035e-201">Aplicación de ejemplo de este artículo</span><span class="sxs-lookup"><span data-stu-id="d035e-201">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="d035e-202">Código fuente de HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d035e-202">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="d035e-203">Hospedar aplicaciones de WPF</span><span class="sxs-lookup"><span data-stu-id="d035e-203">Hosting</span></span>](xref:fundamentals/hosting)
