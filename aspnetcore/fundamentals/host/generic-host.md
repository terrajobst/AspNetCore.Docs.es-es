---
title: Host genérico de .NET
author: guardrex
description: Obtenga información sobre el host genérico en .NET, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: de9044875c8ebc62c80a129d721e7d37be5d846d
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927814"
---
# <a name="net-generic-host"></a>Host genérico de .NET

Por [Luke Latham](https://github.com/guardrex)

Las aplicaciones de .NET Core configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. En este tema se trata el host genérico de ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), muy útil para hospedar aplicaciones que no procesan solicitudes HTTP. Para la cobertura del host de web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), vea <xref:fundamentals/host/web-host>.

El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una variedad de escenarios de host. Se trata de la mensajería, tareas en segundo plano y otras cargas de trabajo no HTTP basadas en la ventaja de host genérico de capacidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.

El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web. Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host). El host genérico se está desarrollando para reemplazar el host web en una próxima versión y actuar como la API del host principal tanto en escenarios que sean del tipo HTTP como los que no.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*. No ejecute el ejemplo en `internalConsole`.

Para establecer la consola en Visual Studio Code:

1. Abra el archivo *.vscode/launch.json*.
1. En la configuración de **.NET Core Launch (console)**, busque la entrada **console**. Establezca el valor en `externalTerminal` o `integratedTerminal`.

## <a name="introduction"></a>Introducción

La biblioteca de host genérico está disponible en el [espacio de nombres de Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) y la proporciona el [paquete Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) es el punto de entrada para la ejecución de código. Cada implementación de `IHostedService` se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices). Se llama a [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) en cada `IHostedService` cuando se inicia el host, y se llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) en orden inverso del registro cuando el host se cierra de forma estable.

## <a name="set-up-a-host"></a>Configuración de un host

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Configuración de host

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:

* Generador de configuración
* Configuración del método de extensión

### <a name="configuration-builder"></a>Generador de configuración

La configuración del generador de host se crea mediante una llamada a [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) en la implementación de [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureHostConfiguration` usa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para crear [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para el host. El generador de configuración inicializa [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) para su uso en el proceso de compilación de la aplicación.

La configuración de las variables de entorno no se agrega de forma predeterminada. Llame a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el generador de host para configurar el host a partir de las variables de entorno. `AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario. La aplicación de ejemplo usa el prefijo `PREFIX_`. El prefijo se quita cuando se leen las variables de entorno. Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.

Durante el desarrollo, al usar [Visual Studio](https://www.visualstudio.com/) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*. En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo. Para obtener más información, vea <xref:fundamentals/environments>.

Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma. El host usa cualquier opción que establece un valor en último lugar.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Configuración de `HostBuilder` de ejemplo con `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> El método de extensión [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por ejemplo, `.AddConfiguration(Configuration.GetSection("section"))`). El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:environment`). El método `AddConfiguration` espera que las claves coincidan con las claves `HostBuilder` (por ejemplo, `environment`). La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host. Este problema se corregirá en una versión futura. Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).

### <a name="extension-method-configuration"></a>Configuración del método de extensión

Se llama a métodos de extensión en la implementación de `IHostBuilder` para configurar la raíz de contenido y el entorno.

#### <a name="application-key-name"></a>Clave de aplicación (nombre)

La propiedad [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) se establece desde la configuración del host durante la construcción de este. Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):

**Clave**: nombreDeAplicación  
**Tipo**: *cadena*  
**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.  
**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Variable de entorno**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a>Raíz del contenido

Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.

**Clave**: contentRoot  
**Tipo**: *cadena*  
**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.  
**Establecer mediante**: `UseContentRoot`  
**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))

Si no existe la ruta de acceso, el host no se puede iniciar.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Entorno

Establece el [entorno](xref:fundamentals/environments) de la aplicación.

**Clave**: environment  
**Tipo**: *cadena*  
**Valor predeterminado**: producción  
**Establecer mediante**: `UseEnvironment`  
**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))

