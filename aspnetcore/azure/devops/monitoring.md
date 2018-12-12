---
title: Supervisar y depurar - DevOps con ASP.NET Core y Azure
author: CamSoper
description: Supervisión y depurar el código como parte de una solución de DevOps con ASP.NET Core y Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: 00489bd92dfff8fd80bd24c2e60193d32031d7c4
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284375"
---
# <a name="monitor-and-debug"></a>Supervisar y depurar

Al haber implementado la aplicación y crea una canalización de DevOps, es importante comprender cómo supervisar y solucionar problemas de la aplicación.

En esta sección, deberá completar las tareas siguientes:

* Buscar básicas de supervisión y solución de problemas de datos en Azure portal
* Obtenga información sobre cómo Azure Monitor proporciona una visión más profunda de las métricas en todos los servicios de Azure
* Conectar la aplicación web con Application Insights para la generación de perfiles de aplicación
* Activar el registro y obtenga información sobre dónde descargar los registros
* Stream se registra en tiempo real
* Obtenga información sobre dónde establecer alertas
* Obtenga información acerca de Azure App Service web apps depuración remotas.

## <a name="basic-monitoring-and-troubleshooting"></a>Supervisión básica y solución de problemas

App Service web apps se supervisan con facilidad en tiempo real. El portal de Azure procesa las métricas en gráficos fáciles de entender y gráficos.

