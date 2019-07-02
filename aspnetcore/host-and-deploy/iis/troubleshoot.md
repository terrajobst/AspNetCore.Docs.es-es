---
title: Solución de problemas de ASP.NET Core en IIS
author: guardrex
description: Aprenda a diagnosticar problemas con las implementaciones de Internet Information Services (IIS) de aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313650"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Solución de problemas de ASP.NET Core en IIS

Por [Luke Latham](https://github.com/guardrex)

En este artículo se proporcionan instrucciones sobre cómo diagnosticar un problema de inicio de ASP.NET Core al hospedarse con [Internet Information Services (IIS)](/iis). La información de este artículo se aplica al hospedaje en IIS en Windows Server y el escritorio de Windows.

Temas adicionales de solución de problemas:

* Azure App Service también usa el [módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e IIS para hospedar aplicaciones. Para ver consejos de solución de problemas pertenecientes específicamente a Azure App Service, consulte <xref:host-and-deploy/azure-apps/troubleshoot>.
* En <xref:fundamentals/error-handling> se explica cómo controlar los errores de aplicaciones de ASP.NET Core durante el desarrollo en un sistema local.
* En [Información sobre cómo depurar con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) se presentan las características del depurador de Visual Studio.
* En [Depuración con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) se describe la compatibilidad de depuración integrada en Visual Studio Code.

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a>Solución de problemas de errores de inicio de la aplicación

### <a name="enable-the-aspnet-core-module-debug-log"></a>Habilitar el registro de depuración del módulo de ASP.NET Core

Agregue la siguiente configuración de controlador al archivo *web.config* de la aplicación para habilitar los registros de depuración del módulo ASP.NET Core:

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

Confirme que la ruta de acceso especificada para el registro exista y que la identidad del grupo de aplicaciones tenga permisos de escritura en la ubicación.

### <a name="application-event-log"></a>Registro de eventos de aplicación

Acceda al registro de eventos de la aplicación:

1. Abra el menú Inicio, busque **Visor de eventos** y luego seleccione la aplicación **Visor de eventos**.
1. En **Visor de eventos**, abra el nodo **Registros de Windows**.
1. Seleccione **Aplicación** para abrir el registro de eventos de la aplicación.
1. Busque los errores asociados a la aplicación objeto del error. Los errores tienen un valor de *Módulo AspNetCore de IIS* o *Módulo AspNetCore de IIS Express* en la columna *Origen*.

### <a name="run-the-app-at-a-command-prompt"></a>Ejecución de la aplicación en un símbolo del sistema

Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación. La causa de algunos errores se puede encontrar mediante la ejecución de la aplicación en un símbolo del sistema en el sistema de hospedaje.

#### <a name="framework-dependent-deployment"></a>Implementación dependiente de marco de trabajo

Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. En un símbolo del sistema, vaya a la carpeta de implementación y ejecute la aplicación mediante la ejecución del ensamblado de la aplicación con *dotnet.exe*. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `dotnet .\<assembly_name>.dll`.
1. La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.
1. Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel. Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.

#### <a name="self-contained-deployment"></a>Implementación autocontenida

Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):

1. En un símbolo del sistema, vaya a la carpeta de implementación y ejecute el archivo ejecutable de la aplicación. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `<assembly_name>.exe`.
1. La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.
1. Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel. Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.

### <a name="aspnet-core-module-stdout-log"></a>Registro de stdout del módulo ASP.NET Core

Para habilitar y ver los registros de stdout:

1. Vaya a la carpeta de implementación del sitio en el sistema de hospedaje.
1. Si la carpeta *Logs* no existe, cree la carpeta. Para obtener instrucciones sobre cómo habilitar MSBuild para crear la carpeta *logs* automáticamente en la implementación, consulte el tema [Estructura de directorios](xref:host-and-deploy/directory-structure).
1. Edite el archivo *web.config*. Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** para que apunte a la carpeta *logs* (por ejemplo, `.\logs\stdout`). `stdout` en la ruta de acceso es el prefijo del nombre del archivo de registro. Cuando se crea el registro, se agregan automáticamente una marca de tiempo, un identificador de proceso y una extensión de archivo. Cuando se usa `stdout` como prefijo para el nombre de archivo, un archivo de registro se llama normalmente *stdout_20180205184032_5412.log*.
1. Asegúrese de que la identidad del grupo de aplicaciones tiene permisos de escritura en la carpeta *logs*.
1. Guarde el archivo *web.config* actualizado.
1. Realice una solicitud a la aplicación.
1. Vaya a la carpeta *logs*. Busque y abra el registro más reciente de stdout.
1. Estudie el registro para ver los errores.

> [!IMPORTANT]
> Deshabilite el registro de stdout cuando la solución de problemas haya finalizado.

1. Edite el archivo *web.config*.
1. Establezca **stdoutLogEnabled** en `false`.
1. Guarde el archivo.

> [!WARNING]
> La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor. No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.
>
> Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enable-the-developer-exception-page"></a>Habilitación de la página de excepciones para el desarrollador

La variable de entorno `ASPNETCORE_ENVIRONMENT` [ se puede agregar a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo. Siempre y cuando el entorno no se invalide al inicio de la aplicación con `UseEnvironment` en el generador de host, la configuración de la variable de entorno permite que aparezca la [página de excepciones del desarrollador](xref:fundamentals/error-handling) cuando se ejecuta la aplicación.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` cuando se use en servidores de ensayo o pruebas que no estén expuestos a Internet. Quite la variable de entorno del archivo *web.config* cuando termine de solucionar los problemas. Para información sobre la configuración de variables de entorno en *web.config*, consulte el [elemento secundario environmentVariables de aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Errores comunes de inicio

Vea <xref:host-and-deploy/azure-iis-errors-reference>. La mayoría de los problemas comunes que impiden el inicio de la aplicación se tratan en el tema de referencia.

## <a name="obtain-data-from-an-app"></a>Obtención de datos de una aplicación

Si una aplicación es capaz de responder a solicitudes, obtenga solicitudes, conexiones y datos adicionales de la aplicación mediante el middleware en línea del terminal. Para obtener más información y un código de ejemplo, vea <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="create-a-dump"></a>Creación de un volcado de memoria

Un *volcado de memoria* es una instantánea de la memoria del sistema que puede ayudar a determinar la causa de un bloqueo de la aplicación, un error de inicio o una aplicación lenta.

### <a name="app-crashes-or-encounters-an-exception"></a>Bloqueo o excepción de la aplicación

Obtenga y analice un volcado de memoria en [Informe de errores de Windows (WER)](/windows/desktop/wer/windows-error-reporting):

1. Cree una carpeta para almacenar los archivos de volcado de memoria en `c:\dumps`. El grupo de aplicaciones debe tener acceso de escritura a la carpeta.
1. Ejecute el [script EnableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):
   * Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. Ejecute la aplicación en las condiciones que hacen que se produzca el bloqueo.
1. Una vez que se haya producido el bloqueo, ejecute el [script DisableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):
   * Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:

     ```console
     .\DisableDumps w3wp.exe
     ```

   * Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:

     ```console
     .\DisableDumps dotnet.exe
     ```

Después de que se bloquee la aplicación y se complete la recopilación del volcado de memoria, la aplicación puede finalizar con normalidad. El script de PowerShell configura WER de modo que recopile un máximo de cinco volcados de memoria por aplicación.

> [!WARNING]
> Los volcados de memoria pueden ocupar una gran cantidad de espacio en disco (hasta varios gigabytes cada uno).

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>La aplicación deja de responder, produce un error durante el inicio o se ejecuta con normalidad

Si una aplicación *deja de responder* (se detiene, pero no se bloquea), produce un error durante el inicio o se ejecuta con normalidad, vea [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Archivos de volcado de memoria en modo de usuario: selección de la mejor herramienta) para seleccionar una herramienta apropiada para generar el volcado de memoria.

### <a name="analyze-the-dump"></a>Análisis del volcado de memoria

El volcado de memoria se puede analizar de varias maneras. Para obtener más información, vea [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Análisis de un archivo de volcado de memoria en modo de usuario).

## <a name="remote-debugging"></a>Depuración remota

Consulte [Depuración remota de ASP.NET Core en un equipo remoto de IIS en Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) en la documentación de Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) proporciona telemetría de las aplicaciones hospedadas en IIS, lo que incluye las características de registro de errores y generación de informes. Application Insights solo puede notificar los errores que se producen después de que la aplicación se inicia cuando las características de registro de la aplicación se vuelven disponibles. Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-advice"></a>Consejos adicionales

En ocasiones, una aplicación en funcionamiento deja de funcionar inmediatamente después de actualizar el SDK de .NET Core en la máquina de desarrollo o las versiones del paquete en la aplicación. En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes. La mayoría de estos problemas puede corregirse siguiendo estas instrucciones:

1. Elimine las carpetas *bin* y *obj*.
1. Borre las memorias caché del paquete en *%UserProfile%\\.nuget\\packages* y *%LocalAppData%\\Nuget\\v3-cache*.
1. Restaure el proyecto y vuelva a compilarlo.
1. Confirme que la implementación anterior en el servidor se ha eliminado por completo antes de volver a implementar la aplicación.

> [!TIP]
> Una forma práctica de borrar las memorias cachés del paquete es ejecutar `dotnet nuget locals all --clear` desde un símbolo del sistema.
>
> Otra manera de borrar las memorias caché del paquete es usar la herramienta [nuget.exe](https://www.nuget.org/downloads) y ejecutar el comando `nuget locals all -clear`. *nuget.exe* no es una instalación agrupada con el sistema operativo de escritorio de Windows y se debe obtener de forma independiente en el [sitio web de NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
