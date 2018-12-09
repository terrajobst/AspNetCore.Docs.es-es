---
title: Supervisar y depurar - DevOps con ASP.NET Core y Azure
author: CamSoper
description: Supervisión y depurar el código como parte de una solución de DevOps con ASP.NET Core y Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: e005b951aec578b396fc19dec5d2f55cbce4f664
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121614"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="aae10-103">Supervisar y depurar</span><span class="sxs-lookup"><span data-stu-id="aae10-103">Monitor and debug</span></span>

<span data-ttu-id="aae10-104">Al haber implementado la aplicación y crea una canalización de DevOps, es importante comprender cómo supervisar y solucionar problemas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aae10-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="aae10-105">En esta sección, deberá completar las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="aae10-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="aae10-106">Buscar básicas de supervisión y solución de problemas de datos en Azure portal</span><span class="sxs-lookup"><span data-stu-id="aae10-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="aae10-107">Obtenga información sobre cómo Azure Monitor proporciona una visión más profunda de las métricas en todos los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="aae10-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="aae10-108">Conectar la aplicación web con Application Insights para la generación de perfiles de aplicación</span><span class="sxs-lookup"><span data-stu-id="aae10-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="aae10-109">Activar el registro y obtenga información sobre dónde descargar los registros</span><span class="sxs-lookup"><span data-stu-id="aae10-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="aae10-110">Stream se registra en tiempo real</span><span class="sxs-lookup"><span data-stu-id="aae10-110">Stream logs in real time</span></span>
* <span data-ttu-id="aae10-111">Obtenga información sobre dónde establecer alertas</span><span class="sxs-lookup"><span data-stu-id="aae10-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="aae10-112">Obtenga información acerca de Azure App Service web apps depuración remotas.</span><span class="sxs-lookup"><span data-stu-id="aae10-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="aae10-113">Supervisión básica y solución de problemas</span><span class="sxs-lookup"><span data-stu-id="aae10-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="aae10-114">App Service web apps se supervisan con facilidad en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="aae10-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="aae10-115">El portal de Azure procesa las métricas en gráficos fáciles de entender y gráficos.</span><span class="sxs-lookup"><span data-stu-id="aae10-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="aae10-116">Abra el [portal de Azure](https://portal.azure.com)y, a continuación, navegue hasta la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="aae10-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="aae10-117">El **Introducción** ficha muestra información de "rápida" útil, incluidos gráficos de mostrar las métricas recientes.</span><span class="sxs-lookup"><span data-stu-id="aae10-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![Panel de captura de pantalla que muestra información general](./media/monitoring/overview.png)

    * <span data-ttu-id="aae10-119">**Http 5xx**: recuento de errores de servidor, normalmente excepciones en el código de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aae10-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="aae10-120">**Datos de entrada**: entrada de datos que entran en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="aae10-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="aae10-121">**Datos de salida**: datos de la salida desde la aplicación web a los clientes.</span><span class="sxs-lookup"><span data-stu-id="aae10-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="aae10-122">**Las solicitudes**: número de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="aae10-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="aae10-123">**Tiempo promedio de respuesta**: promedio de tiempo para la aplicación web responder a las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="aae10-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="aae10-124">También se encuentran varias herramientas de autoservicio para la optimización y solución de problemas en esta página.</span><span class="sxs-lookup"><span data-stu-id="aae10-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![Herramientas de autoservicio de captura de pantalla que muestra](./media/monitoring/wizards.png)

    * <span data-ttu-id="aae10-126">**Diagnosticar y resolver problemas** es un solucionador de problemas de autoservicio.</span><span class="sxs-lookup"><span data-stu-id="aae10-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="aae10-127">**Application Insights** es para la generación de perfiles de rendimiento y el comportamiento de la aplicación y se describe más adelante en esta sección.</span><span class="sxs-lookup"><span data-stu-id="aae10-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="aae10-128">**App Service Advisor** hace recomendaciones para optimizar su experiencia de aplicación.</span><span class="sxs-lookup"><span data-stu-id="aae10-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="aae10-129">Supervisión avanzada</span><span class="sxs-lookup"><span data-stu-id="aae10-129">Advanced monitoring</span></span>

<span data-ttu-id="aae10-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) es el servicio centralizado para todas las métricas de supervisión y configuración de alertas a través de servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="aae10-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="aae10-131">Dentro de Azure Monitor, los administradores pueden realizar un seguimiento de rendimiento forma granular e identificar tendencias.</span><span class="sxs-lookup"><span data-stu-id="aae10-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="aae10-132">Cada servicio de Azure ofrece su propio [conjunto de métricas](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) a Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="aae10-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="aae10-133">Perfil con Application Insights</span><span class="sxs-lookup"><span data-stu-id="aae10-133">Profile with Application Insights</span></span>

