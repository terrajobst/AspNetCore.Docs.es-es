---
title: Implementación del servidor web WebListener en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre WebListener, un servidor web para ASP.NET Core en Windows que se puede usar para conectarse directamente a Internet sin IIS.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5d72672cc48243f8ee17df615e3379143ed868f6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206447"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a98e0-103">Implementación del servidor web WebListener en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a98e0-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a98e0-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="a98e0-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="a98e0-105">Este tema se aplica solo a ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a98e0-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="a98e0-106">En ASP.NET Core 2.0, WebListener se denomina [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="a98e0-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="a98e0-107">WebListener es un [servidor web para ASP.NET Core](index.md) que se ejecuta solo en Windows.</span><span class="sxs-lookup"><span data-stu-id="a98e0-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="a98e0-108">Está basado en el [controlador de modo kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="a98e0-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a98e0-109">WebListener es una alternativa a [Kestrel](kestrel.md) para conectarse directamente a Internet sin depender de IIS como un servidor proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="a98e0-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="a98e0-110">De hecho, **WebListener no se puede usar con IIS o IIS Express, ya que estos no son compatibles con el [módulo ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="a98e0-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="a98e0-111">Aunque WebListener se desarrolló para ASP.NET Core, se puede usar directamente en cualquier aplicación .NET Core o .NET Framework a través del paquete NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="a98e0-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="a98e0-112">WebListener admite estas características:</span><span class="sxs-lookup"><span data-stu-id="a98e0-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="a98e0-113">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="a98e0-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="a98e0-114">Uso compartido de puertos</span><span class="sxs-lookup"><span data-stu-id="a98e0-114">Port sharing</span></span>
- <span data-ttu-id="a98e0-115">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="a98e0-115">HTTPS with SNI</span></span>
- <span data-ttu-id="a98e0-116">HTTP/2 a través de TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="a98e0-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="a98e0-117">Transmisión directa de archivos</span><span class="sxs-lookup"><span data-stu-id="a98e0-117">Direct file transmission</span></span>
- <span data-ttu-id="a98e0-118">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="a98e0-118">Response caching</span></span>
- <span data-ttu-id="a98e0-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="a98e0-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="a98e0-120">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="a98e0-120">Supported Windows versions:</span></span>

- <span data-ttu-id="a98e0-121">Windows 7 y Windows Server 2008 R2 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="a98e0-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="a98e0-122">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a98e0-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="a98e0-123">Cuándo usar WebListener</span><span class="sxs-lookup"><span data-stu-id="a98e0-123">When to use WebListener</span></span>

<span data-ttu-id="a98e0-124">WebListener es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="a98e0-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="a98e0-126">Al estar basado en Http.Sys, WebListener no necesita un servidor proxy inverso para protegerse de ataques.</span><span class="sxs-lookup"><span data-stu-id="a98e0-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="a98e0-127">Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características.</span><span class="sxs-lookup"><span data-stu-id="a98e0-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="a98e0-128">El propio IIS se ejecuta como una escucha HTTP sobre Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="a98e0-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span>

<span data-ttu-id="a98e0-129">WebListener también es una buena elección para implementaciones internas cuando se necesita alguna de las características que ofrece y estas no se pueden obtener usando Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a98e0-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="a98e0-131">Autenticación de modo kernel con Kerberos</span><span class="sxs-lookup"><span data-stu-id="a98e0-131">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="a98e0-132">WebListener delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="a98e0-132">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="a98e0-133">La autenticación de modo usuario no se admite con Kerberos y WebListener.</span><span class="sxs-lookup"><span data-stu-id="a98e0-133">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="a98e0-134">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="a98e0-134">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="a98e0-135">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a98e0-135">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-weblistener"></a><span data-ttu-id="a98e0-136">Cómo usar WebListener</span><span class="sxs-lookup"><span data-stu-id="a98e0-136">How to use WebListener</span></span>

<span data-ttu-id="a98e0-137">Este es un resumen de las tareas de instalación para el sistema operativo del host y la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a98e0-137">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="a98e0-138">Configurar Windows Server</span><span class="sxs-lookup"><span data-stu-id="a98e0-138">Configure Windows Server</span></span>

* <span data-ttu-id="a98e0-139">Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="a98e0-139">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="a98e0-140">Registre previamente los prefijos de URL para enlazarlos a WebListener y configure los certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="a98e0-140">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="a98e0-141">Si no registra previamente los prefijos de URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="a98e0-141">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="a98e0-142">La única excepción es si enlaza a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024; en ese caso, no se necesitan privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="a98e0-142">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="a98e0-143">Para más información, vea [Cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="a98e0-143">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="a98e0-144">Abra puertos del firewall para permitir que el tráfico llegue a WebListener.</span><span class="sxs-lookup"><span data-stu-id="a98e0-144">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="a98e0-145">Puede usar netsh.exe o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="a98e0-145">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="a98e0-146">También hay [valores de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="a98e0-146">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="a98e0-147">Configurar la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a98e0-147">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="a98e0-148">Instale el paquete NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="a98e0-148">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="a98e0-149">También se instalará [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como una dependencia.</span><span class="sxs-lookup"><span data-stu-id="a98e0-149">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="a98e0-150">Llame al método de extensión `UseWebListener` en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) en el método `Main` y especifique cualquier [opción](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) y [configuración](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) de WebListener que necesite, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a98e0-150">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="a98e0-151">Configurar puertos y direcciones URL de escucha</span><span class="sxs-lookup"><span data-stu-id="a98e0-151">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="a98e0-152">ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a98e0-152">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a98e0-153">Para configurar los puertos y los prefijos de URL, puede usar el método de extensión `UseURLs`, el argumento de línea de comandos `urls` o el sistema de configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a98e0-153">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="a98e0-154">Para obtener más información, consulte Hospedaje en ASP.NET Core (xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="a98e0-154">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="a98e0-155">WebListener usa los [formatos de cadena de prefijo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="a98e0-155">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="a98e0-156">No hay ningún requisito de formato de cadena de prefijo que sea específico de WebListener.</span><span class="sxs-lookup"><span data-stu-id="a98e0-156">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a98e0-157">Los enlaces de carácter comodín de nivel superior (`http://*:80/` y `http://+:80`) **no** se deben usar.</span><span class="sxs-lookup"><span data-stu-id="a98e0-157">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="a98e0-158">Los enlaces de carácter comodín de nivel superior pueden exponer su aplicación a vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="a98e0-158">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="a98e0-159">Esto se aplica tanto a los caracteres comodín fuertes como a los débiles.</span><span class="sxs-lookup"><span data-stu-id="a98e0-159">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="a98e0-160">Use nombres de host explícitos en lugar de caracteres comodín.</span><span class="sxs-lookup"><span data-stu-id="a98e0-160">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="a98e0-161">Los enlaces de carácter comodín de subdominio (por ejemplo, `*.mysub.com`) no suponen este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable).</span><span class="sxs-lookup"><span data-stu-id="a98e0-161">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="a98e0-162">Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a98e0-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="a98e0-163">Asegúrese de especificar en `UseUrls` las mismas cadenas de prefijo que registró previamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a98e0-163">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="a98e0-164">Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a98e0-164">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="a98e0-165">En Visual Studio, el perfil de inicio predeterminado es para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a98e0-165">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="a98e0-166">Para ejecutar el proyecto como una aplicación de consola, tiene que cambiar manualmente el perfil seleccionado, como se muestra en esta captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="a98e0-166">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Seleccionar perfil de aplicación de consola](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="a98e0-168">Cómo usar WebListener fuera de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a98e0-168">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="a98e0-169">Instale el paquete NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="a98e0-169">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="a98e0-170">[Registre previamente los prefijos de URL para enlazarlos a WebListener y configure los certificados SSL](#preregister-url-prefixes-and-configure-ssl) como lo haría para usarlos en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a98e0-170">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="a98e0-171">También hay [valores de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="a98e0-171">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="a98e0-172">Este es un ejemplo de código que muestra el uso de WebListener fuera de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a98e0-172">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="a98e0-173">Registrar previamente prefijos de URL y configurar SSL</span><span class="sxs-lookup"><span data-stu-id="a98e0-173">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="a98e0-174">Tanto IIS como WebListener se basan en el controlador de modo kernel de Http.Sys subyacente para escuchar las solicitudes y realizar el procesamiento inicial.</span><span class="sxs-lookup"><span data-stu-id="a98e0-174">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="a98e0-175">En IIS, la interfaz de usuario de administración ofrece una forma relativamente fácil de configurarlo todo.</span><span class="sxs-lookup"><span data-stu-id="a98e0-175">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="a98e0-176">Aunque si usa WebListener tendrá que configurar Http.Sys por su cuenta.</span><span class="sxs-lookup"><span data-stu-id="a98e0-176">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="a98e0-177">La herramienta integrada para hacerlo es netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="a98e0-177">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="a98e0-178">Las tareas más comunes para las que tiene que usar netsh.exe son reservar prefijos de URL y asignar certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="a98e0-178">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="a98e0-179">NetSh.exe no es una herramienta fácil de usar para principiantes.</span><span class="sxs-lookup"><span data-stu-id="a98e0-179">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="a98e0-180">En el ejemplo de abajo se muestra el mínimo necesario para reservar prefijos de URL para los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="a98e0-180">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="a98e0-181">En este ejemplo se muestra cómo asignar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="a98e0-181">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="a98e0-182">Aquí está la documentación de referencia oficial:</span><span class="sxs-lookup"><span data-stu-id="a98e0-182">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="a98e0-183">Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)</span><span class="sxs-lookup"><span data-stu-id="a98e0-183">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="a98e0-184">Cadenas de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="a98e0-184">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="a98e0-185">Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="a98e0-185">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="a98e0-186">Los artículos que hacen referencia a `HttpListener` también son aplicables a `WebListener`, ya que ambos se basan en Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="a98e0-186">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="a98e0-187">Configuración de un puerto con un certificado SSL</span><span class="sxs-lookup"><span data-stu-id="a98e0-187">How to: Configure a Port with an SSL Certificate</span></span>](/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="a98e0-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicación HTTPS: Certificación de cliente y hospedaje basado en HttpListener): blog de terceros bastante antiguo, pero que aún tiene información útil.</span><span class="sxs-lookup"><span data-stu-id="a98e0-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="a98e0-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedimientos con código no administrado de HttpListener o Http Server (C++) como un servidor simple SSL): también es un blog antiguo con información útil.</span><span class="sxs-lookup"><span data-stu-id="a98e0-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="a98e0-190">¿Cómo configurar WebListener de .NET Core con SSL?</span><span class="sxs-lookup"><span data-stu-id="a98e0-190">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="a98e0-191">Estas son algunas herramientas de terceros que pueden resultar más sencillas que la línea de comandos netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="a98e0-191">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="a98e0-192">Dichas herramientas no las proporciona ni las promociona Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a98e0-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="a98e0-193">Las herramientas se ejecutan como administrador de forma predeterminada, debido a que netsh.exe requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="a98e0-193">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="a98e0-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario para mostrar una lista de certificados SSL y configurarlos, además de configurar otras opciones, reservas de prefijo y listas de certificados de confianza.</span><span class="sxs-lookup"><span data-stu-id="a98e0-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="a98e0-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar una lista de certificados SSL y prefijos de dirección URL, así como configurarlos.</span><span class="sxs-lookup"><span data-stu-id="a98e0-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="a98e0-196">Su interfaz de usuario está más cuidada que la de http.sys Manager e incluye algunas opciones de configuración adicionales, aunque proporciona funciones similares.</span><span class="sxs-lookup"><span data-stu-id="a98e0-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="a98e0-197">No puede crear una nueva lista de certificados de confianza de (CTL), pero puede asignar las existentes.</span><span class="sxs-lookup"><span data-stu-id="a98e0-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="a98e0-198">Para generar certificados SSL autofirmados, Microsoft proporciona herramientas de línea de comandos: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) y el cmdlet de PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="a98e0-198">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="a98e0-199">También hay herramientas de interfaz de usuario de otros fabricantes que facilitan la tarea de generar certificados SSL autofirmados:</span><span class="sxs-lookup"><span data-stu-id="a98e0-199">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="a98e0-200">SelfCert</span><span class="sxs-lookup"><span data-stu-id="a98e0-200">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="a98e0-201">Interfaz de usuario de Makecert</span><span class="sxs-lookup"><span data-stu-id="a98e0-201">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="a98e0-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a98e0-202">Next steps</span></span>

<span data-ttu-id="a98e0-203">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="a98e0-203">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a98e0-204">Aplicación de ejemplo para este artículo</span><span class="sxs-lookup"><span data-stu-id="a98e0-204">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="a98e0-205">Código fuente de WebListener</span><span class="sxs-lookup"><span data-stu-id="a98e0-205">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="a98e0-206">Hospedar aplicaciones de WPF</span><span class="sxs-lookup"><span data-stu-id="a98e0-206">Hosting</span></span>](xref:fundamentals/host/index)
