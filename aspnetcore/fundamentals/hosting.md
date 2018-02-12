---
title: Hospedaje en ASP.NET Core
author: guardrex
description: "Obtenga información sobre el host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a>Hospedaje en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Las aplicaciones de ASP.NET Core configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes.

## <a name="setting-up-a-host"></a>Configuración de un host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`. En las plantillas de proyecto, `Main` se encuentra en *Program.cs*. Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder` realiza las tareas siguientes:

* Configura [Kestrel](servers/kestrel.md) como el servidor web. Para conocer las opciones predeterminadas de Kestrel, consulte [la sección Opciones de Kestrel de la implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Carga configuración opcional de:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development`.
  * Variables de entorno.
  * Argumentos de la línea de comandos.
* Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración. El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.
* Cuando se ejecuta detrás de IIS, permite la [integración con IIS](xref:host-and-deploy/iis/index). Configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). El módulo crea a un proxy inverso entre IIS y Kestrel. También configura la aplicación para que [capture errores de inicio](#capture-startup-errors). Para conocer las opciones predeterminadas de IIS, consulte [la sección Opciones de IIS de Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index#iis-options).

La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC. Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido. Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).

Para más información sobre la configuración de la aplicación, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index).

> [!NOTE]
> Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x. Para más información, vea la pestaña ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). La creación de un host se suele realizar en el punto de entrada de la aplicación, el método `Main`. En las plantillas de proyecto, `Main` se encuentra en *Program.cs*:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder` requiere un [servidor que implemente IServer](servers/index.md). Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET Core 2.0, HTTP.sys se llamaba [WebListener](xref:fundamentals/servers/weblistener)). En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.

La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC. La raíz de contenido predeterminada se obtiene para `UseContentRoot` mediante [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido. Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).

Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host. A diferencia de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` no configura un *servidor*. `UseIISIntegration` configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS. Para utilizar IIS con ASP.NET Core, deben especificarse `UseKestrel` y `UseIISIntegration`. `UseIISIntegration` solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express. Para obtener más información, consulte [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo ASP.NET Core).

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

---

Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1). Si se especifica una clase `Startup`, debe definir un método `Configure`. Para obtener más información, vea [Application Startup in ASP.NET Core](startup.md) (Inicio de la aplicación en ASP.NET Core). Varias llamadas a `ConfigureServices` se anexan entre sí. Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.

## <a name="host-configuration-values"></a>Valores de configuración de host

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:

* Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`. Por ejemplo: `ASPNETCORE_URLS`.
* Métodos explícitos, como `CaptureStartupErrors`.
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada. Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.

El host usa cualquier opción que establece un valor en último lugar. Para obtener más información, consulte [Invalidación de la configuración](#overriding-configuration) en la sección siguiente.

### <a name="capture-startup-errors"></a>Capturar errores de inicio

Esta configuración controla la captura de errores de inicio.

**Clave**: captureStartupErrors  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.  
**Establecer mediante**: `CaptureStartupErrors`  
**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Cuando es `false`, los errores durante el inicio provocan la salida del host. Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Raíz del contenido

Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC. 

**Clave**: contentRoot  
**Tipo**: *cadena*  
**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.  
**Establecer mediante**: `UseContentRoot`  
**Variable de entorno**: `ASPNETCORE_CONTENTROOT`

La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root). Si no existe la ruta de acceso, el host no se puede iniciar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Errores detallados

Determina si se deben capturar los errores detallados.

**Clave**: detailedErrors  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: false  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`

Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Entorno

Establece el entorno de la aplicación.

**Clave**: environment  
**Tipo**: *cadena*  
**Valor predeterminado**: producción  
**Establecer mediante**: `UseEnvironment`  
**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`

El entorno se puede establecer en cualquier valor. Los valores definidos por el marco son `Development`, `Staging` y `Production`. Los valores no distinguen mayúsculas de minúsculas. De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`. Cuando se usa [Visual Studio](https://www.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *launchSettings.json*. Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Ensamblados de inicio de hospedaje

Establece los ensamblados de inicio de hospedaje de la aplicación.

**Clave**: hostingStartupAssemblies  
**Tipo**: *cadena*  
**Valor predeterminado**: una cadena vacía  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio. Esta característica es nueva en ASP.NET Core 2.0.

Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación. Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Esta característica no está disponible en ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Preferir las direcciones URL de hospedaje

Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.

**Clave**: preferHostingUrls  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: true  
**Establecer mediante**: `PreferHostingUrls`  
**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`

Esta característica es nueva en ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Esta característica no está disponible en ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Evitar el inicio de hospedaje

Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación. Consulte [Incorporación de características de aplicación desde un ensamblado externo mediante IHostingStartup](xref:host-and-deploy/ihostingstartup) para obtener más información.

**Clave**: preventHostingStartup  
**Tipo**: *bool* (`true` o `1`)  
**Valor predeterminado**: false  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

Esta característica es nueva en ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Esta característica no está disponible en ASP.NET Core 1.x.

---

