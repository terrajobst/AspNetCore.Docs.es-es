---
title: Hospedaje de ASP.NET Core en Windows con IIS
author: guardrex
description: Obtenga información sobre cómo hospedar aplicaciones de ASP.NET Core en Windows Server Internet Information Services (IIS).
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 5408fb04231a61e0c4c7a91eb15196bf754ddfa7
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191378"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hospedaje de ASP.NET Core en Windows con IIS

Por [Luke Latham](https://github.com/guardrex)

[Instalación del conjunto de hospedaje de .NET Core](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Los siguientes sistemas operativos son compatibles:

* Windows 7 o posterior
* Windows Server 2008 R2 o posterior

El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)) no funciona en una configuración de proxy inverso con IIS. Use el [servidor Kestrel](xref:fundamentals/servers/kestrel).

Para obtener información sobre el hospedaje en Azure, vea <xref:host-and-deploy/azure-apps/index>.

## <a name="http2-support"></a>Compatibilidad con HTTP/2

::: moniker range=">= aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación de IIS siguientes:

* En proceso
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Plataforma de destino: .NET Core 2.2 o posterior
  * Conexión con TLS 1.2 o una versión posterior
* Fuera de proceso
  * Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
  * Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso al [servidor de Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.
  * Plataforma de destino: no es aplicable a las implementaciones fuera de proceso, ya que IIS controla completamente la conexión HTTP/2.
  * Conexión con TLS 1.2 o una versión posterior

Para una implementación en proceso cuando se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`. Para una implementación fuera de proceso cuando se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/1.1`.

Para obtener más información sobre los modelos de hospedaje en proceso y fuera de proceso, consulte el tema <xref:fundamentals/servers/aspnet-core-module> y <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con las implementaciones fuera de proceso que cumplen los requisitos básicos siguientes:

* Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior
* Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso al [servidor de Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.
* Plataforma de destino: no es aplicable a las implementaciones fuera de proceso, ya que IIS controla completamente la conexión HTTP/2.
* Conexión con TLS 1.2 o una versión posterior

Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/1.1`.

::: moniker-end

HTTP/2 está habilitado de forma predeterminada. Las conexiones vuelven a HTTP/1.1 si no se establece una conexión HTTP/2. Para más información sobre la configuración HTTP/2 con implementaciones de IIS, vea [HTTP/2 en IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

## <a name="application-configuration"></a>Configuración de aplicación

### <a name="enable-the-iisintegration-components"></a>Habilitación de los componentes de integración con IIS

::: moniker range=">= aspnetcore-2.2"

**Modelo de hospedaje en proceso**

Un archivo *Program.cs* estándar llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> para empezar a configurar un host. `CreateDefaultBuilder` llama al método `UseIIS` para iniciar [CoreCLR](/dotnet/standard/glossary#coreclr) y hospedar la aplicación dentro del proceso de trabajo de IIS (`w3wp.exe`). Las pruebas de rendimiento indican que el hospedaje de una aplicación .NET Core en proceso proporciona un mayor rendimiento de solicitud en comparación con el hospedaje de solicitudes de aplicaciones fuera de proceso y de proxy para [Kestrel](xref:fundamentals/servers/kestrel).

**Modelo de hospedaje fuera de proceso**

Un archivo *Program.cs* estándar llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> para empezar a configurar un host. Para el hospedaje fuera de proceso con IIS, `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y habilita la integración de IIS configurando la ruta de acceso base y el puerto para el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

El módulo ASP.NET Core genera un puerto dinámico que se asigna al proceso back-end. `CreateDefaultBuilder` llama al método <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>. `UseIISIntegration` configura Kestrel para escuchar en el puerto dinámico en la dirección IP de localhost (`127.0.0.1`). Si el puerto dinámico es 1234, Kestrel escucha en `127.0.0.1:1234`. Esta configuración reemplaza a otras configuraciones de dirección URL proporcionadas por:

* `UseUrls`
* [API de escucha de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configuración](xref:fundamentals/configuration/index) (u [opción --urls de la línea de comandos](xref:fundamentals/host/web-host#override-configuration))

No es necesario realizar llamadas a `UseUrls` o a la API `Listen` de Kestrel cuando se usa el módulo. Si se llama a `UseUrls` o `Listen`, Kestrel escucha en los puertos especificados cuando se ejecuta la aplicación sin IIS.

Para obtener más información sobre los modelos de hospedaje en proceso y fuera de proceso, consulte el tema <xref:fundamentals/servers/aspnet-core-module> y <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Un archivo *Program.cs* estándar llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> para empezar a configurar un host. `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y habilita IIS Integration configurando la ruta de acceso base y el puerto para el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

El módulo ASP.NET Core genera un puerto dinámico que se asigna al proceso back-end. `CreateDefaultBuilder` llama al método [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration). `UseIISIntegration` configura Kestrel para escuchar en el puerto dinámico en la dirección IP de localhost (`127.0.0.1`). Si el puerto dinámico es 1234, Kestrel escucha en `127.0.0.1:1234`. Esta configuración reemplaza a otras configuraciones de dirección URL proporcionadas por:

* `UseUrls`
* [API de escucha de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configuración](xref:fundamentals/configuration/index) (u [opción --urls de la línea de comandos](xref:fundamentals/host/web-host#override-configuration))

No es necesario realizar llamadas a `UseUrls` o a la API `Listen` de Kestrel cuando se usa el módulo. Si se llama a `UseUrls` o `Listen`, Kestrel escucha en el puerto especificado cuando se ejecuta la aplicación sin IIS.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un archivo *Program.cs* estándar llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> para empezar a configurar un host. `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y habilita IIS Integration configurando la ruta de acceso base y el puerto para el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

El módulo ASP.NET Core genera un puerto dinámico que se asigna al proceso back-end. `CreateDefaultBuilder` llama al método [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration). `UseIISIntegration` configura Kestrel para escuchar en el puerto dinámico en la dirección IP de localhost (`localhost`). Si el puerto dinámico es 1234, Kestrel escucha en `localhost:1234`. Esta configuración reemplaza a otras configuraciones de dirección URL proporcionadas por:

* `UseUrls`
* [API de escucha de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configuración](xref:fundamentals/configuration/index) (u [opción --urls de la línea de comandos](xref:fundamentals/host/web-host#override-configuration))

No es necesario realizar llamadas a `UseUrls` o a la API `Listen` de Kestrel cuando se usa el módulo. Si se llama a `UseUrls` o `Listen`, Kestrel escucha en el puerto especificado cuando se ejecuta la aplicación sin IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Incluya una dependencia en el paquete [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) de dependencias de la aplicación. Use el middleware de integración con IIS agregando el método de extensión [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) a [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Tanto [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) como [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) son necesarios. La llamada al código `UseIISIntegration` no afecta a la portabilidad del código. Si la aplicación no se ejecuta detrás de IIS (por ejemplo, si se ejecuta directamente en Kestrel), `UseIISIntegration` no funciona.

El módulo ASP.NET Core genera un puerto dinámico que se asigna al proceso back-end. `UseIISIntegration` configura Kestrel para escuchar en el puerto dinámico en la dirección IP de localhost (`localhost`). Si el puerto dinámico es 1234, Kestrel escucha en `localhost:1234`. Esta configuración reemplaza a otras configuraciones de dirección URL proporcionadas por:

* `UseUrls`
* [Configuración](xref:fundamentals/configuration/index) (u [opción --urls de la línea de comandos](xref:fundamentals/host/web-host#override-configuration))

No es necesario realizar una llamada a `UseUrls` cuando se usa el módulo. Si se llama a `UseUrls`, Kestrel escucha en el puerto especificado cuando se ejecuta la aplicación sin IIS.

Si se llama a `UseUrls` en una aplicación de ASP.NET Core 1.0, hágalo **antes** de llamar a `UseIISIntegration` para que no se sobrescriba el puerto configurado por el módulo. Este orden de llamada no es necesario en ASP.NET Core 1.1, ya que la configuración del módulo reemplaza a `UseUrls`.

::: moniker-end

Para obtener más información sobre el hospedaje, consulte [Hospedaje en ASP.NET Core](xref:fundamentals/host/index).

### <a name="iis-options"></a>Opciones de IIS

Para configurar las opciones de IIS, incluya una configuración del servicio para [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) en [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). En el ejemplo siguiente, el reenvío de los certificados de cliente a la aplicación para rellenar `HttpContext.Connection.ClientCertificate` está deshabilitado:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Opción                         | Default | Parámetro |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Si es `true`, el middleware de integración con IIS establece el `HttpContext.User` autenticado mediante [autenticación de Windows](xref:security/authentication/windowsauth). Si es `false`, el middleware solo proporciona una identidad para `HttpContext.User` y responde a los desafíos cuando se le solicita explícitamente mediante el `AuthenticationScheme`. Autenticación de Windows debe estar habilitado en IIS para que `AutomaticAuthentication` funcione. Para más información, consulte el tema [Autenticación de Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Establece el nombre para mostrar que se muestra a los usuarios en las páginas de inicio de sesión. |
| `ForwardClientCertificate`     | `true`  | Si `HttpContext.Connection.ClientCertificate` y el encabezado de solicitud `true` está presente, se rellena `MS-ASPNETCORE-CLIENTCERT`. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

El software intermedio de integración con IIS, que configura el software intermedio de encabezados reenviados, y el módulo de ASP.NET Core están configurados para reenviar el esquema (HTTP/HTTPS) y la dirección IP remota donde se originó la solicitud. Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga adicionales. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>Archivo web.config

El archivo *web.config* configura el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). La creación, transformación y publicación del archivo *web.config* se controla por medio de un destino de MSBuild (`_TransformWebConfig`) cuando el proyecto se publica. Este destino está incluido entre los destinos del SDK web (`Microsoft.NET.Sdk.Web`). El SDK se establece al inicio del archivo del proyecto:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Si el proyecto no incluye un archivo *web.config*, el archivo se crea con los elementos *processPath* y *arguments* correctos para configurar el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y se mueve a la [salida publicada](xref:host-and-deploy/directory-structure).

Si el proyecto incluye un archivo *web.config*, el archivo se transforma con los elementos *processPath* y *arguments* correctos para configurar el módulo ASP.NET Core y se mueve a la salida publicada. La transformación no modifica los valores de configuración de IIS del archivo.

El archivo *web.config* puede proporcionar valores de configuración de IIS adicionales que controlan los módulos activos de IIS. Para información sobre los módulos de IIS que son capaces de procesar las solicitudes con aplicaciones ASP.NET Core, vea el tema [Módulos IIS](xref:host-and-deploy/iis/modules).

Para evitar que el SDK web transforme el archivo *web.config*, use la propiedad **\<IsTransformWebConfigDisabled>** en el archivo del proyecto:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Al deshabilitar el SDK web para la transformación del archivo, el desarrollador debe establecer el elemento *processPath* y los *argumentos* manualmente. Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core).

### <a name="webconfig-file-location"></a>Ubicación del archivo web.config

Para crear el proxy inverso entre IIS y el servidor de Kestrel, el archivo *web.config* debe estar presente en la ruta de acceso raíz del contenido (normalmente la ruta de acceso base de la aplicación) de la aplicación implementada. Se trata de la misma ubicación que la ruta de acceso física del sitio web proporcionada a IIS. El archivo *web.config* debe estar en la raíz de la aplicación para habilitar la publicación de varias aplicaciones mediante Web Deploy.

Los archivos confidenciales están en la ruta de acceso física de la aplicación, como *\<ensamblado>.runtimeconfig.json*, *\<ensamblado>.xml* (comentarios de documentación XML) y *\<ensamblado>.deps.json*. Cuando el archivo *web.config* está presente y el sitio se inicia normalmente, IIS no sirve estos archivos confidenciales si se solicitan. Si el archivo *web.config* no está presente, se le asignó un nombre incorrecto o no se puede configurar el sitio para un inicio normal, IIS puede servir archivos confidenciales públicamente.

**El archivo *web.config* debe estar presente en la implementación en todo momento, se le debe asignar un nombre correcto y debe ser capaz de configurar el sitio para el inicio normal. Nunca quite el archivo *web.config* de una implementación de producción.**

## <a name="iis-configuration"></a>Configuración de IIS

**Sistemas operativos de servidor Windows**

Habilite el rol de servidor **Servidor web (IIS)** y establezca los servicios de rol.

1. Use el asistente **Agregar roles y características** del menú **Administrar** o el vínculo de **Administrador del servidor**. En el paso **Roles de servidor**, active la casilla de **Servidor web (IIS)**.

   ![El rol Servidor web (IIS) se activa en el paso Seleccionar roles de servidor.](index/_static/server-roles-ws2016.png)

1. Después del paso **Características**, el paso **Servicios de rol** se carga para el servidor Web (IIS). Seleccione los servicios de rol IIS que quiera o acepte los servicios de rol predeterminados proporcionados.

   ![Los servicios de rol predeterminados se activan en el paso Seleccionar servicios de rol.](index/_static/role-services-ws2016.png)

   **Autenticación de Windows (opcional)**  
   Para habilitar la autenticación de Windows, expanda los nodos siguientes: **Servidor web** > **Seguridad**. Seleccione la característica **Autenticación de Windows**. Para más información, consulte [Windows Authentication \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) (Autenticación de Windows <windowsAuthentication>) y [Configure Windows authentication](xref:security/authentication/windowsauth) (Configurar la autenticación de Windows).

   **WebSockets (opcional)**  
   WebSockets es compatible con ASP.NET Core 1.1 o posterior. Para habilitar WebSockets, expanda los nodos siguientes: **Servidor web** > **Desarrollo de aplicaciones**. Seleccione la característica **Protocolo WebSocket**. Para más información, vea [WebSockets](xref:fundamentals/websockets).

1. Continúe con el paso **Confirmación** para instalar el rol y los servicios de servidor web. No es necesario reiniciar el servidor ni IIS después de instalar el rol **Servidor web (IIS)**.

**Sistemas operativos de escritorio Windows**

Habilite **Consola de administración de IIS** y **Servicios World Wide Web**.

1. Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).

1. Abra el nodo **Internet Information Services**. Abra el nodo **Herramientas de administración web**.

1. Active la casilla de **Consola de administración de IIS**.

1. Active la casilla de **Servicios World Wide Web**.

1. Acepte las características predeterminadas de **Servicios World Wide Web** o personalice las características de IIS.

   **Autenticación de Windows (opcional)**  
   Para habilitar la autenticación de Windows, expanda los nodos siguientes: **Servicios World Wide Web** > **Seguridad**. Seleccione la característica **Autenticación de Windows**. Para más información, consulte [Windows Authentication \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) (Autenticación de Windows <windowsAuthentication>) y [Configure Windows authentication](xref:security/authentication/windowsauth) (Configurar la autenticación de Windows).

   **WebSockets (opcional)**  
   WebSockets es compatible con ASP.NET Core 1.1 o posterior. Para habilitar WebSockets, expanda los nodos siguientes: **Servicios World Wide Web** > **Características de desarrollo de aplicaciones**. Seleccione la característica **Protocolo WebSocket**. Para más información, vea [WebSockets](xref:fundamentals/websockets).

1. Si la instalación de IIS requiere un reinicio, reinicie el sistema.

![Consola de administración de IIS y Servicios World Wide Web se activan en Características de Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Instalación del conjunto de hospedaje de .NET Core

Instale el *conjunto de hospedaje de .NET Core* en el sistema de hospedaje. El lote instala .NET Core Runtime, .NET Core Library y el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). El módulo crea el proxy inverso entre IIS y el servidor Kestrel. Si el sistema no tiene conexión a Internet, obtenga e instale [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar el conjunto de hospedaje de .NET Core.

> [!IMPORTANT]
> Si el conjunto de hospedaje se instala antes que IIS, se debe reparar la instalación de dicho conjunto. Vuelva a ejecutar el instalador del conjunto de hospedaje después de instalar IIS.

### <a name="direct-download-current-version"></a>Descarga directa (versión actual)

Descargue al instalador mediante el vínculo siguiente:

[Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a>Versiones anteriores del instalador

Para obtener una versión anterior del instalador:

1. Vaya a los [archivos de descarga de .NET](https://www.microsoft.com/net/download/archives).
1. En **.NET Core**, seleccione la versión de .NET Core.
1. En la columna **Run apps - Runtime** (Ejecutar aplicaciones - Runtime), busque la fila de la versión del runtime de .NET Core que quiera instalar.
1. Descargue el instalador mediante el vínculo **Runtime & Hosting Bundle** (Runtime y conjunto de hospedaje).

> [!WARNING]
> Algunos instaladores contienen versiones que han alcanzado el final del ciclo de vida (EOL) y ya no son compatibles con Microsoft. Para obtener más información, consulte la [política de soporte técnico](https://www.microsoft.com/net/download/dotnet-core/2.0).

### <a name="install-the-hosting-bundle"></a>Instalación del conjunto de hospedaje

1. Ejecute el instalador en el servidor. Los modificadores siguientes están disponibles cuando se ejecuta el instalador desde un símbolo del sistema de administrador:

   * `OPT_NO_ANCM=1` &ndash; Omita la instalación del módulo de ASP.NET Core.
   * `OPT_NO_RUNTIME=1` &ndash; Omita la instalación del entorno de ejecución de .NET Core.
   * `OPT_NO_SHAREDFX=1` &ndash; Omita la instalación del marco compartido de ASP.NET (entorno de ejecución de ASP.NET).
   * `OPT_NO_X86=1` &ndash; Omita la instalación de entornos de ejecución x86. Utilice este conmutador cuando sepa que no va a hospedar aplicaciones de 32 bits. Si hay alguna posibilidad que vaya a hospedar aplicaciones de 32 bits y 64 bits en el futuro, no use este modificador e instale ambos entornos de ejecución.
1. Reinicie el sistema o ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema. Al reiniciar IIS, se recoge un cambio en la variable PATH del sistema, que es una variable de entorno, realizado por el programa de instalación.

Si el instalador del conjunto de hospedaje de Windows detecta que IIS requiere un reinicio para completar la instalación, el instalador reinicia IIS. Si el instalador desencadena un reinicio de IIS, se reiniciarán todos los sitios web y los grupos de aplicaciones de IIS.

> [!NOTE]
> Para obtener información sobre la configuración compartida de IIS, vea [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration) (Módulo de ASP.NET Core con configuración compartida de IIS).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Instalación de Web Deploy al publicar con Visual Studio

Al implementar aplicaciones en servidores con [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), instale la versión más reciente de Web Deploy en el servidor. Para instalar Web Deploy, use el [Instalador de plataforma web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) u obtenga un instalador directamente desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). El método preferido es usar WebPI. WebPI ofrece una instalación independiente y una configuración para los proveedores de hospedaje.

## <a name="create-the-iis-site"></a>Creación del sitio de IIS

1. En el sistema de hospedaje, cree una carpeta para que contenga los archivos y las carpetas publicados de la aplicación. En el tema [Estructura de directorios](xref:host-and-deploy/directory-structure) se describe el diseño de implementación de una aplicación.

1. Dentro de la nueva carpeta, cree una carpeta *logs* para hospedar los registros de stdout del módulo ASP.NET Core cuando esté habilitado el registro de stdout. Si la aplicación se ha implementado con una carpeta *logs* en la carga, omita este paso. Para instrucciones sobre cómo habilitar MSBuild para crear la carpeta *logs* automáticamente cuando el proyecto se crea de forma local, consulte el tema [Estructura de directorios](xref:host-and-deploy/directory-structure).

   > [!IMPORTANT]
   > Use solamente el registro de stdout para solucionar errores de inicio de aplicación. Nunca use el registro de stdout para el registro de aplicaciones rutinarias. No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados. El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros. Todas las carpetas de la ruta de acceso a la ubicación del registro deben existir. Para más información sobre el registro de stdout, consulte [Creación y redirección de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection). Para información sobre el registro en una aplicación ASP.NET Core, vea el tema [Registro](xref:fundamentals/logging/index).

1. En **Administrador de IIS**, abra el nodo del servidor en el panel **Conexiones**. Haga clic con el botón derecho en la carpeta **Sitios**. Haga clic en **Agregar sitio web** en el menú contextual.

1. Proporcione el **Nombre del sitio** y establezca la **Ruta de acceso física** a la carpeta de implementación de la aplicación. Proporcione la configuración de **Enlace** y cree el sitio web seleccionando **Aceptar**:

   ![Proporcione el nombre del sitio, la ruta de acceso física y el nombre de host en el paso Agregar sitio web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Los enlaces de carácter comodín de nivel superior (`http://*:80/` y `http://+:80`) **no** se deben usar. Los enlaces de carácter comodín de nivel superior pueden exponer su aplicación a vulnerabilidades de seguridad. Esto se aplica tanto a los caracteres comodín fuertes como a los débiles. Use nombres de host explícitos en lugar de caracteres comodín. Los enlaces de carácter comodín de subdominio (por ejemplo, `*.mysub.com`) no suponen este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable). Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.

1. En el nodo del servidor, seleccione **Grupos de aplicaciones**.

1. Haga clic con el botón derecho en el grupo de aplicaciones del sitio y seleccione **Configuración básica** en el menú contextual.

1. En la ventana **Modificar grupo de aplicaciones**, establezca **Versión de .NET CLR** en **Sin código administrado**:

   ![Establezca Sin código administrado para Versión de .NET CLR.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core se ejecuta en un proceso independiente y administra el runtime. ASP.NET Core no se basa en la carga de CLR de escritorio. El establecimiento de **Versión de .NET CLR** en **Sin código administrado** es opcional.

1. Confirme que la identidad del modelo de proceso tiene los permisos adecuados.

   Si cambia la identidad predeterminada del grupo de aplicaciones (**Modelo de proceso** > **Identidad**) de **ApplicationPoolIdentity** a otra identidad, compruebe que la nueva identidad tenga los permisos necesarios para obtener acceso a la carpeta de la aplicación, la base de datos y otros recursos necesarios. Por ejemplo, el grupo de aplicaciones requiere acceso de lectura y escritura a las carpetas donde la aplicación lee y escribe archivos.

**Configuración de la autenticación de Windows (opcional)**  
Para más información, consulte [Configurar la autenticación de Windows](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Implementación de la aplicación

Implemente la aplicación en la carpeta que ha creado en el sistema de hospedaje. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) es el mecanismo recomendado para la implementación.

### <a name="web-deploy-with-visual-studio"></a>Web Deploy con Visual Studio

Vea el tema [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Perfiles de publicación de Visual Studio para la implementación de aplicaciones de ASP.NET Core) para obtener más información sobre cómo crear un perfil de publicación para su uso con Web Deploy. Si el proveedor de hospedaje proporciona un perfil de publicación o admite la creación de uno, descargue su perfil e impórtelo mediante el cuadro de diálogo **Publicar** de Visual Studio.

![Cuadro de diálogo Publicar](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web Deploy fuera de Visual Studio

También puede usar [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) fuera de Visual Studio desde la línea de comandos. Para más información, vea [Web Deployment Tool (Herramienta de implementación web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativas a Web Deploy

Use cualquiera de los métodos disponibles para mover la aplicación al sistema de hospedaje, como copia manual, Xcopy, Robocopy o PowerShell.

Para obtener más información sobre ASP.NET Core en IIS, vea la sección [Recursos de implementación para administradores de IIS](#deployment-resources-for-iis-administrators).

## <a name="browse-the-website"></a>Examinar el sitio web

![El explorador Microsoft Edge ha cargado la página de inicio de IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Archivos de implementación bloqueados

Los archivos de la carpeta de implementación se bloquean cuando se ejecuta la aplicación. Los archivos bloqueados no se pueden sobrescribir durante la implementación. Para liberar archivos bloqueados de una implementación, detenga el grupo de aplicaciones mediante **uno** de los enfoques siguientes:

* Use Web Deploy con una referencia a `Microsoft.NET.Sdk.Web` en el archivo de proyecto. Se coloca un archivo *app_offline.htm* en la raíz del directorio de aplicación web. Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación. Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm) (Referencia de configuración del módulo de ASP.NET Core).
* Detenga manualmente el grupo de aplicaciones en el Administrador de IIS en el servidor.
* Use PowerShell para detener y reiniciar el grupo de aplicaciones (requiere PowerShell 5 o posterior):

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a>Protección de datos

La [pila de protección de datos de ASP.NET Core](xref:security/data-protection/introduction) la usan varios [middlewares](xref:fundamentals/middleware/index) de ASP.NET Core, incluidos los que se emplean en la autenticación. Aunque el código de usuario no llame a las API de protección de datos, la protección de datos se debe configurar con un script de implementación o en el código de usuario para crear un [almacén de claves](xref:security/data-protection/implementation/key-management) criptográficas persistente. Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación.

Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:

* Todos los tokens de autenticación basados en cookies se invalidan. 
* Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud. 
* Ya no se pueden descifrar los datos protegidos con el conjunto de claves. Esto puede incluir [tokens CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) y [cookies de TempData de ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).

Para configurar la protección de datos en IIS para conservar el conjunto de claves, use **uno** de los enfoques siguientes:

* **Crear claves del Registro de protección de datos**

  Las claves de protección de datos que las aplicaciones de ASP.NET usan se almacenan en el Registro externo a las aplicaciones. Para conservar las claves de una determinada aplicación, cree claves del Registro para el grupo de aplicaciones.

  En las instalaciones independientes de IIS que no son de granja de servidores web, puede usar el [script de PowerShell Provision-AutoGenKeys.ps1 de protección de datos](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) para cada grupo de aplicaciones usado con una aplicación de ASP.NET Core. Este script crea una clave del Registro en el registro HKLM que solo es accesible a la cuenta de proceso de trabajo del grupo de aplicaciones de la aplicación. Las claves se cifran en reposo mediante DPAPI con una clave de equipo.

  En escenarios de granja de servidores web, una aplicación puede configurarse para usar una ruta de acceso UNC para almacenar su conjunto de claves de protección de datos. De forma predeterminada, las claves de protección de datos no se cifran. Asegúrese de que los permisos de archivo de un recurso compartido de red se limitan a la cuenta de Windows bajo la que se ejecuta la aplicación. Puede usar un certificado X509 para proteger las claves en reposo. Considere un mecanismo que permita a los usuarios cargar certificados: coloque los certificados en el almacén de certificados de confianza del usuario y asegúrese de que están disponibles en todos los equipos en los que se ejecuta la aplicación del usuario. Vea [Configurar la protección de datos en ASP.NET Core](xref:security/data-protection/configuration/overview) para más información.

* **Configurar el grupo de aplicaciones de IIS para cargar el perfil de usuario**

  Esta opción está en la sección **Modelo de proceso**, en la **Configuración avanzada** del grupo de aplicaciones. Establezca Cargar perfil de usuario en `True`. Esto almacena las claves en el directorio del perfil de usuario y las protege mediante DPAPI con una clave específica de la cuenta de usuario que el grupo de aplicaciones usa.

* **Usar el sistema de archivos como un almacén de conjunto de claves**

  Ajuste el código de la aplicación para [usar el sistema de archivos como un almacén de conjunto de claves](xref:security/data-protection/configuration/overview). Use un certificado X509 para proteger el conjunto de claves y asegúrese de que sea un certificado de confianza. Si es un certificado autofirmado, colóquelo en el almacén raíz de confianza.

  Cuando se usa IIS en una granja de servidores web:

  * Use un recurso compartido de archivos al que puedan acceder todos los equipos.
  * Implemente un certificado X509 en cada equipo. Configure la [protección de datos en el código](xref:security/data-protection/configuration/overview).

* **Establecer una directiva para todo el equipo para la protección de datos**

  El sistema de protección de datos tiene compatibilidad limitada con el establecimiento de una [directiva de equipo](xref:security/data-protection/configuration/machine-wide-policy) predeterminada para todas las aplicaciones que usan las API de protección de datos. Para obtener más información, vea <xref:security/data-protection/introduction>.

## <a name="sub-application-configuration"></a>Configuración de aplicaciones secundarias

Las aplicaciones secundarias agregadas bajo la aplicación raíz no deben incluir el módulo de ASP.NET Core como controlador. Si se agrega el módulo como controlador al archivo *web.config* de una aplicación secundaria, aparece un *error 500.19 (Error interno del servidor)* que hace referencia al archivo de configuración erróneo al intentar examinar la aplicación secundaria.

En el ejemplo siguiente se muestra un archivo *web.config* publicado para una aplicación secundaria ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Al hospedar una aplicación secundaria que no es de ASP.NET Core bajo una aplicación de ASP.NET Core, quite explícitamente el controlador heredado del archivo *web.config* de la aplicación secundaria:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Para más información sobre la configuración del módulo de ASP.NET Core, vea el tema [Introducción al módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core).

## <a name="configuration-of-iis-with-webconfig"></a>Configuración de IIS con web.config

La configuración de IIS aún se ve afectada por la sección **\<system.webServer>** de *web.config* en las características de IIS que se aplican a una configuración de proxy inverso. Si IIS está configurado en el nivel de servidor para usar compresión dinámica, el elemento **\<urlCompression>** del archivo *web.config* de la aplicación puede deshabilitarla.

Para más información, vea la [referencia de configuración para \<system.webServer>](/iis/configuration/system.webServer/), [Referencia de configuración del módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module) y [Módulos IIS con ASP.NET Core](xref:host-and-deploy/iis/modules). Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite para IIS 10.0 o posterior), vea la sección *AppCmd.exe command* (Comando AppCmd.exe) del tema [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno <environmentVariables>) de la documentación de referencia de IIS.

## <a name="configuration-sections-of-webconfig"></a>Secciones de configuración de web.config

Las aplicaciones ASP.NET Core no usan las secciones de configuración de aplicaciones ASP.NET 4.x en *web.config* para la configuración:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

Las aplicaciones de ASP.NET Core se configuran mediante otros proveedores de configuración. Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Grupos de aplicaciones

Al hospedar varios sitios web en un servidor, nuestra recomendación es aislar las aplicaciones entre sí ejecutándolas en su propio grupo de aplicaciones. El valor predeterminado del cuadro de diálogo **Agregar sitio web** es esta configuración. Cuando se proporciona el **Nombre del sitio**, el texto se transfiere automáticamente al cuadro de texto **Grupo de aplicaciones**. Al agregar el sitio se crea un grupo de aplicaciones con el nombre del sitio.

## <a name="application-pool-identity"></a>Identidad del grupo de aplicaciones

Una cuenta de identidad del grupo de aplicaciones permite ejecutar una aplicación en una cuenta única sin tener que crear ni administrar dominios o cuentas locales. En IIS 8.0 o versiones posteriores, el proceso de trabajo de administración de IIS (WAS) crea una cuenta virtual con el nombre del nuevo grupo de aplicaciones y ejecuta los procesos de trabajo del grupo de aplicaciones en esta cuenta de forma predeterminada. En la Consola de administración de IIS, en la opción **Configuración avanzada** del grupo de aplicaciones, asegúrese de que la **Identidad** está establecida para usar **ApplicationPoolIdentity**:

![Cuadro de diálogo Configuración avanzada del grupo de aplicaciones](index/_static/apppool-identity.png)

El proceso de administración de IIS crea un identificador seguro con el nombre del grupo de aplicaciones en el sistema de seguridad de Windows. Los recursos se pueden proteger mediante esta identidad. Sin embargo, no es una cuenta de usuario real ni se muestra en la consola de administración de usuario de Windows.

Si el proceso de trabajo de IIS requiere acceso con privilegios elevados a la aplicación, modifique la lista de control de acceso (ACL) del directorio que contiene la aplicación:

1. Abra el Explorador de Windows y vaya al directorio.

1. Haga clic con el botón derecho en el directorio y seleccione **Propiedades**.

1. En la pestaña **Seguridad**, haga clic en el botón **Editar** y en el botón **Agregar**.

1. Haga clic en el botón **Ubicaciones** y asegúrese de seleccionar el sistema.

1. Escriba **IIS AppPool\\<nombre_del_grupo_de_aplicaciones>** en el área **Escribir los nombres de objeto para seleccionar**. Haga clic en el botón **Comprobar nombres**. Para *DefaultAppPool* compruebe los nombres con **IIS AppPool\DefaultAppPool**. Cuando el botón **Comprobar nombres** está seleccionado, un valor de **DefaultAppPool** se indica en el área de los nombres de objeto. No es posible escribir el nombre del grupo de aplicaciones directamente en el área de los nombres de objeto. Use el formato **IIS AppPool\\<nombre_del_grupo_de_aplicaciones>** cuando compruebe el nombre del objeto.

   ![Cuadro de diálogo Seleccionar usuarios o grupos para la carpeta de la aplicación: el nombre del grupo de aplicaciones de "DefaultAppPool" se anexa al "IIS AppPool\" en el área de los nombres de objeto antes de seleccionar "Comprobar nombres".](index/_static/select-users-or-groups-1.png)

1. Seleccione **Aceptar**.

   ![Cuadro de diálogo Seleccionar usuarios o grupos para la carpeta de la aplicación: después de seleccionar "Comprobar nombres", el nombre del objeto "DefaultAppPool" se muestra en el área de los nombres de objeto.](index/_static/select-users-or-groups-2.png)

1. Los permisos de lectura y ejecución se deben conceder de forma predeterminada. Proporcione permisos adicionales según sea necesario.

El acceso también se puede conceder mediante un símbolo del sistema con la herramienta **ICACLS**. En el siguiente comando se usa *DefaultAppPool* como ejemplo:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Para más información, consulte el tema [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="deployment-resources-for-iis-administrators"></a>Recursos de implementación para administradores de IIS

Obtenga información detallada sobre IIS en su documentación.  
[Documentación de IIS](/iis)

Obtenga información sobre los modelos de implementación de aplicaciones .NET Core.  
[Implementación de aplicaciones .NET Core](/dotnet/core/deploying/)

Obtenga información sobre cómo el módulo de ASP.NET Core permite que el servidor web de Kestrel use IIS o IIS Express como servidor proxy inverso.  
[Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)

Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.  
[Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Obtenga información sobre la estructura de directorios de las aplicaciones ASP.NET Core publicadas.  
[Estructura de directorios](xref:host-and-deploy/directory-structure)

Descubra módulos activos e inactivos de IIS para aplicaciones ASP.NET Core y cómo administrar módulos de IIS.  
[Módulos de IIS](xref:host-and-deploy/iis/troubleshoot)

Obtenga información sobre cómo diagnosticar problemas con las implementaciones de aplicaciones ASP.NET Core por parte de IIS.  
[Solucionar problemas](xref:host-and-deploy/iis/troubleshoot)

Identifique los errores comunes al hospedar aplicaciones ASP.NET Core en IIS.  
[Referencia de errores comunes de Azure App Service e IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a ASP.NET Core](xref:index)
* [Sitio oficial de Microsoft IIS](https://www.iis.net/)
* [Biblioteca de contenido técnico de Windows Server](/windows-server/windows-server)
* [HTTP/2 en IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
