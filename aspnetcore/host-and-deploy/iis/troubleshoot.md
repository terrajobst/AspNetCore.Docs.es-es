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
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="b5be4-103">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="b5be4-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="b5be4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b5be4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b5be4-105">En este artículo se proporcionan instrucciones sobre cómo diagnosticar un problema de inicio de ASP.NET Core al hospedarse con [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="b5be4-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="b5be4-106">La información de este artículo se aplica al hospedaje en IIS en Windows Server y el escritorio de Windows.</span><span class="sxs-lookup"><span data-stu-id="b5be4-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="b5be4-107">Temas adicionales de solución de problemas:</span><span class="sxs-lookup"><span data-stu-id="b5be4-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="b5be4-108">Azure App Service también usa el [módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e IIS para hospedar aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b5be4-108">Azure App Service also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps.</span></span> <span data-ttu-id="b5be4-109">Para ver consejos de solución de problemas pertenecientes específicamente a Azure App Service, consulte <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="b5be4-109">For troubleshooting advice that pertains specifically to Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
* <span data-ttu-id="b5be4-110">En <xref:fundamentals/error-handling> se explica cómo controlar los errores de aplicaciones de ASP.NET Core durante el desarrollo en un sistema local.</span><span class="sxs-lookup"><span data-stu-id="b5be4-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="b5be4-111">En [Información sobre cómo depurar con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) se presentan las características del depurador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5be4-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="b5be4-112">En [Depuración con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) se describe la compatibilidad de depuración integrada en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b5be4-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="b5be4-113">Solución de problemas de errores de inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b5be4-113">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="b5be4-114">Habilitar el registro de depuración del módulo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5be4-114">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="b5be4-115">Agregue la siguiente configuración de controlador al archivo *web.config* de la aplicación para habilitar los registros de depuración del módulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b5be4-115">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="b5be4-116">Confirme que la ruta de acceso especificada para el registro exista y que la identidad del grupo de aplicaciones tenga permisos de escritura en la ubicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-116">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="b5be4-117">Registro de eventos de aplicación</span><span class="sxs-lookup"><span data-stu-id="b5be4-117">Application Event Log</span></span>

<span data-ttu-id="b5be4-118">Acceda al registro de eventos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b5be4-118">Access the Application Event Log:</span></span>

1. <span data-ttu-id="b5be4-119">Abra el menú Inicio, busque **Visor de eventos** y luego seleccione la aplicación **Visor de eventos**.</span><span class="sxs-lookup"><span data-stu-id="b5be4-119">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="b5be4-120">En **Visor de eventos**, abra el nodo **Registros de Windows**.</span><span class="sxs-lookup"><span data-stu-id="b5be4-120">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="b5be4-121">Seleccione **Aplicación** para abrir el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-121">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="b5be4-122">Busque los errores asociados a la aplicación objeto del error.</span><span class="sxs-lookup"><span data-stu-id="b5be4-122">Search for errors associated with the failing app.</span></span> <span data-ttu-id="b5be4-123">Los errores tienen un valor de *Módulo AspNetCore de IIS* o *Módulo AspNetCore de IIS Express* en la columna *Origen*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-123">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="b5be4-124">Ejecución de la aplicación en un símbolo del sistema</span><span class="sxs-lookup"><span data-stu-id="b5be4-124">Run the app at a command prompt</span></span>

<span data-ttu-id="b5be4-125">Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-125">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5be4-126">La causa de algunos errores se puede encontrar mediante la ejecución de la aplicación en un símbolo del sistema en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="b5be4-126">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="b5be4-127">Implementación dependiente de marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="b5be4-127">Framework-dependent deployment</span></span>

<span data-ttu-id="b5be4-128">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="b5be4-128">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="b5be4-129">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute la aplicación mediante la ejecución del ensamblado de la aplicación con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-129">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="b5be4-130">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-130">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="b5be4-131">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="b5be4-131">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5be4-132">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b5be4-132">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5be4-133">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-133">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5be4-134">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-134">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="b5be4-135">Implementación autocontenida</span><span class="sxs-lookup"><span data-stu-id="b5be4-135">Self-contained deployment</span></span>

<span data-ttu-id="b5be4-136">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="b5be4-136">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="b5be4-137">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute el archivo ejecutable de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-137">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="b5be4-138">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-138">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="b5be4-139">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="b5be4-139">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5be4-140">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b5be4-140">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5be4-141">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-141">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5be4-142">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-142">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="b5be4-143">Registro de stdout del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5be4-143">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="b5be4-144">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="b5be4-144">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5be4-145">Vaya a la carpeta de implementación del sitio en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="b5be4-145">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5be4-146">Si la carpeta *Logs* no existe, cree la carpeta.</span><span class="sxs-lookup"><span data-stu-id="b5be4-146">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="b5be4-147">Para obtener instrucciones sobre cómo habilitar MSBuild para crear la carpeta *logs* automáticamente en la implementación, consulte el tema [Estructura de directorios](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="b5be4-147">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="b5be4-148">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-148">Edit the *web.config* file.</span></span> <span data-ttu-id="b5be4-149">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** para que apunte a la carpeta *logs* (por ejemplo, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="b5be4-149">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="b5be4-150">`stdout` en la ruta de acceso es el prefijo del nombre del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="b5be4-150">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="b5be4-151">Cuando se crea el registro, se agregan automáticamente una marca de tiempo, un identificador de proceso y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="b5be4-151">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="b5be4-152">Cuando se usa `stdout` como prefijo para el nombre de archivo, un archivo de registro se llama normalmente *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-152">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="b5be4-153">Asegúrese de que la identidad del grupo de aplicaciones tiene permisos de escritura en la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-153">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="b5be4-154">Guarde el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="b5be4-154">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5be4-155">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-155">Make a request to the app.</span></span>
1. <span data-ttu-id="b5be4-156">Vaya a la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-156">Navigate to the *logs* folder.</span></span> <span data-ttu-id="b5be4-157">Busque y abra el registro más reciente de stdout.</span><span class="sxs-lookup"><span data-stu-id="b5be4-157">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="b5be4-158">Estudie el registro para ver los errores.</span><span class="sxs-lookup"><span data-stu-id="b5be4-158">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5be4-159">Deshabilite el registro de stdout cuando la solución de problemas haya finalizado.</span><span class="sxs-lookup"><span data-stu-id="b5be4-159">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="b5be4-160">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-160">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="b5be4-161">Establezca **stdoutLogEnabled** en `false`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-161">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5be4-162">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="b5be4-162">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="b5be4-163">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5be4-163">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5be4-164">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="b5be4-164">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5be4-165">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="b5be4-165">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5be4-166">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="b5be4-166">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="b5be4-167">Habilitación de la página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="b5be4-167">Enable the Developer Exception Page</span></span>

<span data-ttu-id="b5be4-168">La variable de entorno `ASPNETCORE_ENVIRONMENT` [ se puede agregar a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="b5be4-168">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="b5be4-169">Siempre y cuando el entorno no se invalide al inicio de la aplicación con `UseEnvironment` en el generador de host, la configuración de la variable de entorno permite que aparezca la [página de excepciones del desarrollador](xref:fundamentals/error-handling) cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-169">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="b5be4-170">Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` cuando se use en servidores de ensayo o pruebas que no estén expuestos a Internet.</span><span class="sxs-lookup"><span data-stu-id="b5be4-170">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="b5be4-171">Quite la variable de entorno del archivo *web.config* cuando termine de solucionar los problemas.</span><span class="sxs-lookup"><span data-stu-id="b5be4-171">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="b5be4-172">Para información sobre la configuración de variables de entorno en *web.config*, consulte el [elemento secundario environmentVariables de aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="b5be4-172">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="b5be4-173">Errores comunes de inicio</span><span class="sxs-lookup"><span data-stu-id="b5be4-173">Common startup errors</span></span>

<span data-ttu-id="b5be4-174">Vea <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="b5be4-174">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="b5be4-175">La mayoría de los problemas comunes que impiden el inicio de la aplicación se tratan en el tema de referencia.</span><span class="sxs-lookup"><span data-stu-id="b5be4-175">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="b5be4-176">Obtención de datos de una aplicación</span><span class="sxs-lookup"><span data-stu-id="b5be4-176">Obtain data from an app</span></span>

<span data-ttu-id="b5be4-177">Si una aplicación es capaz de responder a solicitudes, obtenga solicitudes, conexiones y datos adicionales de la aplicación mediante el middleware en línea del terminal.</span><span class="sxs-lookup"><span data-stu-id="b5be4-177">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="b5be4-178">Para obtener más información y un código de ejemplo, vea <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="b5be4-178">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="b5be4-179">Creación de un volcado de memoria</span><span class="sxs-lookup"><span data-stu-id="b5be4-179">Create a dump</span></span>

<span data-ttu-id="b5be4-180">Un *volcado de memoria* es una instantánea de la memoria del sistema que puede ayudar a determinar la causa de un bloqueo de la aplicación, un error de inicio o una aplicación lenta.</span><span class="sxs-lookup"><span data-stu-id="b5be4-180">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="b5be4-181">Bloqueo o excepción de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b5be4-181">App crashes or encounters an exception</span></span>

<span data-ttu-id="b5be4-182">Obtenga y analice un volcado de memoria en [Informe de errores de Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="b5be4-182">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="b5be4-183">Cree una carpeta para almacenar los archivos de volcado de memoria en `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-183">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="b5be4-184">El grupo de aplicaciones debe tener acceso de escritura a la carpeta.</span><span class="sxs-lookup"><span data-stu-id="b5be4-184">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="b5be4-185">Ejecute el [script EnableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="b5be4-185">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="b5be4-186">Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="b5be4-186">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="b5be4-187">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="b5be4-187">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="b5be4-188">Ejecute la aplicación en las condiciones que hacen que se produzca el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="b5be4-188">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="b5be4-189">Una vez que se haya producido el bloqueo, ejecute el [script DisableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="b5be4-189">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="b5be4-190">Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="b5be4-190">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="b5be4-191">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="b5be4-191">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="b5be4-192">Después de que se bloquee la aplicación y se complete la recopilación del volcado de memoria, la aplicación puede finalizar con normalidad.</span><span class="sxs-lookup"><span data-stu-id="b5be4-192">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="b5be4-193">El script de PowerShell configura WER de modo que recopile un máximo de cinco volcados de memoria por aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-193">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="b5be4-194">Los volcados de memoria pueden ocupar una gran cantidad de espacio en disco (hasta varios gigabytes cada uno).</span><span class="sxs-lookup"><span data-stu-id="b5be4-194">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="b5be4-195">La aplicación deja de responder, produce un error durante el inicio o se ejecuta con normalidad</span><span class="sxs-lookup"><span data-stu-id="b5be4-195">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="b5be4-196">Si una aplicación *deja de responder* (se detiene, pero no se bloquea), produce un error durante el inicio o se ejecuta con normalidad, vea [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Archivos de volcado de memoria en modo de usuario: selección de la mejor herramienta) para seleccionar una herramienta apropiada para generar el volcado de memoria.</span><span class="sxs-lookup"><span data-stu-id="b5be4-196">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="b5be4-197">Análisis del volcado de memoria</span><span class="sxs-lookup"><span data-stu-id="b5be4-197">Analyze the dump</span></span>

<span data-ttu-id="b5be4-198">El volcado de memoria se puede analizar de varias maneras.</span><span class="sxs-lookup"><span data-stu-id="b5be4-198">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="b5be4-199">Para obtener más información, vea [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Análisis de un archivo de volcado de memoria en modo de usuario).</span><span class="sxs-lookup"><span data-stu-id="b5be4-199">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="b5be4-200">Depuración remota</span><span class="sxs-lookup"><span data-stu-id="b5be4-200">Remote debugging</span></span>

<span data-ttu-id="b5be4-201">Consulte [Depuración remota de ASP.NET Core en un equipo remoto de IIS en Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) en la documentación de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5be4-201">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="b5be4-202">Application Insights</span><span class="sxs-lookup"><span data-stu-id="b5be4-202">Application Insights</span></span>

<span data-ttu-id="b5be4-203">[Application Insights](/azure/application-insights/) proporciona telemetría de las aplicaciones hospedadas en IIS, lo que incluye las características de registro de errores y generación de informes.</span><span class="sxs-lookup"><span data-stu-id="b5be4-203">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="b5be4-204">Application Insights solo puede notificar los errores que se producen después de que la aplicación se inicia cuando las características de registro de la aplicación se vuelven disponibles.</span><span class="sxs-lookup"><span data-stu-id="b5be4-204">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="b5be4-205">Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="b5be4-205">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="b5be4-206">Consejos adicionales</span><span class="sxs-lookup"><span data-stu-id="b5be4-206">Additional advice</span></span>

<span data-ttu-id="b5be4-207">En ocasiones, una aplicación en funcionamiento deja de funcionar inmediatamente después de actualizar el SDK de .NET Core en la máquina de desarrollo o las versiones del paquete en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-207">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="b5be4-208">En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes.</span><span class="sxs-lookup"><span data-stu-id="b5be4-208">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="b5be4-209">La mayoría de estos problemas puede corregirse siguiendo estas instrucciones:</span><span class="sxs-lookup"><span data-stu-id="b5be4-209">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="b5be4-210">Elimine las carpetas *bin* y *obj*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-210">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="b5be4-211">Borre las memorias caché del paquete en *%UserProfile%\\.nuget\\packages* y *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="b5be4-211">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="b5be4-212">Restaure el proyecto y vuelva a compilarlo.</span><span class="sxs-lookup"><span data-stu-id="b5be4-212">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="b5be4-213">Confirme que la implementación anterior en el servidor se ha eliminado por completo antes de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5be4-213">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="b5be4-214">Una forma práctica de borrar las memorias cachés del paquete es ejecutar `dotnet nuget locals all --clear` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="b5be4-214">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="b5be4-215">Otra manera de borrar las memorias caché del paquete es usar la herramienta [nuget.exe](https://www.nuget.org/downloads) y ejecutar el comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="b5be4-215">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="b5be4-216">*nuget.exe* no es una instalación agrupada con el sistema operativo de escritorio de Windows y se debe obtener de forma independiente en el [sitio web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="b5be4-216">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5be4-217">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b5be4-217">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
