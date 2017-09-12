---
title: WebListener implementaciones del servidor web de ASP.NET Core
author: rick-anderson
description: "Presenta WebListener, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo de núcleo de Http.Sys, WebListener es una alternativa a Kestrel que puede usarse para una conexión directa a Internet sin IIS."
keywords: "ASP.NET Core, WebListener, HttpListener, prefijos de dirección url SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 93e8b99e7fbac88aabd347c077d923214ba7aebe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="7d640-105">WebListener implementaciones del servidor web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d640-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="7d640-106">Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="7d640-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="7d640-107">En este tema se aplica solo a ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="7d640-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="7d640-108">En el núcleo de ASP.NET 2.0, se denomina WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="7d640-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="7d640-109">WebListener es un [servidor web de ASP.NET Core](index.md) que sólo se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="7d640-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="7d640-110">Se basa en el [controlador de modo de núcleo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d640-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="7d640-111">WebListener es una alternativa a [Kestrel](kestrel.md) que se puede utilizar para una conexión directa a Internet sin tener que depender de IIS como un servidor proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="7d640-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="7d640-112">De hecho, **WebListener no se puede usar con IIS o IIS Express, ya que no es compatible con la [módulo principal de ASP.NET](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="7d640-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="7d640-113">Aunque WebListener se desarrolló para ASP.NET Core, se puede utilizar directamente en cualquier aplicación .NET Core o .NET Framework a través de la [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="7d640-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="7d640-114">WebListener admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="7d640-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="7d640-115">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="7d640-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="7d640-116">Uso compartido de puertos</span><span class="sxs-lookup"><span data-stu-id="7d640-116">Port sharing</span></span>
- <span data-ttu-id="7d640-117">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="7d640-117">HTTPS with SNI</span></span>
- <span data-ttu-id="7d640-118">HTTP/2 a través de TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="7d640-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="7d640-119">Transmisión de archivos directa</span><span class="sxs-lookup"><span data-stu-id="7d640-119">Direct file transmission</span></span>
- <span data-ttu-id="7d640-120">Las respuestas en caché</span><span class="sxs-lookup"><span data-stu-id="7d640-120">Response caching</span></span>
- <span data-ttu-id="7d640-121">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="7d640-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="7d640-122">Versiones admitidas de Windows:</span><span class="sxs-lookup"><span data-stu-id="7d640-122">Supported Windows versions:</span></span>

- <span data-ttu-id="7d640-123">Windows 7 y Windows Server 2008 R2 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="7d640-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

[<span data-ttu-id="7d640-124">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7d640-124">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)

## <a name="when-to-use-weblistener"></a><span data-ttu-id="7d640-125">Cuándo utilizar WebListener</span><span class="sxs-lookup"><span data-stu-id="7d640-125">When to use WebListener</span></span>

<span data-ttu-id="7d640-126">WebListener es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="7d640-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="7d640-128">Dado que se basa en Http.Sys, WebListener no requiere un servidor proxy inverso para la protección frente a ataques.</span><span class="sxs-lookup"><span data-stu-id="7d640-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="7d640-129">Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características.</span><span class="sxs-lookup"><span data-stu-id="7d640-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="7d640-130">El propio IIS se ejecuta como un agente de escucha HTTP sobre Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="7d640-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="7d640-131">WebListener también es una buena elección para implementaciones internas cuando necesite una de las características que ofrece que no se puede obtener mediante el uso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7d640-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="7d640-133">Cómo usar WebListener</span><span class="sxs-lookup"><span data-stu-id="7d640-133">How to use WebListener</span></span>

<span data-ttu-id="7d640-134">Este es un resumen de tareas de instalación para el sistema operativo del host y la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d640-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="7d640-135">Configurar Windows Server</span><span class="sxs-lookup"><span data-stu-id="7d640-135">Configure Windows Server</span></span>

* <span data-ttu-id="7d640-136">Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="7d640-136">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="7d640-137">Registrar previamente los prefijos de dirección URL para enlazar a WebListener y configurar los certificados de SSL</span><span class="sxs-lookup"><span data-stu-id="7d640-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="7d640-138">Si no registrar previamente los prefijos de dirección URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="7d640-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="7d640-139">La única excepción es si enlazará a localhost mediante HTTP (HTTPS no) con un número de puerto mayor que 1024; en ese caso no se requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="7d640-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="7d640-140">Para obtener más información, consulte [cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="7d640-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="7d640-141">Abrir puertos del firewall para permitir el tráfico llegar a WebListener.</span><span class="sxs-lookup"><span data-stu-id="7d640-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="7d640-142">Puede utilizar netsh.exe o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="7d640-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="7d640-143">También hay [configuración del registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="7d640-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="7d640-144">Configurar la aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d640-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="7d640-145">Instale el paquete NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="7d640-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="7d640-146">Esto también instala [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como una dependencia.</span><span class="sxs-lookup"><span data-stu-id="7d640-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="7d640-147">Llame a la [ `UseWebListener` ](https://docs.microsoft.com/aspnet/core/api) método de extensión [WebHostBuilder](https://docs.microsoft.com/aspnet/core/api) en su `Main` método especifica cualquier WebListener [opciones](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) y [ configuración](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) que necesite, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d640-147">Call the [`UseWebListener`](https://docs.microsoft.com/aspnet/core/api) extension method on [WebHostBuilder](https://docs.microsoft.com/aspnet/core/api) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="7d640-148">Configurar los puertos y las direcciones URL para escuchar en</span><span class="sxs-lookup"><span data-stu-id="7d640-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="7d640-149">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7d640-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7d640-150">Para configurar los puertos y los prefijos de dirección URL, puede usar el `UseURLs` método de extensión, el `urls` argumento de línea de comandos o el sistema de configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d640-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="7d640-151">Para obtener más información, consulte [hospedaje](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="7d640-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="7d640-152">Usos de agente de escucha de Web la [formatos de cadena de prefijo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d640-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="7d640-153">No hay ningún requisito de formato de cadena de prefijo que es específica de WebListener.</span><span class="sxs-lookup"><span data-stu-id="7d640-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="7d640-154">Asegúrese de especificar las mismas cadenas de prefijo en `UseUrls` que registrar previamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7d640-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="7d640-155">Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7d640-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="7d640-156">En Visual Studio, el perfil de inicio predeterminado es de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7d640-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="7d640-157">Para ejecutar el proyecto como una aplicación de consola tiene que cambiar manualmente el perfil seleccionado, como se muestra en la siguiente captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="7d640-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Seleccione el perfil de aplicación de consola](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="7d640-159">Cómo usar WebListener fuera de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d640-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="7d640-160">Instalar el [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="7d640-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="7d640-161">[Registrar previamente los prefijos de dirección URL para enlazar a WebListener y configurar los certificados SSL](#preregister-url-prefixes-and-configure-ssl) como lo haría para su uso en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d640-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="7d640-162">También hay [configuración del registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="7d640-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="7d640-163">Este es un ejemplo de código que muestra el uso de WebListener fuera de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7d640-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="7d640-164">Registrar previamente prefijos de dirección URL y la configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="7d640-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="7d640-165">Tanto IIS como WebListener se basan en el controlador de modo kernel de subyacente Http.Sys para escuchar las solicitudes e inicial del procesamiento.</span><span class="sxs-lookup"><span data-stu-id="7d640-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="7d640-166">En IIS, la interfaz de usuario de administración ofrece una forma es relativamente fácil de configurar todos los elementos.</span><span class="sxs-lookup"><span data-stu-id="7d640-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="7d640-167">Sin embargo, si usa WebListener necesite configurar Http.Sys usted mismo.</span><span class="sxs-lookup"><span data-stu-id="7d640-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="7d640-168">La herramienta integrada para hacer es netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="7d640-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="7d640-169">Las tareas más comunes que se debe usar netsh.exe para son reservar prefijos de dirección URL y asignación de certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="7d640-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="7d640-170">NetSh.exe no es una herramienta fácil de usar para principiantes.</span><span class="sxs-lookup"><span data-stu-id="7d640-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="7d640-171">En el ejemplo siguiente se muestra el mínimo necesario para reservar los prefijos de dirección URL para los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="7d640-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="7d640-172">En el ejemplo siguiente se muestra cómo asignar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="7d640-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="7d640-173">Aquí está la documentación de referencia oficial:</span><span class="sxs-lookup"><span data-stu-id="7d640-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="7d640-174">Comandos Netsh para hipertexto transferir protocolo (HTTP)</span><span class="sxs-lookup"><span data-stu-id="7d640-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="7d640-175">Cadenas de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="7d640-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="7d640-176">Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="7d640-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="7d640-177">Artículos que hacen referencia a `HttpListener` se aplican igualmente a `WebListener`, ya que ambas se basan en Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="7d640-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="7d640-178">Cómo: configurar un puerto con un certificado SSL</span><span class="sxs-lookup"><span data-stu-id="7d640-178">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="7d640-179">[Comunicación HTTPS - HttpListener en función de hospedaje y la certificación de cliente](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) esto es un blog de terceros y es bastante antiguo pero aún tiene información útil.</span><span class="sxs-lookup"><span data-stu-id="7d640-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="7d640-180">[Cómo: Tutorial utilizando HttpListener o servidor Http de código no administrado (C++) como un servidor Simple SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Esto también es un blog anterior con información útil.</span><span class="sxs-lookup"><span data-stu-id="7d640-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="7d640-181">¿Cómo configuro un WebListener de núcleo de .NET con SSL?</span><span class="sxs-lookup"><span data-stu-id="7d640-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="7d640-182">A continuación, presentamos algunas herramientas de terceros que pueden ser más fáciles de usar que la línea de comandos de netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="7d640-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="7d640-183">Estos no son proporcionados por o están aprobados por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7d640-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="7d640-184">Las herramientas de ejecutarán como administrador de forma predeterminada, debido a que netsh.exe propio requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="7d640-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="7d640-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario de la lista y configurar certificados SSL y opciones, las reservas de prefijo y listas de confianza de certificados.</span><span class="sxs-lookup"><span data-stu-id="7d640-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="7d640-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar o configurar certificados SSL y los prefijos de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7d640-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="7d640-187">La interfaz de usuario es más refinada que http.sys Manager y expone unas cuantas más opciones de configuración, pero en caso contrario, proporciona una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="7d640-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="7d640-188">No se puede crear una nueva lista de confianza de certificados (CTL), pero puede asignar los existentes.</span><span class="sxs-lookup"><span data-stu-id="7d640-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="7d640-189">Para generar certificados SSL autofirmados, Microsoft proporciona herramientas de línea de comandos: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) y el cmdlet de PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="7d640-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="7d640-190">También hay herramientas de interfaz de usuario de otros fabricantes que resulte más fácil para generar certificados SSL autofirmados:</span><span class="sxs-lookup"><span data-stu-id="7d640-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="7d640-191">SelfCert</span><span class="sxs-lookup"><span data-stu-id="7d640-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="7d640-192">Interfaz de usuario de Makecert</span><span class="sxs-lookup"><span data-stu-id="7d640-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="7d640-193">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7d640-193">Next steps</span></span>

<span data-ttu-id="7d640-194">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7d640-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="7d640-195">Aplicación de ejemplo de este artículo</span><span class="sxs-lookup"><span data-stu-id="7d640-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="7d640-196">Código fuente de WebListener</span><span class="sxs-lookup"><span data-stu-id="7d640-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="7d640-197">Hospedar aplicaciones de WPF</span><span class="sxs-lookup"><span data-stu-id="7d640-197">Hosting</span></span>](../hosting.md)
