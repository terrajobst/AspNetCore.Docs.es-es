---
title: Referencia de errores comunes de IIS con ASP.NET Core y de servicio de aplicaciones de Azure
author: guardrex
description: Distinguir los errores comunes al hospedar aplicaciones ASP.NET Core en IIS y el servicio de aplicaciones de Azure.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: fb833ef8797ea7851cbaf53bb5681df248d07a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="3e73f-103">Referencia de errores comunes de IIS con ASP.NET Core y de servicio de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="3e73f-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="3e73f-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3e73f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3e73f-105">La siguiente no es una lista completa de errores.</span><span class="sxs-lookup"><span data-stu-id="3e73f-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="3e73f-106">Si se produce un error no aparece aquí, [abrir un nuevo asunto](https://github.com/aspnet/Docs/issues/new) con instrucciones detalladas para reproducir el error.</span><span class="sxs-lookup"><span data-stu-id="3e73f-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="3e73f-107">Recopile la información siguiente:</span><span class="sxs-lookup"><span data-stu-id="3e73f-107">Collect the following information:</span></span>

* <span data-ttu-id="3e73f-108">Comportamiento del explorador</span><span class="sxs-lookup"><span data-stu-id="3e73f-108">Browser behavior</span></span>
* <span data-ttu-id="3e73f-109">Entradas de registro de eventos de aplicación</span><span class="sxs-lookup"><span data-stu-id="3e73f-109">Application Event Log entries</span></span>
* <span data-ttu-id="3e73f-110">Entradas del registro de stdout de módulo principal ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3e73f-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="3e73f-111">Compare la información con los siguientes errores comunes.</span><span class="sxs-lookup"><span data-stu-id="3e73f-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="3e73f-112">Si se encuentra una coincidencia, siga los consejos para solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="3e73f-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="3e73f-113">El instalador no puede obtener VC++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="3e73f-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="3e73f-114">**Excepción del instalador:** 0x80072efd o 0x80072f76 - Error no especificado</span><span class="sxs-lookup"><span data-stu-id="3e73f-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="3e73f-115">**Excepción del registro del instalador&#8224;:** Error 0x80072efd o 0x80072f76: Error al ejecutar el paquete EXE</span><span class="sxs-lookup"><span data-stu-id="3e73f-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="3e73f-116">&#8224;El registro se encuentra en C:\Users\\{USUARIO}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="3e73f-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="3e73f-117">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-117">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-118">Si el sistema no tiene acceso a Internet al instalar el lote de hospedaje del servidor, se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="3e73f-118">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="3e73f-119">Obtener un instalador de la [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="3e73f-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="3e73f-120">Si se produce un error en el programa de instalación, es posible que el servidor no reciba el tiempo de ejecución de .NET Core necesaria para hospedar una implementación de framework dependiente (FDD).</span><span class="sxs-lookup"><span data-stu-id="3e73f-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="3e73f-121">Si hospeda un FDD, confirme que está instalado el tiempo de ejecución en programas &amp; características.</span><span class="sxs-lookup"><span data-stu-id="3e73f-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="3e73f-122">Si es necesario, obtenga un instalador en tiempo de ejecución de [todas las descargas de .NET](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="3e73f-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="3e73f-123">Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="3e73f-124">La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits</span><span class="sxs-lookup"><span data-stu-id="3e73f-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="3e73f-125">**Registro de aplicación:** Error al cargar el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**.</span><span class="sxs-lookup"><span data-stu-id="3e73f-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="3e73f-126">Los datos son el error.</span><span class="sxs-lookup"><span data-stu-id="3e73f-126">The data is the error.</span></span>

<span data-ttu-id="3e73f-127">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-127">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-128">Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="3e73f-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="3e73f-129">Si está instalado el módulo de núcleo de ASP.NET anterior a una actualización del sistema operativo y, a continuación, en cualquier grupo de aplicaciones se ejecuta en modo de 32 bits después de actualizar el sistema operativo, donde se produzca este problema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="3e73f-130">Después de actualizar el sistema operativo, repare el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e73f-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="3e73f-131">Vea [Instalación del lote de hospedaje .NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="3e73f-131">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="3e73f-132">Seleccione **reparación** cuando se ejecuta el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="3e73f-133">Conflictos de plataforma con RID</span><span class="sxs-lookup"><span data-stu-id="3e73f-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="3e73f-134">**Explorador:** Error HTTP 502.5 - Error en el proceso</span><span class="sxs-lookup"><span data-stu-id="3e73f-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3e73f-135">**Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\{ruta}\' no se pudo iniciar el proceso con la línea de comandos ' "C:\\{PATH} {ensamblado}. {} exe | dll} "', ErrorCode = ' 0 x 80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="3e73f-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="3e73f-136">**Registro de módulo de ASP.NET Core:** una excepción no controlada: System.BadImageFormatException: no se pudo cargar archivo o ensamblado '.dll {ensamblado}'.</span><span class="sxs-lookup"><span data-stu-id="3e73f-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="3e73f-137">Se ha intentado cargar un programa con un formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="3e73f-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="3e73f-138">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-138">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-139">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3e73f-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3e73f-140">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3e73f-141">Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3e73f-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="3e73f-142">Confirme que la `<PlatformTarget>` en el *.csproj* no entra en conflicto con el RID.</span><span class="sxs-lookup"><span data-stu-id="3e73f-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="3e73f-143">Por ejemplo, no se especifica un `<PlatformTarget>` de `x86` y publicar con un RID de `win10-x64`, ya sea mediante *dotnet publicar - c - r de versión win10-x64* o estableciendo la `<RuntimeIdentifiers>` en el *.csproj*  a `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="3e73f-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="3e73f-144">El proyecto se publica sin advertencias ni errores, pero se produce un error con las excepciones registradas anteriores en el sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="3e73f-145">Si esta excepción se produce para ver una implementación de aplicaciones de Azure al actualizar una aplicación y la implementación de ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior.</span><span class="sxs-lookup"><span data-stu-id="3e73f-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="3e73f-146">Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.</span><span class="sxs-lookup"><span data-stu-id="3e73f-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="3e73f-147">Punto de conexión de URI incorrecto o sitio web detenido</span><span class="sxs-lookup"><span data-stu-id="3e73f-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="3e73f-148">**Explorador:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="3e73f-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="3e73f-149">**Registro de aplicación:** Sin entrada</span><span class="sxs-lookup"><span data-stu-id="3e73f-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="3e73f-150">**Registro del módulo ASP.NET Core:** Archivo de registro no creado</span><span class="sxs-lookup"><span data-stu-id="3e73f-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="3e73f-151">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-151">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-152">Confirme que se usa el extremo URI correcto para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="3e73f-153">Compruebe los enlaces.</span><span class="sxs-lookup"><span data-stu-id="3e73f-153">Check the bindings.</span></span>

* <span data-ttu-id="3e73f-154">Confirme que el sitio Web IIS no se encuentra en la *detenido* estado.</span><span class="sxs-lookup"><span data-stu-id="3e73f-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="3e73f-155">Características de servidor CoreWebEngine o W3SVC deshabilitadas</span><span class="sxs-lookup"><span data-stu-id="3e73f-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="3e73f-156">**Excepción de sistema operativo:** Se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC para usar el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e73f-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="3e73f-157">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-157">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-158">Confirme que la función adecuada y las características están habilitados.</span><span class="sxs-lookup"><span data-stu-id="3e73f-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="3e73f-159">Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="3e73f-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="3e73f-160">Ruta de acceso física del sitio Web incorrecto o falta de aplicación</span><span class="sxs-lookup"><span data-stu-id="3e73f-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="3e73f-161">**Explorador:** 403 Prohibido - Acceso denegado **--O BIEN--** 403.14 Prohibido - El servidor web está configurado para no mostrar una lista de los contenidos de este directorio.</span><span class="sxs-lookup"><span data-stu-id="3e73f-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="3e73f-162">**Registro de aplicación:** Sin entrada</span><span class="sxs-lookup"><span data-stu-id="3e73f-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="3e73f-163">**Registro del módulo ASP.NET Core:** Archivo de registro no creado</span><span class="sxs-lookup"><span data-stu-id="3e73f-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="3e73f-164">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-164">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-165">Consulte el sitio Web IIS **configuración básica** y la carpeta de aplicaciones físicas.</span><span class="sxs-lookup"><span data-stu-id="3e73f-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="3e73f-166">Confirme que la aplicación se encuentra en la carpeta en el sitio Web IIS **ruta de acceso física**.</span><span class="sxs-lookup"><span data-stu-id="3e73f-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="3e73f-167">Rol incorrecto, módulo no instalado o permisos incorrectos</span><span class="sxs-lookup"><span data-stu-id="3e73f-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="3e73f-168">**Explorador:** 500.19 Error interno del servidor - No se puede obtener acceso a la página solicitada porque los datos de configuración relacionados de la página no son válidos.</span><span class="sxs-lookup"><span data-stu-id="3e73f-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="3e73f-169">**Registro de aplicación:** Sin entrada</span><span class="sxs-lookup"><span data-stu-id="3e73f-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="3e73f-170">**Registro del módulo ASP.NET Core:** Archivo de registro no creado</span><span class="sxs-lookup"><span data-stu-id="3e73f-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="3e73f-171">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-171">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-172">Confirme que está habilitada la función adecuada.</span><span class="sxs-lookup"><span data-stu-id="3e73f-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="3e73f-173">Vea [Configuración de IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="3e73f-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="3e73f-174">Compruebe **Programas &amp; Características** y confirme que el **módulo Microsoft ASP.NET Core** se ha instalado.</span><span class="sxs-lookup"><span data-stu-id="3e73f-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="3e73f-175">Si el **módulo Microsoft ASP.NET Core** no aparece en la lista de programas instalados, instálelo.</span><span class="sxs-lookup"><span data-stu-id="3e73f-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="3e73f-176">Vea [Instalación del lote de hospedaje .NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="3e73f-176">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="3e73f-177">Asegúrese de que el **grupo de aplicaciones** > **modelo de proceso** > **identidad** está establecido en **ApplicationPoolIdentity** o la identidad personalizada tiene los permisos correctos para tener acceso a la carpeta de implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="3e73f-178">Elemento processPath incorrecto, variable PATH que falta, lote de hospedaje no instalado, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="3e73f-178">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="3e73f-179">**Explorador:** Error HTTP 502.5 - Error en el proceso</span><span class="sxs-lookup"><span data-stu-id="3e73f-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3e73f-180">**Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' no se pudo iniciar el proceso con la línea de comandos ' ".\{ ".exe" ensamblado} ', ErrorCode = ' 0 x 80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="3e73f-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="3e73f-181">**Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío</span><span class="sxs-lookup"><span data-stu-id="3e73f-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="3e73f-182">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-182">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-183">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3e73f-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3e73f-184">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3e73f-185">Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3e73f-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="3e73f-186">Compruebe el *processPath* del atributo en el `<aspNetCore>` elemento *web.config* para confirmar que es *dotnet* para una implementación de framework dependiente (FDD) o *. \{ensamblado} .exe* para una implementación independiente (SCD).</span><span class="sxs-lookup"><span data-stu-id="3e73f-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="3e73f-187">En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH.</span><span class="sxs-lookup"><span data-stu-id="3e73f-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="3e73f-188">Confirme que *C:\Archivos de programa\dotnet\* existe en el valor PATH del sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="3e73f-189">En el caso de una FDD, *dotnet.exe* podría no ser accesible para la identidad del usuario del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="3e73f-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="3e73f-190">Confirme que la identidad del usuario de AppPool tiene acceso al directorio *C:\Archivos de programa\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="3e73f-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="3e73f-191">Confirme que no hay ninguna regla de denegación configurada para la identidad del usuario AppPool en el *Files\dotnet C:\Program* y directorios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="3e73f-192">Se han implementado un FDD y .NET Core instalado sin necesidad de reiniciar IIS.</span><span class="sxs-lookup"><span data-stu-id="3e73f-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="3e73f-193">Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.</span><span class="sxs-lookup"><span data-stu-id="3e73f-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="3e73f-194">Puede haberse implementado un FDD sin necesidad de instalar el runtime de .NET Core en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="3e73f-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="3e73f-195">Si no se instaló el runtime de .NET Core, ejecute el **instalador del paquete de hospedaje de .NET Core Windows Server** en el sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="3e73f-196">Vea [Instalación del lote de hospedaje .NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="3e73f-196">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="3e73f-197">Si intenta instalar el runtime de .NET Core en un sistema sin una conexión a Internet, obtener el tiempo de ejecución de [todas las descargas de .NET](https://www.microsoft.com/net/download/all) y ejecute el instalador de paquete de hospedaje para instalar el módulo de núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3e73f-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="3e73f-198">Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="3e73f-199">Se han implementado un FDD y *Microsoft Visual C++ 2015 Redistributable (x64)* no está instalado en el sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="3e73f-200">Obtener un instalador de la [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="3e73f-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="3e73f-201">Argumentos incorrectos del elemento \<aspNetCore\></span><span class="sxs-lookup"><span data-stu-id="3e73f-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="3e73f-202">**Explorador:** Error HTTP 502.5 - Error en el proceso</span><span class="sxs-lookup"><span data-stu-id="3e73f-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3e73f-203">**Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' no se pudo iniciar el proceso con la línea de comandos ' "dotnet".\{ DLL de ensamblado}', ErrorCode = ' 0 x 80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="3e73f-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="3e73f-204">**Registro de módulo de ASP.NET Core:** la aplicación para ejecutar no existe: ' ruta de acceso\{ensamblado} .dll '</span><span class="sxs-lookup"><span data-stu-id="3e73f-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="3e73f-205">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-205">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-206">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3e73f-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3e73f-207">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3e73f-208">Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3e73f-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="3e73f-209">Examinar el *argumentos* del atributo en el `<aspNetCore>` elemento *web.config* para confirmar que es (a) *.\{ DLL de ensamblado}* para una implementación de framework dependiente (FDD); o (b) no está presente, una cadena vacía (*argumentos = ""*), o una lista de argumentos de la aplicación (*argumentos = "arg1, arg2,..."*) para una implementación independiente (SCD).</span><span class="sxs-lookup"><span data-stu-id="3e73f-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="3e73f-210">Falta la versión de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="3e73f-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="3e73f-211">**Explorador:** 502.3 Puerta de enlace incorrecta - Error de conexión al intentar enrutar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3e73f-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="3e73f-212">**Registro de aplicación:** ErrorCode = Application ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' no se pudo iniciar el proceso con la línea de comandos ' "dotnet".\{ DLL de ensamblado}', ErrorCode = ' 0 x 80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="3e73f-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="3e73f-213">**Registro del módulo ASP.NET Core:** Excepción de falta de método, archivo o ensamblado.</span><span class="sxs-lookup"><span data-stu-id="3e73f-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="3e73f-214">El método, el archivo o el ensamblado especificado en la excepción es un método, archivo o ensamblado de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3e73f-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="3e73f-215">Solución del problema:</span><span class="sxs-lookup"><span data-stu-id="3e73f-215">Troubleshooting:</span></span>

* <span data-ttu-id="3e73f-216">Instale la versión de .NET Framework que falta en el sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="3e73f-217">Para una implementación de framework dependiente (FDD), confirme que el tiempo de ejecución correcto instalado en el sistema.</span><span class="sxs-lookup"><span data-stu-id="3e73f-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="3e73f-218">Si el proyecto se actualiza de la versión 1.1 a 2.0, implementado en el sistema de hospedaje, y produce esta excepción, compruebe que el marco de trabajo 2.0 en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="3e73f-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="3e73f-219">Grupo de aplicaciones detenido</span><span class="sxs-lookup"><span data-stu-id="3e73f-219">Stopped Application Pool</span></span>

* <span data-ttu-id="3e73f-220">**Explorador:** 503 Servicio no disponible</span><span class="sxs-lookup"><span data-stu-id="3e73f-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="3e73f-221">**Registro de aplicación:** Sin entrada</span><span class="sxs-lookup"><span data-stu-id="3e73f-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="3e73f-222">**Registro del módulo ASP.NET Core:** Archivo de registro no creado</span><span class="sxs-lookup"><span data-stu-id="3e73f-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="3e73f-223">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3e73f-223">Troubleshooting</span></span>

* <span data-ttu-id="3e73f-224">Confirme que el grupo de aplicaciones no se encuentra en la *detenido* estado.</span><span class="sxs-lookup"><span data-stu-id="3e73f-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="3e73f-225">Middleware de IIS Integration no implementado</span><span class="sxs-lookup"><span data-stu-id="3e73f-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="3e73f-226">**Explorador:** Error HTTP 502.5 - Error en el proceso</span><span class="sxs-lookup"><span data-stu-id="3e73f-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3e73f-227">**Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' creó el proceso con la línea de comandos ' "C:\\{PATH}\{ensamblado}. {} exe | dll} "' pero se bloqueó o no la respuesta no o no atendió en el puerto especificado '{PORT}', ErrorCode = '0x800705B4 '.</span><span class="sxs-lookup"><span data-stu-id="3e73f-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="3e73f-228">**Registro del módulo ASP.NET Core:** Archivo de registro creado que muestra un funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="3e73f-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="3e73f-229">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3e73f-229">Troubleshooting</span></span>

* <span data-ttu-id="3e73f-230">Confirme que la aplicación se ejecuta localmente en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3e73f-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="3e73f-231">Un error de proceso puede ser el resultado de un problema en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="3e73f-232">Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3e73f-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="3e73f-233">Confirme que ya sea:</span><span class="sxs-lookup"><span data-stu-id="3e73f-233">Confirm that either:</span></span>
  * <span data-ttu-id="3e73f-234">El middleware de integración de IIS es referencedby al llamar a la `UseIISIntegration` método en la aplicación `WebHostBuilder` (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="3e73f-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="3e73f-235">Los usos de las aplicaciones la `CreateDefaultBuilder` método (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="3e73f-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="3e73f-236">Vea [Hospedar en ASP.NET Core](xref:fundamentals/hosting) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="3e73f-236">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="3e73f-237">La aplicación secundaria incluye una sección de \<controladores\></span><span class="sxs-lookup"><span data-stu-id="3e73f-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="3e73f-238">**Explorador:** Error HTTP 500.19 - Error interno del servidor</span><span class="sxs-lookup"><span data-stu-id="3e73f-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="3e73f-239">**Registro de aplicación:** Sin entrada</span><span class="sxs-lookup"><span data-stu-id="3e73f-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="3e73f-240">**Registro de módulo de ASP.NET Core:** archivo de registro se crea y se muestra el funcionamiento normal de la aplicación raíz.</span><span class="sxs-lookup"><span data-stu-id="3e73f-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="3e73f-241">Archivo de registro no se creó para la aplicación de sub.</span><span class="sxs-lookup"><span data-stu-id="3e73f-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="3e73f-242">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3e73f-242">Troubleshooting</span></span>

* <span data-ttu-id="3e73f-243">Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="3e73f-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="3e73f-244">ruta de acceso de registro a stdout incorrecto</span><span class="sxs-lookup"><span data-stu-id="3e73f-244">stdout log path incorrect</span></span>

* <span data-ttu-id="3e73f-245">**Explorador:** la aplicación responde normalmente.</span><span class="sxs-lookup"><span data-stu-id="3e73f-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="3e73f-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span><span class="sxs-lookup"><span data-stu-id="3e73f-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="3e73f-247">**Registro del módulo ASP.NET Core:** Archivo de registro no creado</span><span class="sxs-lookup"><span data-stu-id="3e73f-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="3e73f-248">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3e73f-248">Troubleshooting</span></span>

* <span data-ttu-id="3e73f-249">El `stdoutLogFile` ruta de acceso especificada en el `<aspNetCore>` elemento de *web.config* no existe.</span><span class="sxs-lookup"><span data-stu-id="3e73f-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="3e73f-250">Para obtener más información, consulte el [creación y redirección de registro](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) sección del tema de referencia de configuración de módulo principal de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3e73f-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="3e73f-251">Problema general de configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="3e73f-251">Application configuration general issue</span></span>

* <span data-ttu-id="3e73f-252">**Explorador:** Error HTTP 502.5 - Error en el proceso</span><span class="sxs-lookup"><span data-stu-id="3e73f-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="3e73f-253">**Registro de aplicación:** aplicación ' MACHINE/WEBROOT/APPHOST / {ENSAMBLADO}' con raíz física ' C:\\{PATH}\' creó el proceso con la línea de comandos ' "C:\\{PATH}\{ensamblado}. {} exe | dll} "' pero se bloqueó o no la respuesta no o no atendió en el puerto especificado '{PORT}', ErrorCode = '0x800705B4 '.</span><span class="sxs-lookup"><span data-stu-id="3e73f-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="3e73f-254">**Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío</span><span class="sxs-lookup"><span data-stu-id="3e73f-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="3e73f-255">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3e73f-255">Troubleshooting</span></span>

* <span data-ttu-id="3e73f-256">Esta excepción general indica que el proceso no pudo iniciarse, probablemente debido a un problema de configuración de aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e73f-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="3e73f-257">Que hace referencia a [estructura de directorios](xref:host-and-deploy/directory-structure), confirme que la aplicación implementa archivos y carpetas son adecuadas y que están presentes los archivos de configuración de la aplicación y contienen la configuración correcta para la aplicación y el entorno.</span><span class="sxs-lookup"><span data-stu-id="3e73f-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="3e73f-258">Para obtener más información, consulte [solución de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="3e73f-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
