---
title: Trabajar con varios entornos de ASP.NET Core
author: ardalis
description: "Obtenga información acerca de cómo ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos."
keywords: "Núcleo de ASP.NET, la configuración del entorno, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 2a92c50085e70b4a505913c86348ba5fe54f6d13
ms.sourcegitcommit: 67811da1278c75cb10994602c13bd5adec3f0907
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2017
---
# <a name="working-with-multiple-environments"></a>Trabajar con varios entornos

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos, como desarrollo, ensayo y producción. Las variables de entorno se utilizan para indicar el entorno en tiempo de ejecución, que permite a la aplicación que se configurará para ese entorno.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Desarrollo, prueba, producción

ASP.NET Core hace referencia a una variable de entorno en particular, `ASPNETCORE_ENVIRONMENT` para describir el entorno se está ejecutando actualmente en la aplicación. Esta variable se puede establecer en cualquier valor que desee, pero por convención, se utilizan tres valores: `Development`, `Staging`, y `Production`. Encontrará estos valores utilizados en los ejemplos y las plantillas proporcionadas con ASP.NET Core.

Se pueden detectar mediante programación la configuración del entorno actual desde dentro de la aplicación. Además, puede usar el entorno de [etiqueta auxiliar](../mvc/views/tag-helpers/index.md) para incluir determinadas secciones en su [vista](../mvc/views/index.md) según el entorno de aplicación actual.

Nota: En Windows y Mac OS, distingue mayúsculas de minúsculas del nombre de entorno especificado. Si se establece la variable `Development` o `development` o `DEVELOPMENT` los resultados serán los mismos. Sin embargo, Linux es un **entre mayúsculas y minúsculas** sistema operativo de forma predeterminada. Las variables de entorno, los nombres de archivo y configuración requiere entre mayúsculas y minúsculas.

### <a name="development"></a>Desarrollo