<span data-ttu-id="aae10-134">[Application Insights](/azure/application-insights/app-insights-overview) es un servicio de Azure para analizar el rendimiento y la estabilidad de las aplicaciones web y cómo usar los usuarios.</span><span class="sxs-lookup"><span data-stu-id="aae10-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="aae10-135">Los datos de Application Insights son más amplias y profundas que el de Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="aae10-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="aae10-136">Los datos pueden proporcionar a los desarrolladores y administradores con información de clave para mejorar las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="aae10-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="aae10-137">Application Insights pueden agregarse a un recurso de Azure App Service sin cambios de código.</span><span class="sxs-lookup"><span data-stu-id="aae10-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="aae10-138">Abra el [portal de Azure](https://portal.azure.com)y, a continuación, navegue hasta la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="aae10-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="aae10-139">Desde el **Introducción** pestaña, haga clic en el **Application Insights** icono.</span><span class="sxs-lookup"><span data-stu-id="aae10-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Icono Application Insights](./media/monitoring/app-insights.png)

1. <span data-ttu-id="aae10-141">Seleccione el **crear recurso nuevo** botón de radio.</span><span class="sxs-lookup"><span data-stu-id="aae10-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="aae10-142">Use el nombre de recurso predeterminado y seleccione la ubicación del recurso de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="aae10-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="aae10-143">La ubicación no tiene que coincidir con el de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="aae10-143">The location doesn't need to match that of your web app.</span></span>

    ![El programa de instalación de Application Insights](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="aae10-145">Para **en tiempo de ejecución o marco**, seleccione **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="aae10-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="aae10-146">Acepte la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="aae10-146">Accept the default settings.</span></span>
1. <span data-ttu-id="aae10-147">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="aae10-147">Select **OK**.</span></span> <span data-ttu-id="aae10-148">Si se le pedirá que confirme, seleccione **continuar**.</span><span class="sxs-lookup"><span data-stu-id="aae10-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="aae10-149">Una vez creado el recurso, haga clic en el nombre del recurso de Application Insights para navegar directamente a la página de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="aae10-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![Nuevo recurso de Application Insights está listo](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="aae10-151">Se usa la aplicación, se acumulan datos.</span><span class="sxs-lookup"><span data-stu-id="aae10-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="aae10-152">Seleccione **actualizar** para volver a cargar la hoja con nuevos datos.</span><span class="sxs-lookup"><span data-stu-id="aae10-152">Select **Refresh** to reload the blade with new data.</span></span>

![Ficha información general de Application Insights](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="aae10-154">Application Insights proporciona información útil de servidor sin ninguna configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="aae10-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="aae10-155">Para obtener el máximo partido de Application Insights, [instrumentar la aplicación con el SDK de Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="aae10-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="aae10-156">Cuando se configura correctamente, el servicio proporciona la supervisión de extremo a otro entre el servidor web y el explorador, incluidos el rendimiento del cliente.</span><span class="sxs-lookup"><span data-stu-id="aae10-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="aae10-157">Para obtener más información, consulte el [documentación de Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="aae10-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="aae10-158">Registro</span><span class="sxs-lookup"><span data-stu-id="aae10-158">Logging</span></span>

<span data-ttu-id="aae10-159">Los registros de aplicación y el servidor Web están deshabilitados de forma predeterminada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="aae10-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="aae10-160">Habilitar los registros con los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="aae10-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="aae10-161">Abra el [portal de Azure](https://portal.azure.com)y navegue hasta la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="aae10-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="aae10-162">En el menú a la izquierda, desplácese hacia abajo hasta la **supervisión** sección.</span><span class="sxs-lookup"><span data-stu-id="aae10-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="aae10-163">Seleccione **los registros de diagnóstico**.</span><span class="sxs-lookup"><span data-stu-id="aae10-163">Select **Diagnostics logs**.</span></span>

    ![Vínculo de registros de diagnóstico](./media/monitoring/logging.png)

1. <span data-ttu-id="aae10-165">Activar **registro de la aplicación (Filesystem)**.</span><span class="sxs-lookup"><span data-stu-id="aae10-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="aae10-166">Si se le solicite, haga clic en el cuadro para instalar las extensiones para habilitar el registro en la aplicación web de aplicación.</span><span class="sxs-lookup"><span data-stu-id="aae10-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="aae10-167">Establecer **registro del servidor Web** a **sistema de archivos**.</span><span class="sxs-lookup"><span data-stu-id="aae10-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="aae10-168">Escriba el **período de retención** en días.</span><span class="sxs-lookup"><span data-stu-id="aae10-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="aae10-169">Por ejemplo, 30.</span><span class="sxs-lookup"><span data-stu-id="aae10-169">For example, 30.</span></span>
1. <span data-ttu-id="aae10-170">Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="aae10-170">Click **Save**.</span></span>

<span data-ttu-id="aae10-171">Se generan registros de servidor (App Service) de ASP.NET Core y web para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="aae10-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="aae10-172">Se pueden descargar mediante la información de FTP o FTPS que aparece.</span><span class="sxs-lookup"><span data-stu-id="aae10-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="aae10-173">La contraseña es el mismo que las credenciales de implementación que creó anteriormente en esta guía.</span><span class="sxs-lookup"><span data-stu-id="aae10-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="aae10-174">Los registros pueden ser [transmite directamente a la máquina local con PowerShell o CLI de Azure](/azure/app-service/web-sites-enable-diagnostic-log#download).</span><span class="sxs-lookup"><span data-stu-id="aae10-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="aae10-175">Los registros también pueden ser [ven en Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span><span class="sxs-lookup"><span data-stu-id="aae10-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="aae10-176">Secuencias de registro</span><span class="sxs-lookup"><span data-stu-id="aae10-176">Log streaming</span></span>

<span data-ttu-id="aae10-177">Registros de servidor web y de aplicación se pueden transmitir en tiempo real a través del portal.</span><span class="sxs-lookup"><span data-stu-id="aae10-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="aae10-178">Abra el [portal de Azure](https://portal.azure.com)y navegue hasta la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="aae10-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="aae10-179">En el menú a la izquierda, desplácese hacia abajo hasta la **supervisión** sección y seleccione **secuencia de registro**.</span><span class="sxs-lookup"><span data-stu-id="aae10-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![Vínculo de la secuencia de registro de captura de pantalla que muestra](./media/monitoring/log-stream.png)

<span data-ttu-id="aae10-181">Los registros también pueden ser [transmite a través de CLI de Azure o Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), incluido a través de Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="aae10-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="aae10-182">Alertas</span><span class="sxs-lookup"><span data-stu-id="aae10-182">Alerts</span></span>

<span data-ttu-id="aae10-183">Azure Monitor proporciona también [alertas en tiempo real](/azure/monitoring-and-diagnostics/insights-alerts-portal) basadas en métricas, eventos administrativos y otros criterios.</span><span class="sxs-lookup"><span data-stu-id="aae10-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="aae10-184">*Nota: Solo está disponible en el servicio de alertas (clásico) actualmente alertas de métricas de la aplicación web.*</span><span class="sxs-lookup"><span data-stu-id="aae10-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="aae10-185">El [alertas (clásico) servicio](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) puede encontrarse en Azure Monitor o en el **supervisión** sección de la configuración de App Service.</span><span class="sxs-lookup"><span data-stu-id="aae10-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![Vínculo de alertas (clásico)](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="aae10-187">Depuración activa</span><span class="sxs-lookup"><span data-stu-id="aae10-187">Live debugging</span></span>

<span data-ttu-id="aae10-188">Azure App Service puede ser [depurar de manera remota con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) cuando los registros no proporcionan suficiente información.</span><span class="sxs-lookup"><span data-stu-id="aae10-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="aae10-189">Sin embargo, la depuración remota requiere que la aplicación para compilarse con símbolos de depuración.</span><span class="sxs-lookup"><span data-stu-id="aae10-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="aae10-190">Depuración no debería realizarse en producción, excepto como último recurso.</span><span class="sxs-lookup"><span data-stu-id="aae10-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="aae10-191">Conclusión</span><span class="sxs-lookup"><span data-stu-id="aae10-191">Conclusion</span></span>

<span data-ttu-id="aae10-192">En esta sección, se completó las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="aae10-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="aae10-193">Buscar básicas de supervisión y solución de problemas de datos en Azure portal</span><span class="sxs-lookup"><span data-stu-id="aae10-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="aae10-194">Obtenga información sobre cómo Azure Monitor proporciona una visión más profunda de las métricas en todos los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="aae10-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="aae10-195">Conectar la aplicación web con Application Insights para la generación de perfiles de aplicación</span><span class="sxs-lookup"><span data-stu-id="aae10-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="aae10-196">Activar el registro y obtenga información sobre dónde descargar los registros</span><span class="sxs-lookup"><span data-stu-id="aae10-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="aae10-197">Stream se registra en tiempo real</span><span class="sxs-lookup"><span data-stu-id="aae10-197">Stream logs in real time</span></span>
* <span data-ttu-id="aae10-198">Obtenga información sobre dónde establecer alertas</span><span class="sxs-lookup"><span data-stu-id="aae10-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="aae10-199">Obtenga información acerca de Azure App Service web apps depuración remotas.</span><span class="sxs-lookup"><span data-stu-id="aae10-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="aae10-200">Lecturas adicionales</span><span class="sxs-lookup"><span data-stu-id="aae10-200">Additional reading</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="aae10-201">Supervisar el rendimiento de la aplicación web de Azure con Application Insights</span><span class="sxs-lookup"><span data-stu-id="aae10-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="aae10-202">Habilitar el registro de diagnósticos para las aplicaciones web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="aae10-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="aae10-203">Solución de problemas de una aplicación web en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aae10-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="aae10-204">Creación de alertas de métricas clásicas en Azure Monitor para servicios de Azure - portal de Azure</span><span class="sxs-lookup"><span data-stu-id="aae10-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