### <a name="server-urls"></a>Direcciones URL de servidor

Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.

**Clave**: urls  
**Tipo**: *cadena*  
**Valor predeterminado**: http://localhost:5000  
**Establecer mediante**: `UseUrls`  
**Variable de entorno**: `ASPNETCORE_URLS`

Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor. Por ejemplo: `http://localhost:123`. Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`). El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL. Los formatos admitidos varían en función del servidor.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel tiene su propia API de configuración de punto de conexión. Para más información, vea [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) (Implementación del servidor web de Kestrel en ASP.NET Core).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Tiempo de espera de apagado

Especifica la cantidad de tiempo que se espera hasta que se apague el host de web.

**Clave**: shutdownTimeoutSeconds  
**Tipo**: *int*  
**Valor predeterminado**: 5  
**Establecer mediante**: `UseShutdownTimeout`  
**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión `UseShutdownTimeout` toma `TimeSpan`. Esta característica es nueva en ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Esta característica no está disponible en ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Ensamblado de inicio

Determina el ensamblado en el que se va a buscar la clase `Startup`.

**Clave**: startupAssembly  
**Tipo**: *cadena*  
**Valor predeterminado**: el ensamblado de la aplicación  
**Establecer mediante**: `UseStartup`  
**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`

Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`). Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Raíz web

Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.

**Clave**: webroot  
**Tipo**: *cadena*  
**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso. Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.  
**Establecer mediante**: `UseWebRoot`  
**Variable de entorno**: `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Invalidación de la configuración

Use [Configuración](xref:fundamentals/configuration/index) para configurar el host. En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hosting.json*. Cualquier configuración que se carga desde el archivo *hosting.json* puede reemplazarse por argumentos de línea de comandos. La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Invalidar la configuración proporcionada por `UseUrls` primero con la configuración de *hosting.json* y segundo con la configuración del argumento de línea de comandos:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Invalidar la configuración proporcionada por `UseUrls` primero con la configuración de *hosting.json* y segundo con la configuración del argumento de línea de comandos:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

---

> [!NOTE]
> El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`. El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`). El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`). La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host. Este problema se corregirá en una versión futura. Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).

Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar `dotnet run`. El argumento de línea de comandos reemplaza el valor `urls` del archivo *hosting.json*, y el servidor escucha en el puerto 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>Inicio del host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

**Start(acción<IRouteBuilder> routeBuilder)**

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

**Start(url de cadena, acción<IRouteBuilder> routeBuilder)**

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce el mismo resultado que **Start(acción<IRouteBuilder> routeBuilder)**, excepto que la aplicación responde en `http://localhost:8080`.

**StartWith(acción<IApplicationBuilder> app)**

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!" `WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.

**StartWith(url de cadena, acción<IApplicationBuilder> app)**

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce el mismo resultado que **StartWith(acción<IApplicationBuilder> app)**, excepto que la aplicación responde en `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="ihostingenvironment-interface"></a>Interfaz IHostingEnvironment

La [interfaz IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación. Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:

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

Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#startup-conventions) para configurar la aplicación en el inicio según el entorno. Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:

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
> Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`. Para más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).

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

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado. Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado. También hay un método `StopApplication`.

| Token de cancelación    | Se desencadena cuando&#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | El host se ha iniciado completamente. |
| `ApplicationStopping` | El host está realizando un apagado estable. Puede que todavía se estén procesando las solicitudes. El apagado se bloquea hasta que se complete este evento. |
| `ApplicationStopped`  | El host está completando un apagado estable. Deben procesarse todas las solicitudes. El apagado se bloquea hasta que se complete este evento. |

| Método            | Acción                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Solicita la terminación de la aplicación actual. |

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

## <a name="troubleshooting-systemargumentexception"></a>Solución de problemas de System.ArgumentException

**Se aplica únicamente a ASP.NET Core 2.0**

Se puede crear un host mediante la inserción directa de `IStartup` en el contenedor de inserción de dependencias en lugar de llamar a `UseStartup` o `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Si el host se crea de esta forma, puede producirse el error siguiente:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Esto ocurre porque [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`. Si la aplicación inserta manualmente `IStartup` en el contenedor de inserción de dependencias, agregue la siguiente llamada a `WebHostBuilder` con el nombre de ensamblado especificado:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

O bien, agregue un `Configure` ficticio a `WebHostBuilder`, que establece `applicationName`(`ApplicationKey`) automáticamente:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**NOTA**: Esto solo es necesario con la versión ASP.NET Core 2.0 y únicamente cuando la aplicación no llama a `UseStartup` o `Configure`.

Para obtener más información, vea [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario) y el [ejemplo StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje en Windows con IIS](xref:host-and-deploy/iis/index)
* [Hospedaje en Linux con Nginx](xref:host-and-deploy/linux-nginx)
* [Hospedaje en Linux con Apache](xref:host-and-deploy/linux-apache)
* [Hospedaje en un servicio de Windows](xref:host-and-deploy/windows-service)