Debe ser el entorno que se utiliza al desarrollar una aplicación. Normalmente se utiliza para habilitar las características que no desea que estén disponibles cuando se ejecute la aplicación en producción, como el [página de excepción para desarrolladores](xref:fundamentals/error-handling#the-developer-exception-page).

Si está utilizando Visual Studio, el entorno se puede configurar en perfiles de depuración del proyecto. Depurar perfiles especificar el [server](xref:fundamentals/servers/index) a utilizar al iniciar la aplicación y las variables de entorno para establecerse. El proyecto puede tener varios perfiles de depuración que establecen variables de entorno de manera diferente. Estos perfiles se administran mediante el **depurar** ficha de su proyecto de aplicación web **propiedades** menú. Se conservan los valores establecidos en Propiedades del proyecto en el *launchSettings.json* archivo y también puede configurar perfiles editando directamente el archivo.

El perfil de IIS Express se muestra aquí:

![Variables de entorno de configuración de propiedades de proyecto](environments/_static/project-properties-debug.png)

Este es un `launchSettings.json` archivo que incluye los perfiles para `Development` y `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

Podrán que los cambios realizados en los perfiles de los proyectos no surtan efecto hasta que se reinicie el servidor web usado (en particular, Kestrel debe reiniciarse para que se detectarán los cambios realizados en su entorno).

>[!WARNING]
> Las variables de entorno se almacenan en *launchSettings.json* no están protegidos de cualquier forma y va a formar parte del repositorio de código fuente para el proyecto, si utiliza uno. **Nunca almacenan credenciales ni otros datos secretos en este archivo.** Si necesita un lugar para almacenar estos datos, use la *Manager secreto* herramienta se describe en [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](../security/app-secrets.md#security-app-secrets).

### <a name="staging"></a>Almacenamiento provisional

Por convención, un `Staging` entorno es un entorno de preproducción utilizado para la prueba final antes de la implementación en producción. Idealmente, sus características físicas deben reflejar de producción, para que se produzcan los problemas que pueden surgir en producción primero en el entorno de ensayo, donde pueden tratarse sin afectar a los usuarios.

### <a name="production"></a>Producción

El `Production` entorno es el entorno en el que se ejecuta la aplicación cuando está activa y que se va a utilizar los usuarios finales. Este entorno debe configurarse para maximizar la seguridad, rendimiento y eficacia de la aplicación. Algunas configuraciones comunes que podría tener un entorno de producción que serían diferentes entornos de desarrollo incluyen:

* Activar el almacenamiento en caché

* Asegúrese de todos los recursos de cliente están agrupados, reducidos y potencialmente provienen de una CDN

* Desactivar ErrorPages diagnóstico

* Activar las páginas de error descriptivo

* Habilitar la supervisión y el registro de producción (por ejemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

Esto es no pretende ser una lista completa. Es mejor evitar esparcirlo comprobaciones de entorno en muchas partes de la aplicación. En su lugar, el enfoque recomendado es realizar tales comprobaciones dentro de la aplicación `Startup` clases siempre que sea posible

## <a name="setting-the-environment"></a>Configurar el entorno

El método para establecer el entorno depende del sistema operativo.

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

**Web.config**

Consulte la *establecer variables de entorno* sección de la [referencia de configuración de ASP.NET Core módulo](xref:hosting/aspnet-core-module#setting-environment-variables) tema.

**Por grupo de aplicaciones de IIS**

Si tiene que establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *Comando AppCmd.exe* del tema [Variables de entorno \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) de la documentación de referencia de IIS.

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

## <a name="determining-the-environment-at-runtime"></a>Determinar el entorno en tiempo de ejecución

El `IHostingEnvironment` servicio proporciona la abstracción básica para trabajar con entornos. Este servicio es proporcionado por ASP.NET hospeda las capas y puede insertarse en la lógica de inicio a través de [inyección de dependencia](dependency-injection.md). La plantilla de sitio web de ASP.NET Core en Visual Studio usa este enfoque para cargar los archivos de configuración específicos del entorno (si existe) y para personalizar la configuración de control de errores de la aplicación. En ambos casos, este comportamiento se logra consultando el entorno especificado actualmente mediante una llamada a `EnvironmentName` o `IsEnvironment` en la instancia de `IHostingEnvironment` pasada en el método adecuado.

> [!NOTE]
> Si tiene que comprobar si la aplicación se ejecuta en un entorno concreto, use `env.IsEnvironment("environmentname")` desde correctamente omitirá caso (en lugar de comprobar si `env.EnvironmentName == "Development"` por ejemplo).

Por ejemplo, puede usar el siguiente código en el método de configuración para configurar el control de errores específicos de entorno:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Si la aplicación se ejecuta en un `Development` entorno, a continuación, se habilita la compatibilidad en tiempo de ejecución necesaria para utilizar la característica de "BrowserLink" en Visual Studio, las páginas de error específico de desarrollo (que normalmente no se deben ejecutar en producción) y error especiales de la base de datos páginas (que proporcionan una manera de aplicar las migraciones y, por tanto, solo deben usarse en el desarrollo). En caso contrario, si la aplicación no se está ejecutando en un entorno de desarrollo, una página de control de errores estándar está configurada para mostrarse en respuesta a las excepciones no controladas.

Debe determinar qué contenido para enviar al cliente en tiempo de ejecución, dependiendo del entorno actual. Por ejemplo, en un entorno de desarrollo generalmente actúan no minimiza las secuencias de comandos y hojas de estilos, lo que simplifica la depuración. Entornos de prueba y producción deben servir las versiones reducidas y genera normalmente a partir de una CDN. Puede hacerlo mediante el entorno de [auxiliar de etiquetas](../mvc/views/tag-helpers/intro.md). La aplicación auxiliar de etiquetas de entorno solo representará su contenido si el entorno actual coincide con uno de los entornos especificados mediante el `names` atributo.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Para empezar a trabajar con el uso de aplicaciones auxiliares de etiquetas en la aplicación, vea [Introducción a las aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Convenciones de inicio

ASP.NET Core es compatible con un enfoque basado en la convención a la configuración de inicio de la aplicación según el entorno actual. Mediante programación, puede controlar cómo comportará su aplicación, según a qué entorno está en, lo que le permite crear y administrar sus propias convenciones.

Cuando se inicia una aplicación de ASP.NET Core, el `Startup` clase se utiliza para arrancar la aplicación, cargar su configuración, etc. ([más información acerca del inicio de ASP.NET](startup.md)). Sin embargo, si existe una clase denominada `Startup{EnvironmentName}` (por ejemplo `StartupDevelopment`) y el `ASPNETCORE_ENVIRONMENT` variable de entorno coincide con ese nombre, a continuación, que `Startup` clase se utiliza en su lugar. Por lo tanto, puede configurar `Startup` para el desarrollo, pero tiene otro `StartupProduction` que se utilizará cuando se ejecute la aplicación en producción. O viceversa.

> [!NOTE]
> Al llamar a `WebHostBuilder.UseStartup<TStartup>()` invalida las secciones de configuración.

Además de usar un totalmente independientes `Startup` clase basándose en el entorno actual, también puede realizar ajustes a cómo la aplicación está configurada en un `Startup` clase. El `Configure()` y `ConfigureServices()` métodos admiten versiones específicas del entorno similar a la `Startup` propia del formulario de clase `Configure{EnvironmentName}()` y `Configure{EnvironmentName}Services()`. Si se define un método `ConfigureDevelopment()` se llamará en lugar de `Configure()` cuando se establece el entorno de desarrollo. Del mismo modo, `ConfigureDevelopmentServices()` se denominaría en lugar de `ConfigureServices()` en el mismo entorno.

## <a name="summary"></a>Resumen

Núcleo de ASP.NET proporciona una serie de características y las convenciones que permiten a los desarrolladores controlar fácilmente el comportamiento de sus aplicaciones en entornos diferentes. Al publicar una aplicación desde el desarrollo hasta el ensayo a producción, conjunto de variables de entorno correctamente para el entorno de permitir para la optimización de la aplicación para fines de depuración, pruebas o producción, según corresponda.

## <a name="additional-resources"></a>Recursos adicionales

* [Configuración](configuration.md)

* [Introducción a las aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/intro.md)
