---
title: Host genérico de .NET
author: guardrex
description: Obtenga información sobre el host genérico en .NET, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 9943c9dd2d6dd67a79186ee880b181a5915d06be
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505718"
---
# <a name="net-generic-host"></a>Host genérico de .NET

Por [Luke Latham](https://github.com/guardrex)

Las aplicaciones de .NET Core configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. En este tema se trata el host genérico de ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), muy útil para hospedar aplicaciones que no procesan solicitudes HTTP. Para la cobertura del host de web (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), consulte <xref:fundamentals/host/web-host>.

El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una variedad de escenarios de host. Se trata de la mensajería, tareas en segundo plano y otras cargas de trabajo no HTTP basadas en la ventaja de host genérico de capacidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.

El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web. Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host). El host genérico se está desarrollando para reemplazar el host web en una próxima versión y actuar como la API del host principal tanto en escenarios que sean del tipo HTTP como los que no.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*. No ejecute el ejemplo en `internalConsole`.

Para establecer la consola en Visual Studio Code:

1. Abra el archivo *.vscode/launch.json*.
1. En la configuración de **.NET Core Launch (console)**, busque la entrada **console**. Establezca el valor en `externalTerminal` o `integratedTerminal`.

## <a name="introduction"></a>Introducción

La biblioteca de host genérico está disponible en el espacio de nombres <xref:Microsoft.Extensions.Hosting> y la proporciona el paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).

<xref:Microsoft.Extensions.Hosting.IHostedService> es el punto de entrada para la ejecución de código. Cada implementación de `IHostedService` se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices). Se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> en cada `IHostedService` cuando se inicia el host, y se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> en orden inverso del registro cuando el host se cierra de forma estable.

## <a name="set-up-a-host"></a>Configuración de un host

<xref:Microsoft.Extensions.Hosting.IHostBuilder> es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a>Servicios predeterminados

Estos son los servicios que se registran durante la inicialización del host:

* [Entorno](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [Configuración](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [Opciones](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Registro](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Configuración de host

La configuración del host se crea:

* Llamando a métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder> para establecer la [raíz del contenido](#content-root) y el [entorno](#environment).
* Leyendo la configuración desde los proveedores de configuración de <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.

### <a name="extension-methods"></a>Métodos de extensión

### <a name="application-key-name"></a>Clave de aplicación (nombre)

La propiedad [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) se establece desde la configuración del host durante la construcción de este. Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):

**Clave**: nombreDeAplicación  
**Tipo**: *cadena*  
**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.  
**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Variable de entorno**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))

### <a name="content-root"></a>Raíz del contenido

Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.

**Clave**: contentRoot  
**Tipo**: *cadena*  
**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.  
**Establecer mediante**: `UseContentRoot`  
**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))

Si no existe la ruta de acceso, el host no se puede iniciar.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>Entorno

Establece el [entorno](xref:fundamentals/environments) de la aplicación.

**Clave**: environment  
**Tipo**: *cadena*  
**Valor predeterminado**: producción  
**Establecer mediante**: `UseEnvironment`  
**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))

El entorno se puede establecer en cualquier valor. Los valores definidos por el marco son `Development`, `Staging` y `Production`. Los valores no distinguen mayúsculas de minúsculas.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

`ConfigureHostConfiguration` usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para el host. La configuración del host se usa para inicializar el <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> para su uso en el proceso de compilación de la aplicación.

Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma. El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.

