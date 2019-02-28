---
title: Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo solucionar errores comunes al hospedar aplicaciones ASP.NET Core en Azure App Service e IIS.
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: d1cdac4d27ee1bc3ebb4329c1bbd3bdacb34a58c
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743952"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="db786-103">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db786-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="db786-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="db786-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="db786-105">En este tema, se ofrece información sobre cómo solucionar errores comunes al hospedar aplicaciones ASP.NET Core en Azure App Service e IIS.</span><span class="sxs-lookup"><span data-stu-id="db786-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="db786-106">Recopile la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="db786-106">Collect the following information:</span></span>

* <span data-ttu-id="db786-107">Comportamiento del explorador (código de estado y mensaje de error)</span><span class="sxs-lookup"><span data-stu-id="db786-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="db786-108">Entradas de registro de eventos de la aplicación</span><span class="sxs-lookup"><span data-stu-id="db786-108">Application Event Log entries</span></span>
  * <span data-ttu-id="db786-109">Azure App Service : vea <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="db786-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="db786-110">IIS</span><span class="sxs-lookup"><span data-stu-id="db786-110">IIS</span></span>
    1. <span data-ttu-id="db786-111">Seleccione **Inicio** en el menú **Windows**, escriba *Visor de eventos* y presione **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="db786-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="db786-112">Una vez abierto el **Visor de eventos**, amplíe **Registros de Windows** > **Aplicación** en la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="db786-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="db786-113">Entradas de registro de stdout y depuración de módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db786-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="db786-114">Azure App Service : vea <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="db786-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="db786-115">IIS: siga las instrucciones de las secciones [Creación y redireccionamiento de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) y [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) del tema Módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db786-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="db786-116">Compare la información sobre errores con los siguientes errores comunes.</span><span class="sxs-lookup"><span data-stu-id="db786-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="db786-117">Si se encuentra una coincidencia, siga los consejos de solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="db786-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="db786-118">La lista de errores en este tema no es exhaustiva.</span><span class="sxs-lookup"><span data-stu-id="db786-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="db786-119">Si se produce algún error que no aparezca aquí, abra un problema nuevo mediante el botón **Comentarios sobre el contenido** situado en la parte inferior de este tema con instrucciones detalladas sobre cómo reproducir el error.</span><span class="sxs-lookup"><span data-stu-id="db786-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="db786-120">El instalador no puede obtener VC++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="db786-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="db786-121">**Excepción del instalador:** 0x80072efd **--O--** 0x80072f76: error no especificado</span><span class="sxs-lookup"><span data-stu-id="db786-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="db786-122">**Excepción del registro del instalador&#8224;:** error 0x80072efd **--O--** 0x80072f76: No se ha podido ejecutar el paquete EXE</span><span class="sxs-lookup"><span data-stu-id="db786-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="db786-123">&#8224;El registro se encuentra en *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span><span class="sxs-lookup"><span data-stu-id="db786-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="db786-124">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-124">Troubleshooting:</span></span>

