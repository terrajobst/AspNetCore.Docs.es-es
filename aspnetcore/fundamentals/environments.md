---
title: Usar varios entornos en ASP.NET Core
author: rick-anderson
description: Aprenda a controlar el comportamiento de las aplicaciones en varios entornos en aplicaciones ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433953"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Usar varios entornos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core configura el comportamiento de las aplicaciones en función del entorno en tiempo de ejecución mediante una variable de entorno.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Entornos

ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena el valor en [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Puede establecer `ASPNETCORE_ENVIRONMENT` en cualquier valor, pero el marco de trabajo admite [tres valores](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) y [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Si no se establece `ASPNETCORE_ENVIRONMENT`, el valor predeterminado es `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

El código anterior:

* Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) y [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.
* Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:

    * `Staging`
    * `Production`
    * `Staging_2`

La [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas. Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.

### <a name="development"></a>Desarrollo

El entorno de desarrollo puede habilitar características que no deben exponerse en producción. Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.

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

::: moniker range=">= aspnetcore-2.1"

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

::: moniker-end

Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usa el primer perfil con `"commandName": "Project"`. El valor de `commandName` especifica el servidor web que se va a iniciar. `commandName` puede ser uno de los siguientes:

* IIS Express
* IIS
* Project (que inicia Kestrel)

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

A menudo resulta útil establecer un entorno específico para la realización de pruebas. Si el entorno no está establecido, el valor predeterminado es `Production`, lo que deshabilita la mayoría de las características de depuración. El método para establecer el entorno depende del sistema operativo.

### <a name="azure-app-service"></a>Azure App Service

Para establecer el entorno en [Azure App Service](https://azure.microsoft.com/services/app-service/), realice los pasos siguientes:

1. Seleccione la aplicación desde la hoja **App Services**.
1. En el grupo **CONFIGURACIÓN**, seleccione la hoja **Configuración de la aplicación**.
1. En el área **Configuración de la aplicación**, haga clic en **Agregar nueva configuración**.
1. Para **Escriba un nombre**, proporcione `ASPNETCORE_ENVIRONMENT`. Para **Escriba un valor**, proporcione el entorno (por ejemplo, `Staging`).
1. Active la casilla **Configuración de ranuras** si quiere que la configuración del entorno permanezca con la ranura actual cuando se intercambien las ranuras de implementación. Para obtener más información, vea [Configuración de entornos de ensayo en Azure App Service](/azure/app-service/web-sites-staged-publishing).
1. Haga clic en **Guardar** en la parte superior de la hoja.

Azure App Service reinicia automáticamente la aplicación después de que se agregue, cambie o elimine una configuración de aplicación (variable de entorno) en Azure Portal.

### <a name="windows"></a>Windows

Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, cuando la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:

**Símbolo del sistema**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Estos comandos solo tienen efecto en la ventana actual. Cuando se cierre la ventana, la configuración de `ASPNETCORE_ENVIRONMENT` volverá a la configuración predeterminada o al valor del equipo. Para establecer el valor globalmente en Windows, abra **Panel de Control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`:

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)

**web.config**

Vea la sección *Establecer variables de entorno* del tema [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Por grupo de aplicaciones de IIS**

Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *AppCmd.exe command* (Comando AppCmd.exe) del tema [Environment Variables &lt;environmentVariables>&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno <environmentVariables>).

### <a name="macos"></a>macOS

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

### <a name="linux"></a>Linux

Para distribuciones de Linux, use el comando `export` en un símbolo del sistema para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.

### <a name="configuration-by-environment"></a>Configuración de entorno

Para más información, vea [Configuración de entorno](xref:fundamentals/configuration/index#configuration-by-environment).

## <a name="environment-based-startup-class-and-methods"></a>Métodos y clase Startup basados en entorno

Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación. Si existe una clase `Startup{EnvironmentName}`, se llama para ese `EnvironmentName`:

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) invalida las secciones de configuración.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) son compatibles con versiones específicas del entorno con el formato `Configure{EnvironmentName}` y `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
