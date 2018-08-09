---
title: Host web de ASP.NET Core
author: guardrex
description: Obtenga información sobre el host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 476795645b0430962b61f7a61de29d5d1819602b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356719"
---
# <a name="aspnet-core-web-host"></a>Host web de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Las aplicaciones de ASP.NET Core configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes. En este tema se aborda el host web de ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), muy útil para hospedar aplicaciones web. Para conocer la cobertura del host genérico de .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vea <xref:fundamentals/host/generic-host>.

## <a name="set-up-a-host"></a>Configuración de un host

::: moniker range=">= aspnetcore-2.0"

Cree un host con una instancia de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`. En las plantillas de proyecto, `Main` se encuentra en *Program.cs*. Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

`CreateDefaultBuilder` realiza las tareas siguientes:

* Configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y configura el servidor por medio de los proveedores de configuración de hospedaje de la aplicación. Para consultar las opciones predeterminadas de Kestrel, vea <xref:fundamentals/servers/kestrel#kestrel-options>.
* Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Carga la [configuración de host](#host-configuration-values) de:
  * Variables de entorno con el prefijo `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`).
  * Argumentos de la línea de comandos.
* Carga la configuración de aplicación de:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.
  * Variables de entorno.
  * Argumentos de la línea de comandos.
* Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración. El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.
* Cuando se ejecuta detrás de IIS, permite la [integración con IIS](xref:host-and-deploy/iis/index). Configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). El módulo crea a un proxy inverso entre IIS y Kestrel. También configura la aplicación para que [capture errores de inicio](#capture-startup-errors). Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.
* Establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo. Para más información, vea [Validación del ámbito](#scope-validation).

La configuración definida en `CreateDefaultBuilder` se puede reemplazar y aumentar mediante [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) y otros métodos y métodos de extensión de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). A continuación, se presentan algunos ejemplos:

* [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) se usa para especificar `IConfiguration` adicionales para la aplicación. La siguiente llamada `ConfigureAppConfiguration` agrega un delegado para incluir la configuración de la aplicación en el archivo *appsettings.xml*. Es posible llamar a `ConfigureAppConfiguration` varias veces. Tenga en cuenta que esta configuración no se aplica al host (por ejemplo, direcciones URL de servidor o entorno). Vea la sección [Valores de configuración de host](#host-configuration-values).

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* La siguiente llamada `ConfigureLogging` agrega un delegado para configurar el nivel de registro mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) en [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel). Esta configuración reemplaza la configuración de *appsettings.Development.JSON* (`LogLevel.Debug`) y *appsettings.Production.JSON* (`LogLevel.Error`) configurada mediante `CreateDefaultBuilder`. Es posible llamar a `ConfigureLogging` varias veces.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* La siguiente llamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC. Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido. Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).

Para obtener más información sobre la configuración de la aplicación, vea <xref:fundamentals/configuration/index>.

> [!NOTE]
> Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x. Para más información, vea la pestaña ASP.NET Core 1.x.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). La creación de un host se suele realizar en el punto de entrada de la aplicación, el método `Main`. En las plantillas de proyecto, `Main` se encuentra en *Program.cs*:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

`WebHostBuilder` requiere un [servidor que implemente IServer](xref:fundamentals/servers/index). Los servidores integrados son [Kestrel](xref:fundamentals/servers/kestrel) y [HTTP.sys](xref:fundamentals/servers/httpsys) (antes del lanzamiento de ASP.NET Core 2.0, HTTP.sys se llamaba [WebListener](xref:fundamentals/servers/weblistener)). En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.

La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC. La raíz de contenido predeterminada se obtiene para `UseContentRoot` mediante [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido. Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).

Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host. A diferencia de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` no configura un *servidor*. `UseIISIntegration` configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS. Para utilizar IIS con ASP.NET Core, deben especificarse `UseKestrel` y `UseIISIntegration`. `UseIISIntegration` solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express. Para obtener más información, consulte <xref:fundamentals/servers/aspnet-core-module> y <xref:host-and-deploy/aspnet-core-module>.

Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de la canalización de solicitudes de la aplicación:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

::: moniker-end

Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1). Si se especifica una clase `Startup`, debe definir un método `Configure`. Para obtener más información, vea <xref:fundamentals/startup>. Varias llamadas a `ConfigureServices` se anexan entre sí. Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.

## <a name="host-configuration-values"></a>Valores de configuración de host

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:

* Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`. Por ejemplo: `ASPNETCORE_ENVIRONMENT`.
* Extensiones como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) y [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vea la sección [Invalidación de la configuración](#override-configuration)).
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada. Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.

El host usa cualquier opción que establece un valor en último lugar. Para obtener más información, consulte [Invalidación de la configuración](#override-configuration) en la sección siguiente.

### <a name="application-key-name"></a>Clave de aplicación (nombre)

La propiedad [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) se establece automáticamente cuando se llama a [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la construcción del host. El valor se establece en el nombre del ensamblado que contiene el punto de entrada de la aplicación. Para establecer el valor explícitamente, use [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):

**Clave**: nombreDeAplicación  
**Tipo**: *cadena*  
**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_APPLICATIONKEY`

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a>Capturar errores de inicio

Esta configuración controla la captura de errores de inicio.

**Clave**: captureStartupErrors  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.  
**Establecer mediante**: `CaptureStartupErrors`  
**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Cuando es `false`, los errores durante el inicio provocan la salida del host. Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a>Raíz del contenido

Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC. 

**Clave**: contentRoot  
**Tipo**: *cadena*  
**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.  
**Establecer mediante**: `UseContentRoot`  
**Variable de entorno**: `ASPNETCORE_CONTENTROOT`

La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root). Si no existe la ruta de acceso, el host no se puede iniciar.

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a>Errores detallados

Determina si se deben capturar los errores detallados.

**Clave**: detailedErrors  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: false  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`

Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a>Entorno

Establece el entorno de la aplicación.

**Clave**: environment  
**Tipo**: *cadena*  
**Valor predeterminado**: producción  
**Establecer mediante**: `UseEnvironment`  
**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`

El entorno se puede establecer en cualquier valor. Los valores definidos por el marco son `Development`, `Staging` y `Production`. Los valores no distinguen mayúsculas de minúsculas. De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`. Cuando se usa [Visual Studio](https://www.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *launchSettings.json*. Para obtener más información, vea <xref:fundamentals/environments>.

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a>Ensamblados de inicio de hospedaje

Establece los ensamblados de inicio de hospedaje de la aplicación.

**Clave**: hostingStartupAssemblies  
**Tipo**: *cadena*  
**Valor predeterminado**: una cadena vacía  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.

Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación. Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a>Puerto HTTPS

Establezca puerto de redirección HTTPS. Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).

**Clave**: https_port **Tipo**: *string*
**Valor predeterminada**: no se establece un valor predeterminado.
**Establecer mediante**: `UseSetting`
**Variable de entorno**: `ASPNETCORE_HTTPS_PORT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a>Preferir las direcciones URL de hospedaje

Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.

**Clave**: preferHostingUrls  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: true  
**Establecer mediante**: `PreferHostingUrls`  
**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a>Evitar el inicio de hospedaje

Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación. Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.

**Clave**: preventHostingStartup  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: false  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a>Direcciones URL de servidor

Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.

**Clave**: urls  
**Tipo**: *cadena*  
**Predeterminado**: http://localhost:5000  
**Establecer mediante**: `UseUrls`  
**Variable de entorno**: `ASPNETCORE_URLS`

Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor. Por ejemplo: `http://localhost:123`. Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`). El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL. Los formatos admitidos varían en función del servidor.

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

Kestrel tiene su propia API de configuración de punto de conexión. Para obtener más información, vea <xref:fundamentals/servers/kestrel#endpoint-configuration>.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a>Tiempo de espera de apagado

Especifica la cantidad de tiempo que se espera hasta el cierre del host de web.

**Clave**: shutdownTimeoutSeconds  
**Tipo**: *int*  
**Valor predeterminado**: 5  
**Establecer mediante**: `UseShutdownTimeout`  
**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) toma [TimeSpan](/dotnet/api/system.timespan).

Durante el período de tiempo de espera, el hospedaje:

* Activa [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Trata de detener los servicios hospedados y registra cualquier error que se produzca en los servicios que no se puedan detener.

Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación. Los servicios se detienen aun cuando no hayan terminado de procesarse. Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a>Ensamblado de inicio

Determina el ensamblado en el que se va a buscar la clase `Startup`.

**Clave**: startupAssembly  
**Tipo**: *cadena*  
**Valor predeterminado**: el ensamblado de la aplicación  
**Establecer mediante**: `UseStartup`  
**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`

Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`). Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a>Raíz web

Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.

**Clave**: webroot  
**Tipo**: *cadena*  
**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso. Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.  
**Establecer mediante**: `UseWebRoot`  
**Variable de entorno**: `ASPNETCORE_WEBROOT`

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a>Invalidación de la configuración

Use [Configuración](xref:fundamentals/configuration/index) para configurar el host web. En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hostsettings.json*. Cualquier configuración que se cargue del archivo *hostsettings.json* se puede reemplazar por argumentos de línea de comandos. La configuración generada (en `config`) se usa para configurar el host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration). La configuración de `IWebHostBuilder` se agrega a la de la aplicación, pero lo contrario no es aplicable: `ConfigureAppConfiguration` no afecta a la configuración de `IWebHostBuilder`.

::: moniker range=">= aspnetcore-2.0"

Reemplazar primero la configuración proporcionada por `UseUrls` por la de *hostsettings.json* y, luego, por la del argumento de línea de comandos:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

*hostsettings.json*:

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Reemplazar primero la configuración proporcionada por `UseUrls` por la de *hostsettings.json* y, luego, por la del argumento de línea de comandos:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

*hostsettings.json*:

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`. El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`). El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`). La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host. Este problema se corregirá en una versión futura. Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).
>
> `UseConfiguration` solo copia las claves del elemento `IConfiguration` proporcionado a la configuración del generador de host. Por consiguiente, el hecho de configurar `reloadOnChange: true` para los archivos de configuración XML, JSON e INI no tiene ningún efecto.

Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar [dotnet run](/dotnet/core/tools/dotnet-run). El argumento de línea de comandos reemplaza el valor `urls` del archivo *hostsettings.json*, y el servidor efectúa la escucha en el puerto 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>Administración del host

