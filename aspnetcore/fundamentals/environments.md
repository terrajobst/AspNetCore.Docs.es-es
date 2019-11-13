---
title: Usar varios entornos en ASP.NET Core
author: rick-anderson
description: Aprenda a controlar el comportamiento de las aplicaciones en varios entornos en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/environments
ms.openlocfilehash: 91fa2a78e62dff65704a3dda826f45f27bad6064
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634088"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Usar varios entornos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core configura el comportamiento de las aplicaciones en función del entorno en tiempo de ejecución mediante una variable de entorno.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="environments"></a>Entornos

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena el valor en [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName). `ASPNETCORE_ENVIRONMENT` se puede establecer en cualquier valor, pero el marco proporciona tres valores:

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <xref:Microsoft.Extensions.Hosting.Environments.Production> (valor predeterminado)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena el valor en [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName). `ASPNETCORE_ENVIRONMENT` se puede establecer en cualquier valor, pero el marco proporciona tres valores:

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (valor predeterminado)

::: moniker-end

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

El código anterior:

* Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.
* Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:

  * `Staging`
  * `Production`
  * `Staging_2`

El [asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas. Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.

### <a name="development"></a>Desarrollo

El entorno de desarrollo puede habilitar características que no deben exponerse en producción. Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#developer-exception-page) en el entorno de desarrollo.

El entorno para el desarrollo del equipo local se puede establecer en el archivo *Properties\launchSettings.json* del proyecto. Los valores de entorno establecidos en *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.

En el siguiente fragmento de JSON se muestran tres perfiles de un archivo *launchSettings.json*:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

> [!NOTE]
> La propiedad `applicationUrl` en *launchSettings.json* puede especificar una lista de direcciones URL del servidor. Use un punto y coma entre las direcciones URL de la lista:
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usa el primer perfil con `"commandName": "Project"`. El valor de `commandName` especifica el servidor web que se va a iniciar. `commandName` puede ser uno de los siguientes:

* `IISExpress`
* `IIS`
* `Project` (que inicia Kestrel)

Cuando una aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run):

* Se lee *launchSettings.json*, si está disponible. La configuración de `environmentVariables` de *launchSettings.json* reemplaza las variables de entorno.
* Se muestra el entorno de hospedaje.

En la siguiente salida se muestra una aplicación que se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

La pestaña **Depurar** de las propiedades de proyecto de Visual Studio proporciona una GUI para editar el archivo *launchSettings.json*:

![Variables de entorno de configuración de las propiedades del proyecto](environments/_static/project-properties-debug.png)

Los cambios realizados en los perfiles de proyecto podrían no surtir efecto hasta que se reinicie el servidor web. Es necesario reiniciar Kestrel para que pueda detectar los cambios realizados en su entorno.

> [!WARNING]
> En *launchSettings.json* no se deben almacenar secretos. Se puede usar la [herramienta Administrador de secretos](xref:security/app-secrets) a fin de almacenar secretos para el desarrollo local.