<span data-ttu-id="db786-125">Si el sistema no tiene acceso a Internet al [instalar la agrupación de hospedaje .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="db786-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="db786-126">Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="db786-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="db786-127">Si se produce un error en el instalador, es posible que el servidor no reciba el entorno de tiempo de ejecución de .NET Core necesario para hospedar una [implementación dependiente del marco (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="db786-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="db786-128">Si va a hospedar una FDD, confirme que el tiempo de ejecución está instalado en **Programas y características** o en **Aplicaciones y características**.</span><span class="sxs-lookup"><span data-stu-id="db786-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="db786-129">Si se requiere un tiempo de ejecución específico, descargue el tiempo de ejecución de [Archivos de descarga de .NET](https://dotnet.microsoft.com/download/archives) e instálelo en el sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="db786-130">Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="db786-131">La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits</span><span class="sxs-lookup"><span data-stu-id="db786-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="db786-132">**Registro de aplicación:** el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** no se ha podido cargar.</span><span class="sxs-lookup"><span data-stu-id="db786-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="db786-133">Los datos son el error.</span><span class="sxs-lookup"><span data-stu-id="db786-133">The data is the error.</span></span>

<span data-ttu-id="db786-134">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-134">Troubleshooting:</span></span>

<span data-ttu-id="db786-135">Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="db786-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="db786-136">Este problema se produce si ha instalado el módulo ASP.NET Core antes de una actualización del sistema operativo y, a continuación, ejecuta cualquier grupo de aplicaciones en modo de 32 bits después de una actualización del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="db786-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="db786-137">Después de actualizar el sistema operativo, repare el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db786-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="db786-138">Consulte [Instalación de la agrupación de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="db786-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="db786-139">Seleccione **Reparar** cuando se ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="db786-139">Select **Repair** when the installer is run.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="db786-140">Se ha implementado una aplicación x86, pero el grupo de aplicaciones no está habilitado para aplicaciones de 32 bits</span><span class="sxs-lookup"><span data-stu-id="db786-140">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="db786-141">**Explorador:** Error HTTP 500.30: error de inicio en el proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="db786-141">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="db786-142">**Registro de aplicación:** En la aplicación '/LM/W3SVC/5/ROOT' con la raíz física '{PATH}' se ha producido una excepción administrada inesperada, código de excepción = '0xe0434352'.</span><span class="sxs-lookup"><span data-stu-id="db786-142">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="db786-143">Compruebe los registros stderr para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="db786-143">Please check the stderr logs for more information.</span></span> <span data-ttu-id="db786-144">La aplicación '/LM/W3SVC/5/ROOT' con la raíz física '{PATH}' no ha podido cargar el CLR ni la aplicación administrada.</span><span class="sxs-lookup"><span data-stu-id="db786-144">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="db786-145">El subproceso de trabajo CLR se ha cerrado prematuramente</span><span class="sxs-lookup"><span data-stu-id="db786-145">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="db786-146">**Registro de stdout del módulo ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.</span><span class="sxs-lookup"><span data-stu-id="db786-146">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-147">**Registro de depuración del módulo de ASP.NET Core:** Se ha devuelto un valor HRESULT con errores: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="db786-147">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="db786-148">El SDK intercepta este escenario al publicar una aplicación independiente.</span><span class="sxs-lookup"><span data-stu-id="db786-148">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="db786-149">El SDK genera un error si el RID no coincide con el destino de plataforma (por ejemplo, RID de `win10-x64` con `<PlatformTarget>x86</PlatformTarget>` en el archivo del proyecto).</span><span class="sxs-lookup"><span data-stu-id="db786-149">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="db786-150">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-150">Troubleshooting:</span></span>

<span data-ttu-id="db786-151">En el caso de una implementación dependiente del marco x86 (`<PlatformTarget>x86</PlatformTarget>`), habilite el grupo de aplicaciones de IIS para aplicaciones de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="db786-151">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="db786-152">En el Administrador de IIS, abra la **Configuración avanzada** del grupo de aplicaciones y establezca **Habilitar aplicaciones de 32 bits** en **True**.</span><span class="sxs-lookup"><span data-stu-id="db786-152">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="db786-153">Conflictos de plataforma con RID</span><span class="sxs-lookup"><span data-stu-id="db786-153">Platform conflicts with RID</span></span>

* <span data-ttu-id="db786-154">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="db786-154">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="db786-155">**Registro de aplicación:** la aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"C:\{{PATH}{ASSEMBLY}.{exe|dll}"', código de error = '0x80004005 : ff'.</span><span class="sxs-lookup"><span data-stu-id="db786-155">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="db786-156">**Registro de stdout del módulo ASP.NET Core:** Excepción no controlada: System.BadImageFormatException: No se ha podido cargar el archivo ni el ensamblado '{ASSEMBLY}.dll'.</span><span class="sxs-lookup"><span data-stu-id="db786-156">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="db786-157">Se ha intentado cargar un programa con un formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="db786-157">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="db786-158">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-158">Troubleshooting:</span></span>

* <span data-ttu-id="db786-159">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="db786-159">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="db786-160">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-160">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="db786-161">Para obtener más información, consulte [Solución de problemas (IIS)](xref:host-and-deploy/iis/troubleshoot) o [Solución de problemas (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="db786-161">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="db786-162">Si esta excepción se produce en una implementación de Azure Apps al actualizar una aplicación e implementar ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior.</span><span class="sxs-lookup"><span data-stu-id="db786-162">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="db786-163">Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.</span><span class="sxs-lookup"><span data-stu-id="db786-163">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="db786-164">Punto de conexión de URI incorrecto o sitio web detenido</span><span class="sxs-lookup"><span data-stu-id="db786-164">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="db786-165">**Explorador:** ERR_CONNECTION_REFUSED **--O--** No se podido establecer la conexión</span><span class="sxs-lookup"><span data-stu-id="db786-165">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="db786-166">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="db786-166">**Application Log:** No entry</span></span>

* <span data-ttu-id="db786-167">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-167">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-168">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-168">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="db786-169">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-169">Troubleshooting:</span></span>

* <span data-ttu-id="db786-170">Confirme que se usa el punto de conexión de URI correcto para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-170">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="db786-171">Compruebe los enlaces.</span><span class="sxs-lookup"><span data-stu-id="db786-171">Check the bindings.</span></span>

* <span data-ttu-id="db786-172">Confirme que el sitio web de IIS no está en estado *Detenido*.</span><span class="sxs-lookup"><span data-stu-id="db786-172">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="db786-173">Características de servidor CoreWebEngine o W3SVC deshabilitadas</span><span class="sxs-lookup"><span data-stu-id="db786-173">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="db786-174">**Excepción de sistema operativo:** Para usar el módulo ASP.NET Core, se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC.</span><span class="sxs-lookup"><span data-stu-id="db786-174">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="db786-175">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-175">Troubleshooting:</span></span>

<span data-ttu-id="db786-176">Confirme que están habilitados el rol y las características correctos.</span><span class="sxs-lookup"><span data-stu-id="db786-176">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="db786-177">Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="db786-177">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="db786-178">Ruta de acceso física de sitio web incorrecta o aplicación que falta</span><span class="sxs-lookup"><span data-stu-id="db786-178">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="db786-179">**Explorador:** 403 Prohibido: acceso denegado **--O BIEN--** 403.14 Prohibido: el servidor web está configurado para no mostrar una lista de los contenidos de este directorio.</span><span class="sxs-lookup"><span data-stu-id="db786-179">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="db786-180">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="db786-180">**Application Log:** No entry</span></span>

* <span data-ttu-id="db786-181">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-181">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-182">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-182">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="db786-183">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-183">Troubleshooting:</span></span>

<span data-ttu-id="db786-184">Consulte la opción **Configuración básica** del sitio web de IIS y la carpeta de la aplicación física.</span><span class="sxs-lookup"><span data-stu-id="db786-184">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="db786-185">Confirme que la aplicación está en la carpeta en la **ruta de acceso física** del sitio web de IIS.</span><span class="sxs-lookup"><span data-stu-id="db786-185">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="db786-186">Rol incorrecto, módulo de ASP.NET Core no instalado o permisos incorrectos</span><span class="sxs-lookup"><span data-stu-id="db786-186">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="db786-187">**Explorador:** 500.19 Error interno del servidor: no se puede acceder a la página solicitada porque los datos de configuración relacionados de la página no son válidos.</span><span class="sxs-lookup"><span data-stu-id="db786-187">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="db786-188">**--O--** No se puede mostrar esta página</span><span class="sxs-lookup"><span data-stu-id="db786-188">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="db786-189">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="db786-189">**Application Log:** No entry</span></span>

* <span data-ttu-id="db786-190">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-190">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-191">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-191">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="db786-192">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-192">Troubleshooting:</span></span>

* <span data-ttu-id="db786-193">Confirme que está habilitado el rol adecuado.</span><span class="sxs-lookup"><span data-stu-id="db786-193">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="db786-194">Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="db786-194">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="db786-195">Abra **Programas y características** o **Aplicaciones y características** y confirme que **Hospedaje de Windows Server** está instalado.</span><span class="sxs-lookup"><span data-stu-id="db786-195">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="db786-196">Si **Hospedaje de Windows Server** no está presente en la lista de programas instalados, descargue e instale el conjunto de hospedaje de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="db786-196">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="db786-197">Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)</span><span class="sxs-lookup"><span data-stu-id="db786-197">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="db786-198">Para obtener más información, consulte [Instalar el conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="db786-198">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="db786-199">Asegúrese de que **Grupo de aplicaciones** > **Modelo de proceso** > **Identidad** esté establecido en **ApplicationPoolIdentity** o que la identidad personalizada tenga los permisos correctos para acceder a la carpeta de implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-199">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="db786-200">Si ha desinstalado el paquete de hospedaje de ASP.NET Core y ha instalado una versión anterior de dicho paquete, el archivo *applicationHost.config* no incluirá ninguna sección para el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db786-200">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="db786-201">Abra *applicationHost.config* en *%windir%/System32/inetsrv/config* y busque el grupo de secciones `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="db786-201">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="db786-202">Si falta la sección del módulo ASP.NET Core en el grupo de secciones, añada el elemento de sección:</span><span class="sxs-lookup"><span data-stu-id="db786-202">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```
  
  <span data-ttu-id="db786-203">Si quiere, también puede instalar la versión más reciente del paquete de hospedaje de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db786-203">Alternatively, install the lastest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="db786-204">La versión más reciente es compatible con las versiones anteriores de las aplicaciones ASP.NET Core admitidas.</span><span class="sxs-lookup"><span data-stu-id="db786-204">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="db786-205">Elemento processPath incorrecto, falta la variable PATH, agrupación de hospedaje no instalada, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="db786-205">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-206">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="db786-206">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="db786-207">**Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"{...}"</span><span class="sxs-lookup"><span data-stu-id="db786-207">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="db786-208">', código de error = '0 x 80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="db786-208">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="db786-209">La aplicación '{PATH}' no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="db786-209">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="db786-210">No se ha encontrado el archivo ejecutable en '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="db786-210">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="db786-211">No se ha podido iniciar la aplicación '/LM/W3SVC/2/ROOT', código de error '0x8007023e'.</span><span class="sxs-lookup"><span data-stu-id="db786-211">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="db786-212">**Registro de stdout del módulo ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-212">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="db786-213">**Registro de depuración del módulo de ASP.NET Core:** Registro de eventos: "La aplicación '{PATH}'" no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="db786-213">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="db786-214">No se ha encontrado el archivo ejecutable en '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="db786-214">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="db786-215">Se ha devuelto un valor HRESULT con errores: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="db786-215">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="db786-216">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="db786-216">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="db786-217">**Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"{...}"</span><span class="sxs-lookup"><span data-stu-id="db786-217">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="db786-218">', código de error = '0 x 80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="db786-218">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="db786-219">**Registro de stdout del módulo ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.</span><span class="sxs-lookup"><span data-stu-id="db786-219">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="db786-220">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-220">Troubleshooting:</span></span>

* <span data-ttu-id="db786-221">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="db786-221">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="db786-222">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-222">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="db786-223">Para obtener más información, consulte [Solución de problemas (IIS)](xref:host-and-deploy/iis/troubleshoot) o [Solución de problemas (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="db786-223">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="db786-224">Compruebe el atributo *processPath* del elemento `<aspNetCore>` de *web.config* para confirmar que es `dotnet` para una implementación dependiente del marco (FDD) o `.\{ASSEMBLY}.exe` para una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="db786-224">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="db786-225">En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH.</span><span class="sxs-lookup"><span data-stu-id="db786-225">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="db786-226">Confirme que *C:\Archivos de programa\dotnet\\* existe en el valor PATH del sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-226">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="db786-227">En el caso de una FDD, es posible que la identidad del usuario del grupo de aplicaciones no pueda acceder a *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="db786-227">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="db786-228">Confirme que la identidad del usuario del grupo de aplicaciones tiene acceso al directorio *C:\Archivos de programa\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="db786-228">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="db786-229">Confirme que no haya ninguna regla de denegación configurada para la identidad del usuario del grupo de aplicaciones en los directorios *C:\Archivos de programa\dotnet* y de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-229">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="db786-230">Puede que se haya implementado una FDD y que se instalara .NET Core sin reiniciar IIS.</span><span class="sxs-lookup"><span data-stu-id="db786-230">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="db786-231">Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.</span><span class="sxs-lookup"><span data-stu-id="db786-231">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="db786-232">Puede que se haya implementado una FDD sin instalar el entorno de tiempo de ejecución de .NET Core en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="db786-232">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="db786-233">Si no se ha instalado el entorno de tiempo de ejecución de .NET Core, ejecute el **instalador de la agrupación de hospedaje de .NET Core** en el sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-233">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="db786-234">Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)</span><span class="sxs-lookup"><span data-stu-id="db786-234">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="db786-235">Para obtener más información, consulte [Instalar el conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="db786-235">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="db786-236">Si se requiere un tiempo de ejecución específico, descargue el tiempo de ejecución de [Archivos de descarga de .NET](https://dotnet.microsoft.com/download/archives) e instálelo en el sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-236">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="db786-237">Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-237">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="db786-238">Puede que se haya implementado una FDD y que el paquete *Microsoft Visual C++ 2015 Redistributable (x64)* no esté instalado en el sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-238">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="db786-239">Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="db786-239">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="db786-240">Argumentos incorrectos del elemento \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="db786-240">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-241">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="db786-241">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="db786-242">**Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="db786-242">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="db786-243">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="db786-243">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="db786-244">No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="db786-244">Could not find inprocess request handler.</span></span> <span data-ttu-id="db786-245">Resultado obtenido al invocar a hostfxr: ¿pretendía ejecutar comandos SDK de dotnet?</span><span class="sxs-lookup"><span data-stu-id="db786-245">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="db786-246">Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 No se ha podido iniciar la aplicación '/LM/W3SVC/3/ROOT', código de error '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="db786-246">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="db786-247">**Registro de stdout del módulo ASP.NET Core:** ¿pretendía ejecutar comandos SDK de dotnet?</span><span class="sxs-lookup"><span data-stu-id="db786-247">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="db786-248">Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="db786-248">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="db786-249">**Registro de depuración del módulo de ASP.NET Core:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="db786-249">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="db786-250">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="db786-250">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="db786-251">Se ha devuelto un valor HRESULT con errores: 0x8000ffff No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="db786-251">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="db786-252">Resultado obtenido al invocar a hostfxr: ¿pretendía ejecutar comandos SDK de dotnet?</span><span class="sxs-lookup"><span data-stu-id="db786-252">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="db786-253">Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Se ha devuelto un valor HRESULT con errores: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="db786-253">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="db786-254">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="db786-254">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="db786-255">**Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"dotnet" .\{ASSEMBLY}.dll', código de error = '0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="db786-255">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="db786-256">**Registro de stdout del módulo de ASP.NET Core:** La aplicación que se va a ejecutar no existe: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="db786-256">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="db786-257">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-257">Troubleshooting:</span></span>

* <span data-ttu-id="db786-258">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="db786-258">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="db786-259">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-259">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="db786-260">Para obtener más información, consulte [Solución de problemas (IIS)](xref:host-and-deploy/iis/troubleshoot) o [Solución de problemas (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="db786-260">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="db786-261">Examine el atributo *arguments* del elemento `<aspNetCore>` en *web.config* para confirmar que (a) es `.\{ASSEMBLY}.dll` para una implementación dependiente del marco (FDD); o (b) no está presente, es una cadena vacía (`arguments=""`) o es una lista de argumentos de la aplicación (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) para una implementación independiente (SCD).</span><span class="sxs-lookup"><span data-stu-id="db786-261">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="db786-262">Falta el marco compartido de .NET Core</span><span class="sxs-lookup"><span data-stu-id="db786-262">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="db786-263">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="db786-263">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="db786-264">**Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="db786-264">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="db786-265">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="db786-265">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="db786-266">No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="db786-266">Could not find inprocess request handler.</span></span> <span data-ttu-id="db786-267">Resultado obtenido al invocar a hostfxr: No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="db786-267">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="db786-268">El marco especificado 'Microsoft.AspNetCore.App', versión '{VERSION}', no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="db786-268">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="db786-269">No se ha podido iniciar la aplicación '/LM/W3SVC/5/ROOT', código de error '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="db786-269">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="db786-270">**Registro de stdout del módulo de ASP.NET Core:** No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="db786-270">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="db786-271">El marco especificado 'Microsoft.AspNetCore.App', versión '{VERSION}', no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="db786-271">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="db786-272">**Registro de depuración del módulo de ASP.NET Core:** Se ha devuelto un valor HRESULT con errores: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="db786-272">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="db786-273">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-273">Troubleshooting:</span></span>

<span data-ttu-id="db786-274">En el caso de una implementación dependiente del marco (FDD), confirme que tiene instalado el entorno de tiempo de ejecución correcto en el sistema.</span><span class="sxs-lookup"><span data-stu-id="db786-274">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="db786-275">Grupo de aplicaciones detenido</span><span class="sxs-lookup"><span data-stu-id="db786-275">Stopped Application Pool</span></span>

* <span data-ttu-id="db786-276">**Explorador:** 503 Servicio no disponible</span><span class="sxs-lookup"><span data-stu-id="db786-276">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="db786-277">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="db786-277">**Application Log:** No entry</span></span>

* <span data-ttu-id="db786-278">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-278">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-279">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-279">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="db786-280">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-280">Troubleshooting:</span></span>

<span data-ttu-id="db786-281">Confirme que el grupo de aplicaciones no está en estado *Detenido*.</span><span class="sxs-lookup"><span data-stu-id="db786-281">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="db786-282">La aplicación secundaria incluye una sección de \<controladores></span><span class="sxs-lookup"><span data-stu-id="db786-282">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="db786-283">**Explorador:** error HTTP 500.19: error interno del servidor</span><span class="sxs-lookup"><span data-stu-id="db786-283">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="db786-284">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="db786-284">**Application Log:** No entry</span></span>

* <span data-ttu-id="db786-285">**Registro de stdout del módulo de ASP.NET Core:** El archivo de registro de la aplicación raíz se ha creado y muestra un funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="db786-285">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="db786-286">No se ha creado el archivo de registro de la aplicación secundaria.</span><span class="sxs-lookup"><span data-stu-id="db786-286">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-287">**Registro de depuración del módulo de ASP.NET Core:** El archivo de registro de la aplicación raíz se ha creado y muestra un funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="db786-287">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="db786-288">No se ha creado el archivo de registro de la aplicación secundaria.</span><span class="sxs-lookup"><span data-stu-id="db786-288">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="db786-289">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-289">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="db786-290">Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>` o que la aplicación secundaria no hereda los controladores de la aplicación primaria.</span><span class="sxs-lookup"><span data-stu-id="db786-290">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="db786-291">La sección `<system.webServer>` de la aplicación primaria de *web.config* está colocada dentro de un elemento `<location>`.</span><span class="sxs-lookup"><span data-stu-id="db786-291">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="db786-292">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación primaria no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="db786-292">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="db786-293">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="db786-293">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="db786-294">Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="db786-294">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="db786-295">Ruta de acceso incorrecta al registro de stdout</span><span class="sxs-lookup"><span data-stu-id="db786-295">stdout log path incorrect</span></span>

* <span data-ttu-id="db786-296">**Explorador:** la aplicación responde normalmente.</span><span class="sxs-lookup"><span data-stu-id="db786-296">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-297">**Registro de aplicación:** No se ha podido iniciar la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="db786-297">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="db786-298">Mensaje de excepción: HRESULT 0 x 80070005 se ha devuelto en {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="db786-298">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="db786-299">No se ha podido detener la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="db786-299">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="db786-300">Mensaje de excepción: HRESULT 0 x 80070002 se ha devuelto en {PATH}.</span><span class="sxs-lookup"><span data-stu-id="db786-300">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="db786-301">No se ha podido iniciar la redirección de stdout en {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="db786-301">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="db786-302">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-302">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="db786-303">**Registro de depuración del módulo de ASP.NET Core:** No se ha podido iniciar la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="db786-303">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="db786-304">Mensaje de excepción: HRESULT 0 x 80070005 se ha devuelto en {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="db786-304">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="db786-305">No se ha podido detener la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="db786-305">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="db786-306">Mensaje de excepción: HRESULT 0 x 80070002 se ha devuelto en {PATH}.</span><span class="sxs-lookup"><span data-stu-id="db786-306">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="db786-307">No se ha podido iniciar la redirección de stdout en {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="db786-307">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="db786-308">**Registro de aplicación:** Advertencia: No se ha podido crear el archivo de registro de stdout \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, código de error = -2147024893.</span><span class="sxs-lookup"><span data-stu-id="db786-308">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="db786-309">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="db786-309">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="db786-310">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-310">Troubleshooting:</span></span>

* <span data-ttu-id="db786-311">La ruta de acceso `stdoutLogFile` especificada en el elemento `<aspNetCore>` de *web.config* no existe.</span><span class="sxs-lookup"><span data-stu-id="db786-311">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="db786-312">Para obtener más información, consulte [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creación y redireccionamiento de registros: módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="db786-312">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="db786-313">El usuario del grupo de aplicaciones no tiene acceso de escritura a la ruta de acceso del registro de stdout.</span><span class="sxs-lookup"><span data-stu-id="db786-313">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="db786-314">Problema general de configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="db786-314">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="db786-315">**Explorador:** error HTTP 500.0: error de carga del controlador en proceso ANCM **--O--** Error HTTP 500.30: error de inicio en el proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="db786-315">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="db786-316">**Registro de aplicación:** Variable</span><span class="sxs-lookup"><span data-stu-id="db786-316">**Application Log:** Variable</span></span>

* <span data-ttu-id="db786-317">**Registro de stdout del módulo de ASP.NET Core:** El archivo de registro se ha creado, pero está vacío o se ha creado con entradas normales, hasta el punto en que se producen errores en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-317">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="db786-318">**Registro de depuración del módulo de ASP.NET Core:** Variable</span><span class="sxs-lookup"><span data-stu-id="db786-318">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="db786-319">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="db786-319">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="db786-320">**Registro de aplicación:** la aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{PATH}\'' ha creado el proceso con la línea de comandos '"C:\{PATH}\{ASSEMBLY}.{exe|dll}"', pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado "{PORT}", código de error = '{ERROR CODE}'</span><span class="sxs-lookup"><span data-stu-id="db786-320">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="db786-321">**Registro de stdout del módulo de ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.</span><span class="sxs-lookup"><span data-stu-id="db786-321">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="db786-322">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="db786-322">Troubleshooting:</span></span>

<span data-ttu-id="db786-323">El proceso no se ha iniciado, probablemente debido a un problema de configuración o programación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db786-323">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="db786-324">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="db786-324">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
