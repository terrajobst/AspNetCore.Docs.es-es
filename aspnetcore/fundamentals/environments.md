---
title: Trabajar con varios entornos de ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de cómo ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos."
keywords: "Núcleo de ASP.NET, la configuración del entorno, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a>Trabajar con varios entornos

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core proporciona compatibilidad para establecer el comportamiento de la aplicación en tiempo de ejecución con las variables de entorno.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Entornos

ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena ese valor en [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT`se puede establecer en cualquier valor, pero [tres valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) son compatibles con el marco de trabajo: [desarrollo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [ensayo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), y [producción](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Si `ASPNETCORE_ENVIRONMENT` no se establece, el valor predeterminado será `Production`.

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

El código anterior:

* Llamadas [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.
* Llamadas [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando el valor de `ASPNETCORE_ENVIRONMENT` se establece uno de los siguientes:

    * `Staging`
    * `Production`
    * `Staging_2`

El [auxiliar de etiquetas de entorno ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utiliza el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

Nota: En Windows y Mac OS, valores y variables de entorno no son distinguen mayúsculas de minúsculas. Las variables de entorno de Linux y los valores son **entre mayúsculas y minúsculas** de forma predeterminada.

### <a name="development"></a>Desarrollo

El entorno de desarrollo puede habilitar las características que no deben exponerse en producción. Por ejemplo, las plantillas de ASP.NET Core permiten la [página de excepción para desarrolladores](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.

El entorno de desarrollo de equipo local se puede establecer el *Properties\launchSettings.json* archivo del proyecto. Los valores de entorno establecer *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.

El siguiente XML muestra tres perfiles de un *launchSettings.json* archivo:

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

Cuando se inicia la aplicación con `dotnet run`, el primer perfil con `"commandName": "Project"` se usará. El valor de `commandName` especifica el servidor web para iniciar. `commandName`puede ser uno de:

* IIS Express
* IIS
* Proyecto (que inicia Kestrel)

Cuando se inicia una aplicación con `dotnet run`:

* *launchSettings.json* se lee si está disponible. `environmentVariables`configuración de *launchSettings.json* reemplazan a las variables de entorno.
* Se muestra el entorno de hospedaje.


La siguiente salida muestra una aplicación que se inició con `dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio **depurar** ficha proporciona una interfaz gráfica de usuario para editar la *launchSettings.json* archivo:

![Variables de entorno de configuración de propiedades de proyecto](environments/_static/project-properties-debug.png)

Los cambios realizados en los perfiles de los proyectos podrán no surtan efecto hasta que se reinicie el servidor web. Kestrel debe reiniciarse para que se detectarán los cambios realizados en su entorno.

>[!WARNING]
> *launchSettings.json* no debería almacenar secretos. El [herramienta Administrador de secreto](xref:security/app-secrets) puede usarse para almacenar secretos de desarrollo local.

### <a name="production"></a>Producción

El entorno de producción debe configurarse para maximizar la seguridad, rendimiento y eficacia de la aplicación. Algunas configuraciones comunes que podría tener un entorno de producción que serían diferentes entornos de desarrollo incluyen:

* Almacenamiento en caché.
* Recursos del cliente están agrupados, reducidos y potencialmente provienen de una CDN.
* Páginas de error de diagnóstico deshabilitadas.
* Páginas de error descriptivo habilitadas.
* El registro de producción y supervisión habilitada. Por ejemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).

## <a name="setting-the-environment"></a>Configurar el entorno

A menudo resulta útil establecer un entorno de pruebas específico. Si no está configurado el entorno, serán `Production` que deshabilita la mayoría de características de depuración.

El método para establecer el entorno depende del sistema operativo.

### <a name="azure"></a>Azure

Para el servicio de aplicaciones de Azure:

* Seleccione el **configuración de la aplicación** hoja.
* Agregar la clave y el valor de **configuración de la aplicación**.


### <a name="windows"></a>Windows
Para establecer el `ASPNETCORE_ENVIRONMENT` para la sesión actual, si la aplicación se inicia con `dotnet run`, se utilizan los siguientes comandos

**Línea de comandos**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Estos comandos sólo tendrán efecto en la ventana actual. Cuando se cierra la ventana, la configuración de ASPNETCORE_ENVIRONMENT vuelve a la configuración predeterminada o el valor de la máquina. Para establecer el valor global en ventanas abiertas del **el Panel de Control** > **System** > **configuración avanzada del sistema** y agregar o editar la `ASPNETCORE_ENVIRONMENT` valor.

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Consulte la *establecer variables de entorno* sección de la [referencia de configuración de ASP.NET Core módulo](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tema.

**Por grupo de aplicaciones de IIS**

Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aisladas (se admite en IIS 10.0 +), consulte el *comandos AppCmd.exe* sección de la [Variables de entorno \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tema.

### <a name="macos"></a>macOS
Configurar el entorno actual para macOS puede realizarse en línea cuando se ejecuta la aplicación;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
o con `export` establecer antes de ejecutar la aplicación.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Las variables de entorno de nivel de equipo se establecen en los *.bashrc* o *. bash_profile* archivo. Edite el archivo con cualquier editor de texto y agregue la siguiente instrucción.

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Para distribuciones de Linux, use la `export` comando en la línea de comandos para la configuración de la variable basado en sesiones y *bash_profile* archivo de configuración del entorno de nivel de máquina.

### <a name="configuration-by-environment"></a>Configuración de entorno

Vea [configuración entorno](xref:fundamentals/configuration/index#configuration-by-environment) para obtener más información.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Entorno en función de métodos y la clase de inicio

Cuando se inicia una aplicación de ASP.NET Core, el [clase inicio](xref:fundamentals/startup) ejecuta un bootstrap la aplicación. Si una clase `Startup{EnvironmentName}` existe, que se llamará para esa clase `EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Nota: Al llamar a [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) invalida las secciones de configuración.

[Configurar](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) compatible con versiones específicas del entorno del formulario `Configure{EnvironmentName}` y `Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Recursos adicionales

* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Configuración](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
