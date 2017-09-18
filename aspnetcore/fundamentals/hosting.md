---
title: Hospedar en ASP.NET Core
author: guardrex
description: "Obtenga información acerca del host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación."
keywords: "Núcleo de ASP.NET, web host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4eb57cf80399abdb7c6d05546ea2b0d5718c56c3
ms.sourcegitcommit: 0a3f215b4f665afc6f2678642968eea698102346
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2017
---
# <a name="hosting-in-aspnet-core"></a>Hospedar en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aplicaciones de ASP.NET Core configurar e iniciar un *host*, que es responsable de la administración de inicio y duración de la aplicación. Como mínimo, el host configura un servidor y una canalización de procesamiento de la solicitud.

## <a name="setting-up-a-host"></a>Configurar un host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método. En las plantillas de proyecto, `Main` se encuentra en *Program.cs*. Una típica *Program.cs* llamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`realiza las tareas siguientes:

* Configura [Kestrel](servers/kestrel.md) como el servidor web. Para las opciones predeterminadas de Kestrel, consulte [el Kestrel opciones de sección de implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Establece la raíz del contenido en [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Configuración opcional de cargas de:
  * *appSettings.JSON que se*.
  * *appSettings. {Entorno} .json*.
  * [Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el `Development` entorno.
  * Variables de entorno.
  * Argumentos de línea de comandos.
* Configura [registro](xref:fundamentals/logging) de la salida de consola y de depuración a [filtrado del registro](xref:fundamentals/logging#log-filtering) las reglas especificadas en una sección de configuración de registro de un *appSettings.JSON que se* o *appsettings. {Entorno} .json* archivo.
* Cuando se ejecuta detrás de IIS, permite [integración con IIS](xref:publishing/iis) mediante la configuración de la ruta de acceso base y el puerto del servidor debe escuchar al usar el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module). El módulo crea a un proxy inverso entre IIS y Kestrel. También configura la aplicación [capturar errores de inicio](#capture-startup-errors). Para las opciones predeterminadas IIS, consulte [IIS opciones de sección de Host de ASP.NET Core en Windows con IIS](xref:publishing/iis#iis-options).

El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC. La raíz de contenido de forma predeterminada es [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory). Esto resulta en el uso de carpeta raíz del proyecto web como la raíz de contenido cuando la aplicación se inicia desde la carpeta raíz (por ejemplo, al llamar a [dotnet ejecutar](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto). Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).

Vea [configuración en ASP.NET Core](xref:fundamentals/configuration) para obtener más información sobre la configuración de la aplicación.

> [!NOTE]
> Como alternativa al uso el método estático `CreateDefaultBuilder` (método), crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x. Consulte la ficha de 1.x ASP.NET Core para obtener más información.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método. En las plantillas de proyecto, `Main` se encuentra en *Program.cs*. El siguiente *Program.cs* muestra cómo utilizar `WebHostBuilder` para crear el host:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`requiere un [server que implementa IServer](servers/index.md). Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET 2.0 de núcleo, HTTP.sys llamaba [WebListener](xref:fundamentals/servers/weblistener)). En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.

El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC. La raíz de contenido predeterminado proporcionado para `UseContentRoot` es [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Esto resulta en el uso de carpeta raíz del proyecto web como la raíz de contenido cuando la aplicación se inicia desde la carpeta raíz (por ejemplo, al llamar a [dotnet ejecutar](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto). Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).

Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host. `UseIISIntegration`No configure un *server*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does. `UseIISIntegration`configura la ruta de acceso base y el puerto que debe escuchar el servidor cuando se usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS. Para usar IIS con ASP.NET Core, debe especificar tanto `UseKestrel` y `UseIISIntegration`. `UseIISIntegration`solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express. Para obtener más información, consulte [Introducción a ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) y [referencia de configuración de ASP.NET Core módulo](xref:hosting/aspnet-core-module).

Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de canalización de solicitud de la aplicación:

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

Al configurar un host, puede proporcionar [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos. Si especifica un `Startup` (clase), debe definir un `Configure` método. Para obtener más información, consulte [inicio de la aplicación de ASP.NET Core](startup.md). Varias llamadas a `ConfigureServices` anexar entre sí. Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` sustituye la configuración anterior.

## <a name="host-configuration-values"></a>Valores de configuración de host

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) proporciona métodos para establecer la mayoría de los valores de configuración disponibles para el host, que también se puede establecer directamente con [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada. Al establecer un valor con `UseSetting`, el valor se establece como una cadena (entre comillas), independientemente del tipo.

### <a name="capture-startup-errors"></a>Capturar errores de inicio

Esta configuración controla la captura de errores de inicio.

**Clave**: captureStartupErrors  
**Tipo de**: *bool* (`true` o `1`)  
**Valor predeterminado**: valor predeterminado es `false` a menos que la aplicación se ejecuta con Kestrel detrás de IIS, donde el valor predeterminado es `true`.  
**Establecer mediante**:`CaptureStartupErrors`

Cuando `false`, errores durante el resultado de inicio en el host de salir. Cuando `true`, el host captura excepciones durante el inicio y se intenta iniciar el servidor.

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

Esta configuración determina donde ASP.NET Core comienza a buscar archivos de contenido, como las vistas de MVC. 

**Clave**: contentRoot  
**Tipo de**: *cadena*  
**Predeterminado**: valor predeterminado es la carpeta donde se encuentra el ensamblado de la aplicación.  
**Establecer mediante**:`UseContentRoot`

La raíz de contenido también se usa como la ruta de acceso base para la [configuración Web raíz](#web-root). Si no existe la ruta de acceso, el host no se puede iniciar.

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

Determina si el registro detallado se deben capturar los errores.

**Clave**: detailedErrors  
**Tipo de**: *bool* (`true` o `1`)  
**Predeterminado**: false  
**Establecer mediante**:`UseSetting`

Cuando habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.

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

**Clave**: entorno  
**Tipo de**: *cadena*  
**Predeterminado**: producción  
**Establecer mediante**:`UseEnvironment`

Puede establecer la *entorno* en cualquier valor. Incluyen valores definidos por el marco de trabajo `Development`, `Staging`, y `Production`. Valores no distinguen mayúsculas de minúsculas. De forma predeterminada, el *entorno* se lee desde el `ASPNETCORE_ENVIRONMENT` variable de entorno. Cuando se usa [Visual Studio](https://www.visualstudio.com/), se pueden establecer variables de entorno el *launchSettings.json* archivo. Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).

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

### <a name="hosting-startup-assemblies"></a>Hospedaje de ensamblados de inicio

Establece los ensamblados de inicio de la aplicación host.

**Clave**: hostingStartupAssemblies  
**Tipo de**: *cadena*  
**Predeterminado**: una cadena vacía  
**Establecer mediante**:`UseSetting`

Una cadena delimitada por punto y coma de hospedaje de ensamblados de inicio para cargar en el inicio. Esta característica es nueva en ASP.NET 2.0 de núcleo.

Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio hospedaje incluyen siempre el ensamblado de la aplicación. Cuando se proporcionan hospedaje ensamblados de inicio, se agregan al ensamblado de la aplicación de carga cuando la aplicación genera sus servicios comunes durante el inicio.

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

Indica si el host debe escuchar en las direcciones URL configuradas con la `WebHostBuilder` en lugar de los que estén configurados con el `IServer` implementación.

**Clave**: preferHostingUrls  
**Tipo de**: *bool* (`true` o `1`)  
**Predeterminado**: true  
**Establecer mediante**:`PreferHostingUrls`

Esta característica es nueva en ASP.NET 2.0 de núcleo.

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

Impide que la carga automática de ensamblados de inicio, incluidos los ensamblados de la aplicación de hospedaje.

**Clave**: preventHostingStartup  
**Tipo de**: *bool* (`true` o `1`)  
**Predeterminado**: false  
**Establecer mediante**:`UseSetting`

Esta característica es nueva en ASP.NET 2.0 de núcleo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Esta característica no está disponible en ASP.NET Core 1.x.

---

### <a name="server-urls"></a>Direcciones URL del servidor

Indica las direcciones IP o direcciones de host con los puertos y protocolos que el servidor debe escuchar las solicitudes.

**Clave**: las direcciones URL  
**Tipo de**: *cadena*  
**Predeterminado**: http://localhost: 5000  
**Establecer mediante**:`UseUrls`

Establézcalo separados por punto y coma (;) de prefijos de lista de direcciones URL que debe responder el servidor. Por ejemplo: `http://localhost:123`. Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto especificado y el protocolo (por ejemplo, `http://*:5000`). El protocolo (`http://` o `https://`) debe incluirse en cada dirección URL. Los formatos admitidos varían entre los servidores.

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

### <a name="shutdown-timeout"></a>Tiempo de espera de cierre

Especifica la cantidad de tiempo de espera para el host de web en estado de cierre.

**Clave**: shutdownTimeoutSeconds  
**Tipo de**: *int*  
**Predeterminado**: 5  
**Establecer mediante**:`UseShutdownTimeout`

Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el `UseShutdownTimeout` método de extensión toma un `TimeSpan`. Esta característica es nueva en ASP.NET 2.0 de núcleo.

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

Determina el ensamblado que se va a buscar la `Startup` clase.

**Clave**: startupAssembly  
**Tipo de**: *cadena*  
**Predeterminado**: ensamblado de la aplicación  
**Establecer mediante**:`UseStartup`

Puede hacer referencia al ensamblado por su nombre (`string`) o tipo (`TStartup`). Si hay varios `UseStartup` se llaman a métodos, la última de ellas tiene prioridad.

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

### <a name="web-root"></a>Raíz de Web

Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.

**Clave**: webroot  
**Tipo de**: *cadena*  
**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Content Root)/wwwroot", si existe la ruta de acceso. Si la ruta de acceso no existe, se utiliza un proveedor de archivos de la operación inefectiva.  
**Establecer mediante**:`UseWebRoot`

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

## <a name="overriding-configuration"></a>Reemplazar configuración

Use [configuración](configuration.md) para configurar el host. En el ejemplo siguiente, la configuración del host, opcionalmente, se especifica en un *hosting.json* archivo. Cualquier configuración que se carga desde el *hosting.json* archivo puede reemplazarse por los argumentos de línea de comandos. La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:

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

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:

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
> El `UseConfiguration` no está actualmente capaz de analizar una sección de configuración devuelta por el método de extensión `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`. El `GetSection` método filtra las claves de configuración a la sección solicitado, pero deja el nombre de sección de las claves (por ejemplo, `section:urls`, `section:environment`). El `UseConfiguration` método espera las claves para que coincida con el `WebHostBuilder` claves (por ejemplo, `urls`, `environment`). La presencia del nombre de sección de las claves evita que los valores de la sección de configuración del host. Este problema se corregirá en una versión futura. Para obtener más información y soluciones alternativas, consulte [pasar la sección de configuración a WebHostBuilder.UseConfiguration utiliza claves completas](https://github.com/aspnet/Hosting/issues/839).

Para especificar el host que se ejecutan en una dirección URL determinada, podría pasar el valor deseado desde un símbolo del sistema al ejecutar `dotnet run`. El argumento de línea de comandos reemplaza la `urls` valor de la *hosting.json* archivo y el servidor escucha en el puerto 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>Orden de importancia

Algunos de los `WebHostBuilder` configuración primero se lee desde variables de entorno, si establece. Estas variables de entorno use el formato `ASPNETCORE_{configurationKey}`. Para establecer las direcciones URL que el servidor escucha en de forma predeterminada, se establece `ASPNETCORE_URLS`.

Puede invalidar cualquiera de estos valores de variable de entorno mediante la especificación de configuración (mediante `UseConfiguration`) o estableciendo el valor explícitamente (mediante `UseSetting` o uno de los métodos de extensión explícita, como `UseUrls`). El host usa cualquier opción establece el valor de la última. Si desea establecer la dirección URL predeterminada a un valor mediante programación pero permite que se reemplazará por la configuración, puede usar la configuración de línea de comandos después de establecer la dirección URL. Vea [configuración reemplazando](#overriding-configuration).

## <a name="starting-the-host"></a>Iniciando el host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Run**

El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:

```csharp
host.Run();
```

**Start**

Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:

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

Puede inicializar e iniciar un nuevo host utilizando los valores predeterminados configurados previamente de `CreateDefaultBuilder` utilizando un método estáticos. Estos métodos iniciar el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperar un salto (Ctrl-C/SIGINT o SIGTERM):

**Inicio (RequestDelegate aplicación)**

Comience con un `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!" `WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.

**Inicio (dirección url de cadena, RequestDelegate aplicación)**

Comenzar con una dirección URL y `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Genera el mismo resultado que **inicio (aplicación RequestDelegate)**, excepto la aplicación responde en `http://localhost:8080`.

**Iniciar (acción<IRouteBuilder> routeBuilder)**

Usar una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:

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
| `http://localhost:5000/hello/Martin`       | Hola, Martin                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Produce una excepción con la cadena "ooops!" |
| `http://localhost:5000/throw`              | Produce una excepción con la cadena "EH oh!" |
| `http://localhost:5000/Sante/Kevin`        | ¡Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.

**Iniciar (dirección url, acción de cadena<IRouteBuilder> routeBuilder)**

Usar una dirección URL y una instancia de `IRouteBuilder`:

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

Genera el mismo resultado que **iniciar (acción<IRouteBuilder> routeBuilder)**, excepto la aplicación responde en `http://localhost:8080`.

**StartWith (acción<IApplicationBuilder> aplicación)**

Proporciona un delegado para configurar un `IApplicationBuilder`:

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

Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!" `WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM). La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.

**StartWith (dirección url, acción de cadena<IApplicationBuilder> aplicación)**

Proporcione una dirección URL y un delegado que se va a configurar un `IApplicationBuilder`:

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

Genera el mismo resultado que **StartWith (acción<IApplicationBuilder> aplicación)**, excepto la aplicación responde en `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Run**

El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:

```csharp
host.Run();
```

**Start**

Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:


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

El [IHostingEnvironment interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de alojamiento web de la aplicación. Puede usar [inyección de constructor](xref:fundamentals/dependency-injection) para obtener el `IHostingEnvironment` para poder usar sus propiedades y métodos de extensión:

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

Puede usar un [enfoque basado en la convención](xref:fundamentals/environments#startup-conventions) para configurar la aplicación en el inicio según el entorno. Como alternativa, también puede insertar el `IHostingEnvironment` en el `Startup` constructor para su uso en `ConfigureServices`:

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
> Además el `IsDevelopment` método de extensión, `IHostingEnvironment` ofrece `IsStaging`, `IsProduction`, y `IsEnvironment(string environmentName)` métodos. Vea [trabajar con varios entornos](xref:fundamentals/environments) para obtener más información.

El `IHostingEnvironment` servicio también puede insertar directamente en el `Configure` método para configurar la canalización de procesamiento:

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

También puede insertar `IHostingEnvironment` en el `Invoke` método al crear personalizado [middleware](xref:fundamentals/middleware#writing-middleware):

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

El [IApplicationLifetime interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) le permite realizar las actividades posteriores a la de inicio y cierre. Tres propiedades en la interfaz son tokens de cancelación que puede registrar con `Action` métodos para definir los eventos de inicio y cierre. También hay un `StopApplication` método.

| Token de cancelación    | Desencadene cuando &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | El host se ha iniciado completamente. |
| `ApplicationStopping` | El host está realizando un cierre estable. Todavía pueden procesar las solicitudes. Bloques de apagado hasta que se complete este evento. |
| `ApplicationStopped`  | El host está completando un cierre estable. Todas las solicitudes se deben procesar completamente. Bloques de apagado hasta que se complete este evento. |

| Método            | Acción                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Finalización de las solicitudes de la aplicación actual. |

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

**Se aplica a los núcleos de ASP.NET 2.0 sólo**

Si compila el host insertando `IStartup` directamente en el contenedor de inyección de dependencia en lugar de llamar a `UseStartup` o `Configure`, puede encontrar el siguiente error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.

Esto ocurre porque la [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`. Si insertan manualmente `IStartup` en el contenedor de inyección de dependencia, agregue la llamada siguiente a su `WebHostBuilder` con el nombre de ensamblado especificado:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

O bien, agregar un ficticia `Configure` a su `WebHostBuilder`, que establece el `applicationName`(`ApplicationKey`) automáticamente:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Tenga en cuenta**: Esto solo es necesario con la versión 2.0 de ASP.NET Core y solo cuando no llama al método `UseStartup` o `Configure`.

Para obtener más información, vea [anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) y [StartupInjection ejemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Recursos adicionales

* [Publicar en Windows que usan IIS](../publishing/iis.md)
* [Publicar en Linux con Nginx](../publishing/linuxproduction.md)
* [Publicar en Linux con Apache](../publishing/apache-proxy.md)
* [Host en un servicio de Windows](xref:hosting/windows-service)
