---
title: Solución de problemas de ASP.NET Core en Azure App Service
author: guardrex
description: Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 7a0bb7df27ebbea0eac79771452295846fad563a
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470445"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Solución de problemas de ASP.NET Core en Azure App Service

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

En este artículo se proporcionan instrucciones sobre cómo diagnosticar un problema de inicio de aplicaciones ASP.NET Core mediante herramientas de diagnóstico de Azure App Service. Para obtener consejos adicionales de solución de problemas, consulte [Introducción a los diagnósticos de Azure App Service](/azure/app-service/app-service-diagnostics) y [Supervisión de aplicaciones en Azure App Service](/azure/app-service/web-sites-monitor) en la documentación de Azure.

## <a name="app-startup-errors"></a>Errores de inicio de aplicación

**502.5 Error de proceso** El proceso de trabajo no funciona. La aplicación no se inicia.

El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el proceso de trabajo, pero no lo consigue. Con frecuencia, examinar el registro de eventos de la aplicación ayuda a solucionar problemas de este tipo. El acceso al registro se explica en la sección [Registro de eventos de la aplicación](#application-event-log).

La página *502.5 Error de proceso* se devuelve cuando una aplicación mal configurada provoca que el proceso de trabajo genere un error:

![Ventana del explorador que muestra la página 502.5 Error de proceso](troubleshoot/_static/process-failure-page.png)

**500 Error interno del servidor**

La aplicación se inicia, pero un error impide que el servidor complete la solicitud.

Este error se produce dentro del código de la aplicación durante el inicio o mientras se crea una respuesta. La respuesta no puede contener nada o puede aparecer como *500 Error interno del servidor* en el explorador. El registro de eventos de la aplicación normalmente indica que la aplicación se ha iniciado normalmente. Desde la perspectiva del servidor, eso es correcto. La aplicación se inició, pero no puede generar una respuesta válida. [Ejecute la aplicación en la consola de Kudu](#run-the-app-in-the-kudu-console) o [habilite el registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log) para solucionar el problema.

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 Error de inicio en proceso

El proceso de trabajo no funciona. La aplicación no se inicia.

El módulo ASP.NET Core intenta iniciar el proceso CLR de .NET Core, pero no lo consigue. La causa del error de inicio del proceso se suele determinar a partir de las entradas del [registro de eventos de la aplicación](#application-event-log) y del [registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 ANMC no pudo encontrar las dependencias nativas

El proceso de trabajo no funciona. La aplicación no se inicia.

El módulo ASP.NET Core intenta iniciar el proceso del entorno de ejecución de .NET Core, pero no lo consigue. Este error de inicio suele deberse a que el entorno de ejecución de `Microsoft.NETCore.App` o `Microsoft.AspNetCore.App` no está instalado. Si la aplicación se implementa para ASP.NET Core 3.0 y esa versión no existe en el equipo, se produce este error. Este es un mensaje de error de ejemplo:

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

El mensaje de error enumera todas las versiones instaladas de .NET Core y la versión solicitada por la aplicación. Para corregir este error, hay dos posibilidades:

* Instalar la versión adecuada de .NET Core en la máquina.
* Cambiar el destino de la aplicación a una versión de .NET Core que exista en la máquina.
* Publique la aplicación como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd).

Cuando se ejecuta en desarrollo (la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece en `Development`), el error específico se escribe en la respuesta HTTP. La causa de un error de inicio de proceso también se encuentra en el [registro de eventos de la aplicación](#application-event-log).

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 ANCM No se pudo cargar el archivo .dll

El proceso de trabajo no funciona. La aplicación no se inicia.

La causa más común de este error es que la aplicación se haya publicado para una arquitectura de procesador no compatible. Si el proceso de trabajo se ejecuta como una aplicación de 32 bits y la aplicación se diseñó para 64 bits, se produce este error.

Para corregir este error, hay dos posibilidades:

* Volver a publicar la aplicación para la misma arquitectura de procesador que el proceso de trabajo.
* Publicar la aplicación como una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-executables-fde).

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 Error de carga del controlador de solicitud

El proceso de trabajo no funciona. La aplicación no se inicia.

La aplicación no hizo referencia al marco `Microsoft.AspNetCore.App`. Solo las aplicaciones diseñadas para el marco `Microsoft.AspNetCore.App` pueden hospedarse en el módulo ASP.NET Core.

Para corregir este error, confirme que la aplicación tiene como destino el marco `Microsoft.AspNetCore.App`. Compruebe en `.runtimeconfig.json` cuál es el marco de destino de la aplicación.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 ANCM Modelos de hospedaje mixto no admitidos

El proceso de trabajo no puede ejecutar a la vez una aplicación en proceso y una aplicación fuera de proceso.

Para corregir este error, ejecute las aplicaciones en grupos de aplicaciones de IIS independientes.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 ANCM Varias aplicaciones en proceso en el mismo proceso

El proceso de trabajo no puede ejecutar a la vez una aplicación en proceso y una aplicación fuera de proceso.

Para corregir este error, ejecute las aplicaciones en grupos de aplicaciones de IIS independientes.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 Error de carga del controlador fuera de proceso

El controlador de solicitudes de fuera de proceso, *aspnetcorev2_outofprocess.dll*, no está junto al archivo *aspnetcorev2.dll*. Esto indica una instalación dañada del módulo ASP.NET Core.

Para corregir este error, repare la instalación del [conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (para IIS) o Visual Studio (para IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM no se pudo iniciar en el límite de tiempo de inicio

ANCM no se pudo iniciar dentro del límite de tiempo de inicio proporcionado. De forma predeterminada, el tiempo de espera es de 120 segundos.

Este error puede producirse cuando se inicia un gran número de aplicaciones en el mismo equipo. Busque picos de uso de CPU o memoria en el servidor durante el inicio. Es posible que deba escalonar el proceso de inicio de varias aplicaciones.

### <a name="50030-in-process-startup-failure"></a>500.30 Error de inicio en proceso

El proceso de trabajo no funciona. La aplicación no se inicia.

El módulo ASP.NET Core intenta iniciar el proceso del entorno de ejecución de .NET Core, pero no lo consigue. La causa del error de inicio del proceso se suele determinar a partir de las entradas del [registro de eventos de la aplicación](#application-event-log) y del [registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="5000-in-process-handler-load-failure"></a>500.0 Error de carga del controlador en proceso

El proceso de trabajo no funciona. La aplicación no se inicia.

La causa de un error de inicio de proceso también se encuentra en el [registro de eventos de la aplicación](#application-event-log).

::: moniker-end

**Restablecimiento de la conexión**

Si se produce un error después de que se envían los encabezados, el servidor no tiene tiempo para enviar un mensaje **500 Error interno del servidor** cuando se produce un error. Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos en una respuesta. Este tipo de error aparece como un error de *restablecimiento de la conexión* en el cliente. El [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.

## <a name="default-startup-limits"></a>Límites de inicio predeterminados

El módulo ASP.NET Core está configurado con un valor predeterminado *startupTimeLimit* de 120 segundos. Cuando se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registre un error de proceso. Para información sobre la configuración del módulo, consulte [Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Solución de problemas de errores de inicio de la aplicación

### <a name="application-event-log"></a>Registro de eventos de aplicación

Para acceder al registro de eventos de la aplicación, use la hoja **Diagnose and solve problems** (Diagnosticar y solucionar problemas) de Azure Portal:

1. En Azure Portal, abra la aplicación en **App Services**.
1. Seleccione **Diagnosticar y solucionar problemas**.
1. Seleccione el título **Herramientas de diagnóstico**.
1. En **Herramientas de soporte técnico**, seleccione el botón **Eventos de la aplicación**.
1. Examine el error más reciente que hayan proporcionado las entradas *IIS AspNetCoreModule* o *IIS AspNetCoreModule V2* en la columna **Origen**.

Una alternativa al uso de la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) es examinar el archivo de registro de eventos de la aplicación directamente mediante [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;** . Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Abra la carpeta **LogFiles**.
1. Seleccione el icono de lápiz junto al archivo *eventlog.xml*.
1. Examine el registro. Desplácese al final del registro para ver los eventos más recientes.

### <a name="run-the-app-in-the-kudu-console"></a>Ejecución de la aplicación en la consola de Kudu

Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación. Puede ejecutar la aplicación en la consola de ejecución remota de [Kudu](https://github.com/projectkudu/kudu/wiki) para detectar el error:

1. Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;** . Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.

#### <a name="test-a-32-bit-x86-app"></a>Prueba de una aplicación de 32 bits (x86)

##### <a name="current-release"></a>Versión actual

1. `cd d:\home\site\wwwroot`
1. Ejecute la aplicación:
   * Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):

     ```console
     {ASSEMBLY NAME}.exe
     ```

La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Implementación dependiente de marco de trabajo en ejecución en una versión preliminar

*Requiere la instalación de la extensión de sitio ASP.NET Core {VERSION} (x86) Runtime.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` es la versión del runtime)
1. Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.

#### <a name="test-a-64-bit-x64-app"></a>Prueba de una aplicación de 64 bits (x64)

##### <a name="current-release"></a>Versión actual

* Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) de 64 bits (x64):
  1. `cd D:\Program Files\dotnet`
  1. Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):
  1. `cd D:\home\site\wwwroot`
  1. Ejecute la aplicación: `{ASSEMBLY NAME}.exe`

La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Implementación dependiente de marco de trabajo en ejecución en una versión preliminar

*Requiere la instalación de la extensión de sitio ASP.NET Core {VERSION} (x64) Runtime.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` es la versión del runtime)
1. Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.

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
1. Vuelva a Azure Portal. Seleccione la hoja **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;** . Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Seleccione la carpeta **LogFiles**.
1. Inspeccione la columna **Modificado** y seleccione el icono de lápiz para editar el registro de stdout con la última fecha de modificación.
1. Cuando se abre el archivo de registro, se muestra el error.

Deshabilite el registro de stdout una vez haya solucionado los problemas:

1. En la **consola de diagnóstico** de Kudu, vuelva a la ruta de acceso **site** > **wwwroot** para mostrar el archivo *web.config*. Seleccione el icono de lápiz para abrir de nuevo el archivo **web.config**.
1. Establezca **stdoutLogEnabled** en `false`.
1. Seleccione **Save** (Guardar) para guardar el archivo.

> [!WARNING]
> La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor. No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados. Use únicamente el registro de stdout para solucionar problemas de inicio de la aplicación.
>
> Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a>Registro de depuración del módulo de ASP.NET Core

El registro de depuración del módulo de ASP.NET Core ofrece un registro adicional, más profundo, del módulo ASP.NET Core. Para habilitar y ver los registros de stdout:

1. Para habilitar el registro de diagnóstico mejorado, realice cualquiera de las acciones siguientes:
   * Siga las instrucciones de [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) para configurar la aplicación para un registro de diagnóstico mejorado. Vuelva a implementar la aplicación.
   * Agregue la `<handlerSettings>` que se muestra en [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) al archivo *web.config* de la aplicación activa mediante la consola de Kudu:
     1. Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;** . Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
     1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
     1. Abra las carpetas para la ruta de acceso **site** > **wwwroot**. Seleccione el icono de lápiz para editar el archivo *web.config*. Agregue la sección `<handlerSettings>` como se muestra en [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). Seleccione el botón **Guardar**.
1. Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**. Seleccione el botón **Ir&rarr;** . Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
1. Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.
1. Abra las carpetas para la ruta de acceso **site** > **wwwroot**. Si no ha proporcionado una ruta de acceso para el archivo *aspnetcore debug.log*, el archivo aparece en la lista. Si ha proporcionado una ruta de acceso, vaya a la ubicación del archivo de registro.
1. Abra el archivo de registro con el botón de lápiz situado junto al nombre del archivo.

Deshabilite el registro de depuración una vez haya solucionado los problemas:

1. Para deshabilitar el registro de depuración mejorado, realice cualquiera de las acciones siguientes:
   * Quite localmente la `<handlerSettings>` del archivo *web.config* y vuelva a implementar la aplicación.
   * Use la consola de Kudu para editar el archivo *web.config* y quite la sección `<handlerSettings>`. Guarde el archivo.

> [!WARNING]
> Si no deshabilita el registro de depuración, es posible que se produzcan errores en la aplicación o el servidor. No hay ningún límite para el tamaño del archivo de registro. Use únicamente el registro de depuración para solucionar problemas de inicio de la aplicación.
>
> Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros. Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker-end

## <a name="common-startup-errors"></a>Errores comunes de inicio

Vea <xref:host-and-deploy/azure-iis-errors-reference>. La mayoría de los problemas comunes que impiden el inicio de la aplicación se tratan en el tema de referencia.

## <a name="slow-or-hanging-app"></a>Aplicación lenta o bloqueada

Cuando una aplicación responde con lentitud o se bloquea en una solicitud, consulte los artículos siguientes:

* [Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) (Uso de la extensión de sitio de diagnóstico de bloqueo para capturar el volcado de memoria para problemas de excepciones intermitentes o problemas de rendimiento en Azure Web Apps)

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

1. En Azure Portal, seleccione la hoja **Herramientas avanzadas** en el área **HERRAMIENTAS DE DESARROLLO**. Seleccione el botón **Ir&rarr;** . Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.
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

No olvide deshabilitar el registro de stdout cuando finalice la solución de problemas. Consulte las instrucciones de la sección [Registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).

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

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Solucionar los errores HTTP de "502 Puerta de enlace no válida" y "503 Servicio no disponible" en las aplicaciones web de Azure](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Espacio aislado de Azure Web App [limitaciones de ejecución del entono de tiempo de ejecución de App Service])
* [Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
