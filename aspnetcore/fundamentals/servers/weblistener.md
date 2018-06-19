---
title: Implementación del servidor web WebListener en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre WebListener, un servidor web para ASP.NET Core en Windows que se puede usar para conectarse directamente a Internet sin IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 46871edb744ad152df8eb958b344068b7408dd1e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
ms.locfileid: "34248458"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementación del servidor web WebListener en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Este tema se aplica solo a ASP.NET Core 1.x. En ASP.NET Core 2.0, WebListener se denomina [HTTP.sys](httpsys.md).

WebListener es un [servidor web para ASP.NET Core](index.md) que se ejecuta solo en Windows. Está basado en el [controlador de modo kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener es una alternativa a [Kestrel](kestrel.md) para conectarse directamente a Internet sin depender de IIS como un servidor proxy inverso. De hecho, **WebListener no se puede usar con IIS o IIS Express, ya que estos no son compatibles con el [módulo ASP.NET Core](aspnet-core-module.md).**

Aunque WebListener se desarrolló para ASP.NET Core, se puede usar directamente en cualquier aplicación .NET Core o .NET Framework a través del paquete NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

WebListener admite estas características:

- [Autenticación de Windows](xref:security/authentication/windowsauth)
- Uso compartido de puertos
- HTTPS con SNI
- HTTP/2 a través de TLS (Windows 10)
- Transmisión directa de archivos
- Almacenamiento en caché de respuestas
- WebSockets (Windows 8)

Versiones de Windows compatibles:

- Windows 7 y Windows Server 2008 R2 y versiones posteriores

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Cuándo usar WebListener

WebListener es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

Al estar basado en Http.Sys, WebListener no necesita un servidor proxy inverso para protegerse de ataques. Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características. El propio IIS se ejecuta como una escucha HTTP sobre Http.Sys. 

WebListener también es una buena elección para implementaciones internas cuando se necesita alguna de las características que ofrece y estas no se pueden obtener usando Kestrel.

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Cómo usar WebListener

Este es un resumen de las tareas de instalación para el sistema operativo del host y la aplicación ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar Windows Server

* Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.

* Registre previamente los prefijos de URL para enlazarlos a WebListener y configure los certificados SSL.

   Si no registra previamente los prefijos de URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador. La única excepción es si enlaza a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024; en ese caso, no se necesitan privilegios de administrador.

   Para más información, vea [Cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.

* Abra puertos del firewall para permitir que el tráfico llegue a WebListener.

   Puede usar netsh.exe o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).

También hay [valores de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurar la aplicación ASP.NET Core

* Instale el paquete NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). También se instalará [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como una dependencia.

* Llame al método de extensión `UseWebListener` en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) en el método `Main` y especifique cualquier [opción](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) y [configuración](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) de WebListener que necesite, como se muestra en este ejemplo:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurar puertos y direcciones URL de escucha 

  ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada. Para configurar los puertos y los prefijos de URL, puede usar el método de extensión `UseURLs`, el argumento de línea de comandos `urls` o el sistema de configuración de ASP.NET Core. Para obtener más información, consulte Hospedaje en ASP.NET Core (xref:fundamentals/host/index).

  WebListener usa los [formatos de cadena de prefijo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). No hay ningún requisito de formato de cadena de prefijo que sea específico de WebListener.

  > [!WARNING]
  > Los enlaces de carácter comodín de nivel superior (`http://*:80/` y `http://+:80`) **no** se deben usar. Los enlaces de carácter comodín de nivel superior pueden exponer su aplicación a vulnerabilidades de seguridad. Esto se aplica tanto a los caracteres comodín fuertes como a los débiles. Use nombres de host explícitos en lugar de caracteres comodín. Los enlaces de carácter comodín de subdominio (por ejemplo, `*.mysub.com`) no suponen este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable). Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.

  > [!NOTE]
  > Asegúrese de especificar en `UseUrls` las mismas cadenas de prefijo que registró previamente en el servidor. 

* Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.

  En Visual Studio, el perfil de inicio predeterminado es para IIS Express.  Para ejecutar el proyecto como una aplicación de consola, tiene que cambiar manualmente el perfil seleccionado, como se muestra en esta captura de pantalla.

  ![Seleccionar perfil de aplicación de consola](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Cómo usar WebListener fuera de ASP.NET Core

* Instale el paquete NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

* [Registre previamente los prefijos de URL para enlazarlos a WebListener y configure los certificados SSL](#preregister-url-prefixes-and-configure-ssl) como lo haría para usarlos en ASP.NET Core.

También hay [valores de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).


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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Registrar previamente prefijos de URL y configurar SSL

Tanto IIS como WebListener se basan en el controlador de modo kernel de Http.Sys subyacente para escuchar las solicitudes y realizar el procesamiento inicial. En IIS, la interfaz de usuario de administración ofrece una forma relativamente fácil de configurarlo todo. Aunque si usa WebListener tendrá que configurar Http.Sys por su cuenta. La herramienta integrada para hacerlo es netsh.exe. 

Las tareas más comunes para las que tiene que usar netsh.exe son reservar prefijos de URL y asignar certificados SSL.

NetSh.exe no es una herramienta fácil de usar para principiantes. En el ejemplo de abajo se muestra el mínimo necesario para reservar prefijos de URL para los puertos 80 y 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

En este ejemplo se muestra cómo asignar un certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Aquí está la documentación de referencia oficial:

* [Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadenas de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios. Los artículos que hacen referencia a `HttpListener` también son aplicables a `WebListener`, ya que ambos se basan en Http.Sys.

* [Configuración de un puerto con un certificado SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicación HTTPS: Certificación de cliente y hospedaje basado en HttpListener): blog de terceros bastante antiguo, pero que aún tiene información útil.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedimientos con código no administrado de HttpListener o Http Server (C++) como un servidor simple SSL): también es un blog antiguo con información útil.
* [¿Cómo configurar WebListener de .NET Core con SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Estas son algunas herramientas de terceros que pueden resultar más sencillas que la línea de comandos netsh.exe. Dichas herramientas no las proporciona ni las promociona Microsoft. Las herramientas se ejecutan como administrador de forma predeterminada, debido a que netsh.exe requiere privilegios de administrador.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario para mostrar una lista de certificados SSL y configurarlos, además de configurar otras opciones, reservas de prefijo y listas de certificados de confianza. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar una lista de certificados SSL y prefijos de dirección URL, así como configurarlos. Su interfaz de usuario está más cuidada que la de http.sys Manager e incluye algunas opciones de configuración adicionales, aunque proporciona funciones similares. No puede crear una nueva lista de certificados de confianza de (CTL), pero puede asignar las existentes.

Para generar certificados SSL autofirmados, Microsoft proporciona herramientas de línea de comandos: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) y el cmdlet de PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). También hay herramientas de interfaz de usuario de otros fabricantes que facilitan la tarea de generar certificados SSL autofirmados:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interfaz de usuario de Makecert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Aplicación de ejemplo para este artículo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Código fuente de WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hospedar aplicaciones de WPF](xref:fundamentals/host/index)
