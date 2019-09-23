---
title: Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo solucionar errores comunes al hospedar aplicaciones ASP.NET Core en Azure App Service e IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: f6afd6491181830f4d79486fa26a64423cd4a0ac
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963676"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="ba1a9-103">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba1a9-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="ba1a9-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ba1a9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ba1a9-105">En este tema se describen los errores comunes y se proporcionan consejos de solución de problemas para errores específicos al hospedar aplicaciones ASP.NET Core en Azure Apps Service e IIS.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-105">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="ba1a9-106">Para obtener instrucciones de solución de problemas, vea <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-106">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="ba1a9-107">Recopile la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-107">Collect the following information:</span></span>

* <span data-ttu-id="ba1a9-108">Comportamiento del explorador (código de estado y mensaje de error)</span><span class="sxs-lookup"><span data-stu-id="ba1a9-108">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="ba1a9-109">Entradas de registro de eventos de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ba1a9-109">Application Event Log entries</span></span>
  * <span data-ttu-id="ba1a9-110">Azure App Service : vea <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-110">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="ba1a9-111">IIS</span><span class="sxs-lookup"><span data-stu-id="ba1a9-111">IIS</span></span>
    1. <span data-ttu-id="ba1a9-112">Seleccione **Inicio** en el menú **Windows**, escriba *Visor de eventos* y presione **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-112">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="ba1a9-113">Una vez abierto el **Visor de eventos**, amplíe **Registros de Windows** > **Aplicación** en la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-113">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="ba1a9-114">Entradas de registro de stdout y depuración de módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba1a9-114">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="ba1a9-115">Azure App Service : vea <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-115">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="ba1a9-116">IIS: siga las instrucciones de las secciones [Creación y redireccionamiento de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) y [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) del tema Módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-116">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="ba1a9-117">Compare la información sobre errores con los siguientes errores comunes.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-117">Compare error information to the following common errors.</span></span> <span data-ttu-id="ba1a9-118">Si se encuentra una coincidencia, siga los consejos de solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-118">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="ba1a9-119">La lista de errores en este tema no es exhaustiva.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-119">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="ba1a9-120">Si se produce algún error que no aparezca aquí, abra un problema nuevo mediante el botón **Comentarios sobre el contenido** situado en la parte inferior de este tema con instrucciones detalladas sobre cómo reproducir el error.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-120">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="ba1a9-121">El instalador no puede obtener VC++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="ba1a9-121">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="ba1a9-122">**Excepción del instalador:** 0x80072efd **--O--** 0x80072f76: error no especificado</span><span class="sxs-lookup"><span data-stu-id="ba1a9-122">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="ba1a9-123">**Excepción del registro del instalador&#8224;:** error 0x80072efd **--O--** 0x80072f76: No se ha podido ejecutar el paquete EXE</span><span class="sxs-lookup"><span data-stu-id="ba1a9-123">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="ba1a9-124">&#8224;El registro se encuentra en *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-124">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="ba1a9-125">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-125">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-126">Si el sistema no tiene acceso a Internet al [instalar la agrupación de hospedaje .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-126">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="ba1a9-127">Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-127">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="ba1a9-128">Si se produce un error en el instalador, es posible que el servidor no reciba el entorno de tiempo de ejecución de .NET Core necesario para hospedar una [implementación dependiente del marco (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-128">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="ba1a9-129">Si va a hospedar una FDD, confirme que el tiempo de ejecución está instalado en **Programas y características** o en **Aplicaciones y características**.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-129">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="ba1a9-130">Si se requiere un tiempo de ejecución específico, descargue el tiempo de ejecución de [Archivos de descarga de .NET](https://dotnet.microsoft.com/download/archives) e instálelo en el sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-130">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="ba1a9-131">Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-131">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="ba1a9-132">La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits</span><span class="sxs-lookup"><span data-stu-id="ba1a9-132">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="ba1a9-133">**Registro de aplicación:** el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** no se ha podido cargar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-133">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="ba1a9-134">Los datos son el error.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-134">The data is the error.</span></span>

<span data-ttu-id="ba1a9-135">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-135">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-136">Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-136">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="ba1a9-137">Este problema se produce si ha instalado el módulo ASP.NET Core antes de una actualización del sistema operativo y, a continuación, ejecuta cualquier grupo de aplicaciones en modo de 32 bits después de una actualización del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-137">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="ba1a9-138">Después de actualizar el sistema operativo, repare el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-138">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="ba1a9-139">Consulte [Instalación de la agrupación de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-139">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="ba1a9-140">Seleccione **Reparar** cuando se ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-140">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="ba1a9-141">Falta la extensión de sitio, se han instalado extensiones de sitio de 32 bits (x86) y 64 bits (x64) o se ha definido un valor de bits de proceso incorrecto</span><span class="sxs-lookup"><span data-stu-id="ba1a9-141">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="ba1a9-142">*Se aplica a las aplicaciones hospedadas por Azure App Services.*</span><span class="sxs-lookup"><span data-stu-id="ba1a9-142">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="ba1a9-143">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="ba1a9-143">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ba1a9-144">**Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-144">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ba1a9-145">No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-145">Could not find inprocess request handler.</span></span> <span data-ttu-id="ba1a9-146">Resultado obtenido al invocar a hostfxr: No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-146">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ba1a9-147">El marco especificado "Microsoft.AspNetCore.App", versión "{VERSION}-preview-\*" no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-147">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="ba1a9-148">No se pudo iniciar la aplicación "/LM/W3SVC/1416782824/ROOT", código de error "0x8000ffff".</span><span class="sxs-lookup"><span data-stu-id="ba1a9-148">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="ba1a9-149">**Registro de stdout del módulo de ASP.NET Core:** No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-149">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ba1a9-150">El marco especificado "Microsoft.AspNetCore.App", versión "{VERSION}-preview-\*" no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-150">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-151">**Registro de depuración del módulo de ASP.NET Core:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-151">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ba1a9-152">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-152">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ba1a9-153">Se ha devuelto un valor HRESULT con errores: 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-153">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="ba1a9-154">No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-154">Could not find inprocess request handler.</span></span> <span data-ttu-id="ba1a9-155">No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-155">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ba1a9-156">El marco especificado "Microsoft.AspNetCore.App", versión "{VERSION}-preview-\*" no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-156">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-157">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-157">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-158">Si ejecuta la aplicación en un entorno de ejecución en versión preliminar, instale la extensión de sitio de 32 bits (x86) **o** de 64 bits (x64) que coincida con el valor de bits de la aplicación y la versión del entorno de ejecución de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-158">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="ba1a9-159">**No instale al mismo tiempo ambas extensiones o varias versiones del entorno de ejecución de la extensión.**</span><span class="sxs-lookup"><span data-stu-id="ba1a9-159">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="ba1a9-160">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span><span class="sxs-lookup"><span data-stu-id="ba1a9-160">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="ba1a9-161">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span><span class="sxs-lookup"><span data-stu-id="ba1a9-161">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="ba1a9-162">Reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-162">Restart the app.</span></span> <span data-ttu-id="ba1a9-163">Espere unos segundos a que finalice la operación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-163">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="ba1a9-164">Si ejecuta la aplicación en un entorno de ejecución en versión preliminar y están instaladas las [extensiones de sitio](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) de 32 bits (x86) y 64 bits (x64), desinstale la extensión de sitio que no coincida con el valor de bits de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-164">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="ba1a9-165">A continuación, reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-165">After removing the site extension, restart the app.</span></span> <span data-ttu-id="ba1a9-166">Espere unos segundos a que finalice la operación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-166">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="ba1a9-167">Si va a ejecutar la aplicación en un entorno de ejecución en versión preliminar y el valor de bits de la extensión de sitio coincide con el de la aplicación, confirme que la *versión del entorno de ejecución* de la extensión de sitio coincide con la versión del entorno de ejecución de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-167">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="ba1a9-168">Confirme que el valor de **Plataforma** de la aplicación en **Configuración de la aplicación** coincide con el valor de bits de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-168">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="ba1a9-169">Para más información, consulte <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-169">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="ba1a9-170">Se ha implementado una aplicación x86, pero el grupo de aplicaciones no está habilitado para aplicaciones de 32 bits</span><span class="sxs-lookup"><span data-stu-id="ba1a9-170">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="ba1a9-171">**Explorador:** Error HTTP 500.30: error de inicio en el proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="ba1a9-171">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="ba1a9-172">**Registro de aplicación:** En la aplicación '/LM/W3SVC/5/ROOT' con la raíz física '{PATH}' se ha producido una excepción administrada inesperada, código de excepción = '0xe0434352'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-172">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="ba1a9-173">Compruebe los registros stderr para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-173">Please check the stderr logs for more information.</span></span> <span data-ttu-id="ba1a9-174">La aplicación '/LM/W3SVC/5/ROOT' con la raíz física '{PATH}' no ha podido cargar el CLR ni la aplicación administrada.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-174">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="ba1a9-175">El subproceso de trabajo CLR se ha cerrado prematuramente</span><span class="sxs-lookup"><span data-stu-id="ba1a9-175">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="ba1a9-176">**Registro de stdout del módulo ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-176">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-177">**Registro de depuración del módulo de ASP.NET Core:** Se ha devuelto un valor HRESULT con errores: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="ba1a9-177">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-178">El SDK intercepta este escenario al publicar una aplicación independiente.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-178">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="ba1a9-179">El SDK genera un error si el RID no coincide con el destino de plataforma (por ejemplo, RID de `win10-x64` con `<PlatformTarget>x86</PlatformTarget>` en el archivo del proyecto).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-179">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="ba1a9-180">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-180">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-181">En el caso de una implementación dependiente del marco x86 (`<PlatformTarget>x86</PlatformTarget>`), habilite el grupo de aplicaciones de IIS para aplicaciones de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-181">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="ba1a9-182">En el Administrador de IIS, abra la **Configuración avanzada** del grupo de aplicaciones y establezca **Habilitar aplicaciones de 32 bits** en **True**.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-182">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="ba1a9-183">Conflictos de plataforma con RID</span><span class="sxs-lookup"><span data-stu-id="ba1a9-183">Platform conflicts with RID</span></span>

* <span data-ttu-id="ba1a9-184">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="ba1a9-184">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ba1a9-185">**Registro de aplicación:** la aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"C:\{{PATH}{ASSEMBLY}.{exe|dll}"', código de error = '0x80004005 : ff'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-185">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="ba1a9-186">**Registro de stdout del módulo ASP.NET Core:** Excepción no controlada: System.BadImageFormatException: No se ha podido cargar el archivo ni el ensamblado '{ASSEMBLY}.dll'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-186">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="ba1a9-187">Se ha intentado cargar un programa con un formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-187">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="ba1a9-188">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-188">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-189">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-189">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="ba1a9-190">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-190">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="ba1a9-191">Para más información, consulte <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-191">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="ba1a9-192">Si esta excepción se produce en una implementación de Azure Apps al actualizar una aplicación e implementar ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-192">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="ba1a9-193">Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-193">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="ba1a9-194">Punto de conexión de URI incorrecto o sitio web detenido</span><span class="sxs-lookup"><span data-stu-id="ba1a9-194">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="ba1a9-195">**Explorador:** ERR_CONNECTION_REFUSED **--O--** No se podido establecer la conexión</span><span class="sxs-lookup"><span data-stu-id="ba1a9-195">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="ba1a9-196">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="ba1a9-196">**Application Log:** No entry</span></span>

* <span data-ttu-id="ba1a9-197">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-197">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-198">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-198">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-199">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-199">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-200">Confirme que se usa el punto de conexión de URI correcto para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-200">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="ba1a9-201">Compruebe los enlaces.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-201">Check the bindings.</span></span>

* <span data-ttu-id="ba1a9-202">Confirme que el sitio web de IIS no está en estado *Detenido*.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-202">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="ba1a9-203">Características de servidor CoreWebEngine o W3SVC deshabilitadas</span><span class="sxs-lookup"><span data-stu-id="ba1a9-203">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="ba1a9-204">**Excepción de sistema operativo:** Para usar el módulo ASP.NET Core, se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-204">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="ba1a9-205">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-205">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-206">Confirme que están habilitados el rol y las características correctos.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-206">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="ba1a9-207">Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-207">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="ba1a9-208">Ruta de acceso física de sitio web incorrecta o aplicación que falta</span><span class="sxs-lookup"><span data-stu-id="ba1a9-208">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="ba1a9-209">**Explorador:** 403 Prohibido: acceso denegado **--O BIEN--** 403.14 Prohibido: el servidor web está configurado para no mostrar una lista de los contenidos de este directorio.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-209">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="ba1a9-210">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="ba1a9-210">**Application Log:** No entry</span></span>

* <span data-ttu-id="ba1a9-211">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-211">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-212">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-212">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-213">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-213">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-214">Consulte la opción **Configuración básica** del sitio web de IIS y la carpeta de la aplicación física.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-214">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="ba1a9-215">Confirme que la aplicación está en la carpeta en la **ruta de acceso física** del sitio web de IIS.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-215">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="ba1a9-216">Rol incorrecto, módulo de ASP.NET Core no instalado o permisos incorrectos</span><span class="sxs-lookup"><span data-stu-id="ba1a9-216">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="ba1a9-217">**Explorador:** 500.19 Error interno del servidor: no se puede acceder a la página solicitada porque los datos de configuración relacionados de la página no son válidos.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-217">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="ba1a9-218">**--O--** No se puede mostrar esta página</span><span class="sxs-lookup"><span data-stu-id="ba1a9-218">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="ba1a9-219">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="ba1a9-219">**Application Log:** No entry</span></span>

* <span data-ttu-id="ba1a9-220">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-220">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-221">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-221">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-222">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-222">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-223">Confirme que está habilitado el rol adecuado.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-223">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="ba1a9-224">Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-224">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="ba1a9-225">Abra **Programas y características** o **Aplicaciones y características** y confirme que **Hospedaje de Windows Server** está instalado.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-225">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="ba1a9-226">Si **Hospedaje de Windows Server** no está presente en la lista de programas instalados, descargue e instale el conjunto de hospedaje de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-226">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="ba1a9-227">Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)</span><span class="sxs-lookup"><span data-stu-id="ba1a9-227">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="ba1a9-228">Para obtener más información, consulte [Instalar el conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-228">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="ba1a9-229">Asegúrese de que **Grupo de aplicaciones** > **Modelo de proceso** > **Identidad** esté establecido en **ApplicationPoolIdentity** o que la identidad personalizada tenga los permisos correctos para acceder a la carpeta de implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-229">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="ba1a9-230">Si ha desinstalado el paquete de hospedaje de ASP.NET Core y ha instalado una versión anterior de dicho paquete, el archivo *applicationHost.config* no incluirá ninguna sección para el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-230">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="ba1a9-231">Abra *applicationHost.config* en *%windir%/System32/inetsrv/config* y busque el grupo de secciones `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-231">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="ba1a9-232">Si falta la sección del módulo ASP.NET Core en el grupo de secciones, añada el elemento de sección:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-232">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="ba1a9-233">Si quiere, también puede instalar la versión más reciente del paquete de hospedaje de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-233">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="ba1a9-234">La versión más reciente es compatible con las versiones anteriores de las aplicaciones ASP.NET Core admitidas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-234">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="ba1a9-235">Elemento processPath incorrecto, falta la variable PATH, agrupación de hospedaje no instalada, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="ba1a9-235">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-236">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="ba1a9-236">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ba1a9-237">**Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"{...}"</span><span class="sxs-lookup"><span data-stu-id="ba1a9-237">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="ba1a9-238">', código de error = '0 x 80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-238">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="ba1a9-239">La aplicación '{PATH}' no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-239">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="ba1a9-240">No se ha encontrado el archivo ejecutable en '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-240">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="ba1a9-241">No se ha podido iniciar la aplicación '/LM/W3SVC/2/ROOT', código de error '0x8007023e'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-241">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="ba1a9-242">**Registro de stdout del módulo ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-242">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="ba1a9-243">**Registro de depuración del módulo de ASP.NET Core:** Registro de eventos: "La aplicación '{PATH}'" no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-243">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="ba1a9-244">No se ha encontrado el archivo ejecutable en '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-244">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="ba1a9-245">Se ha devuelto un valor HRESULT con errores: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="ba1a9-245">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ba1a9-246">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="ba1a9-246">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ba1a9-247">**Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"{...}"</span><span class="sxs-lookup"><span data-stu-id="ba1a9-247">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="ba1a9-248">', código de error = '0 x 80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-248">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="ba1a9-249">**Registro de stdout del módulo ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-249">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-250">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-250">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-251">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-251">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="ba1a9-252">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-252">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="ba1a9-253">Para más información, consulte <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-253">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="ba1a9-254">Compruebe el atributo *processPath* del elemento `<aspNetCore>` de *web.config* para confirmar que es `dotnet` para una implementación dependiente del marco (FDD) o `.\{ASSEMBLY}.exe` para una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-254">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="ba1a9-255">En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-255">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="ba1a9-256">Confirme que *C:\Archivos de programa\dotnet\\* existe en el valor PATH del sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-256">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="ba1a9-257">En el caso de una FDD, es posible que la identidad del usuario del grupo de aplicaciones no pueda acceder a *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-257">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="ba1a9-258">Confirme que la identidad del usuario del grupo de aplicaciones tiene acceso al directorio *C:\Archivos de programa\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-258">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="ba1a9-259">Confirme que no haya ninguna regla de denegación configurada para la identidad del usuario del grupo de aplicaciones en los directorios *C:\Archivos de programa\dotnet* y de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-259">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="ba1a9-260">Puede que se haya implementado una FDD y que se instalara .NET Core sin reiniciar IIS.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-260">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="ba1a9-261">Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-261">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ba1a9-262">Puede que se haya implementado una FDD sin instalar el entorno de tiempo de ejecución de .NET Core en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-262">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="ba1a9-263">Si no se ha instalado el entorno de tiempo de ejecución de .NET Core, ejecute el **instalador de la agrupación de hospedaje de .NET Core** en el sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-263">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="ba1a9-264">Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)</span><span class="sxs-lookup"><span data-stu-id="ba1a9-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="ba1a9-265">Para obtener más información, consulte [Instalar el conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-265">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="ba1a9-266">Si se requiere un tiempo de ejecución específico, descargue el tiempo de ejecución de [Archivos de descarga de .NET](https://dotnet.microsoft.com/download/archives) e instálelo en el sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-266">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="ba1a9-267">Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-267">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ba1a9-268">Puede que se haya implementado una FDD y que el paquete *Microsoft Visual C++ 2015 Redistributable (x64)* no esté instalado en el sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-268">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="ba1a9-269">Obtenga un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-269">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="ba1a9-270">Argumentos incorrectos del elemento \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="ba1a9-270">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-271">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="ba1a9-271">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ba1a9-272">**Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-272">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ba1a9-273">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-273">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ba1a9-274">No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-274">Could not find inprocess request handler.</span></span> <span data-ttu-id="ba1a9-275">Resultado obtenido al invocar a hostfxr: ¿pretendía ejecutar comandos SDK de dotnet?</span><span class="sxs-lookup"><span data-stu-id="ba1a9-275">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="ba1a9-276">Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 No se ha podido iniciar la aplicación '/LM/W3SVC/3/ROOT', código de error '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-276">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="ba1a9-277">**Registro de stdout del módulo ASP.NET Core:** ¿pretendía ejecutar comandos SDK de dotnet?</span><span class="sxs-lookup"><span data-stu-id="ba1a9-277">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="ba1a9-278">Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="ba1a9-278">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="ba1a9-279">**Registro de depuración del módulo de ASP.NET Core:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-279">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ba1a9-280">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-280">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ba1a9-281">Se ha devuelto un valor HRESULT con errores: 0x8000ffff No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-281">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="ba1a9-282">Resultado obtenido al invocar a hostfxr: ¿pretendía ejecutar comandos SDK de dotnet?</span><span class="sxs-lookup"><span data-stu-id="ba1a9-282">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="ba1a9-283">Instale el SDK de dotnet de: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Se ha devuelto un valor HRESULT con errores: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="ba1a9-283">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ba1a9-284">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="ba1a9-284">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ba1a9-285">**Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con la raíz física 'C:\{{PATH}\'' no ha podido iniciar el proceso con la línea de comandos '"dotnet" .\{ASSEMBLY}.dll', código de error = '0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-285">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="ba1a9-286">**Registro de stdout del módulo de ASP.NET Core:** La aplicación que se va a ejecutar no existe: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="ba1a9-286">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-287">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-287">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-288">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-288">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="ba1a9-289">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-289">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="ba1a9-290">Para más información, consulte <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-290">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="ba1a9-291">Examine el atributo *arguments* del elemento `<aspNetCore>` en *web.config* para confirmar que (a) es `.\{ASSEMBLY}.dll` para una implementación dependiente del marco (FDD); o (b) no está presente, es una cadena vacía (`arguments=""`) o es una lista de argumentos de la aplicación (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) para una implementación independiente (SCD).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-291">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="ba1a9-292">Falta el marco compartido de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ba1a9-292">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="ba1a9-293">**Explorador:** Error HTTP 500.0: error de carga del controlador en proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="ba1a9-293">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ba1a9-294">**Registro de aplicación:** No se ha podido invocar a hostfxr para encontrar el controlador de la solicitud en proceso ni se han encontrado dependencias nativas.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-294">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ba1a9-295">Lo más probable es que esto signifique que la aplicación no está configurada correctamente. Compruebe las versiones de Microsoft.NetCore.App y de Microsoft.AspNetCore.App que la aplicación tiene como destino y que están instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-295">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ba1a9-296">No se ha podido encontrar el controlador de la solicitud en proceso.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-296">Could not find inprocess request handler.</span></span> <span data-ttu-id="ba1a9-297">Resultado obtenido al invocar a hostfxr: No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-297">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ba1a9-298">El marco especificado 'Microsoft.AspNetCore.App', versión '{VERSION}', no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-298">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="ba1a9-299">No se ha podido iniciar la aplicación '/LM/W3SVC/5/ROOT', código de error '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-299">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="ba1a9-300">**Registro de stdout del módulo de ASP.NET Core:** No se ha podido encontrar ninguna versión de Framework compatible.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-300">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ba1a9-301">El marco especificado 'Microsoft.AspNetCore.App', versión '{VERSION}', no se ha podido encontrar.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-301">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="ba1a9-302">**Registro de depuración del módulo de ASP.NET Core:** Se ha devuelto un valor HRESULT con errores: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="ba1a9-302">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-303">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-303">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-304">En el caso de una implementación dependiente del marco (FDD), confirme que tiene instalado el entorno de tiempo de ejecución correcto en el sistema.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-304">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="ba1a9-305">Grupo de aplicaciones detenido</span><span class="sxs-lookup"><span data-stu-id="ba1a9-305">Stopped Application Pool</span></span>

* <span data-ttu-id="ba1a9-306">**Explorador:** 503 Servicio no disponible</span><span class="sxs-lookup"><span data-stu-id="ba1a9-306">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="ba1a9-307">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="ba1a9-307">**Application Log:** No entry</span></span>

* <span data-ttu-id="ba1a9-308">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-308">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-309">**Registro de depuración del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-309">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-310">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-310">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-311">Confirme que el grupo de aplicaciones no está en estado *Detenido*.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-311">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="ba1a9-312">La aplicación secundaria incluye una sección de \<controladores></span><span class="sxs-lookup"><span data-stu-id="ba1a9-312">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="ba1a9-313">**Explorador:** error HTTP 500.19: error interno del servidor</span><span class="sxs-lookup"><span data-stu-id="ba1a9-313">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="ba1a9-314">**Registro de aplicación:** No hay ninguna entrada</span><span class="sxs-lookup"><span data-stu-id="ba1a9-314">**Application Log:** No entry</span></span>

* <span data-ttu-id="ba1a9-315">**Registro de stdout del módulo de ASP.NET Core:** El archivo de registro de la aplicación raíz se ha creado y muestra un funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-315">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="ba1a9-316">No se ha creado el archivo de registro de la aplicación secundaria.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-316">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-317">**Registro de depuración del módulo de ASP.NET Core:** El archivo de registro de la aplicación raíz se ha creado y muestra un funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-317">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="ba1a9-318">No se ha creado el archivo de registro de la aplicación secundaria.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-318">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-319">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-319">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ba1a9-320">Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>` o que la aplicación secundaria no hereda los controladores de la aplicación primaria.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-320">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="ba1a9-321">La sección `<system.webServer>` de la aplicación primaria de *web.config* está colocada dentro de un elemento `<location>`.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-321">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="ba1a9-322">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación primaria no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-322">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="ba1a9-323">Para más información, consulte <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-323">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ba1a9-324">Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-324">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="ba1a9-325">Ruta de acceso incorrecta al registro de stdout</span><span class="sxs-lookup"><span data-stu-id="ba1a9-325">stdout log path incorrect</span></span>

* <span data-ttu-id="ba1a9-326">**Explorador:** la aplicación responde normalmente.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-326">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-327">**Registro de aplicación:** No se ha podido iniciar la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-327">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ba1a9-328">Mensaje de excepción: HRESULT 0 x 80070005 se ha devuelto en {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-328">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="ba1a9-329">No se ha podido detener la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-329">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ba1a9-330">Mensaje de excepción: HRESULT 0 x 80070002 se ha devuelto en {PATH}.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-330">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="ba1a9-331">No se ha podido iniciar la redirección de stdout en {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-331">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="ba1a9-332">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-332">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="ba1a9-333">**Registro de depuración del módulo de ASP.NET Core:** No se ha podido iniciar la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-333">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ba1a9-334">Mensaje de excepción: HRESULT 0 x 80070005 se ha devuelto en {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-334">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="ba1a9-335">No se ha podido detener la redirección de stdout en C:\Archivos de programa\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-335">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ba1a9-336">Mensaje de excepción: HRESULT 0 x 80070002 se ha devuelto en {PATH}.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-336">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="ba1a9-337">No se ha podido iniciar la redirección de stdout en {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-337">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ba1a9-338">**Registro de aplicación:** Advertencia: No se ha podido crear el archivo de registro de stdout \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, código de error = -2147024893.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-338">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="ba1a9-339">**Registro de stdout del módulo de ASP.NET Core:** No se ha creado el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-339">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-340">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-340">Troubleshooting:</span></span>

* <span data-ttu-id="ba1a9-341">La ruta de acceso `stdoutLogFile` especificada en el elemento `<aspNetCore>` de *web.config* no existe.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-341">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="ba1a9-342">Para obtener más información, consulte [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creación y redireccionamiento de registros: módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="ba1a9-342">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="ba1a9-343">El usuario del grupo de aplicaciones no tiene acceso de escritura a la ruta de acceso del registro de stdout.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-343">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="ba1a9-344">Problema general de configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="ba1a9-344">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ba1a9-345">**Explorador:** error HTTP 500.0: error de carga del controlador en proceso ANCM **--O--** Error HTTP 500.30: error de inicio en el proceso ANCM</span><span class="sxs-lookup"><span data-stu-id="ba1a9-345">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="ba1a9-346">**Registro de aplicación:** Variable</span><span class="sxs-lookup"><span data-stu-id="ba1a9-346">**Application Log:** Variable</span></span>

* <span data-ttu-id="ba1a9-347">**Registro de stdout del módulo de ASP.NET Core:** El archivo de registro se ha creado, pero está vacío o se ha creado con entradas normales, hasta el punto en que se producen errores en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-347">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="ba1a9-348">**Registro de depuración del módulo de ASP.NET Core:** Variable</span><span class="sxs-lookup"><span data-stu-id="ba1a9-348">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ba1a9-349">**Explorador:** error HTTP 502.5: error en el proceso</span><span class="sxs-lookup"><span data-stu-id="ba1a9-349">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ba1a9-350">**Registro de aplicación:** la aplicación 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con la raíz física 'C:\{PATH}\'' ha creado el proceso con la línea de comandos '"C:\{PATH}\{ASSEMBLY}.{exe|dll}"', pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado "{PORT}", código de error = '{ERROR CODE}'</span><span class="sxs-lookup"><span data-stu-id="ba1a9-350">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="ba1a9-351">**Registro de stdout del módulo de ASP.NET Core:** El archivo de registro se ha creado, pero está vacío.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-351">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="ba1a9-352">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-352">Troubleshooting:</span></span>

<span data-ttu-id="ba1a9-353">El proceso no se ha iniciado, probablemente debido a un problema de configuración o programación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ba1a9-353">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="ba1a9-354">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="ba1a9-354">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