::: moniker range=">= aspnetcore-2.0"

**Run**

El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:

```csharp
host.Run();
```

**Start**

Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

La aplicación puede inicializar un nuevo host usando los valores preconfigurados de `CreateDefaultBuilder` mediante un método práctico estático. Estos métodos inician el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperando una interrupción (Ctrl-C/SIGINT o SIGTERM):

**Start(RequestDelegate app)**

Start con `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!" `WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.

**Start(url de cadena, RequestDelegate app)**

Start con una dirección URL y `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce el mismo resultado que **Start(RequestDelegate app)**, excepto que la aplicación responde en `http://localhost:8080`.

**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**

Use una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Utilice las siguientes solicitudes de explorador con el ejemplo:

| Solicitud                                    | Respuesta                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Produce una excepción con la cadena "ooops!" |
| `http://localhost:5000/throw`              | Produce una excepción con la cadena "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.

**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**

Use una dirección URL y una instancia de `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Produce el mismo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, salvo que la aplicación responde en `http://localhost:8080`.

**StartWith(Action&lt;IApplicationBuilder&gt; app)**

Proporciona un delegado para configurar `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!" `WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.

**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**

Proporcione una dirección URL y un delegado para configurar `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Produce el mismo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, salvo que la aplicación responde en `http://localhost:8080`.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

**Run**

El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:

```csharp
host.Run();
```

**Start**

Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

::: moniker-end

## <a name="ihostingenvironment-interface"></a>Interfaz IHostingEnvironment

La [interfaz IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación. Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#environment-based-startup-class-and-methods) para configurar la aplicación en el inicio según el entorno. Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`. Para obtener más información, vea <xref:fundamentals/environments>.

El servicio `IHostingEnvironment` también se puede insertar directamente en el método `Configure` para configurar la canalización de procesamiento:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment` puede insertarse en el método `Invoke` al crear [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizado:

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>Interfaz IApplicationLifetime

[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado. Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.

| Token de cancelación    | Se desencadena cuando&#8230; |
| --------------------- | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | El host se ha iniciado completamente. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | El host está completando un apagado estable. Deben procesarse todas las solicitudes. El apagado se bloquea hasta que se complete este evento. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | El host está realizando un apagado estable. Puede que todavía se estén procesando las solicitudes. El apagado se bloquea hasta que se complete este evento. |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación. La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a>Validación del ámbito

::: moniker range=">= aspnetcore-2.0"

[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.

::: moniker-end

Cuando `ValidateScopes` está establecido en `true`, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:

* Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.
* Los servicios con ámbito no se insertan directa o indirectamente en singletons.

El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.

De la eliminación de los servicios con ámbito se encarga el contenedor que los creó. Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran. Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.

Para validar los ámbitos siempre, incluso en el entorno de producción, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) en el generador de hosts:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a>Solución de problemas de System.ArgumentException

**Lo siguiente solo es pertinente en las aplicaciones de ASP.NET Core 2.0 cuando la aplicación no llama a `UseStartup` o `Configure`.**

Se puede crear un host mediante la inserción directa de `IStartup` en el contenedor de inserción de dependencias en lugar de llamar a `UseStartup` o `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Si el host se crea de esta forma, puede producirse el error siguiente:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Esto ocurre porque es necesario el nombre de la aplicación (el nombre del ensamblado actual) para buscar `HostingStartupAttributes`. Si la aplicación inserta manualmente `IStartup` en el contenedor de inserción de dependencias, agregue la siguiente llamada a `WebHostBuilder` con el nombre de ensamblado especificado:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

O bien, agregue un `Configure` ficticio a `WebHostBuilder`, que establece el nombre de la aplicación automáticamente:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

Para obtener más información, vea [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario) y el [ejemplo StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
