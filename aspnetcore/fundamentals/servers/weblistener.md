---
title: WebListener implementaciones del servidor web de ASP.NET Core
author: rick-anderson
description: "Presenta WebListener, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo de núcleo de Http.Sys, WebListener es una alternativa a Kestrel que puede usarse para una conexión directa a Internet sin IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1bdbc723e4602c2e53723aff91ec5d254f4bd93
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>WebListener implementaciones del servidor web de ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> En este tema se aplica solo a ASP.NET Core 1.x. En el núcleo de ASP.NET 2.0, se denomina WebListener [HTTP.sys](httpsys.md).

WebListener es un [servidor web de ASP.NET Core](index.md) que sólo se ejecuta en Windows. Se basa en el [controlador de modo de núcleo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener es una alternativa a [Kestrel](kestrel.md) que se puede utilizar para una conexión directa a Internet sin tener que depender de IIS como un servidor proxy inverso. De hecho, **WebListener no se puede usar con IIS o IIS Express, ya que no es compatible con la [módulo principal de ASP.NET](aspnet-core-module.md).**

Aunque WebListener se desarrolló para ASP.NET Core, se puede utilizar directamente en cualquier aplicación .NET Core o .NET Framework a través de la [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) paquete NuGet.

WebListener admite las siguientes características:

- [Autenticación de Windows](xref:security/authentication/windowsauth)
- Uso compartido de puertos
- HTTPS con SNI
- HTTP/2 a través de TLS (Windows 10)
- Transmisión de archivos directa
- Las respuestas en caché
- WebSockets (Windows 8)

Versiones admitidas de Windows:

- Windows 7 y Windows Server 2008 R2 y versiones posteriores

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Cuándo utilizar WebListener

WebListener es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

Dado que se basa en Http.Sys, WebListener no requiere un servidor proxy inverso para la protección frente a ataques. Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características. El propio IIS se ejecuta como un agente de escucha HTTP sobre Http.Sys. 

WebListener también es una buena elección para implementaciones internas cuando necesite una de las características que ofrece que no se puede obtener mediante el uso de Kestrel.

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Cómo usar WebListener

Este es un resumen de tareas de instalación para el sistema operativo del host y la aplicación de ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar Windows Server

* Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.

* Registrar previamente los prefijos de dirección URL para enlazar a WebListener y configurar los certificados de SSL

   Si no registrar previamente los prefijos de dirección URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador. La única excepción es si enlazará a localhost mediante HTTP (HTTPS no) con un número de puerto mayor que 1024; en ese caso no se requiere privilegios de administrador.

   Para obtener más información, consulte [cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.

* Abrir puertos del firewall para permitir el tráfico llegar a WebListener.

   Puede utilizar netsh.exe o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).

También hay [configuración del registro de Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurar la aplicación de ASP.NET Core

* Instale el paquete NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Esto también instala [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como una dependencia.

* Llame a la `UseWebListener` método de extensión [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) en su `Main` método especifica cualquier WebListener [opciones](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) y [configuración](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) que necesita , como se muestra en el ejemplo siguiente:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurar los puertos y las direcciones URL para escuchar en 

  De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`. Para configurar los puertos y los prefijos de dirección URL, puede usar el `UseURLs` método de extensión, el `urls` argumento de línea de comandos o el sistema de configuración de ASP.NET Core. Para más información, vea [Hospedaje](../../fundamentals/hosting.md).

  Usos de agente de escucha de Web la [formatos de cadena de prefijo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). No hay ningún requisito de formato de cadena de prefijo que es específica de WebListener.

  > [!NOTE]
  > Asegúrese de especificar las mismas cadenas de prefijo en `UseUrls` que registrar previamente en el servidor. 

* Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.

  En Visual Studio, el perfil de inicio predeterminado es de IIS Express.  Para ejecutar el proyecto como una aplicación de consola tiene que cambiar manualmente el perfil seleccionado, como se muestra en la siguiente captura de pantalla.

  ![Seleccione el perfil de aplicación de consola](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Cómo usar WebListener fuera de ASP.NET Core

* Instalar el [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) paquete NuGet.

* [Registrar previamente los prefijos de dirección URL para enlazar a WebListener y configurar los certificados SSL](#preregister-url-prefixes-and-configure-ssl) como lo haría para su uso en ASP.NET Core.

También hay [configuración del registro de Http.Sys](https://support.microsoft.com/kb/820129).


Este es un ejemplo de código que muestra el uso de WebListener fuera de ASP.NET Core:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Registrar previamente prefijos de dirección URL y la configuración de SSL

Tanto IIS como WebListener se basan en el controlador de modo kernel de subyacente Http.Sys para escuchar las solicitudes e inicial del procesamiento. En IIS, la interfaz de usuario de administración ofrece una forma es relativamente fácil de configurar todos los elementos. Sin embargo, si usa WebListener necesite configurar Http.Sys usted mismo. La herramienta integrada para hacer es netsh.exe. 

Las tareas más comunes que se debe usar netsh.exe para son reservar prefijos de dirección URL y asignación de certificados SSL.

NetSh.exe no es una herramienta fácil de usar para principiantes. En el ejemplo siguiente se muestra el mínimo necesario para reservar los prefijos de dirección URL para los puertos 80 y 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

En el ejemplo siguiente se muestra cómo asignar un certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Aquí está la documentación de referencia oficial:

* [Comandos Netsh para hipertexto transferir protocolo (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadenas de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios. Artículos que hacen referencia a `HttpListener` se aplican igualmente a `WebListener`, ya que ambas se basan en Http.Sys.

* [Configuración de un puerto con un certificado SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Comunicación HTTPS - HttpListener en función de hospedaje y la certificación de cliente](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) esto es un blog de terceros y es bastante antiguo pero aún tiene información útil.
* [Cómo: Tutorial utilizando HttpListener o servidor Http de código no administrado (C++) como un servidor Simple SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Esto también es un blog anterior con información útil.
* [¿Cómo configuro un WebListener de núcleo de .NET con SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

A continuación, presentamos algunas herramientas de terceros que pueden ser más fáciles de usar que la línea de comandos de netsh.exe. Estos no son proporcionados por o están aprobados por Microsoft. Las herramientas de ejecutarán como administrador de forma predeterminada, debido a que netsh.exe propio requiere privilegios de administrador.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario de la lista y configurar certificados SSL y opciones, las reservas de prefijo y listas de confianza de certificados. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar o configurar certificados SSL y los prefijos de dirección URL. La interfaz de usuario es más refinada que http.sys Manager y expone unas cuantas más opciones de configuración, pero en caso contrario, proporciona una funcionalidad similar. No se puede crear una nueva lista de confianza de certificados (CTL), pero puede asignar los existentes.

Para generar certificados SSL autofirmados, Microsoft proporciona herramientas de línea de comandos: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) y el cmdlet de PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). También hay herramientas de interfaz de usuario de otros fabricantes que resulte más fácil para generar certificados SSL autofirmados:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interfaz de usuario de Makecert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Aplicación de ejemplo de este artículo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Código fuente de WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hospedar aplicaciones de WPF](../hosting.md)
