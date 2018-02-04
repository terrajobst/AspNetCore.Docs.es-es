---
title: "Solucionar problemas de núcleo de ASP.NET en el servicio de aplicaciones de Azure"
author: guardrex
description: "Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 144af8e93bb935d07fd064d5f45b40faea4a2664
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Solucionar problemas de núcleo de ASP.NET en el servicio de aplicaciones de Azure

Por [Luke Latham](https://github.com/guardrex)

Este artículo proporciona instrucciones sobre cómo diagnosticar un ASP.NET Core problema de inicio de aplicación mediante herramientas de diagnóstico del servicio de aplicaciones de Azure. Si desea obtener consejos de solución de problemas adicionales, consulte [información general sobre diagnóstico de servicio de aplicaciones de Azure](/azure/app-service/app-service-diagnostics) y [Cómo: supervisar aplicaciones en el servicio de aplicaciones de Azure](/azure/app-service/web-sites-monitor) en la documentación de Azure.

## <a name="app-startup-errors"></a>Errores de inicio de aplicación

**502.5 Error del proceso de**  
Se produce un error en el proceso de trabajo. No se inicia la aplicación.

El [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) intentos para iniciar el proceso de trabajo, pero no se puede iniciar. Examinar el registro de eventos de aplicación a menudo le ayuda a solucionar problemas de este tipo de problema. Obtener acceso a los registros se explica en la [registro de eventos de aplicación](#application-event-log) sección.

El *502.5 Error de un proceso* página de error se devuelve cuando una aplicación mal configurada da lugar a un error del proceso de trabajo:

![Ventana del explorador que muestra la página de error de un proceso 502.5](troubleshoot/_static/process-failure-page.png)

**Error interno del servidor 500**  
Inicie la aplicación, pero un error impide que el servidor se completara la solicitud.

Este error se produce dentro del código de la aplicación durante el inicio o durante la creación de una respuesta. La respuesta no puede contener ningún contenido o la respuesta puede aparecer como un *500 Error interno del servidor* en el explorador. El registro de eventos de aplicación normalmente indica que la aplicación se inicia normalmente. Desde la perspectiva del servidor, que es correcta. Se inició la aplicación, pero que no puede generar una respuesta válida. [Ejecute la aplicación en la consola de Kudu](#run-the-app-in-the-kudu-console) o [habilite el registro de ASP.NET Core módulo stdout](#aspnet-core-module-stdout-log) para solucionar el problema.

## <a name="troubleshoot-app-startup-errors"></a>Solucionar problemas de errores de inicio de aplicación

### <a name="application-event-log"></a>Registro de eventos de aplicación

Para acceder al registro de eventos de aplicación, use la **diagnosticar y resolver problemas** hoja en el portal de Azure:

1. En el portal de Azure, abra la hoja de la aplicación en el **servicios de aplicaciones** hoja.
1. Seleccione el **diagnosticar y resolver problemas** hoja.
1. En **Seleccionar categoría de problema**, seleccione la **aplicación hacia abajo de la Web** botón.
1. En **sugiere soluciones**, abra el panel de **abrir registros de eventos de aplicación**. Seleccione el **abrir registros de eventos de aplicación** botón.
1. Examine el error más reciente proporcionado por el *IIS AspNetCoreModule* en el **origen** columna.

Una alternativa al uso de la **diagnosticar y solucionar problemas de** hoja es examinar el archivo de registro de eventos de aplicación directamente mediante [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Seleccione el **herramientas avanzadas** hoja en el **herramientas de desarrollo** área. Seleccione el **vaya&rarr;**  botón. Se abre la consola de Kudu en una nueva pestaña del explorador o una ventana.
1. Use la barra de navegación en la parte superior de la página para abrir **consola de depuración** y seleccione **CMD**.
1. Abra la **LogFiles** carpeta.
1. Seleccione el icono de lápiz junto a la *eventlog.xml* archivo.
1. Examine el registro. Desplácese hasta el final del registro para ver los eventos más recientes.

### <a name="run-the-app-in-the-kudu-console"></a>Ejecute la aplicación en la consola de Kudu

Muchos errores de inicio no producen información útil en el registro de eventos de aplicación. Puede ejecutar la aplicación el [Kudu](https://github.com/projectkudu/kudu/wiki) consola remota de ejecución para detectar el error:

1. Seleccione el **herramientas avanzadas** hoja en el **herramientas de desarrollo** área. Seleccione el **vaya&rarr;**  botón. Se abre la consola de Kudu en una nueva pestaña del explorador o una ventana.
1. Use la barra de navegación en la parte superior de la página para abrir **consola de depuración** y seleccione **CMD**.
1. Abra las carpetas para la ruta de acceso **sitio** > **wwwroot**.
1. En la consola, ejecute la aplicación mediante la ejecución de ensamblado de la aplicación con *dotnet.exe*. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación para `<assembly_name>`:
   ```console
   dotnet .\<assembly_name>.dll
   ```
1. Se canaliza la salida desde la aplicación, que muestra los errores, en la consola en la consola de Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Registro de stdout del módulo principal ASP.NET

El registro de stdout módulo principal de ASP.NET a menudo registra mensajes de error útiles que no se encuentra en el registro de eventos de aplicación. Para habilitar y ver los registros de stdout:

1. Navegue hasta la **diagnosticar y resolver problemas** hoja en el portal de Azure.
1. En **Seleccionar categoría de problema**, seleccione la **aplicación hacia abajo de la Web** botón.
1. En **sugiere soluciones** > **habilitar la redirección de registro Stdout**, seleccione el botón de **abrir la consola de Kudu editar el archivo Web.Config**.
1. En el Kudu **consola de diagnóstico**, abra las carpetas para la ruta de acceso **sitio** > **wwwroot**. Desplácese hacia abajo para mostrar la *web.config* archivo en la parte inferior de la lista.
1. Haga clic en el icono de lápiz junto a la *web.config* archivo.
1. Establecer **stdoutLogEnabled** a `true` y cambie el **stdoutLogFile** ruta de acceso: `\\?\%home%\LogFiles\stdout`.
1. Seleccione **guardar** guardar actualizado *web.config* archivo.
1. Realizar una solicitud a la aplicación.
1. Vuelva al portal de Azure. Seleccione el **herramientas avanzadas** hoja en el **herramientas de desarrollo** área. Seleccione el **vaya&rarr;**  botón. Se abre la consola de Kudu en una nueva pestaña del explorador o una ventana.
1. Use la barra de navegación en la parte superior de la página para abrir **consola de depuración** y seleccione **CMD**.
1. Seleccione el **LogFiles** carpeta.
1. Inspeccionar el **Modified** columna y seleccione el icono de lápiz para editar el stdout, inicie sesión con la última fecha de modificación.
1. Cuando se abre el archivo de registro, se muestra el error.

**Importante:** Deshabilitar el registro de stdout cuando se completa la solución de problemas.

1. En el Kudu **consola de diagnóstico**, vuelva a la ruta de acceso **sitio** > **wwwroot** para revelar el *web.config* archivo. Abra la **web.config** archivo nuevo, seleccione el icono de lápiz.
1. Establecer **stdoutLogEnabled** a `false`.
1. Seleccione **guardar** para guardar el archivo.

> [!WARNING]
> Error al deshabilitar el registro de stdout puede provocar errores de aplicación o un servidor. No hay ningún límite en el tamaño del archivo de registro o el número de archivos de registro creados.
>
> Para registrar la rutina en una aplicación de ASP.NET Core, utiliza una biblioteca de registro que limita el tamaño del archivo de registro y gira registros. Para obtener más información, consulte [proveedores de registro de aplicaciones de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Errores comunes de inicio 

Consulte la [referencia de errores comunes de ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). En el tema de referencia se trata la mayoría de los problemas comunes que impiden el inicio de la aplicación.

## <a name="process-dump-for-a-slow-or-hanging-app"></a>Volcado de memoria de proceso para una aplicación lenta o bloqueada

Cuando una aplicación responde con lentitud o se bloquea en una solicitud, consulte [solucionar problemas de rendimiento de aplicación de web lenta en el servicio de aplicación de Azure](/azure/app-service/app-service-web-troubleshoot-performance-degradation) para depurar instrucciones.

## <a name="remote-debugging"></a>Depuración remota

Vea [sección de aplicaciones web de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio de la solución de problemas de la depuración remota](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) en la documentación de Azure.

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) proporciona telemetría de aplicaciones hospedadas en el servicio de aplicación de Azure, incluido el registro y características de informes de errores. Visión de la aplicación sólo puede generar informes sobre los errores que se producen después de la aplicación se inicia cuando se habilitan características de registro de la aplicación. Para obtener más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Módulos de supervisión

Módulos de supervisión proporcionan una alternativa a la experiencia de los métodos descritos anteriormente en el tema de solución de problemas. Estos módulos se pueden utilizar para diagnosticar errores de la serie 500.

Confirme que se instalan las principales extensiones de ASP.NET. Si no están instaladas las extensiones, instalarlos manualmente:

1. En el **herramientas de desarrollo** sección de hoja, seleccione la **extensiones** hoja.
1. El **principales extensiones de ASP.NET** debe aparecer en la lista.
1. Si no están instaladas las extensiones, seleccione la **agregar** botón.
1. Elija la **principales extensiones de ASP.NET** en la lista.
1. Seleccione **Aceptar** para aceptar los términos legales.
1. Seleccione **Aceptar** en el **Agregar extensión** hoja.
1. Un mensaje emergente informativo indica si las extensiones se instalan correctamente.

Si el registro de stdout no está habilitado, siga estos pasos:

1. En el portal de Azure, seleccione la **herramientas avanzadas** hoja en el **herramientas de desarrollo** área. Seleccione el **vaya&rarr;**  botón. Se abre la consola de Kudu en una nueva pestaña del explorador o una ventana.
1. Use la barra de navegación en la parte superior de la página para abrir **consola de depuración** y seleccione **CMD**.
1. Abra las carpetas para la ruta de acceso **sitio** > **wwwroot** y desplácese hacia abajo para mostrar la *web.config* archivo en la parte inferior de la lista.
1. Haga clic en el icono de lápiz junto a la *web.config* archivo.
1. Establecer **stdoutLogEnabled** a `true` y cambie el **stdoutLogFile** ruta de acceso: `\\?\%home%\LogFiles\stdout`.
1. Seleccione **guardar** guardar actualizado *web.config* archivo.

Continúe para activar el registro de diagnóstico:

1. En el portal de Azure, seleccione la **registros de diagnóstico** hoja.
1. Seleccione el **en** cambie para **(Filesystem) de registro de aplicaciones** y **mensajes de error detallados**. Seleccione el **guardar** situado en la parte superior de la hoja.
1. Para incluir el seguimiento de solicitudes con error, también conocido como registro de error de solicitud eventos de almacenamiento en búfer (FREB), seleccione la **en** cambie para **seguimiento de solicitudes con error**. 
1. Seleccione el **flujo de registro** hoja, que aparece inmediatamente en el **registros de diagnóstico** hoja en el portal.
1. Realizar una solicitud a la aplicación.
1. Dentro de los datos de la secuencia de registro, se indica la causa del error.

**Importante:** No olvide deshabilitar el registro de stdout cuando se completa la solución de problemas. Consulte las instrucciones de la [registro de ASP.NET Core módulo stdout](#aspnet-core-module-stdout-log) sección.

Para ver los registros de seguimiento de solicitudes con error (registros FREB):

1. Navegue hasta la **diagnosticar y resolver problemas** hoja en el portal de Azure.
1. Seleccione **no se pudo registros de seguimiento de solicitudes con** desde el **herramientas de soporte técnico** área no cliente de la barra lateral.

Vea [error solicitud realiza un seguimiento de la sección del registro de diagnósticos de habilitar para las aplicaciones web en el tema de servicio de aplicaciones de Azure](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) y [rendimiento preguntas más frecuentes de la aplicación para las aplicaciones Web en Azure: ¿cómo activar el seguimiento de solicitudes con error?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Para obtener más información.

Para obtener más información, consulte [habilitar el registro de diagnósticos para aplicaciones web en el servicio de aplicaciones de Azure](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Error al deshabilitar el registro de stdout puede provocar errores de aplicación o un servidor. No hay ningún límite en el tamaño del archivo de registro o el número de archivos de registro creados.
>
> Para registrar la rutina en una aplicación de ASP.NET Core, utiliza una biblioteca de registro que limita el tamaño del archivo de registro y gira registros. Para obtener más información, consulte [proveedores de registro de aplicaciones de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a control de errores en ASP.NET Core](xref:fundamentals/error-handling)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Solucionar problemas de errores HTTP de "502 pasarela incorrecta" y "503 Servicio no disponible" en las aplicaciones web de Azure](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Solucionar problemas de rendimiento de aplicación web lenta en el servicio de aplicación de Azure](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Preguntas más frecuentes de rendimiento de aplicaciones para las aplicaciones Web en Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure el viernes: Diagnóstico de servicio de aplicación de Azure y experiencia de solución de problemas (vídeo de 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
