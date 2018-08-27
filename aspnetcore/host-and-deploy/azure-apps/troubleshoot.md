---
title: Solución de problemas de ASP.NET Core en Azure App Service
author: guardrex
description: Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: a995c743b4e43be8bea5329affb3f2c736b1d016
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902559"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Solución de problemas de ASP.NET Core en Azure App Service

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

En este artículo se proporcionan instrucciones sobre cómo diagnosticar un problema de inicio de aplicaciones ASP.NET Core mediante herramientas de diagnóstico de Azure App Service. Puede encontrar consejos adicionales de solución de problemas en [Introducción a los diagnósticos de Azure App Service](/azure/app-service/app-service-diagnostics) y [Supervisión de Aplicaciones en Azure App Service](/azure/app-service/web-sites-monitor) en la documentación de Azure.

## <a name="app-startup-errors"></a>Errores de inicio de aplicación

**502.5 Error de proceso**  
El proceso de trabajo no funciona. La aplicación no se inicia.

El [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) intenta iniciar el proceso de trabajo, pero no lo consigue. Con frecuencia, examinar el registro de eventos de la aplicación ayuda a solucionar problemas de este tipo. El acceso al registro se explica en la sección [Registro de eventos de la aplicación](#application-event-log).

La página *502.5 Error de proceso* se devuelve cuando una aplicación mal configurada provoca que el proceso de trabajo genere un error:

![Ventana del explorador que muestra la página 502.5 Error de proceso](troubleshoot/_static/process-failure-page.png)

**500 Error interno del servidor**  
La aplicación se inicia, pero un error impide que el servidor complete la solicitud.

Este error se produce dentro del código de la aplicación durante el inicio o mientras se crea una respuesta. La respuesta no puede contener nada o puede aparecer como *500 Error interno del servidor* en el explorador. El registro de eventos de la aplicación normalmente indica que la aplicación se ha iniciado normalmente. Desde la perspectiva del servidor, eso es correcto. La aplicación se inició, pero no puede generar una respuesta válida. [Ejecute la aplicación en la consola de Kudu](#run-the-app-in-the-kudu-console) o [habilite el registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log) para solucionar el problema.

**Restablecimiento de la conexión**

Si se produce un error después de que se envían los encabezados, el servidor no tiene tiempo para enviar un mensaje **500 Error interno del servidor** cuando se produce un error. Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos en una respuesta. Este tipo de error aparece como un error de *restablecimiento de la conexión* en el cliente. El [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.

## <a name="default-startup-limits"></a>Límites de inicio predeterminados

El módulo ASP.NET Core está configurado con un valor predeterminado *startupTimeLimit* de 120 segundos. Cuando se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registre un error de proceso. Para información sobre la configuración del módulo, consulte [Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Solución de problemas de errores de inicio de la aplicación

### <a name="application-event-log"></a>Registro de eventos de aplicación

Para acceder al registro de eventos de la aplicación, use la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal:

1. En Azure Portal, abra la hoja de la aplicación en la hoja **App Services**.
1. Seleccione la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas).
1. En **Seleccione una categoría de problema**, seleccione el botón abajo **Aplicación web**.
1. En **Suggested Solutions** (Soluciones sugeridas), abra el panel de **Open Application Event Logs** (Abrir registros de eventos de la aplicación). Seleccione el botón **Open Application Event Logs** (Abrir registros de eventos de la aplicación).
1. Examine el error más reciente proporcionado por *IIS AspNetCoreModule* en la columna **Origen**.

Una alternativa al uso de la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) es examinar el archivo de registro de eventos de la aplicación directamente mediante [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Seleccione la hoja **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;**. Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Abra la carpeta **LogFiles**.
1. Seleccione el icono de lápiz junto al archivo *eventlog.xml*.
1. Examine el registro. Desplácese al final del registro para ver los eventos más recientes.

### <a name="run-the-app-in-the-kudu-console"></a>Ejecución de la aplicación en la consola de Kudu

Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación. Puede ejecutar la aplicación en la consola de ejecución remota de [Kudu](https://github.com/projectkudu/kudu/wiki) para detectar el error:

1. Seleccione la hoja **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;**. Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Abra las carpetas para la ruta de acceso **site** > **wwwroot**.
1. En la consola, ejecute la aplicación mediante la ejecución del ensamblado de la aplicación.
   * Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd), ejecute el ensamblado de la aplicación con *dotnet.exe*. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd), ejecute el archivo ejecutable de la aplicación. En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por `<assembly_name>`: `<assembly_name>.exe`
1. La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Registro de stdout del módulo ASP.NET Core

El registro stdout del módulo ASP.NET Core con frecuencia registra mensajes de error útiles que no se encuentran en el registro de eventos de la aplicación. Para habilitar y ver los registros de stdout:

1. Vaya a la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal.
1. En **Seleccione una categoría de problema**, seleccione el botón abajo **Aplicación web**.
1. En **Suggested Solutions** >  (Soluciones sugeridas) **Enable Stdout Log Redirection** (Habilitar el redireccionamiento de registros stdout), seleccione el botón para **abrir la consola de Kudu y editar web.config**.
1. En la **consola de diagnóstico** de Kudu, abra las carpetas para la ruta de acceso **site** > **wwwroot**. Desplácese hacia abajo para mostrar el archivo *web.config* en la parte inferior de la lista.
1. Haga clic en el icono de lápiz junto al archivo *web.config*.
1. Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** a: `\\?\%home%\LogFiles\stdout`.
1. Seleccione **Save** (Guardar) para guardar el archivo *web.config* actualizado.
1. Realice una solicitud a la aplicación.
1. Vuelva a Azure Portal. Seleccione la hoja **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;**. Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Seleccione la carpeta **LogFiles**.
1. Inspeccione la columna **Modificado** y seleccione el icono de lápiz para editar el registro de stdout con la última fecha de modificación.
1. Cuando se abre el archivo de registro, se muestra el error.

**¡Importante!** Deshabilite el registro de stdout cuando la solución de problemas haya finalizado.

1. En la **consola de diagnóstico** de Kudu, vuelva a la ruta de acceso **site** > **wwwroot** para mostrar el archivo *web.config*. Seleccione el icono de lápiz para abrir de nuevo el archivo **web.config**.
1. Establezca **stdoutLogEnabled** en `false`.
1. Seleccione **Save** (Guardar) para guardar el archivo.

> [!WARNING]
> La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor. No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados. Use únicamente el registro de stdout para solucionar problemas de inicio de la aplicación.
>
> Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Errores comunes de inicio 

Consulte la [referencia de errores comunes de ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). La mayoría de los problemas comunes que impiden el inicio de la aplicación se tratan en el tema de referencia.

## <a name="slow-or-hanging-app"></a>Aplicación lenta o bloqueada

Cuando una aplicación responda con lentitud o se bloquee en una solicitud, consulte [Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) para obtener instrucciones de depuración.

## <a name="remote-debugging"></a>Depuración remota

Consulte los temas siguientes:

* [Sección sobre la depuración remota de las aplicaciones web del artículo Solución de problemas de una aplicación web en Azure App Service con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (documentación de Azure)
* [Depuración remota de ASP.NET Core en IIS en Azure para Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (documentación de Visual Studio)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) proporciona telemetría de las aplicaciones hospedadas en Azure App Service, lo que incluye las características de registro de errores y generación de informes. Application Insights solo puede notificar los errores que se producen después de que la aplicación se inicia cuando las características de registro de la aplicación se vuelven disponibles. Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Hojas de supervisión

Las hojas de supervisión proporcionan una alternativa a la experiencia de solución de problemas de los métodos descritos anteriormente en el tema. Estas hojas se pueden usar para diagnosticar errores de la serie 500.

Confirme que están instaladas las extensiones de ASP.NET Core. Si no lo están, instálelas manualmente:

1. En la sección de la hoja **HERRAMIENTAS DE DESARROLLO**, seleccione la hoja **Extensiones**.
1. Aparecerán en la lista las **extensiones de ASP.NET Core**.
1. Si las extensiones no están instaladas, seleccione el botón **Add** (Agregar).
1. Elija las **extensiones de ASP.NET Core** de la lista.
1. Seleccione **Aceptar** para aceptar los términos legales.
1. Seleccione **Aceptar** en la hoja **Agregar extensión**.
1. Un mensaje emergente informativo indica si las extensiones se han instalado correctamente.

Si el registro de stdout no está habilitado, siga estos pasos:

1. En Azure Portal, seleccione la hoja **Herramientas avanzadas** en el área **HERRAMIENTAS DE DESARROLLO**. Seleccione el botón **Ir&rarr;**. Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Abra las carpetas a la ruta de acceso **sitio** > **wwwroot** y desplácese hacia abajo para mostrar el archivo *web.config* en la parte inferior de la lista.
1. Haga clic en el icono de lápiz junto al archivo *web.config*.
1. Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** a: `\\?\%home%\LogFiles\stdout`.
1. Seleccione **Save** (Guardar) para guardar el archivo *web.config* actualizado.

Continúe para activar el registro de diagnóstico:

1. En Azure Portal, seleccione la hoja **Registros de diagnóstico**.
1. Seleccione el conmutador **Activado** en **Registro de la aplicación (sistema de archivos)** y **Mensajes de error detallados**. Seleccione el botón **Guardar** en la parte superior de la hoja.
1. Para incluir el seguimiento de solicitudes con error, también conocido como almacenamiento en búfer de eventos de solicitudes con error (FREB), seleccione el conmutador **Activado** en **Seguimiento de solicitudes con error**. 
1. Seleccione la hoja **Secuencia de registro**, que aparece inmediatamente bajo la hoja **Registros de diagnóstico** en el portal.
1. Realice una solicitud a la aplicación.
1. Dentro de los datos de la secuencia de registro, se indica la causa del error.

**¡Importante!** No olvide deshabilitar el registro de stdout cuando finalice la solución de problemas. Consulte las instrucciones de la sección [Registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).

Para ver los registros de seguimiento de solicitudes con error (registros FREB):

1. Vaya a la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal.
1. Seleccione **Failed Request Tracing Logs** (Registros de seguimiento de solicitudes con error) en el área **SUPPORT TOOLS** (HERRAMIENTAS DE SOPORTE TÉCNICO) de la barra lateral.

Para más información, consulte la [sección sobre los seguimientos de solicitudes con error del tema Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) y el artículo [Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure: ¿Cómo se activa el seguimiento de solicitudes con error?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).

Para más información, consulte [Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor. No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.
>
> Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a control de errores en ASP.NET Core](xref:fundamentals/error-handling)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Solucionar los errores HTTP de "502 Puerta de enlace no válida" y "503 Servicio no disponible" en las aplicaciones web de Azure](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Espacio aislado de Azure Web App [limitaciones de ejecución del entono de tiempo de ejecución de App Service])
* [Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
