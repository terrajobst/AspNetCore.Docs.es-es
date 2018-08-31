---
title: Solución de problemas de ASP.NET Core en IIS
author: guardrex
description: Aprenda a diagnosticar problemas con las implementaciones de Internet Information Services (IIS) de aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: f22914c9b0d6d1902dd37c9b21b80a18894c97e7
ms.sourcegitcommit: d1c4580f56656b503cf528ec9f5ba570d790b57d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "41751467"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Solución de problemas de ASP.NET Core en IIS

Por [Luke Latham](https://github.com/guardrex)

En este artículo se proporcionan instrucciones sobre cómo diagnosticar un problema de inicio de ASP.NET Core al hospedarse con [Internet Information Services (IIS)](/iis). La información de este artículo se aplica al hospedaje en IIS en Windows Server y el escritorio de Windows.

En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración. El código de estado *502.5 Error de proceso* que se produce al realizar la depuración localmente se puede solucionar si se sigue el consejo de este tema.

Temas adicionales de solución de problemas:

[Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Aunque App Service usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e IIS para hospedar las aplicaciones, consulte el tema dedicado para obtener instrucciones específicas.

[Control de errores](xref:fundamentals/error-handling)  
Descubra cómo controlar los errores de aplicaciones de ASP.NET Core durante el desarrollo en un sistema local.

[Información sobre cómo depurar con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
En este tema se presentan las características del depurador de Visual Studio.

[Depuración con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)  
Obtenga información sobre la compatibilidad de depuración integrada en Visual Studio Code.

## <a name="app-startup-errors"></a>Errores de inicio de aplicación

**502.5 Error de proceso**  
El proceso de trabajo no funciona. La aplicación no se inicia.

El módulo ASP.NET Core intenta iniciar el proceso de trabajo, pero no lo consigue. La causa del error de inicio del proceso se suele determinar a partir de las entradas del [registro de eventos de la aplicación](#application-event-log) y del [registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).

La página de error *502.5 Error de proceso* se devuelve cuando el proceso de trabajo no se puede iniciar debido a un error de configuración de la aplicación o del hospedaje:

![Ventana del explorador que muestra la página 502.5 Error de proceso](troubleshoot/_static/process-failure-page.png)

**500 Error interno del servidor**  
La aplicación se inicia, pero un error impide que el servidor complete la solicitud.

Este error se produce dentro del código de la aplicación durante el inicio o mientras se crea una respuesta. La respuesta no puede contener nada o puede aparecer como *500 Error interno del servidor* en el explorador. El registro de eventos de la aplicación normalmente indica que la aplicación se ha iniciado normalmente. Desde la perspectiva del servidor, eso es correcto. La aplicación se inició, pero no puede generar una respuesta válida. [Ejecute la aplicación en un símbolo del sistema](#run-the-app-at-a-command-prompt) en el servidor o [habilite el registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log) para solucionar el problema.

**Restablecimiento de la conexión**

Si se produce un error después de que se envían los encabezados, el servidor no tiene tiempo para enviar un mensaje **500 Error interno del servidor** cuando se produce un error. Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos en una respuesta. Este tipo de error aparece como un error de *restablecimiento de la conexión* en el cliente. El [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.

## <a name="default-startup-limits"></a>Límites de inicio predeterminados

El módulo ASP.NET Core está configurado con un valor predeterminado *startupTimeLimit* de 120 segundos. Cuando se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registre un error de proceso. Para información sobre la configuración del módulo, consulte [Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Solución de problemas de errores de inicio de la aplicación

### <a name="application-event-log"></a>Registro de eventos de aplicación

Acceda al registro de eventos de la aplicación:

1. Abra el menú Inicio, busque **Visor de eventos** y luego seleccione la aplicación **Visor de eventos**.
1. En **Visor de eventos**, abra el nodo **Registros de Windows**.
1. Seleccione **Aplicación** para abrir el registro de eventos de la aplicación.
1. Busque los errores asociados a la aplicación objeto del error. Los errores tienen un valor de *Módulo AspNetCore de IIS* o *Módulo AspNetCore de IIS Express* en la columna *Origen*.

### <a name="run-the-app-at-a-command-prompt"></a>Ejecución de la aplicación en un símbolo del sistema

Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación. La causa de algunos errores se puede encontrar mediante la ejecución de la aplicación en un símbolo del sistema en el sistema de hospedaje.

**Implementación dependiente de marco**

Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. En un símbolo del sistema, vaya a la carpeta de implementación y ejecute la aplicación mediante la ejecución del ensamblado de la aplicación con *dotnet.exe*. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `dotnet .\<assembly_name>.dll`.
1. La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.
1. Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel. Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración del proxy inverso que con la propia aplicación.

**Implementación independiente**

Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):

1. En un símbolo del sistema, vaya a la carpeta de implementación y ejecute el archivo ejecutable de la aplicación. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `<assembly_name>.exe`.
1. La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.
1. Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel. Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración del proxy inverso que con la propia aplicación.

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

**¡Importante!** Deshabilite el registro de stdout cuando la solución de problemas haya finalizado.

1. Edite el archivo *web.config*.
1. Establezca **stdoutLogEnabled** en `false`.
1. Guarde el archivo.

> [!WARNING]
> La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor. No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.
>
> Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Habilitación de la página de excepciones del desarrollador

La variable de entorno `ASPNETCORE_ENVIRONMENT` [ se puede agregar a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo. Siempre y cuando el entorno no se invalide al inicio de la aplicación con `UseEnvironment` en el generador de host, la configuración de la variable de entorno permite que aparezca la [página de excepciones del desarrollador](xref:fundamentals/error-handling) cuando se ejecuta la aplicación.

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

Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` cuando se use en servidores de ensayo o pruebas que no estén expuestos a Internet. Quite la variable de entorno del archivo *web.config* cuando termine de solucionar los problemas. Para información sobre la configuración de variables de entorno en *web.config*, consulte el [elemento secundario environmentVariables de aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Errores comunes de inicio 

Consulte la [referencia de errores comunes de ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). La mayoría de los problemas comunes que impiden el inicio de la aplicación se tratan en el tema de referencia.

## <a name="slow-or-hanging-app"></a>Aplicación lenta o bloqueada

Cuando una aplicación responda con lentitud o queda bloqueado en una solicitud, obtenga y analice un [archivo de volcado de memoria](/visualstudio/debugger/using-dump-files). Los archivos de volcado de memoria se pueden obtener mediante cualquiera de las siguientes herramientas:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg) (Descarga de herramientas de depuración para Windows), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg) (Depuración mediante WinDbg)

## <a name="remote-debugging"></a>Depuración remota

Consulte [Depuración remota de ASP.NET Core en un equipo remoto de IIS en Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) en la documentación de Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) proporciona telemetría de las aplicaciones hospedadas en IIS, lo que incluye las características de registro de errores y generación de informes. Application Insights solo puede notificar los errores que se producen después de que la aplicación se inicia cuando las características de registro de la aplicación se vuelven disponibles. Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Consejos adicionales de solución de problemas

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

* [Introducción a control de errores en ASP.NET Core](xref:fundamentals/error-handling)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