1. Abra el [portal de Azure](https://portal.azure.com)y, a continuación, navegue hasta la *mywebapp\<unique_number\>*  App Service.

1. El **Introducción** ficha muestra información de "rápida" útil, incluidos gráficos de mostrar las métricas recientes.

    ![Panel de captura de pantalla que muestra información general](./media/monitoring/overview.png)

    * **Http 5xx**: Recuento de errores de servidor, normalmente excepciones en el código de ASP.NET Core.
    * **Datos de**: Entrada de datos que entran en la aplicación web.
    * **Datos de salida**: Salida de datos de la aplicación web a los clientes.
    * **Las solicitudes**: Número de solicitudes HTTP.
    * **Tiempo promedio de respuesta**: Tiempo medio de la aplicación web responder a las solicitudes HTTP.

    También se encuentran varias herramientas de autoservicio para la optimización y solución de problemas en esta página.

    ![Herramientas de autoservicio de captura de pantalla que muestra](./media/monitoring/wizards.png)

    * **Diagnosticar y resolver problemas** es un solucionador de problemas de autoservicio.
    * **Application Insights** es para la generación de perfiles de rendimiento y el comportamiento de la aplicación y se describe más adelante en esta sección.
    * **App Service Advisor** hace recomendaciones para optimizar su experiencia de aplicación.

## <a name="advanced-monitoring"></a>Supervisión avanzada

[Azure Monitor](/azure/monitoring-and-diagnostics/) es el servicio centralizado para todas las métricas de supervisión y configuración de alertas a través de servicios de Azure. Dentro de Azure Monitor, los administradores pueden realizar un seguimiento de rendimiento forma granular e identificar tendencias. Cada servicio de Azure ofrece su propio [conjunto de métricas](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) a Azure Monitor.

## <a name="profile-with-application-insights"></a>Perfil con Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) es un servicio de Azure para analizar el rendimiento y la estabilidad de las aplicaciones web y cómo usar los usuarios. Los datos de Application Insights son más amplias y profundas que el de Azure Monitor. Los datos pueden proporcionar a los desarrolladores y administradores con información de clave para mejorar las aplicaciones. Application Insights pueden agregarse a un recurso de Azure App Service sin cambios de código.

1. Abra el [portal de Azure](https://portal.azure.com)y, a continuación, navegue hasta la *mywebapp\<unique_number\>*  App Service.
1. Desde el **Introducción** pestaña, haga clic en el **Application Insights** icono.

    ![Icono Application Insights](./media/monitoring/app-insights.png)

1. Seleccione el **crear recurso nuevo** botón de radio. Use el nombre de recurso predeterminado y seleccione la ubicación del recurso de Application Insights. La ubicación no tiene que coincidir con el de la aplicación web.

    ![El programa de instalación de Application Insights](./media/monitoring/new-app-insights.png)

1. Para **en tiempo de ejecución o marco**, seleccione **ASP.NET Core**. Acepte la configuración predeterminada.
1. Seleccione **Aceptar**. Si se le pedirá que confirme, seleccione **continuar**.
1. Una vez creado el recurso, haga clic en el nombre del recurso de Application Insights para navegar directamente a la página de Application Insights.

    ![Nuevo recurso de Application Insights está listo](./media/monitoring/new-app-insights-done.png)

Se usa la aplicación, se acumulan datos. Seleccione **actualizar** para volver a cargar la hoja con nuevos datos.

![Ficha información general de Application Insights](./media/monitoring/app-insights-overview.png)

Application Insights proporciona información útil de servidor sin ninguna configuración adicional. Para obtener el máximo partido de Application Insights, [instrumentar la aplicación con el SDK de Application Insights](/azure/application-insights/app-insights-asp-net-core). Cuando se configura correctamente, el servicio proporciona la supervisión de extremo a otro entre el servidor web y el explorador, incluidos el rendimiento del cliente. Para obtener más información, consulte el [documentación de Application Insights](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Registro

Los registros de aplicación y el servidor Web están deshabilitados de forma predeterminada en Azure App Service. Habilitar los registros con los pasos siguientes:

1. Abra el [portal de Azure](https://portal.azure.com)y navegue hasta la *mywebapp\<unique_number\>*  App Service.
1. En el menú a la izquierda, desplácese hacia abajo hasta la **supervisión** sección. Seleccione **los registros de diagnóstico**.

    ![Vínculo de registros de diagnóstico](./media/monitoring/logging.png)

1. Activar **registro de la aplicación (Filesystem)**. Si se le solicite, haga clic en el cuadro para instalar las extensiones para habilitar el registro en la aplicación web de aplicación.
1. Establecer **registro del servidor Web** a **sistema de archivos**.
1. Escriba el **período de retención** en días. Por ejemplo, 30.
1. Haga clic en **Guardar**.

Se generan registros de servidor (App Service) de ASP.NET Core y web para la aplicación web. Se pueden descargar mediante la información de FTP o FTPS que aparece. La contraseña es el mismo que las credenciales de implementación que creó anteriormente en esta guía. Los registros pueden ser [transmite directamente a la máquina local con PowerShell o CLI de Azure](/azure/app-service/web-sites-enable-diagnostic-log#download). Los registros también pueden ser [ven en Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Secuencias de registro

Registros de servidor web y de aplicación se pueden transmitir en tiempo real a través del portal.

1. Abra el [portal de Azure](https://portal.azure.com)y navegue hasta la *mywebapp\<unique_number\>*  App Service.
1. En el menú a la izquierda, desplácese hacia abajo hasta la **supervisión** sección y seleccione **secuencia de registro**.

    ![Vínculo de la secuencia de registro de captura de pantalla que muestra](./media/monitoring/log-stream.png)

Los registros también pueden ser [transmite a través de CLI de Azure o Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), incluido a través de Cloud Shell.

## <a name="alerts"></a>Alertas

Azure Monitor proporciona también [alertas en tiempo real](/azure/monitoring-and-diagnostics/insights-alerts-portal) basadas en métricas, eventos administrativos y otros criterios.

> *Nota: Actualmente, las alertas en métricas de la aplicación web solo está disponible en el servicio de alertas (clásico).*

El [alertas (clásico) servicio](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) puede encontrarse en Azure Monitor o en el **supervisión** sección de la configuración de App Service.

![Vínculo de alertas (clásico)](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Depuración activa

Azure App Service puede ser [depurar de manera remota con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) cuando los registros no proporcionan suficiente información. Sin embargo, la depuración remota requiere que la aplicación para compilarse con símbolos de depuración. Depuración no debería realizarse en producción, excepto como último recurso.

## <a name="conclusion"></a>Conclusión

En esta sección, se completó las tareas siguientes:

* Buscar básicas de supervisión y solución de problemas de datos en Azure portal
* Obtenga información sobre cómo Azure Monitor proporciona una visión más profunda de las métricas en todos los servicios de Azure
* Conectar la aplicación web con Application Insights para la generación de perfiles de aplicación
* Activar el registro y obtenga información sobre dónde descargar los registros
* Stream se registra en tiempo real
* Obtenga información sobre dónde establecer alertas
* Obtenga información acerca de Azure App Service web apps depuración remotas.

## <a name="additional-reading"></a>Lecturas adicionales

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Supervisar el rendimiento de la aplicación web de Azure con Application Insights](/azure/application-insights/app-insights-azure-web-apps)
* [Habilitar el registro de diagnósticos para las aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)
* [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Creación de alertas de métricas clásicas en Azure Monitor para servicios de Azure - portal de Azure](/azure/monitoring-and-diagnostics/insights-alerts-portal)