Cuando se usa [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json*. En el siguiente ejemplo se establece el entorno en `Development`:

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

Un archivo *.vscode/launch.json* del proyecto no se lee al iniciar la aplicación con `dotnet run` en la misma manera que *Properties/launchSettings.json*. Cuando se inicia una aplicación en desarrollo que no tiene un archivo *launchSettings.json*, establezca el valor del entorno con una variable de entorno o con un argumento de línea de comandos para el comando `dotnet run`.

### <a name="production"></a>Producción

El entorno de producción debe configurarse para maximizar la seguridad, el rendimiento y la solidez de la aplicación. Las opciones de configuración comunes que difieren de las del entorno de desarrollo son:

* Almacenamiento en caché.
* Los recursos del cliente se agrupan, se reducen y se atienden potencialmente desde una CDN.
* Las páginas de error de diagnóstico están deshabilitadas.
* Las páginas de error descriptivas están habilitadas.
* El registro y la supervisión de la producción están habilitados. Por ejemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Establecimiento del entorno

A menudo resulta útil establecer un entorno específico para realizar las pruebas con una variable de entorno o una configuración de plataforma. Si el entorno no está establecido, el valor predeterminado es `Production`, lo que deshabilita la mayoría de las características de depuración. El método para establecer el entorno depende del sistema operativo.

Cuando se compila el host, la última configuración de entorno leída por la aplicación determina el entorno de la aplicación. El entorno de la aplicación no se puede cambiar mientras se ejecuta la aplicación.

### <a name="environment-variable-or-platform-setting"></a>Configuración de plataforma o variable de entorno

#### <a name="azure-app-service"></a>Azure App Service

Para establecer el entorno en [Azure App Service](https://azure.microsoft.com/services/app-service/), realice los pasos siguientes:

1. Seleccione la aplicación desde la hoja **App Services**.
1. En el grupo **CONFIGURACIÓN**, seleccione la hoja **Configuración de la aplicación**.
1. En el área **Configuración de la aplicación**, haga clic en **Agregar nueva configuración**.
1. Para **Escriba un nombre**, proporcione `ASPNETCORE_ENVIRONMENT`. Para **Escriba un valor**, proporcione el entorno (por ejemplo, `Staging`).
1. Active la casilla **Configuración de ranuras** si quiere que la configuración del entorno permanezca con la ranura actual cuando se intercambien las ranuras de implementación. Para obtener más información, consulte [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing) (Documentación de Azure: ¿qué opciones de configuración se intercambian?).
1. Haga clic en **Guardar** en la parte superior de la hoja.

Azure App Service reinicia automáticamente la aplicación después de que se agregue, cambie o elimine una configuración de aplicación (variable de entorno) en Azure Portal.

#### <a name="windows"></a>Windows

Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, cuando la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:

**Símbolo del sistema**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Estos comandos solo tienen efecto en la ventana actual. Cuando se cierre la ventana, la configuración de `ASPNETCORE_ENVIRONMENT` volverá a la configuración predeterminada o al valor del equipo.

Para establecer el valor globalmente en Windows, use cualquiera de los métodos siguientes:

* Abra **Panel de control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`:

  ![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

  ![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)

* Abra un símbolo del sistema con permisos de administrador y use el comando `setx` o abra un símbolo del sistema administrativo de PowerShell y use `[Environment]::SetEnvironmentVariable`:

  **Símbolo del sistema**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  El modificador `/M` indica que hay que establecer la variable de entorno en el nivel de sistema. Si no se usa el modificador `/M`, la variable de entorno se establece para la cuenta de usuario.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  El valor de opción `Machine` indica que hay que establecer la variable de entorno en el nivel de sistema. Si el valor de opción se cambia a `User`, la variable de entorno se establece para la cuenta de usuario.

Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece de forma global, se aplica a `dotnet run` en cualquier ventana de comandos abierta después del establecimiento del valor.

**web.config**

Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` con *web.config*, vea la sección *Establecimiento de variables de entorno* de <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.

::: moniker range=">= aspnetcore-2.2"

**Archivo del proyecto o perfil de publicación**

**Para las implementaciones de IIS de Windows:** Incluya la propiedad `<EnvironmentName>` del perfil de publicación ( *.pubxml*) o un archivo de proyecto. Este método establece el entorno en *web.config* cuando se publica el proyecto:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

**Por grupo de aplicaciones de IIS**

Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` para una aplicación que se ejecuta en un grupo de aplicaciones aislado (se admite en IIS 10.0 o posterior), vea la sección *AppCmd.exe* del tema [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno). Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece para un grupo de aplicaciones, su valor reemplaza a un valor en el nivel de sistema.

> [!IMPORTANT]
> Cuando hospede una aplicación en IIS y agregue o cambie la variable de entorno `ASPNETCORE_ENVIRONMENT`, use cualquiera de los siguientes métodos para que las aplicaciones tomen el nuevo valor:
>
> * Ejecute `net stop was /y` seguido de `net start w3svc` en un símbolo del sistema.
> * Reinicie el servidor.

#### <a name="macos"></a>macOS

Para establecer el entorno actual para macOS, puede hacerlo en línea al ejecutar la aplicación:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

De forma alternativa, defina el entorno con `export` antes de ejecutar la aplicación:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Las variables de entorno de nivel de equipo se establecen en el archivo *.bashrc* o *.bash_profile*. Edite el archivo con cualquier editor de texto. Agregue la siguiente instrucción:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a>Linux

Para distribuciones de Linux, use el comando `export` en un símbolo del sistema para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.

### <a name="set-the-environment-in-code"></a>Establecimiento del entorno en el código

::: moniker range=">= aspnetcore-3.0"

Llame a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> al compilar el host. Vea <xref:fundamentals/host/generic-host#environmentname>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> al compilar el host. Vea <xref:fundamentals/host/web-host#environment>.

::: moniker-end

### <a name="configuration-by-environment"></a>Configuración de entorno

Para cargar la configuración por entorno, se recomienda lo siguiente:

::: moniker range=">= aspnetcore-3.0"

* Archivos *appsettings* (*appsettings.{Entorno}.json*). Vea <xref:fundamentals/configuration/index#json-configuration-provider>.
* Variables de entorno (establecidas en todos los sistemas donde se hospede la aplicación). Vea <xref:fundamentals/host/generic-host#environmentname> y <xref:security/app-secrets#environment-variables>.
* Administrador de secretos (solo en el entorno de desarrollo). Vea <xref:security/app-secrets>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Archivos *appsettings* (*appsettings.{Entorno}.json*). Vea <xref:fundamentals/configuration/index#json-configuration-provider>.
* Variables de entorno (establecidas en todos los sistemas donde se hospede la aplicación). Vea <xref:fundamentals/host/web-host#environment> y <xref:security/app-secrets#environment-variables>.
* Administrador de secretos (solo en el entorno de desarrollo). Vea <xref:security/app-secrets>.

::: moniker-end

## <a name="environment-based-startup-class-and-methods"></a>Métodos y clase Startup basados en entorno

::: moniker range=">= aspnetcore-3.0"

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a>Inserción de IWebHostEnvironment en Startup.Configure

Inserte <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> en `Startup.Configure`. Este enfoque es útil cuando la aplicación solo requiere el ajuste de `Startup.Configure` para algunos entornos con diferencias de código mínimas en cada uno.

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a>Inserción de IWebHostEnvironment en la clase Startup

Inserte <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> en el constructor de `Startup`. Este enfoque es útil cuando la aplicación solo requiere la configuración de `Startup` para algunos entornos con diferencias de código mínimas en cada uno.

En el ejemplo siguiente:

* El entorno se mantiene en el campo `_env`.
* `_env` se usa en `ConfigureServices` y `Configure` para aplicar la configuración de inicio en función del entorno de la aplicación.

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="inject-ihostingenvironment-into-startupconfigure"></a>Inserción de IHostingEnvironment en Startup.Configure

Inserte <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> en `Startup.Configure`. Este enfoque es útil cuando la aplicación solo requiere la configuración de `Startup.Configure` para algunos entornos con diferencias de código mínimas en cada uno.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a>Inserción de IHostingEnvironment en la clase Startup

Inserte <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> en el constructor `Startup` y asigne el servicio a un campo para su uso en la clase `Startup`. Este enfoque es útil cuando la aplicación solo requiere configurar el inicio para algunos entornos con diferencias de código mínimas en cada uno.

En el ejemplo siguiente:

* El entorno se mantiene en el campo `_env`.
* `_env` se usa en `ConfigureServices` y `Configure` para aplicar la configuración de inicio en función del entorno de la aplicación.

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

::: moniker-end

### <a name="startup-class-conventions"></a>Convenciones de la clase Startup

Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación. La aplicación puede definir clases `Startup` independientes para distintos entornos (por ejemplo, `StartupDevelopment`). La clase `Startup` correspondiente se selecciona en tiempo de ejecución. La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria. Si no se encuentra una clase `Startup{EnvironmentName}` coincidente, se usa la clase `Startup`. Este enfoque es útil cuando la aplicación requiere configurar el inicio para algunos entornos con muchas diferencias de código en cada uno.

Para implementar clases `Startup` basadas en entornos, cree una clase `Startup{EnvironmentName}` para cada entorno en uso y una clase `Startup` de reserva:

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

Use la sobrecarga [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) que acepta un nombre de ensamblado:

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a>Convenciones del método Startup

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) son compatibles con versiones específicas del entorno con el formato `Configure<EnvironmentName>` y `Configure<EnvironmentName>Services`. Este enfoque es útil cuando la aplicación requiere configurar el inicio para algunos entornos con muchas diferencias de código en cada uno.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
