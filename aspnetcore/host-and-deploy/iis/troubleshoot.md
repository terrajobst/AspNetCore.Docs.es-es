---
title: "Solucionar problemas de núcleo de ASP.NET en IIS"
author: guardrex
description: "Obtenga información acerca de cómo diagnosticar problemas con las implementaciones de servicios de Internet Information Server (IIS) de las aplicaciones de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Solucionar problemas de núcleo de ASP.NET en IIS

Por [Luke Latham](https://github.com/guardrex)

Este artículo proporciona instrucciones sobre cómo diagnosticar un ASP.NET Core problema de inicio de aplicación al hospedarse con [Internet Information Services (IIS)](/iis). La información de este artículo se aplica a hospedar en IIS en Windows Server y el escritorio de Windows.

En Visual Studio, un proyecto de ASP.NET Core predeterminado [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hospedaje durante la depuración. A *502.5 Error de proceso* que se produce cuando la depuración localmente no se puede solucionar con los consejos de este tema.

Temas de solución de problemas adicionales:

[Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Aunque el servicio de aplicaciones usa la [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) y IIS para las aplicaciones de host, vea el tema dedicado para obtener instrucciones específicas para el servicio de aplicaciones.

[Control de errores](xref:fundamentals/error-handling)  
Descubra cómo controlar errores en aplicaciones de ASP.NET Core durante el desarrollo en un sistema local.

[Información sobre cómo depurar con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
En este tema se presenta las características del depurador de Visual Studio.

## <a name="app-startup-errors"></a>Errores de inicio de aplicación

**502.5 Error del proceso de**  
Se produce un error en el proceso de trabajo. No se inicia la aplicación.

El módulo principal de ASP.NET intenta iniciar el proceso de trabajo, pero no puede iniciar. Normalmente se puede determinar la causa de un error de inicio del proceso de entradas en la [registro de eventos de aplicación](#application-event-log) y [registro stdout de ASP.NET Core módulo](#aspnet-core-module-stdout-log).

El *502.5 Error de un proceso* página de error se devuelve cuando el proceso de trabajo no da lugar a un error de configuración de hospedaje o de aplicación:

![Ventana del explorador que muestra la página de error de un proceso 502.5](troubleshoot/_static/process-failure-page.png)

**Error interno del servidor 500**  
Inicie la aplicación, pero un error impide que el servidor se completara la solicitud.

Este error se produce dentro del código de la aplicación durante el inicio o durante la creación de una respuesta. La respuesta no puede contener ningún contenido o la respuesta puede aparecer como un *500 Error interno del servidor* en el explorador. El registro de eventos de aplicación normalmente indica que la aplicación se inicia normalmente. Desde la perspectiva del servidor, que es correcta. Se inició la aplicación, pero que no puede generar una respuesta válida. [Ejecutar la aplicación en un símbolo del sistema](#run-the-app-at-a-command-prompt) en el servidor o [habilite el registro de ASP.NET Core módulo stdout](#aspnet-core-module-stdout-log) para solucionar el problema.

**Restablecimiento de la conexión**

Si se produce un error después de que se envían los encabezados, es demasiado tarde para que el servidor envíe un **500 Error interno del servidor** cuando se produce un error. Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos de una respuesta. Este tipo de error aparece como un *restablecimiento de la conexión* error en el cliente. [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.

## <a name="default-startup-limits"></a>Límites de inicio predeterminados

El módulo de núcleo de ASP.NET está configurado con un valor predeterminado *startupTimeLimit* de 120 segundos. Si se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registra un error de proceso. Para obtener información acerca de cómo configurar el módulo, consulte [atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Solucionar problemas de errores de inicio de aplicación

### <a name="application-event-log"></a>Registro de eventos de aplicación

Acceder al registro de eventos de aplicación:

1. Abra el menú Inicio, busque **Visor de eventos**y, a continuación, seleccione la **Visor de eventos** aplicación.
1. En **Visor de eventos**, abra el **registros de Windows** nodo.
1. Seleccione **aplicación** para abrir el registro de eventos de aplicación.
1. Busque errores asociados a la aplicación por error. Los errores tienen un valor de *IIS AspNetCore módulo* o *módulo de IIS Express AspNetCore* en el *origen* columna.

### <a name="run-the-app-at-a-command-prompt"></a>Ejecutar la aplicación en un símbolo del sistema

Muchos errores de inicio no producen información útil en el registro de eventos de aplicación. Puede encontrar la causa de algunos errores mediante la ejecución de la aplicación en un símbolo del sistema en el sistema host.

**Implementación del marco de trabajo dependiente**

Si la aplicación es un [implementación dependiente de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. En un símbolo del sistema, navegue hasta la carpeta de implementación y ejecutar la aplicación mediante la ejecución de ensamblado de la aplicación con *dotnet.exe*. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación para \<assembly_name >: `dotnet .\<assembly_name>.dll`.
1. La consola de salida desde la aplicación, que muestra los errores, se escribe en la ventana de consola.
1. Si los errores se producen al realizar una solicitud a la aplicación, realizar una solicitud en el host y el puerto que escucha Kestrel. Con el host predeterminado y post, realizar una solicitud para `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del extremo Kestrel, el problema es más probable relacionadas con la configuración de proxy inverso y menos probable que dentro de la aplicación.

**Implementación independiente**

Si la aplicación es un [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):

1. En un símbolo del sistema, navegue hasta la carpeta de implementación y ejecute el archivo ejecutable de la aplicación. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación para \<assembly_name >: `<assembly_name>.exe`.
1. La consola de salida desde la aplicación, que muestra los errores, se escribe en la ventana de consola.
1. Si los errores se producen al realizar una solicitud a la aplicación, realizar una solicitud en el host y el puerto que escucha Kestrel. Con el host predeterminado y post, realizar una solicitud para `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del extremo Kestrel, el problema es más probable relacionadas con la configuración de proxy inverso y menos probable que dentro de la aplicación.

### <a name="aspnet-core-module-stdout-log"></a>Registro de stdout del módulo principal ASP.NET

Para habilitar y ver los registros de stdout:

1. Navegue hasta la carpeta de implementación del sitio en el sistema host.
1. Si el *registros* carpeta no está presente, cree la carpeta. Para obtener instrucciones sobre cómo habilitar MSBuild crear el *registros* carpeta en la implementación, automáticamente, consulte la [estructura de directorios](xref:host-and-deploy/directory-structure) tema.
1. Editar la *web.config* archivo. Establecer **stdoutLogEnabled** a `true` y cambie el **stdoutLogFile** ruta de acceso para que apunte a la *registros* carpeta (por ejemplo, `.\logs\stdout`). `stdout`en la ruta de acceso es el prefijo del nombre de archivo de registro. Una marca de tiempo, el Id. de proceso y la extensión de archivo se agregan automáticamente cuando se crea el registro. Usar `stdout` como el prefijo de nombre de archivo, se denomina un archivo de registro típico *stdout_20180205184032_5412.log*. 
1. Guardar actualizado *web.config* archivo.
1. Realizar una solicitud a la aplicación.
1. Navegue hasta la *registros* carpeta. Busque y abra el registro más reciente de stdout.
1. Estudie el registro de errores.

**Importante:** Deshabilitar el registro de stdout cuando se completa la solución de problemas.

1. Editar la *web.config* archivo.
1. Establecer **stdoutLogEnabled** a `false`.
1. Guarde el archivo.

> [!WARNING]
> Error al deshabilitar el registro de stdout puede provocar errores de aplicación o un servidor. No hay ningún límite en el tamaño del archivo de registro o el número de archivos de registro creados.
>
> Para registrar la rutina en una aplicación de ASP.NET Core, utiliza una biblioteca de registro que limita el tamaño del archivo de registro y gira registros. Para obtener más información, consulte [proveedores de registro de aplicaciones de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Habilitación de la página de excepción para desarrolladores

El `ASPNETCORE_ENVIRONMENT` [variable de entorno puede agregarse al archivo web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo. Siempre y cuando no se reemplaza el entorno de inicio de la aplicación por `UseEnvironment` en el generador de host, si establece la variable de entorno, permitirá la [Developer excepción página](xref:fundamentals/error-handling) aparecen cuando la aplicación se ejecute.

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

Establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` sólo se recomienda para su uso en probar y ensayar los servidores que no están expuestos a Internet. Quite la variable de entorno desde el *web.config* archivo después de la solución de problemas. Para obtener información acerca de cómo establecer variables de entorno *web.config*, consulte [environmentVariables elemento secundario aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Errores comunes de inicio 

Consulte la [referencia de errores comunes de ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). En el tema de referencia se trata la mayoría de los problemas comunes que impiden el inicio de la aplicación.

## <a name="slow-or-hanging-app"></a>Aplicación lenta o bloqueado

Cuando una aplicación responde con lentitud o se bloquea en una solicitud, obtener y analizar un [archivo de volcado de memoria](/visualstudio/debugger/using-dump-files). Archivos de volcado de memoria se pueden obtener mediante cualquiera de las siguientes herramientas:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [descargar las herramientas de depuración para Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [depuración WinDbg usando](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Depuración remota

Vea [principales de ASP.NET de depuración remota en un equipo de IIS remoto en Visual Studio de 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) en la documentación de Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) proporciona telemetría de aplicaciones hospedadas por IIS, incluido el registro y características de informes de errores. Visión de la aplicación sólo puede generar informes sobre los errores que se producen después de la aplicación se inicia cuando se habilitan características de registro de la aplicación. Para obtener más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Consejos para solucionar problemas adicionales

A veces, una aplicación operativa se produce un error inmediatamente después de actualizar el SDK de núcleo de .NET en las versiones de paquete o la máquina de desarrollo dentro de la aplicación. En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes. La mayoría de estos problemas puede corregirse siguiendo estas instrucciones:

1. Eliminar el *bin* y *obj* carpetas.
1. Desactive el paquete almacena en caché en *% UserProfile %\\.nuget\\paquetes* y *% LocalAppData %\\Nuget\\v3 caché*.
1. Restaurar y volver a generar el proyecto.
1. Confirme que la implementación anterior en el servidor se ha eliminado por completo antes de volver a implementar la aplicación.

> [!TIP]
> Una manera cómoda para borrar las cachés del paquete es ejecutar `dotnet nuget locals all --clear` desde un símbolo del sistema.
> 
> Borrar las memorias caché del paquete también se puede lograr mediante el uso de la [nuget.exe](https://www.nuget.org/downloads) herramienta y ejecutar el comando `nuget locals all -clear`. *NuGet.exe* no es una instalación incluida con el sistema operativo de escritorio Windows y debe obtenerse por separado de la [sitio Web de NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a control de errores en ASP.NET Core](xref:fundamentals/error-handling)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
