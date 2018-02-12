---
title: "Implementación del servidor web WebListener en ASP.NET Core"
author: rick-anderson
description: "Este artículo presenta WebListener, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo kernel de Http.Sys, WebListener es una alternativa a Kestrel que permite conectarse directamente a Internet sin IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: fb2e0621645a48f4e603d754d8babbc07a78cae4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d9efb-104">Implementación del servidor web WebListener en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9efb-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d9efb-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d9efb-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="d9efb-106">Este tema se aplica solo a ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d9efb-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="d9efb-107">En ASP.NET Core 2.0, WebListener se denomina [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="d9efb-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="d9efb-108">WebListener es un [servidor web para ASP.NET Core](index.md) que se ejecuta solo en Windows.</span><span class="sxs-lookup"><span data-stu-id="d9efb-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="d9efb-109">Está basado en el [controlador de modo kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9efb-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d9efb-110">WebListener es una alternativa a [Kestrel](kestrel.md) para conectarse directamente a Internet sin depender de IIS como un servidor proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="d9efb-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="d9efb-111">De hecho, **WebListener no se puede usar con IIS o IIS Express, ya que estos no son compatibles con el [módulo ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="d9efb-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="d9efb-112">Aunque WebListener se desarrolló para ASP.NET Core, se puede usar directamente en cualquier aplicación .NET Core o .NET Framework a través del paquete NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="d9efb-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="d9efb-113">WebListener admite estas características:</span><span class="sxs-lookup"><span data-stu-id="d9efb-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="d9efb-114">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="d9efb-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="d9efb-115">Uso compartido de puertos</span><span class="sxs-lookup"><span data-stu-id="d9efb-115">Port sharing</span></span>
- <span data-ttu-id="d9efb-116">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="d9efb-116">HTTPS with SNI</span></span>
- <span data-ttu-id="d9efb-117">HTTP/2 a través de TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="d9efb-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="d9efb-118">Transmisión directa de archivos</span><span class="sxs-lookup"><span data-stu-id="d9efb-118">Direct file transmission</span></span>
- <span data-ttu-id="d9efb-119">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="d9efb-119">Response caching</span></span>
- <span data-ttu-id="d9efb-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="d9efb-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="d9efb-121">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="d9efb-121">Supported Windows versions:</span></span>

- <span data-ttu-id="d9efb-122">Windows 7 y Windows Server 2008 R2 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d9efb-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="d9efb-123">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9efb-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="d9efb-124">Cuándo usar WebListener</span><span class="sxs-lookup"><span data-stu-id="d9efb-124">When to use WebListener</span></span>

<span data-ttu-id="d9efb-125">WebListener es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="d9efb-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="d9efb-127">Al estar basado en Http.Sys, WebListener no necesita un servidor proxy inverso para protegerse de ataques.</span><span class="sxs-lookup"><span data-stu-id="d9efb-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="d9efb-128">Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características.</span><span class="sxs-lookup"><span data-stu-id="d9efb-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="d9efb-129">El propio IIS se ejecuta como una escucha HTTP sobre Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="d9efb-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="d9efb-130">WebListener también es una buena elección para implementaciones internas cuando se necesita alguna de las características que ofrece y estas no se pueden obtener usando Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d9efb-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="d9efb-132">Cómo usar WebListener</span><span class="sxs-lookup"><span data-stu-id="d9efb-132">How to use WebListener</span></span>

<span data-ttu-id="d9efb-133">Este es un resumen de las tareas de instalación para el sistema operativo del host y la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9efb-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="d9efb-134">Configurar Windows Server</span><span class="sxs-lookup"><span data-stu-id="d9efb-134">Configure Windows Server</span></span>

* <span data-ttu-id="d9efb-135">Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="d9efb-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="d9efb-136">Registre previamente los prefijos de URL para enlazarlos a WebListener y configure los certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="d9efb-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="d9efb-137">Si no registra previamente los prefijos de URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d9efb-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="d9efb-138">La única excepción es si enlaza a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024; en ese caso, no se necesitan privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d9efb-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="d9efb-139">Para más información, vea [Cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="d9efb-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="d9efb-140">Abra puertos del firewall para permitir que el tráfico llegue a WebListener.</span><span class="sxs-lookup"><span data-stu-id="d9efb-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="d9efb-141">Puede usar netsh.exe o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="d9efb-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="d9efb-142">También hay [valores de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="d9efb-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="d9efb-143">Configurar la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9efb-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="d9efb-144">Instale el paquete NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="d9efb-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="d9efb-145">También se instalará [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como una dependencia.</span><span class="sxs-lookup"><span data-stu-id="d9efb-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="d9efb-146">Llame al método de extensión `UseWebListener` en [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) en el método `Main` y especifique cualquier [opción](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) y [configuración](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) de WebListener que necesite, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9efb-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="d9efb-147">Configurar puertos y URL para escuchar en ellos</span><span class="sxs-lookup"><span data-stu-id="d9efb-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="d9efb-148">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d9efb-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d9efb-149">Para configurar los puertos y los prefijos de URL, puede usar el método de extensión `UseURLs`, el argumento de línea de comandos `urls` o el sistema de configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9efb-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="d9efb-150">Para más información, vea [Hospedaje](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="d9efb-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="d9efb-151">WebListener usa los [formatos de cadena de prefijo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9efb-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="d9efb-152">No hay ningún requisito de formato de cadena de prefijo que sea específico de WebListener.</span><span class="sxs-lookup"><span data-stu-id="d9efb-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="d9efb-153">Asegúrese de especificar en `UseUrls` las mismas cadenas de prefijo que registró previamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d9efb-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="d9efb-154">Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d9efb-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="d9efb-155">En Visual Studio, el perfil de inicio predeterminado es para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d9efb-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="d9efb-156">Para ejecutar el proyecto como una aplicación de consola, tiene que cambiar manualmente el perfil seleccionado, como se muestra en esta captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="d9efb-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Seleccionar perfil de aplicación de consola](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="d9efb-158">Cómo usar WebListener fuera de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9efb-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="d9efb-159">Instale el paquete NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="d9efb-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="d9efb-160">[Registre previamente los prefijos de URL para enlazarlos a WebListener y configure los certificados SSL](#preregister-url-prefixes-and-configure-ssl) como lo haría para usarlos en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9efb-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="d9efb-161">También hay [valores de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="d9efb-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="d9efb-162">Este es un ejemplo de código que muestra el uso de WebListener fuera de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d9efb-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="d9efb-163">Registrar previamente prefijos de URL y configurar SSL</span><span class="sxs-lookup"><span data-stu-id="d9efb-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="d9efb-164">Tanto IIS como WebListener se basan en el controlador de modo kernel de Http.Sys subyacente para escuchar las solicitudes y realizar el procesamiento inicial.</span><span class="sxs-lookup"><span data-stu-id="d9efb-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="d9efb-165">En IIS, la interfaz de usuario de administración ofrece una forma relativamente fácil de configurarlo todo.</span><span class="sxs-lookup"><span data-stu-id="d9efb-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="d9efb-166">Aunque si usa WebListener tendrá que configurar Http.Sys por su cuenta.</span><span class="sxs-lookup"><span data-stu-id="d9efb-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="d9efb-167">La herramienta integrada para hacerlo es netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="d9efb-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="d9efb-168">Las tareas más comunes para las que tiene que usar netsh.exe son reservar prefijos de URL y asignar certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="d9efb-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="d9efb-169">NetSh.exe no es una herramienta fácil de usar para principiantes.</span><span class="sxs-lookup"><span data-stu-id="d9efb-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="d9efb-170">En el ejemplo de abajo se muestra el mínimo necesario para reservar prefijos de URL para los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="d9efb-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="d9efb-171">En este ejemplo se muestra cómo asignar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="d9efb-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="d9efb-172">Aquí está la documentación de referencia oficial:</span><span class="sxs-lookup"><span data-stu-id="d9efb-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="d9efb-173">Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)</span><span class="sxs-lookup"><span data-stu-id="d9efb-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="d9efb-174">Cadenas de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="d9efb-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="d9efb-175">Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="d9efb-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="d9efb-176">Los artículos que hacen referencia a `HttpListener` también son aplicables a `WebListener`, ya que ambos se basan en Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="d9efb-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="d9efb-177">Configuración de un puerto con un certificado SSL</span><span class="sxs-lookup"><span data-stu-id="d9efb-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="d9efb-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicación HTTPS - Certificación de cliente y hospedaje basado en HttpListener): es un blog de terceros bastante antiguo pero aún tiene información útil.</span><span class="sxs-lookup"><span data-stu-id="d9efb-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="d9efb-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedimientos con código no administrado de HttpListener o Http Server (C++) como un servidor simple SSL): también es un blog antiguo con información útil.</span><span class="sxs-lookup"><span data-stu-id="d9efb-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="d9efb-180">¿Cómo configurar WebListener de .NET Core con SSL?</span><span class="sxs-lookup"><span data-stu-id="d9efb-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="d9efb-181">Estas son algunas herramientas de terceros que pueden resultar más sencillas que la línea de comandos netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="d9efb-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="d9efb-182">Dichas herramientas no las proporciona ni las promociona Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d9efb-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="d9efb-183">Las herramientas se ejecutan como administrador de forma predeterminada, debido a que netsh.exe requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d9efb-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="d9efb-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario para mostrar una lista de certificados SSL y configurarlos, además de configurar otras opciones, reservas de prefijo y listas de certificados de confianza.</span><span class="sxs-lookup"><span data-stu-id="d9efb-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="d9efb-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar una lista de certificados SSL y prefijos de URL, y configurarlos.</span><span class="sxs-lookup"><span data-stu-id="d9efb-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="d9efb-186">Su interfaz de usuario está más cuidada que la de http.sys Manager e incluye algunas opciones de configuración adicionales, aunque proporciona funciones similares.</span><span class="sxs-lookup"><span data-stu-id="d9efb-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="d9efb-187">No puede crear una nueva lista de certificados de confianza de (CTL), pero puede asignar las existentes.</span><span class="sxs-lookup"><span data-stu-id="d9efb-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="d9efb-188">Para generar certificados SSL autofirmados, Microsoft proporciona herramientas de línea de comandos: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) y el cmdlet de PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="d9efb-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="d9efb-189">También hay herramientas de interfaz de usuario de otros fabricantes que facilitan la tarea de generar certificados SSL autofirmados:</span><span class="sxs-lookup"><span data-stu-id="d9efb-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="d9efb-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="d9efb-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="d9efb-191">Interfaz de usuario de Makecert</span><span class="sxs-lookup"><span data-stu-id="d9efb-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="d9efb-192">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d9efb-192">Next steps</span></span>

<span data-ttu-id="d9efb-193">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="d9efb-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d9efb-194">Aplicación de ejemplo para este artículo</span><span class="sxs-lookup"><span data-stu-id="d9efb-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="d9efb-195">Código fuente de WebListener</span><span class="sxs-lookup"><span data-stu-id="d9efb-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="d9efb-196">Hospedar aplicaciones de WPF</span><span class="sxs-lookup"><span data-stu-id="d9efb-196">Hosting</span></span>](../hosting.md)
