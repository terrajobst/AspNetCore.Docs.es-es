---
title: Solución de problemas de ASP.NET Core en IIS
author: guardrex
description: Aprenda a diagnosticar problemas con las implementaciones de Internet Information Services (IIS) de aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2f36ae2bda8537e91a3bc925505986bdd6a22a47
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841558"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="649c2-103">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="649c2-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="649c2-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="649c2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="649c2-105">En este artículo se proporcionan instrucciones sobre cómo diagnosticar un problema de inicio de ASP.NET Core al hospedarse con [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="649c2-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="649c2-106">La información de este artículo se aplica al hospedaje en IIS en Windows Server y el escritorio de Windows.</span><span class="sxs-lookup"><span data-stu-id="649c2-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="649c2-107">En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="649c2-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="649c2-108">El código de estado *502.5 - Error de proceso* o *500.30 - Error de inicio* que se produce al efectuar la depuración localmente se puede solucionar si se sigue el consejo de este tema.</span><span class="sxs-lookup"><span data-stu-id="649c2-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="649c2-109">En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="649c2-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="649c2-110">El código de estado *502.5 Error de proceso* que se produce al realizar la depuración localmente se puede solucionar si se sigue el consejo de este tema.</span><span class="sxs-lookup"><span data-stu-id="649c2-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="649c2-111">Temas adicionales de solución de problemas:</span><span class="sxs-lookup"><span data-stu-id="649c2-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="649c2-112">Aunque App Service usa el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e IIS para hospedar las aplicaciones, consulte el tema dedicado para obtener instrucciones específicas.</span><span class="sxs-lookup"><span data-stu-id="649c2-112">Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="649c2-113">Descubra cómo controlar los errores de aplicaciones de ASP.NET Core durante el desarrollo en un sistema local.</span><span class="sxs-lookup"><span data-stu-id="649c2-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="649c2-114">Información sobre cómo depurar con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="649c2-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="649c2-115">En este tema se presentan las características del depurador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="649c2-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="649c2-116">Depuración con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="649c2-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="649c2-117">Obtenga información sobre la compatibilidad de depuración integrada en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="649c2-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="649c2-118">Errores de inicio de aplicación</span><span class="sxs-lookup"><span data-stu-id="649c2-118">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="649c2-119">502.5 Error de proceso</span><span class="sxs-lookup"><span data-stu-id="649c2-119">502.5 Process Failure</span></span>

<span data-ttu-id="649c2-120">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="649c2-120">The worker process fails.</span></span> <span data-ttu-id="649c2-121">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="649c2-121">The app doesn't start.</span></span>

<span data-ttu-id="649c2-122">El módulo ASP.NET Core intenta iniciar el proceso dotnet de back-end, pero no lo consigue.</span><span class="sxs-lookup"><span data-stu-id="649c2-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="649c2-123">La causa del error de inicio del proceso se suele determinar a partir de las entradas del [registro de eventos de la aplicación](#application-event-log) y del [registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="649c2-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="649c2-124">Una condición de error habitual es que la aplicación esté mal configurada porque tiene como destino una versión del marco compartido de ASP.NET Core que no está presente.</span><span class="sxs-lookup"><span data-stu-id="649c2-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="649c2-125">Compruebe qué versiones del marco compartido de ASP.NET Core están instaladas en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="649c2-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="649c2-126">La página de error *502.5 Error de proceso* se devuelve cuando el proceso de trabajo no se puede iniciar debido a un error de configuración de la aplicación o del hospedaje:</span><span class="sxs-lookup"><span data-stu-id="649c2-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Ventana del explorador que muestra la página 502.5 Error de proceso](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="649c2-128">500.30 Error de inicio en proceso</span><span class="sxs-lookup"><span data-stu-id="649c2-128">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="649c2-129">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="649c2-129">The worker process fails.</span></span> <span data-ttu-id="649c2-130">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="649c2-130">The app doesn't start.</span></span>

<span data-ttu-id="649c2-131">El módulo ASP.NET Core intenta iniciar el proceso CLR de .NET Core, pero no lo consigue.</span><span class="sxs-lookup"><span data-stu-id="649c2-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="649c2-132">La causa del error de inicio del proceso se suele determinar a partir de las entradas del [registro de eventos de la aplicación](#application-event-log) y del [registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="649c2-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="649c2-133">Una condición de error habitual es que la aplicación esté mal configurada porque tiene como destino una versión del marco compartido de ASP.NET Core que no está presente.</span><span class="sxs-lookup"><span data-stu-id="649c2-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="649c2-134">Compruebe qué versiones del marco compartido de ASP.NET Core están instaladas en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="649c2-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="649c2-135">500.0 Error de carga del controlador en proceso</span><span class="sxs-lookup"><span data-stu-id="649c2-135">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="649c2-136">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="649c2-136">The worker process fails.</span></span> <span data-ttu-id="649c2-137">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="649c2-137">The app doesn't start.</span></span>

<span data-ttu-id="649c2-138">El módulo ASP.NET Core no logra encontrar el CLR de .NET Core y el controlador de solicitudes en proceso (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="649c2-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="649c2-139">Compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="649c2-139">Check that:</span></span>

* <span data-ttu-id="649c2-140">Que la aplicación tiene como destino el paquete de NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="649c2-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="649c2-141">Que la versión del marco compartido de ASP.NET Core que la aplicación tiene como destino esté instalada en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="649c2-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="649c2-142">500.0 Error de carga del controlador fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="649c2-142">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="649c2-143">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="649c2-143">The worker process fails.</span></span> <span data-ttu-id="649c2-144">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="649c2-144">The app doesn't start.</span></span>

<span data-ttu-id="649c2-145">El módulo ASP.NET Core no logra encontrar el controlador de solicitudes de hospedaje fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="649c2-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="649c2-146">Asegúrese de que el archivo *aspnetcorev2_outofprocess.dll* está presente en una subcarpeta junto a *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="649c2-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="649c2-147">500 Error interno del servidor</span><span class="sxs-lookup"><span data-stu-id="649c2-147">500 Internal Server Error</span></span>

<span data-ttu-id="649c2-148">La aplicación se inicia, pero un error impide que el servidor complete la solicitud.</span><span class="sxs-lookup"><span data-stu-id="649c2-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="649c2-149">Este error se produce dentro del código de la aplicación durante el inicio o mientras se crea una respuesta.</span><span class="sxs-lookup"><span data-stu-id="649c2-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="649c2-150">La respuesta no puede contener nada o puede aparecer como *500 Error interno del servidor* en el explorador.</span><span class="sxs-lookup"><span data-stu-id="649c2-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="649c2-151">El registro de eventos de la aplicación normalmente indica que la aplicación se ha iniciado normalmente.</span><span class="sxs-lookup"><span data-stu-id="649c2-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="649c2-152">Desde la perspectiva del servidor, eso es correcto.</span><span class="sxs-lookup"><span data-stu-id="649c2-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="649c2-153">La aplicación se inició, pero no puede generar una respuesta válida.</span><span class="sxs-lookup"><span data-stu-id="649c2-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="649c2-154">[Ejecute la aplicación en un símbolo del sistema](#run-the-app-at-a-command-prompt) en el servidor o [habilite el registro de stdout del módulo ASP.NET Core](#aspnet-core-module-stdout-log) para solucionar el problema.</span><span class="sxs-lookup"><span data-stu-id="649c2-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="649c2-155">No se pudo iniciar la aplicación (código de error "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="649c2-155">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="649c2-156">Error al iniciar la aplicación porque el ensamblado de la aplicación (*.dll*) no se ha podido cargar.</span><span class="sxs-lookup"><span data-stu-id="649c2-156">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="649c2-157">Este error se produce cuando hay un error de coincidencia del valor de bits entre la aplicación publicada y el proceso w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="649c2-157">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="649c2-158">Confirme que la opción de 32 bits del grupo de aplicaciones sea correcta:</span><span class="sxs-lookup"><span data-stu-id="649c2-158">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="649c2-159">Seleccione el grupo de aplicaciones en **Grupos de aplicaciones** del Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="649c2-159">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="649c2-160">Seleccione **Configuración avanzada** en **Modificar grupo de aplicaciones**, en el panel **Acciones**.</span><span class="sxs-lookup"><span data-stu-id="649c2-160">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="649c2-161">Establezca **Habilitar aplicaciones de 32 bits**:</span><span class="sxs-lookup"><span data-stu-id="649c2-161">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="649c2-162">Si implementa una aplicación de 32 bits (x86), establezca el valor en `True`.</span><span class="sxs-lookup"><span data-stu-id="649c2-162">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="649c2-163">Si implementa una aplicación de 64 bits (x64), establezca el valor en `False`.</span><span class="sxs-lookup"><span data-stu-id="649c2-163">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="649c2-164">Restablecimiento de la conexión</span><span class="sxs-lookup"><span data-stu-id="649c2-164">Connection reset</span></span>

<span data-ttu-id="649c2-165">Si se produce un error después de que se envían los encabezados, el servidor no tiene tiempo para enviar un mensaje **500 Error interno del servidor** cuando se produce un error.</span><span class="sxs-lookup"><span data-stu-id="649c2-165">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="649c2-166">Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos en una respuesta.</span><span class="sxs-lookup"><span data-stu-id="649c2-166">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="649c2-167">Este tipo de error aparece como un error de *restablecimiento de la conexión* en el cliente.</span><span class="sxs-lookup"><span data-stu-id="649c2-167">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="649c2-168">El [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="649c2-168">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="649c2-169">Límites de inicio predeterminados</span><span class="sxs-lookup"><span data-stu-id="649c2-169">Default startup limits</span></span>

<span data-ttu-id="649c2-170">El módulo ASP.NET Core está configurado con un valor predeterminado *startupTimeLimit* de 120 segundos.</span><span class="sxs-lookup"><span data-stu-id="649c2-170">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="649c2-171">Cuando se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registre un error de proceso.</span><span class="sxs-lookup"><span data-stu-id="649c2-171">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="649c2-172">Para información sobre la configuración del módulo, consulte [Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="649c2-172">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="649c2-173">Solución de problemas de errores de inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="649c2-173">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="649c2-174">Habilitar el registro de depuración del módulo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="649c2-174">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="649c2-175">Agregue la siguiente configuración de controlador al archivo *web.config* de la aplicación para habilitar los registros de depuración del módulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="649c2-175">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="649c2-176">Confirme que la ruta de acceso especificada para el registro exista y que la identidad del grupo de aplicaciones tenga permisos de escritura en la ubicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-176">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="649c2-177">Registro de eventos de aplicación</span><span class="sxs-lookup"><span data-stu-id="649c2-177">Application Event Log</span></span>

<span data-ttu-id="649c2-178">Acceda al registro de eventos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="649c2-178">Access the Application Event Log:</span></span>

1. <span data-ttu-id="649c2-179">Abra el menú Inicio, busque **Visor de eventos** y luego seleccione la aplicación **Visor de eventos**.</span><span class="sxs-lookup"><span data-stu-id="649c2-179">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="649c2-180">En **Visor de eventos**, abra el nodo **Registros de Windows**.</span><span class="sxs-lookup"><span data-stu-id="649c2-180">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="649c2-181">Seleccione **Aplicación** para abrir el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-181">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="649c2-182">Busque los errores asociados a la aplicación objeto del error.</span><span class="sxs-lookup"><span data-stu-id="649c2-182">Search for errors associated with the failing app.</span></span> <span data-ttu-id="649c2-183">Los errores tienen un valor de *Módulo AspNetCore de IIS* o *Módulo AspNetCore de IIS Express* en la columna *Origen*.</span><span class="sxs-lookup"><span data-stu-id="649c2-183">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="649c2-184">Ejecución de la aplicación en un símbolo del sistema</span><span class="sxs-lookup"><span data-stu-id="649c2-184">Run the app at a command prompt</span></span>

<span data-ttu-id="649c2-185">Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-185">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="649c2-186">La causa de algunos errores se puede encontrar mediante la ejecución de la aplicación en un símbolo del sistema en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="649c2-186">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="649c2-187">Implementación dependiente de marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="649c2-187">Framework-dependent deployment</span></span>

<span data-ttu-id="649c2-188">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="649c2-188">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="649c2-189">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute la aplicación mediante la ejecución del ensamblado de la aplicación con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="649c2-189">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="649c2-190">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="649c2-190">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="649c2-191">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="649c2-191">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="649c2-192">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="649c2-192">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="649c2-193">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="649c2-193">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="649c2-194">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-194">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="649c2-195">Implementación autocontenida</span><span class="sxs-lookup"><span data-stu-id="649c2-195">Self-contained deployment</span></span>

<span data-ttu-id="649c2-196">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="649c2-196">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="649c2-197">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute el archivo ejecutable de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-197">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="649c2-198">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="649c2-198">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="649c2-199">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="649c2-199">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="649c2-200">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="649c2-200">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="649c2-201">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="649c2-201">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="649c2-202">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-202">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="649c2-203">Registro de stdout del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="649c2-203">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="649c2-204">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="649c2-204">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="649c2-205">Vaya a la carpeta de implementación del sitio en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="649c2-205">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="649c2-206">Si la carpeta *Logs* no existe, cree la carpeta.</span><span class="sxs-lookup"><span data-stu-id="649c2-206">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="649c2-207">Para obtener instrucciones sobre cómo habilitar MSBuild para crear la carpeta *logs* automáticamente en la implementación, consulte el tema [Estructura de directorios](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="649c2-207">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="649c2-208">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="649c2-208">Edit the *web.config* file.</span></span> <span data-ttu-id="649c2-209">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** para que apunte a la carpeta *logs* (por ejemplo, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="649c2-209">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="649c2-210">`stdout` en la ruta de acceso es el prefijo del nombre del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="649c2-210">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="649c2-211">Cuando se crea el registro, se agregan automáticamente una marca de tiempo, un identificador de proceso y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="649c2-211">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="649c2-212">Cuando se usa `stdout` como prefijo para el nombre de archivo, un archivo de registro se llama normalmente *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="649c2-212">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="649c2-213">Asegúrese de que la identidad del grupo de aplicaciones tiene permisos de escritura en la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="649c2-213">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="649c2-214">Guarde el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="649c2-214">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="649c2-215">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-215">Make a request to the app.</span></span>
1. <span data-ttu-id="649c2-216">Vaya a la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="649c2-216">Navigate to the *logs* folder.</span></span> <span data-ttu-id="649c2-217">Busque y abra el registro más reciente de stdout.</span><span class="sxs-lookup"><span data-stu-id="649c2-217">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="649c2-218">Estudie el registro para ver los errores.</span><span class="sxs-lookup"><span data-stu-id="649c2-218">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="649c2-219">Deshabilite el registro de stdout cuando la solución de problemas haya finalizado.</span><span class="sxs-lookup"><span data-stu-id="649c2-219">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="649c2-220">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="649c2-220">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="649c2-221">Establezca **stdoutLogEnabled** en `false`.</span><span class="sxs-lookup"><span data-stu-id="649c2-221">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="649c2-222">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="649c2-222">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="649c2-223">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="649c2-223">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="649c2-224">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="649c2-224">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="649c2-225">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="649c2-225">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="649c2-226">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="649c2-226">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="649c2-227">Habilitación de la página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="649c2-227">Enable the Developer Exception Page</span></span>

<span data-ttu-id="649c2-228">La variable de entorno `ASPNETCORE_ENVIRONMENT` [ se puede agregar a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="649c2-228">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="649c2-229">Siempre y cuando el entorno no se invalide al inicio de la aplicación con `UseEnvironment` en el generador de host, la configuración de la variable de entorno permite que aparezca la [página de excepciones del desarrollador](xref:fundamentals/error-handling) cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-229">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="649c2-230">Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` cuando se use en servidores de ensayo o pruebas que no estén expuestos a Internet.</span><span class="sxs-lookup"><span data-stu-id="649c2-230">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="649c2-231">Quite la variable de entorno del archivo *web.config* cuando termine de solucionar los problemas.</span><span class="sxs-lookup"><span data-stu-id="649c2-231">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="649c2-232">Para información sobre la configuración de variables de entorno en *web.config*, consulte el [elemento secundario environmentVariables de aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="649c2-232">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="649c2-233">Errores comunes de inicio</span><span class="sxs-lookup"><span data-stu-id="649c2-233">Common startup errors</span></span>

<span data-ttu-id="649c2-234">Vea <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="649c2-234">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="649c2-235">La mayoría de los problemas comunes que impiden el inicio de la aplicación se tratan en el tema de referencia.</span><span class="sxs-lookup"><span data-stu-id="649c2-235">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="649c2-236">Obtención de datos de una aplicación</span><span class="sxs-lookup"><span data-stu-id="649c2-236">Obtain data from an app</span></span>

<span data-ttu-id="649c2-237">Si una aplicación es capaz de responder a solicitudes, obtenga solicitudes, conexiones y datos adicionales de la aplicación mediante el middleware en línea del terminal.</span><span class="sxs-lookup"><span data-stu-id="649c2-237">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="649c2-238">Para obtener más información y un código de ejemplo, vea <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="649c2-238">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="649c2-239">Creación de un volcado de memoria</span><span class="sxs-lookup"><span data-stu-id="649c2-239">Create a dump</span></span>

<span data-ttu-id="649c2-240">Un *volcado de memoria* es una instantánea de la memoria del sistema que puede ayudar a determinar la causa de un bloqueo de la aplicación, un error de inicio o una aplicación lenta.</span><span class="sxs-lookup"><span data-stu-id="649c2-240">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="649c2-241">Bloqueo o excepción de la aplicación</span><span class="sxs-lookup"><span data-stu-id="649c2-241">App crashes or encounters an exception</span></span>

<span data-ttu-id="649c2-242">Obtenga y analice un volcado de memoria en [Informe de errores de Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="649c2-242">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="649c2-243">Cree una carpeta para almacenar los archivos de volcado de memoria en `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="649c2-243">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="649c2-244">El grupo de aplicaciones debe tener acceso de escritura a la carpeta.</span><span class="sxs-lookup"><span data-stu-id="649c2-244">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="649c2-245">Ejecute el [script EnableDumps de PowerShell](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="649c2-245">Run the [EnableDumps PowerShell script](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="649c2-246">Si la aplicación usa el [modelo de hospedaje en proceso](xref:fundamentals/servers/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="649c2-246">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```
   * <span data-ttu-id="649c2-247">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:fundamentals/servers/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="649c2-247">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```
1. <span data-ttu-id="649c2-248">Ejecute la aplicación en las condiciones que hacen que se produzca el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="649c2-248">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="649c2-249">Una vez que se haya producido el bloqueo, ejecute el [script DisableDumps de PowerShell](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="649c2-249">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="649c2-250">Si la aplicación usa el [modelo de hospedaje en proceso](xref:fundamentals/servers/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="649c2-250">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```
   * <span data-ttu-id="649c2-251">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:fundamentals/servers/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="649c2-251">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="649c2-252">Después de que se bloquee la aplicación y se complete la recopilación del volcado de memoria, la aplicación puede finalizar con normalidad.</span><span class="sxs-lookup"><span data-stu-id="649c2-252">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="649c2-253">El script de PowerShell configura WER de modo que recopile un máximo de cinco volcados de memoria por aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-253">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="649c2-254">Los volcados de memoria pueden ocupar una gran cantidad de espacio en disco (hasta varios gigabytes cada uno).</span><span class="sxs-lookup"><span data-stu-id="649c2-254">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="649c2-255">La aplicación deja de responder, produce un error durante el inicio o se ejecuta con normalidad</span><span class="sxs-lookup"><span data-stu-id="649c2-255">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="649c2-256">Si una aplicación *deja de responder* (se detiene, pero no se bloquea), produce un error durante el inicio o se ejecuta con normalidad, vea [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Archivos de volcado de memoria en modo de usuario: selección de la mejor herramienta) para seleccionar una herramienta apropiada para generar el volcado de memoria.</span><span class="sxs-lookup"><span data-stu-id="649c2-256">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="649c2-257">Análisis del volcado de memoria</span><span class="sxs-lookup"><span data-stu-id="649c2-257">Analyze the dump</span></span>

<span data-ttu-id="649c2-258">El volcado de memoria se puede analizar de varias maneras.</span><span class="sxs-lookup"><span data-stu-id="649c2-258">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="649c2-259">Para obtener más información, vea [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Análisis de un archivo de volcado de memoria en modo de usuario).</span><span class="sxs-lookup"><span data-stu-id="649c2-259">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="649c2-260">Depuración remota</span><span class="sxs-lookup"><span data-stu-id="649c2-260">Remote debugging</span></span>

<span data-ttu-id="649c2-261">Consulte [Depuración remota de ASP.NET Core en un equipo remoto de IIS en Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) en la documentación de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="649c2-261">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="649c2-262">Application Insights</span><span class="sxs-lookup"><span data-stu-id="649c2-262">Application Insights</span></span>

<span data-ttu-id="649c2-263">[Application Insights](/azure/application-insights/) proporciona telemetría de las aplicaciones hospedadas en IIS, lo que incluye las características de registro de errores y generación de informes.</span><span class="sxs-lookup"><span data-stu-id="649c2-263">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="649c2-264">Application Insights solo puede notificar los errores que se producen después de que la aplicación se inicia cuando las características de registro de la aplicación se vuelven disponibles.</span><span class="sxs-lookup"><span data-stu-id="649c2-264">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="649c2-265">Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="649c2-265">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="649c2-266">Consejos adicionales</span><span class="sxs-lookup"><span data-stu-id="649c2-266">Additional advice</span></span>

<span data-ttu-id="649c2-267">En ocasiones, una aplicación en funcionamiento deja de funcionar inmediatamente después de actualizar el SDK de .NET Core en la máquina de desarrollo o las versiones del paquete en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-267">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="649c2-268">En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes.</span><span class="sxs-lookup"><span data-stu-id="649c2-268">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="649c2-269">La mayoría de estos problemas puede corregirse siguiendo estas instrucciones:</span><span class="sxs-lookup"><span data-stu-id="649c2-269">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="649c2-270">Elimine las carpetas *bin* y *obj*.</span><span class="sxs-lookup"><span data-stu-id="649c2-270">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="649c2-271">Borre las memorias caché del paquete en *%UserProfile%\\.nuget\\packages* y *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="649c2-271">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="649c2-272">Restaure el proyecto y vuelva a compilarlo.</span><span class="sxs-lookup"><span data-stu-id="649c2-272">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="649c2-273">Confirme que la implementación anterior en el servidor se ha eliminado por completo antes de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="649c2-273">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="649c2-274">Una forma práctica de borrar las memorias cachés del paquete es ejecutar `dotnet nuget locals all --clear` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="649c2-274">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="649c2-275">Otra manera de borrar las memorias caché del paquete es usar la herramienta [nuget.exe](https://www.nuget.org/downloads) y ejecutar el comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="649c2-275">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="649c2-276">*nuget.exe* no es una instalación agrupada con el sistema operativo de escritorio de Windows y se debe obtener de forma independiente en el [sitio web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="649c2-276">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="649c2-277">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="649c2-277">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
