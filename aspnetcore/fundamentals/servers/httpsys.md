---
title: "Implementación de servidor web de HTTP.sys en ASP.NET Core"
author: rick-anderson
description: "Presenta HTTP.sys, un servidor web para ASP.NET Core en Windows. Basado en el controlador Http.Sys en modo kernel, HTTP.sys es una alternativa a Kestrel que puede usarse para una conexión directa a Internet sin IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 6fc356d8ecc1b699269286f244000b493e48a2c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementación de servidor web de HTTP.sys en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) y [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> En este tema se aplica solo a ASP.NET Core 2.0 y versiones posteriores. En versiones anteriores de ASP.NET Core, se denomina HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys es un [servidor web de ASP.NET Core](index.md) que sólo se ejecuta en Windows. Se basa en el [controlador de modo de núcleo de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys es una alternativa a [Kestrel](kestrel.md) que ofrece algunas características que no Kestel. **HTTP.sys no puede utilizarse con IIS o IIS Express, ya no es compatible con la [módulo principal de ASP.NET](aspnet-core-module.md).**

HTTP.sys es compatible con las siguientes características:

- [Autenticación de Windows](xref:security/authentication/windowsauth)
- Uso compartido de puertos
- HTTPS con SNI
- HTTP/2 a través de TLS (Windows 10)
- Transmisión de archivos directa
- Las respuestas en caché
- WebSockets (Windows 8)

Versiones admitidas de Windows:

- Windows 7 y Windows Server 2008 R2 y versiones posteriores

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Cuándo utilizar HTTP.sys

HTTP.sys es útil para las implementaciones donde es necesario exponer el servidor directamente a Internet sin usar IIS.

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

Dado que se basa en Http.Sys, HTTP.sys no requiere un servidor proxy inverso para la protección frente a ataques. Http.Sys es una tecnología consolidada que le protege contra muchos tipos de ataques y proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características. El propio IIS se ejecuta como un agente de escucha HTTP sobre Http.Sys. 

HTTP.sys es una buena elección para implementaciones internas cuando necesite una característica no está disponible en Kestrel, como la autenticación de Windows.

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Cómo usar HTTP.sys

Este es un resumen de tareas de instalación para el sistema operativo del host y la aplicación de ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar Windows Server

* Instale la versión de .NET que requiera la aplicación, como [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).