La configuración del host fluye automáticamente a la configuración de la aplicación ([ConfigureAppConfiguration](#configureappconfiguration) y al resto de la aplicación).

De forma predeterminada no se incluye ningún proveedor. Debe especificar de forma explícita los proveedores de configuración que requiere la aplicación en `ConfigureHostConfiguration`, así como:

* La configuración de archivo (por ejemplo, de un archivo *hostsettings.json*).
* La configuración de la variable de entorno.
* La configuración del argumento de la línea de comandos.
* Otros proveedores de configuración necesarios.

La configuración de archivo del host se habilita especificando la ruta base de la aplicación con `SetBasePath` seguido de una llamada a uno de los [proveedores de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider). La aplicación de ejemplo usa un archivo JSON, *hostsettings.json*, y llama a <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> para consumir los valores de configuración del host del archivo.

Para agregar una [configuración de la variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) del host, llame a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en el generador del host. `AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario. La aplicación de ejemplo usa el prefijo `PREFIX_`. El prefijo se quita cuando se leen las variables de entorno. Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.

Durante el desarrollo, al usar [Visual Studio](https://www.visualstudio.com/) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*. En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo. Para obtener más información, vea <xref:fundamentals/environments>.

La [configuración de la línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) se agrega llamando a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. La configuración de la línea de comandos se agrega en último lugar para permitir que los argumentos de la línea de comandos invaliden la configuración proporcionada por los proveedores de configuración anteriores.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Se puede proporcionar una configuración adicional con las claves [applicationName](#application-key-name) y [contentRoot](#content-root).

Configuración de `HostBuilder` de ejemplo con `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en la implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder>. `ConfigureAppConfiguration` usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para la aplicación. Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma. La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada. La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y en <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

La configuración de la aplicación recibe automáticamente la configuración del host proporcionada por [ConfigureHostConfiguration](#configurehostconfiguration).

Configuración de aplicación de ejemplo con `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto. La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento `<Content>` siguiente:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación. Se puede llamar varias veces a `ConfigureServices` con resultados de suma.

Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>. Para obtener más información, vea <xref:fundamentals/host/hosted-services>.

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> agrega un delegado para configurar el <xref:Microsoft.Extensions.Logging.ILoggingBuilder> proporcionado. Se puede llamar varias veces a `ConfigureLogging` con resultados de suma.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> escucha `Ctrl+C`/SIGINT o SIGTERM y llama a <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para iniciar el proceso de cierre. `UseConsoleLifetime` desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync). <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> ya está registrado previamente como la implementación de duración predeterminada. Se usa la última duración registrada.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Configuración del contenedor

Para permitir la conexión a otros contenedores, el host puede aceptar <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>. Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.

La configuración personalizada del contenedor está administrada por el método <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>. `ConfigureContainer` proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente. Se puede llamar varias veces a `ConfigureContainer` con resultados de suma.

Crear un contenedor de servicios de la aplicación:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Proporcionar un generador de contenedor de servicio:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Extensibilidad

La extensibilidad de host se realiza con métodos de extensión en `IHostBuilder`. En el ejemplo siguiente se muestra cómo un método de extensión extiende una implementación de `IHostBuilder` con el ejemplo [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) demostrado en <xref:fundamentals/host/hosted-services>.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Una aplicación establece el método de extensión `UseHostedService` para registrar el servicio hospedado pasado en `T`:

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a>Administración del host

La implementación de <xref:Microsoft.Extensions.Hosting.IHost> es la responsable de iniciar y detener las implementaciones de `IHostedService` que están registradas en el contenedor de servicios.

### <a name="run"></a>Run

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve `Task`, que se completa cuando se desencadena el token de cancelación o el cierre:

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

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre `Ctrl+C`/SIGINT o SIGTERM.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.

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

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia la aplicación.

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> detiene la aplicación.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> se desencadena mediante <xref:Microsoft.Extensions.Hosting.IHostLifetime>, como, por ejemplo, <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (escucha `Ctrl+C`/SIGINT o SIGTERM). `WaitForShutdown` llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve `Task`, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, que espera hasta que se complete antes de continuar. Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.

## <a name="ihostingenvironment-interface"></a>Interfaz IHostingEnvironment

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona información sobre el entorno de hospedaje de la aplicación. Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:

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

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable. Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.

| Token de cancelación | Se desencadena cuando&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | El host se ha iniciado completamente. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | El host está completando un apagado estable. Deben procesarse todas las solicitudes. El apagado se bloquea hasta que se complete este evento. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | El host está realizando un apagado estable. Puede que todavía se estén procesando las solicitudes. El apagado se bloquea hasta que se complete este evento. |

Inserción de constructor del servicio `IApplicationLifetime` en cualquier clase. En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de `IHostedService`) para registrar los eventos.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> solicita la terminación de la aplicación. La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:

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
