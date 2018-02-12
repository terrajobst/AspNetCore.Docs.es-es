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
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementación del servidor web HTTP.sys en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Este tema es válido únicamente para ASP.NET Core 2.0 y versiones posteriores. En versiones anteriores de ASP.NET Core, HTTP.sys se denominaba [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys es un [servidor web de ASP.NET Core](index.md) que solo se ejecuta en Windows. Está basado en el [controlador de modo kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys supone una alternativa a [Kestrel](kestrel.md) que ofrece algunas características que este último no facilita. **De hecho, HTTP.sys no se puede usar con IIS o IIS Express, ya que no es compatible con el [módulo ASP.NET Core](aspnet-core-module.md).**

HTTP.sys admite las siguientes características:

- [Autenticación de Windows](xref:security/authentication/windowsauth)
- Uso compartido de puertos
- HTTPS con SNI
- HTTP/2 a través de TLS (Windows 10)
- Transmisión directa de archivos
- Almacenamiento en caché de respuestas
- WebSockets (Windows 8)

Versiones de Windows compatibles:

- Windows 7 y Windows Server 2008 R2 y posteriores

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Cuándo usar HTTP.sys

HTTP.sys es útil en implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

Al estar basado en Http.Sys, HTTP.sys no necesita un servidor proxy inverso para protegerse de ataques. Http.Sys es una tecnología consolidada que protege contra muchos tipos de ataques y que proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características. El propio IIS se ejecuta como una escucha HTTP sobre Http.Sys. 

HTTP.sys constituye una buena opción en las implementaciones internas cuando se necesita una característica que no está disponible en Kestrel, como la autenticación de Windows.

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Cómo usar HTTP.sys

Este es un resumen de las tareas de configuración del sistema operativo del host y la aplicación ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar Windows Server

* Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).

* Registre previamente los prefijos de URL para enlazarlos a HTTP.sys y configure los certificados SSL.

   Si no registra previamente los prefijos de URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador. La única excepción es si enlaza a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024; en ese caso, no se necesitan privilegios de administrador.

   Para más información, vea [Cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.

* Abra puertos del firewall para permitir que el tráfico llegue a HTTP.sys.

   Puede usar *netsh.exe* o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).

También hay [opciones de configuración del Registro de Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurar la aplicación ASP.NET Core para usar HTTP.sys

* No es necesario instalar ningún paquete si se usa el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage). El paquete [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) está incluido en este metapaquete.

* Llame al método de extensión `UseHttpSys` en `WebHostBuilder` en el método `Main` y especifique cualquier [opción de HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) que necesite, como se muestra en el siguiente ejemplo:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurar las opciones de HTTP.sys

Estas son algunas de las opciones y limitaciones de HTTP.sys que se pueden configurar.

**Conexiones de cliente máximas**

Se puede establecer el número máximo de conexiones TCP abiertas simultáneas en toda la aplicación con este código en *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

El número máximo de conexiones es ilimitado de forma predeterminada (null).

**Tamaño máximo del cuerpo de la solicitud**

El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.

El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) en un método de acción:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Este es un ejemplo que muestra cómo configurar la restricción en toda la aplicación y todas las solicitudes:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Puede invalidar la configuración en una solicitud específica en *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud. Hay una propiedad `IsReadOnly` que indica si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.

Para más información sobre otras opciones de HTTP.sys, vea [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurar puertos y direcciones URL de escucha 

ASP.NET Core enlaza a `http://localhost:5000` de forma predeterminada. Para configurar los puertos y los prefijos de dirección URL, puede usar el método de extensión `UseUrls`, el argumento de línea de comandos `urls`, la variable de entorno ASPNETCORE_URLS o la propiedad `UrlPrefixes` de [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). En el siguiente ejemplo se usa `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Una ventaja de `UrlPrefixes` es que se recibe un mensaje de error inmediatamente si se intenta agregar un prefijo con el formato incorrecto. Una ventaja de `UseUrls` (compartida con `urls` y ASPNETCORE_URLS) es que resulta más sencillo alternar entre Kestrel y HTTP.sys.

Si usa tanto `UseUrls` (o `urls` o ASPNETCORE_URLS) como `UrlPrefixes`, la configuración de `UrlPrefixes` invalidará la de `UseUrls`. Para más información, vea [Hospedaje](xref:fundamentals/hosting).

HTTP.sys usa los [formatos de cadena UrlPrefix de la API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Asegúrese de especificar en `UseUrls` o en `UrlPrefixes` las mismas cadenas de prefijo que registró previamente en el servidor. 

### <a name="dont-use-iis"></a>No usar IIS

Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.

En Visual Studio, el perfil de inicio predeterminado corresponde a IIS Express. Para ejecutar el proyecto como una aplicación de consola, cambie manualmente el perfil seleccionado, como se muestra en esta captura de pantalla.

![Seleccionar el perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Registrar previamente prefijos de dirección URL y configurar SSL

Tanto IIS como HTTP.sys se basan en el controlador de modo kernel de Http.Sys subyacente para escuchar las solicitudes y realizar el procesamiento inicial. En IIS, la interfaz de usuario de administración ofrece una forma relativamente fácil de configurarlo todo. Pero tendrá que configurar Http.Sys por su cuenta. La herramienta integrada para llevar esto a cabo es *netsh.exe*. 

Con *netsh.exe* puede reservar prefijos de dirección URL y asignar certificados SSL. Esta herramienta requiere privilegios de administrador.

En este ejemplo se muestra lo mínimo que se necesita para reservar los prefijos de dirección URL de los puertos 80 y 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

En el siguiente ejemplo se muestra cómo asignar un certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Documentación de referencia de *netsh.exe*:

* [Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadenas de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios. Los artículos que hacen referencia a HttpListener son válidos también para HTTP.sys, puesto que ambos se basan en Http.Sys.

* [Configuración de un puerto con un certificado SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicación HTTPS: Certificación de cliente y hospedaje basado en HttpListener): blog de terceros bastante antiguo, pero que aún tiene información útil.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedimientos con código no administrado de HttpListener o Http Server (C++) como un servidor simple SSL): también es un blog antiguo con información útil.

Estas son algunas herramientas de terceros que pueden ser más fáciles de usar que la línea de comandos *netsh.exe*. No están proporcionadas ni aprobadas por Microsoft. Las herramientas se ejecutan como administrador de forma predeterminada, dado que *netsh.exe* requiere privilegios de administrador.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario para mostrar una lista de certificados SSL y configurarlos, además de configurar otras opciones, reservas de prefijo y listas de certificados de confianza. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar una lista de certificados SSL y prefijos de dirección URL, así como configurarlos. La interfaz de usuario es más refinada que la de http.sys Manager e incluye algunas opciones de configuración más, aunque proporciona una funcionalidad similar. No se puede crear una nueva lista de certificados de confianza (CTL), pero puede asignar las existentes.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Aplicación de ejemplo de este artículo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Código fuente de HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Hospedar aplicaciones de WPF](xref:fundamentals/hosting)