* Registrar previamente los prefijos de dirección URL para enlazar con HTTP.sys y configurar los certificados de SSL

   Si no registrar previamente los prefijos de dirección URL en Windows, tendrá que ejecutar la aplicación con privilegios de administrador. La única excepción es si enlazará a localhost mediante HTTP (HTTPS no) con un número de puerto mayor que 1024; en ese caso, no se requiere privilegios de administrador.

   Para obtener más información, consulte [cómo registrar previamente prefijos y configurar SSL](#preregister-url-prefixes-and-configure-ssl) más adelante en este artículo.

* Abrir puertos del firewall para permitir el tráfico llegar a HTTP.sys.

   Puede usar *netsh.exe* o [cmdlets de PowerShell](https://technet.microsoft.com/library/jj554906).

También hay [configuración del registro de Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurar la aplicación de ASP.NET Core para usar HTTP.sys

* No hay ninguna instalación de paquete es necesaria si usa el [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. El [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) paquete se incluye en el metapackage.

* Llame a la `UseHttpSys` método de extensión `WebHostBuilder` en su `Main` método especifica cualquier [HTTP.sys opciones](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) que necesite, como se muestra en el ejemplo siguiente:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurar las opciones de HTTP.sys

Estos son algunos de los límites que se pueden configurar y configuración de HTTP.sys.

**Conexiones de cliente máximo**

Se puede establecer el número máximo de conexiones simultáneas de TCP abiertos en toda la aplicación con el siguiente código en *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

El número máximo de conexiones es un número ilimitado de forma predeterminada (null).

**Tamaño del cuerpo de solicitud máximo**

El tamaño predeterminado del cuerpo de solicitud máximo es: 30.000.000 bytes, que es aproximadamente 28,6 MB.

La manera recomendada para invalidar el límite de una aplicación de MVC de ASP.NET Core es usar el [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo en un método de acción:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación, todas las solicitudes:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Puede invalidar la configuración en una solicitud específica en *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación se ha empezado la lectura de la solicitud. Hay un `IsReadOnly` propiedad que indica si la `MaxRequestBodySize` propiedad está en estado de solo lectura, lo que significa es demasiado tarde para configurar el límite.

Para obtener información sobre otras opciones de HTTP.sys, vea [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurar los puertos y las direcciones URL para escuchar en 

De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`. Para configurar los puertos y los prefijos de dirección URL, puede usar el `UseUrls` método de extensión, el `urls` argumento de línea de comandos, la variable de entorno ASPNETCORE_URLS, o la `UrlPrefixes` propiedad [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). El siguiente ejemplo de código usa `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Una ventaja de `UrlPrefixes` es que se obtiene un mensaje de error inmediatamente si se intenta agregar un prefijo con el formato incorrecto. Una ventaja de `UseUrls` (compartida con `urls` y ASPNETCORE_URLS) es que resulta más sencillo alternar entre Kestrel y HTTP.sys.

Si utiliza ambos `UseUrls` (o `urls` o ASPNETCORE_URLS) y `UrlPrefixes`, la configuración de `UrlPrefixes` invalidar las de `UseUrls`. Para más información, vea [Hospedaje](xref:fundamentals/hosting).

HTTP.sys usa el [formatos de cadena HTTP Server API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Asegúrese de especificar las mismas cadenas de prefijo en `UseUrls` o `UrlPrefixes` que registrar previamente en el servidor. 

### <a name="dont-use-iis"></a>No use IIS

Asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.

En Visual Studio, el perfil de inicio predeterminado es de IIS Express. Para ejecutar el proyecto como una aplicación de consola, cambie manualmente el perfil seleccionado, como se muestra en la siguiente captura de pantalla.

![Seleccione el perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Registrar previamente prefijos de dirección URL y la configuración de SSL

IIS y HTTP.sys se basan en el controlador de modo kernel de subyacente Http.Sys para escuchar las solicitudes e inicial del procesamiento. En IIS, la interfaz de usuario de administración ofrece una forma es relativamente fácil de configurar todos los elementos. Sin embargo, debe configurar Http.Sys usted mismo. La herramienta integrada para hacer que del *netsh.exe*. 

Con *netsh.exe* puede reservar prefijos de dirección URL y asignar los certificados SSL. La herramienta requiere privilegios administrativos.

En el ejemplo siguiente se muestra el mínimo necesario para reservar los prefijos de dirección URL para los puertos 80 y 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

En el ejemplo siguiente se muestra cómo asignar un certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Aquí está la documentación de referencia de *netsh.exe*:

* [Comandos Netsh para hipertexto transferir protocolo (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadenas de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Los siguientes recursos proporcionan instrucciones detalladas para varios escenarios. Artículos que hacen referencia a HttpListener se aplican por igual a HTTP.sys, tal y como se basan en Http.Sys.

* [Configuración de un puerto con un certificado SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Comunicación HTTPS - HttpListener en función de hospedaje y la certificación de cliente](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) esto es un blog de terceros y es bastante antiguo pero aún tiene información útil.
* [Cómo: Tutorial utilizando HttpListener o servidor Http de código no administrado (C++) como un servidor Simple SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Esto también es un blog anterior con información útil.

Estas son algunas herramientas de terceros que pueden ser más fáciles de usar que los *netsh.exe* línea de comandos. Estos no son proporcionados por o están aprobados por Microsoft. Las herramientas se ejecutan como administrador de forma predeterminada, ya que *netsh.exe* propio requiere privilegios de administrador.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) proporciona la interfaz de usuario de la lista y configurar certificados SSL y opciones, las reservas de prefijo y listas de confianza de certificados. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite mostrar o configurar certificados SSL y los prefijos de dirección URL. La interfaz de usuario es más refinada que http.sys Manager y expone unas cuantas más opciones de configuración, pero en caso contrario, proporciona una funcionalidad similar. No se puede crear una nueva lista de confianza de certificados (CTL), pero puede asignar los existentes.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Aplicación de ejemplo de este artículo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Código fuente de HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Hospedar aplicaciones de WPF](xref:fundamentals/hosting)
