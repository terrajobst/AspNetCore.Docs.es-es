---
title: Usar varios entornos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la manera en que ASP.NET Core proporciona compatibilidad para controlar el comportamiento de las aplicaciones en varios entornos.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Usar varios entornos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core proporciona compatibilidad para establecer el comportamiento de las aplicaciones en tiempo de ejecución con variables de entorno.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Entornos

ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena ese valor en [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` se puede establecer en cualquier valor, pero el marco de trabajo admite [tres valores](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) y [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Si no se ha establecido `ASPNETCORE_ENVIRONMENT`, el valor predeterminado será `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

El código anterior:

* Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.
* Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:

    * `Staging`
    * `Production`
    * `Staging_2`

La [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Nota: En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas. Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.

### <a name="development"></a>Desarrollo

El entorno de desarrollo puede habilitar características que no deben exponerse en producción. Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.

El entorno para el desarrollo del equipo local se puede establecer en el archivo *Properties\launchSettings.json* del proyecto. Los valores de entorno establecidos en *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.

En el siguiente fragmento de JSON se muestran tres perfiles de un archivo *launchSettings.json*:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> La propiedad `applicationUrl` en *launchSettings.json* puede especificar una lista de direcciones URL del servidor. Use un punto y coma entre las direcciones URL de la lista:
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usará el primer perfil con `"commandName": "Project"`. El valor de `commandName` especifica el servidor web que se va a iniciar. `commandName` puede ser una de las siguientes opciones:

* IIS Express
* IIS
* Project (que inicia Kestrel)

Cuando una aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run):

* Se lee *launchSettings.json*, si está disponible. La configuración de `environmentVariables` de *launchSettings.json* reemplaza las variables de entorno.
* Se muestra el entorno de hospedaje.


En la siguiente salida se muestra una aplicación que se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run):
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

La pestaña **Depurar** de Visual Studio proporciona una GUI para editar el archivo *launchSettings.json*:

![Variables de entorno de configuración de las propiedades del proyecto](environments/_static/project-properties-debug.png)

Los cambios realizados en los perfiles de proyecto podrían no surtir efecto hasta que se reinicie el servidor web. Es necesario reiniciar Kestrel para que detecte los cambios realizados en su entorno.

>[!WARNING]
> En *launchSettings.json* no se deben almacenar secretos. Se puede usar la [herramienta Administrador de secretos](xref:security/app-secrets) a fin de almacenar secretos para el desarrollo local.

### <a name="production"></a>Producción

El entorno de producción debe configurarse para maximizar la seguridad, el rendimiento y la solidez de la aplicación. Las opciones de configuración comunes que difieren de las del entorno de desarrollo son:

* Almacenamiento en caché.
* Los recursos del cliente se agrupan, se reducen y se atienden potencialmente desde una CDN.
* Las páginas de error de diagnóstico están deshabilitadas.
* Las páginas de error descriptivas están habilitadas.
* El registro y la supervisión de la producción están habilitados. Por ejemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Establecimiento del entorno

A menudo resulta útil establecer un entorno específico para la realización de pruebas. Si el entorno no está establecido, el valor predeterminado será `Production`, lo que deshabilita la mayoría de las características de depuración.

El método para establecer el entorno depende del sistema operativo.

### <a name="azure"></a>Azure

Para Azure App Service:

* Seleccione la hoja **Configuración de la aplicación**.
* Agregue la clave y el valor en **Configuración de la aplicación**.


### <a name="windows"></a>Windows
Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, si la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:

**Línea de comandos**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Estos comandos solo tienen efecto en la ventana actual. Cuando se cierre la ventana, la configuración de ASPNETCORE_ENVIRONMENT volverá a la configuración predeterminada o al valor del equipo. Para establecer el valor globalmente en Windows, abra **Panel de Control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`.

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Vea la sección *Establecer variables de entorno* del tema [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Por grupo de aplicaciones de IIS**

Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *AppCmd.exe command* (Comando AppCmd.exe) del tema [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno <environmentVariables>).

### <a name="macos"></a>macOS
Para establecer el entorno actual para macOS, puede hacerlo en línea al ejecutar la aplicación.

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
También puede usar `export` para establecerlo antes de ejecutar la aplicación.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Las variables de entorno de nivel de equipo se establecen en el archivo *.bashrc* o *.bash_profile*. Para editar el archivo, use cualquier editor de texto y agregue la instrucción siguiente.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Para distribuciones de Linux, use el comando `export` en la línea de comandos para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.

### <a name="configuration-by-environment"></a>Configuración de entorno

Para más información, vea [Configuración de entorno](xref:fundamentals/configuration/index#configuration-by-environment).

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Métodos y clase Startup basados en entorno

Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación. Si existe una clase `Startup{EnvironmentName}`, se llamará para ese `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Nota: Al llamar a [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) se invalidan las secciones de configuración.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) son compatibles con versiones específicas del entorno con el formato `Configure{EnvironmentName}` y `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Recursos adicionales

* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Configuración](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