El entorno se puede establecer en cualquier valor. Los valores definidos por el marco son `Development`, `Staging` y `Production`. Los valores no distinguen mayúsculas de minúsculas.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

La configuración del generador de la aplicación se crea mediante una llamada a [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) en la implementación de [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureAppConfiguration` usa un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para crear un [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para la aplicación. Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma. La aplicación usa cualquier opción que establece un valor en último lugar. La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) para las operaciones posteriores y en [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Configuración de aplicación de ejemplo con `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> El método de extensión [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por ejemplo, `.AddConfiguration(Configuration.GetSection("section"))`). El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:Logging:LogLevel:Default`). El método `AddConfiguration` espera una coincidencia exacta con las claves de configuración (por ejemplo, `Logging:LogLevel:Default`). La presencia del nombre de sección en las claves evita que los valores de la sección configuren la aplicación. Este problema se corregirá en una versión futura. Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).

Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto. La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento **&lt;Content:&gt;** siguiente:

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación. Se puede llamar varias veces a `ConfigureServices` con resultados de suma.

Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Para obtener más información, vea <xref:fundamentals/host/hosted-services>.

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) agrega un delegado para configurar el [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) proporcionado. Se puede llamar varias veces a `ConfigureLogging` con resultados de suma.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) escucha `Ctrl+C`/SIGINT o SIGTERM y llama a [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) para iniciar el proceso de cierre. `UseConsoleLifetime` desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ya está registrado previamente como la implementación de duración predeterminada. Se usa la última duración registrada.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Configuración del contenedor

Para permitir la conexión a otros contenedores, el host puede aceptar [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.

La configuración personalizada del contenedor está administrada por el método [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer). `ConfigureContainer` proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente. Se puede llamar varias veces a `ConfigureContainer` con resultados de suma.

Crear un contenedor de servicios de la aplicación:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Proporcionar un generador de contenedor de servicio:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Extensibilidad

La extensibilidad de host se realiza con métodos de extensión en `IHostBuilder`. El ejemplo siguiente muestra cómo un método de extensión extiende una implementación de `IHostBuilder` con [RabbitMQ](https://www.rabbitmq.com/). El método de extensión (en otra parte de la aplicación) registra un `IHostedService` de RabbitMQ:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Administración del host

La implementación de [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) es la responsable de iniciar y detener las implementaciones de `IHostedService` que están registradas en el contenedor de servicios.

### <a name="run"></a>Run

[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) inicia la aplicación y devuelve `Task`, que se completa cuando se desencadena el token de cancelación o el cierre:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre `Ctrl+C`/SIGINT o SIGTERM.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Start y StopAsync

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) inicia el host de forma sincrónica.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) intenta detener el host en el tiempo de espera proporcionado.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync y StopAsync

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) inicia la aplicación.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) detiene la aplicación.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) se desencadena mediante [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), como [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (escucha `Ctrl+C`/SIGINT o SIGTERM). `WaitForShutdown` llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) devuelve `Task`, que se completa cuando se desencadena el cierre a través del token determinado y llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Control externo

El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) se llama al inicio de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), que espera hasta que se complete antes de continuar. Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.

## <a name="ihostingenvironment-interface"></a>Interfaz IHostingEnvironment

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje de la aplicación. Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Para obtener más información, vea <xref:fundamentals/environments>.

## <a name="iapplicationlifetime-interface"></a>Interfaz IApplicationLifetime

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable. Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.

| Token de cancelación | Se desencadena cuando&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | El host se ha iniciado completamente. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | El host está completando un apagado estable. Deben procesarse todas las solicitudes. El apagado se bloquea hasta que se complete este evento. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | El host está realizando un apagado estable. Puede que todavía se estén procesando las solicitudes. El apagado se bloquea hasta que se complete este evento. |

Inserción de constructor del servicio `IApplicationLifetime` en cualquier clase. En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de `IHostedService`) para registrar los eventos.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación. La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:

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

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/host/hosted-services>
* [Ejemplos de hospedaje de repositorios en GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
