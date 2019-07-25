---
title: Solucionar problemas de ASP.NET Core en Azure App Service e IIS
author: guardrex
description: Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service y Internet Information Services (IIS) de aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/18/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: deae568a6ba88c9a8365b9d7f2df629899bc64a1
ms.sourcegitcommit: 16502797ea749e2690feaa5e652a65b89c007c89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2019
ms.locfileid: "68483317"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="171f8-103">Solucionar problemas de ASP.NET Core en Azure App Service e IIS</span><span class="sxs-lookup"><span data-stu-id="171f8-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="171f8-104">Por [Luke Latham](https://github.com/guardrex) y [Diego Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="171f8-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="171f8-105">En este artículo se proporciona información sobre los errores comunes de inicio de la aplicación e instrucciones sobre cómo diagnosticar errores cuando una aplicación se implementa en Azure App Service o IIS:</span><span class="sxs-lookup"><span data-stu-id="171f8-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="171f8-106">Errores de inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="171f8-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="171f8-107">Explica escenarios comunes de código de Estado HTTP de inicio.</span><span class="sxs-lookup"><span data-stu-id="171f8-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="171f8-108">Solución de problemas en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="171f8-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="171f8-109">Proporciona consejos para la solución de problemas de las aplicaciones implementadas en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="171f8-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="171f8-110">Solución de problemas de IIS</span><span class="sxs-lookup"><span data-stu-id="171f8-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="171f8-111">Proporciona consejos para la solución de problemas de aplicaciones implementadas en IIS o que se ejecutan en IIS Express localmente.</span><span class="sxs-lookup"><span data-stu-id="171f8-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="171f8-112">La guía se aplica a las implementaciones de Windows Server y escritorio de Windows.</span><span class="sxs-lookup"><span data-stu-id="171f8-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="171f8-113">Borrar cachés de paquetes</span><span class="sxs-lookup"><span data-stu-id="171f8-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="171f8-114">Explica qué hacer cuando los paquetes incoherentes interrumpen una aplicación al realizar actualizaciones importantes o cambiar versiones de paquetes.</span><span class="sxs-lookup"><span data-stu-id="171f8-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="171f8-115">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="171f8-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="171f8-116">Muestra temas de solución de problemas adicionales.</span><span class="sxs-lookup"><span data-stu-id="171f8-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="171f8-117">Errores de inicio de aplicación</span><span class="sxs-lookup"><span data-stu-id="171f8-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="171f8-118">En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="171f8-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="171f8-119">Un error de *proceso de 502,5* o un *error de 500,30 inicio* que se produce cuando se puede diagnosticar localmente la depuración con los consejos de este tema.</span><span class="sxs-lookup"><span data-stu-id="171f8-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="171f8-120">En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="171f8-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="171f8-121">Un *error de proceso 502,5* que se produce cuando la depuración local se puede diagnosticar con los consejos de este tema.</span><span class="sxs-lookup"><span data-stu-id="171f8-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="171f8-122">403,14 prohibido</span><span class="sxs-lookup"><span data-stu-id="171f8-122">403.14 Forbidden</span></span>

<span data-ttu-id="171f8-123">No se puede iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-123">The app fails to start.</span></span> <span data-ttu-id="171f8-124">Se registra el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="171f8-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="171f8-125">Normalmente, el error se debe a una implementación interrumpida en el sistema de hospedaje, que incluye cualquiera de los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="171f8-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="171f8-126">La aplicación se implementa en la carpeta incorrecta en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="171f8-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="171f8-127">El proceso de implementación no pudo trasladar todos los archivos y carpetas de la aplicación a la carpeta de implementación en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="171f8-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="171f8-128">Falta el archivo *Web. config* de la implementación o el contenido del archivo *Web. config* no tiene el formato correcto.</span><span class="sxs-lookup"><span data-stu-id="171f8-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="171f8-129">Realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="171f8-129">Perform the following steps:</span></span>

1. <span data-ttu-id="171f8-130">Elimine todos los archivos y carpetas de la carpeta de implementación en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="171f8-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="171f8-131">Vuelva a implementar el contenido de la carpeta de *publicación* de la aplicación en el sistema de hospedaje con el método normal de implementación, como Visual Studio, PowerShell o la implementación manual:</span><span class="sxs-lookup"><span data-stu-id="171f8-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="171f8-132">Confirme que el archivo *Web. config* está presente en la implementación y que su contenido es correcto.</span><span class="sxs-lookup"><span data-stu-id="171f8-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="171f8-133">Cuando hospede en Azure App Service, confirme que la aplicación se implementa en la `D:\home\site\wwwroot` carpeta.</span><span class="sxs-lookup"><span data-stu-id="171f8-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="171f8-134">Cuando la aplicación esté hospedada por IIS, confirme que la aplicación se implementa en la **ruta de acceso física** de IIS que se muestra en la **configuración básica**del **Administrador de IIS**.</span><span class="sxs-lookup"><span data-stu-id="171f8-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="171f8-135">Confirme que todos los archivos y carpetas de la aplicación se han implementado comparando la implementación en el sistema host con el contenido de la carpeta de *publicación* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="171f8-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="171f8-136">Para obtener más información sobre el diseño de una aplicación ASP.NET Core publicada, <xref:host-and-deploy/directory-structure>vea.</span><span class="sxs-lookup"><span data-stu-id="171f8-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="171f8-137">Para obtener más información acerca del archivo *Web. config* , <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>vea.</span><span class="sxs-lookup"><span data-stu-id="171f8-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="171f8-138">500 Error interno del servidor</span><span class="sxs-lookup"><span data-stu-id="171f8-138">500 Internal Server Error</span></span>

<span data-ttu-id="171f8-139">La aplicación se inicia, pero un error impide que el servidor complete la solicitud.</span><span class="sxs-lookup"><span data-stu-id="171f8-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="171f8-140">Este error se produce dentro del código de la aplicación durante el inicio o mientras se crea una respuesta.</span><span class="sxs-lookup"><span data-stu-id="171f8-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="171f8-141">La respuesta no puede contener nada o puede aparecer como *500 Error interno del servidor* en el explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="171f8-142">El registro de eventos de la aplicación normalmente indica que la aplicación se ha iniciado normalmente.</span><span class="sxs-lookup"><span data-stu-id="171f8-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="171f8-143">Desde la perspectiva del servidor, eso es correcto.</span><span class="sxs-lookup"><span data-stu-id="171f8-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="171f8-144">La aplicación se inició, pero no puede generar una respuesta válida.</span><span class="sxs-lookup"><span data-stu-id="171f8-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="171f8-145">Ejecute la aplicación en un símbolo del sistema en el servidor o habilite el ASP.NET Core módulo stdout log para solucionar el problema.</span><span class="sxs-lookup"><span data-stu-id="171f8-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="171f8-146">500.0 Error de carga del controlador en proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="171f8-147">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-147">The worker process fails.</span></span> <span data-ttu-id="171f8-148">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-148">The app doesn't start.</span></span>

<span data-ttu-id="171f8-149">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) no puede encontrar el CLR de .net Core y encontrar el controlador de solicitud en proceso (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="171f8-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="171f8-150">Compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="171f8-150">Check that:</span></span>

* <span data-ttu-id="171f8-151">Que la aplicación tiene como destino el paquete de NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="171f8-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="171f8-152">Que la versión del marco compartido de ASP.NET Core que la aplicación tiene como destino esté instalada en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="171f8-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="171f8-153">500.0 Error de carga del controlador fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="171f8-154">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-154">The worker process fails.</span></span> <span data-ttu-id="171f8-155">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-155">The app doesn't start.</span></span>

<span data-ttu-id="171f8-156">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) no encuentra el controlador de solicitudes de hospedaje fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="171f8-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="171f8-157">Asegúrese de que el archivo *aspnetcorev2_outofprocess.dll* está presente en una subcarpeta junto a *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="171f8-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="171f8-158">500.0 Error de carga del controlador en proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="171f8-159">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-159">The worker process fails.</span></span> <span data-ttu-id="171f8-160">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-160">The app doesn't start.</span></span>

<span data-ttu-id="171f8-161">Error desconocido al cargar los componentes del [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="171f8-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="171f8-162">Realice una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="171f8-162">Take one of the following actions:</span></span>

* <span data-ttu-id="171f8-163">Póngase en contacto con el [equipo de soporte técnico de Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (seleccione **Herramientas de desarrollo** y, después, **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="171f8-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="171f8-164">Formule una pregunta en Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="171f8-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="171f8-165">Registre un problema en nuestro [repositorio de GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="171f8-165">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="171f8-166">500.30 Error de inicio en proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="171f8-167">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-167">The worker process fails.</span></span> <span data-ttu-id="171f8-168">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-168">The app doesn't start.</span></span>

<span data-ttu-id="171f8-169">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el CLR de .net Core en proceso, pero no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="171f8-170">Normalmente, la causa de un error de inicio del proceso se puede determinar a partir de las entradas del registro de eventos de aplicación y el registro de ASP.NET Core del módulo stdout.</span><span class="sxs-lookup"><span data-stu-id="171f8-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="171f8-171">Una condición de error habitual es que la aplicación esté mal configurada porque tiene como destino una versión del marco compartido de ASP.NET Core que no está presente.</span><span class="sxs-lookup"><span data-stu-id="171f8-171">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="171f8-172">Compruebe qué versiones del marco compartido de ASP.NET Core están instaladas en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="171f8-172">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="171f8-173">500.31 ANMC no pudo encontrar las dependencias nativas</span><span class="sxs-lookup"><span data-stu-id="171f8-173">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="171f8-174">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-174">The worker process fails.</span></span> <span data-ttu-id="171f8-175">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-175">The app doesn't start.</span></span>

<span data-ttu-id="171f8-176">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el tiempo de ejecución de .net Core en proceso, pero no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-176">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="171f8-177">Este error de inicio suele deberse a que el entorno de ejecución de `Microsoft.NETCore.App` o `Microsoft.AspNetCore.App` no está instalado.</span><span class="sxs-lookup"><span data-stu-id="171f8-177">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="171f8-178">Si la aplicación se implementa para ASP.NET Core 3.0 y esa versión no existe en el equipo, se produce este error.</span><span class="sxs-lookup"><span data-stu-id="171f8-178">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="171f8-179">Este es un mensaje de error de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="171f8-179">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="171f8-180">El mensaje de error enumera todas las versiones instaladas de .NET Core y la versión solicitada por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-180">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="171f8-181">Para corregir este error, hay dos posibilidades:</span><span class="sxs-lookup"><span data-stu-id="171f8-181">To fix this error, either:</span></span>

* <span data-ttu-id="171f8-182">Instalar la versión adecuada de .NET Core en la máquina.</span><span class="sxs-lookup"><span data-stu-id="171f8-182">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="171f8-183">Cambiar el destino de la aplicación a una versión de .NET Core que exista en la máquina.</span><span class="sxs-lookup"><span data-stu-id="171f8-183">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="171f8-184">Publique la aplicación como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="171f8-184">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="171f8-185">Cuando se ejecuta en desarrollo (la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece en `Development`), el error específico se escribe en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="171f8-185">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="171f8-186">La causa de un error de inicio del proceso también se encuentra en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-186">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="171f8-187">500.32 ANCM No se pudo cargar el archivo .dll</span><span class="sxs-lookup"><span data-stu-id="171f8-187">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="171f8-188">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-188">The worker process fails.</span></span> <span data-ttu-id="171f8-189">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-189">The app doesn't start.</span></span>

<span data-ttu-id="171f8-190">La causa más común de este error es que la aplicación se haya publicado para una arquitectura de procesador no compatible.</span><span class="sxs-lookup"><span data-stu-id="171f8-190">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="171f8-191">Si el proceso de trabajo se ejecuta como una aplicación de 32 bits y la aplicación se diseñó para 64 bits, se produce este error.</span><span class="sxs-lookup"><span data-stu-id="171f8-191">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="171f8-192">Para corregir este error, hay dos posibilidades:</span><span class="sxs-lookup"><span data-stu-id="171f8-192">To fix this error, either:</span></span>

* <span data-ttu-id="171f8-193">Volver a publicar la aplicación para la misma arquitectura de procesador que el proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="171f8-193">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="171f8-194">Publicar la aplicación como una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="171f8-194">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="171f8-195">500.33 Error de carga del controlador de solicitud</span><span class="sxs-lookup"><span data-stu-id="171f8-195">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="171f8-196">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-196">The worker process fails.</span></span> <span data-ttu-id="171f8-197">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-197">The app doesn't start.</span></span>

<span data-ttu-id="171f8-198">La aplicación no hizo referencia al marco `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="171f8-198">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="171f8-199">El [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module)solo `Microsoft.AspNetCore.App` puede hospedar las aplicaciones que tienen como destino el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="171f8-199">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="171f8-200">Para corregir este error, confirme que la aplicación tiene como destino el marco `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="171f8-200">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="171f8-201">Compruebe en `.runtimeconfig.json` cuál es el marco de destino de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-201">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="171f8-202">500.34 ANCM Modelos de hospedaje mixto no admitidos</span><span class="sxs-lookup"><span data-stu-id="171f8-202">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="171f8-203">El proceso de trabajo no puede ejecutar a la vez una aplicación en proceso y una aplicación fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="171f8-203">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="171f8-204">Para corregir este error, ejecute las aplicaciones en grupos de aplicaciones de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="171f8-204">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="171f8-205">500.35 ANCM Varias aplicaciones en proceso en el mismo proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-205">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="171f8-206">El proceso de trabajo no puede ejecutar a la vez una aplicación en proceso y una aplicación fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="171f8-206">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="171f8-207">Para corregir este error, ejecute las aplicaciones en grupos de aplicaciones de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="171f8-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="171f8-208">500.36 Error de carga del controlador fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-208">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="171f8-209">El controlador de solicitudes de fuera de proceso, *aspnetcorev2_outofprocess.dll*, no está junto al archivo *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="171f8-209">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="171f8-210">Esto indica una instalación dañada del [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="171f8-210">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="171f8-211">Para corregir este error, repare la instalación del [conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (para IIS) o Visual Studio (para IIS Express).</span><span class="sxs-lookup"><span data-stu-id="171f8-211">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="171f8-212">500.37 ANCM no se pudo iniciar en el límite de tiempo de inicio</span><span class="sxs-lookup"><span data-stu-id="171f8-212">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="171f8-213">ANCM no se pudo iniciar dentro del límite de tiempo de inicio proporcionado.</span><span class="sxs-lookup"><span data-stu-id="171f8-213">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="171f8-214">De forma predeterminada, el tiempo de espera es de 120 segundos.</span><span class="sxs-lookup"><span data-stu-id="171f8-214">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="171f8-215">Este error puede producirse cuando se inicia un gran número de aplicaciones en el mismo equipo.</span><span class="sxs-lookup"><span data-stu-id="171f8-215">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="171f8-216">Busque picos de uso de CPU o memoria en el servidor durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="171f8-216">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="171f8-217">Es posible que deba escalonar el proceso de inicio de varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="171f8-217">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="171f8-218">502.5 Error de proceso</span><span class="sxs-lookup"><span data-stu-id="171f8-218">502.5 Process Failure</span></span>

<span data-ttu-id="171f8-219">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="171f8-219">The worker process fails.</span></span> <span data-ttu-id="171f8-220">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="171f8-220">The app doesn't start.</span></span>

<span data-ttu-id="171f8-221">El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el proceso de trabajo, pero no lo consigue.</span><span class="sxs-lookup"><span data-stu-id="171f8-221">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="171f8-222">Normalmente, la causa de un error de inicio del proceso se puede determinar a partir de las entradas del registro de eventos de aplicación y el registro de ASP.NET Core del módulo stdout.</span><span class="sxs-lookup"><span data-stu-id="171f8-222">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="171f8-223">Una condición de error habitual es que la aplicación esté mal configurada porque tiene como destino una versión del marco compartido de ASP.NET Core que no está presente.</span><span class="sxs-lookup"><span data-stu-id="171f8-223">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="171f8-224">Compruebe qué versiones del marco compartido de ASP.NET Core están instaladas en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="171f8-224">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="171f8-225">El *marco de trabajo compartido* es el conjunto de ensamblados (archivos *.dll*) que se instalan en la máquina, y se hace referencia a ellos mediante un metapaquete como `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="171f8-225">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="171f8-226">La referencia del metapaquete puede especificar una versión mínima necesaria.</span><span class="sxs-lookup"><span data-stu-id="171f8-226">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="171f8-227">Para más información, consulte este artículo sobre el [marco de trabajo compartido](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="171f8-227">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="171f8-228">La página de error *502.5 Error de proceso* se devuelve cuando el proceso de trabajo no se puede iniciar debido a un error de configuración de la aplicación o del hospedaje:</span><span class="sxs-lookup"><span data-stu-id="171f8-228">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="171f8-229">No se pudo iniciar la aplicación (código de error "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="171f8-229">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="171f8-230">Error al iniciar la aplicación porque el ensamblado de la aplicación ( *.dll*) no se ha podido cargar.</span><span class="sxs-lookup"><span data-stu-id="171f8-230">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="171f8-231">Este error se produce cuando hay un error de coincidencia del valor de bits entre la aplicación publicada y el proceso w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="171f8-231">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="171f8-232">Confirme que la opción de 32 bits del grupo de aplicaciones sea correcta:</span><span class="sxs-lookup"><span data-stu-id="171f8-232">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="171f8-233">Seleccione el grupo de aplicaciones en **Grupos de aplicaciones** del Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="171f8-233">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="171f8-234">Seleccione **Configuración avanzada** en **Modificar grupo de aplicaciones**, en el panel **Acciones**.</span><span class="sxs-lookup"><span data-stu-id="171f8-234">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="171f8-235">Establezca **Habilitar aplicaciones de 32 bits**:</span><span class="sxs-lookup"><span data-stu-id="171f8-235">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="171f8-236">Si implementa una aplicación de 32 bits (x86), establezca el valor en `True`.</span><span class="sxs-lookup"><span data-stu-id="171f8-236">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="171f8-237">Si implementa una aplicación de 64 bits (x64), establezca el valor en `False`.</span><span class="sxs-lookup"><span data-stu-id="171f8-237">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="171f8-238">Confirme que no hay ningún conflicto entre una `<Platform>` propiedad de MSBuild en el archivo de proyecto y el de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-238">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="171f8-239">Restablecimiento de la conexión</span><span class="sxs-lookup"><span data-stu-id="171f8-239">Connection reset</span></span>

<span data-ttu-id="171f8-240">Si se produce un error después de que se envían los encabezados, el servidor no tiene tiempo para enviar un mensaje **500 Error interno del servidor** cuando se produce un error.</span><span class="sxs-lookup"><span data-stu-id="171f8-240">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="171f8-241">Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos en una respuesta.</span><span class="sxs-lookup"><span data-stu-id="171f8-241">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="171f8-242">Este tipo de error aparece como un error de *restablecimiento de la conexión* en el cliente.</span><span class="sxs-lookup"><span data-stu-id="171f8-242">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="171f8-243">El [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="171f8-243">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="171f8-244">Límites de inicio predeterminados</span><span class="sxs-lookup"><span data-stu-id="171f8-244">Default startup limits</span></span>

<span data-ttu-id="171f8-245">El [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module) se configura con un valor predeterminado de *startupTimeLimit* de 120 segundos.</span><span class="sxs-lookup"><span data-stu-id="171f8-245">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="171f8-246">Cuando se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registre un error de proceso.</span><span class="sxs-lookup"><span data-stu-id="171f8-246">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="171f8-247">Para información sobre la configuración del módulo, consulte [Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="171f8-247">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="171f8-248">Solución de problemas en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="171f8-248">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="171f8-249">Registro de eventos de aplicación (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="171f8-249">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="171f8-250">Para acceder al registro de eventos de la aplicación, use la hoja **Diagnose and solve problems** (Diagnosticar y solucionar problemas) de Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="171f8-250">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="171f8-251">En Azure Portal, abra la aplicación en **App Services**.</span><span class="sxs-lookup"><span data-stu-id="171f8-251">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="171f8-252">Seleccione **Diagnosticar y solucionar problemas**.</span><span class="sxs-lookup"><span data-stu-id="171f8-252">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="171f8-253">Seleccione el título **Herramientas de diagnóstico**.</span><span class="sxs-lookup"><span data-stu-id="171f8-253">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="171f8-254">En **Herramientas de soporte técnico**, seleccione el botón **Eventos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="171f8-254">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="171f8-255">Examine el error más reciente que hayan proporcionado las entradas *IIS AspNetCoreModule* o *IIS AspNetCoreModule V2* en la columna **Origen**.</span><span class="sxs-lookup"><span data-stu-id="171f8-255">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="171f8-256">Una alternativa al uso de la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) es examinar el archivo de registro de eventos de la aplicación directamente mediante [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="171f8-256">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="171f8-257">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="171f8-257">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="171f8-258">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="171f8-258">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="171f8-259">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-259">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="171f8-260">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="171f8-260">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="171f8-261">Abra la carpeta **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="171f8-261">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="171f8-262">Seleccione el icono de lápiz junto al archivo *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="171f8-262">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="171f8-263">Examine el registro.</span><span class="sxs-lookup"><span data-stu-id="171f8-263">Examine the log.</span></span> <span data-ttu-id="171f8-264">Desplácese al final del registro para ver los eventos más recientes.</span><span class="sxs-lookup"><span data-stu-id="171f8-264">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="171f8-265">Ejecución de la aplicación en la consola de Kudu</span><span class="sxs-lookup"><span data-stu-id="171f8-265">Run the app in the Kudu console</span></span>

<span data-ttu-id="171f8-266">Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-266">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="171f8-267">Puede ejecutar la aplicación en la consola de ejecución remota de [Kudu](https://github.com/projectkudu/kudu/wiki) para detectar el error:</span><span class="sxs-lookup"><span data-stu-id="171f8-267">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="171f8-268">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="171f8-268">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="171f8-269">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="171f8-269">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="171f8-270">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-270">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="171f8-271">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="171f8-271">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="171f8-272">Prueba de una aplicación de 32 bits (x86)</span><span class="sxs-lookup"><span data-stu-id="171f8-272">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="171f8-273">**Versión actual**</span><span class="sxs-lookup"><span data-stu-id="171f8-273">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="171f8-274">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="171f8-274">Run the app:</span></span>
   * <span data-ttu-id="171f8-275">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="171f8-275">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="171f8-276">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="171f8-276">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="171f8-277">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="171f8-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="171f8-278">**Implementación dependiente del marco de trabajo que se ejecuta en una versión preliminar**</span><span class="sxs-lookup"><span data-stu-id="171f8-278">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="171f8-279">*Requiere la instalación de la extensión de sitio ASP.NET Core {VERSION} (x86) Runtime.*</span><span class="sxs-lookup"><span data-stu-id="171f8-279">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="171f8-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` es la versión del runtime)</span><span class="sxs-lookup"><span data-stu-id="171f8-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="171f8-281">Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="171f8-281">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="171f8-282">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="171f8-282">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="171f8-283">Prueba de una aplicación de 64 bits (x64)</span><span class="sxs-lookup"><span data-stu-id="171f8-283">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="171f8-284">**Versión actual**</span><span class="sxs-lookup"><span data-stu-id="171f8-284">**Current release**</span></span>

* <span data-ttu-id="171f8-285">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) de 64 bits (x64):</span><span class="sxs-lookup"><span data-stu-id="171f8-285">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="171f8-286">Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="171f8-286">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="171f8-287">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="171f8-287">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="171f8-288">Ejecute la aplicación: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="171f8-288">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="171f8-289">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="171f8-289">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="171f8-290">**Implementación dependiente del marco de trabajo que se ejecuta en una versión preliminar**</span><span class="sxs-lookup"><span data-stu-id="171f8-290">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="171f8-291">*Requiere la instalación de la extensión de sitio ASP.NET Core {VERSION} (x64) Runtime.*</span><span class="sxs-lookup"><span data-stu-id="171f8-291">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="171f8-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` es la versión del runtime)</span><span class="sxs-lookup"><span data-stu-id="171f8-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="171f8-293">Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="171f8-293">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="171f8-294">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="171f8-294">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="171f8-295">Registro de ASP.NET Core del módulo stdout (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="171f8-295">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="171f8-296">El registro stdout del módulo ASP.NET Core con frecuencia registra mensajes de error útiles que no se encuentran en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-296">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="171f8-297">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="171f8-297">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="171f8-298">Vaya a la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="171f8-298">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="171f8-299">En **Seleccione una categoría de problema**, seleccione el botón abajo **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="171f8-299">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="171f8-300">En **Suggested Solutions** >  (Soluciones sugeridas) **Enable Stdout Log Redirection** (Habilitar el redireccionamiento de registros stdout), seleccione el botón para **abrir la consola de Kudu y editar web.config**.</span><span class="sxs-lookup"><span data-stu-id="171f8-300">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="171f8-301">En la **consola de diagnóstico** de Kudu, abra las carpetas para la ruta de acceso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="171f8-301">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="171f8-302">Desplácese hacia abajo para mostrar el archivo *web.config* en la parte inferior de la lista.</span><span class="sxs-lookup"><span data-stu-id="171f8-302">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="171f8-303">Haga clic en el icono de lápiz junto al archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="171f8-303">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="171f8-304">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** a: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="171f8-304">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="171f8-305">Seleccione **Save** (Guardar) para guardar el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="171f8-305">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="171f8-306">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-306">Make a request to the app.</span></span>
1. <span data-ttu-id="171f8-307">Vuelva a Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="171f8-307">Return to the Azure portal.</span></span> <span data-ttu-id="171f8-308">Seleccione la hoja **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="171f8-308">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="171f8-309">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="171f8-309">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="171f8-310">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-310">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="171f8-311">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="171f8-311">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="171f8-312">Seleccione la carpeta **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="171f8-312">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="171f8-313">Inspeccione la columna **Modificado** y seleccione el icono de lápiz para editar el registro de stdout con la última fecha de modificación.</span><span class="sxs-lookup"><span data-stu-id="171f8-313">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="171f8-314">Cuando se abre el archivo de registro, se muestra el error.</span><span class="sxs-lookup"><span data-stu-id="171f8-314">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="171f8-315">Deshabilite el registro de stdout una vez haya solucionado los problemas:</span><span class="sxs-lookup"><span data-stu-id="171f8-315">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="171f8-316">En la **consola de diagnóstico** de Kudu, vuelva a la ruta de acceso **site** > **wwwroot** para mostrar el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="171f8-316">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="171f8-317">Seleccione el icono de lápiz para abrir de nuevo el archivo **web.config**.</span><span class="sxs-lookup"><span data-stu-id="171f8-317">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="171f8-318">Establezca **stdoutLogEnabled** en `false`.</span><span class="sxs-lookup"><span data-stu-id="171f8-318">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="171f8-319">Seleccione **Save** (Guardar) para guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="171f8-319">Select **Save** to save the file.</span></span>

<span data-ttu-id="171f8-320">Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="171f8-320">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="171f8-321">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="171f8-321">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="171f8-322">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="171f8-322">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="171f8-323">Use únicamente el registro de stdout para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-323">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="171f8-324">Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="171f8-324">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="171f8-325">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="171f8-325">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="171f8-326">Registro de depuración del módulo de ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="171f8-326">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="171f8-327">El registro de depuración del módulo de ASP.NET Core ofrece un registro adicional, más profundo, del módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="171f8-327">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="171f8-328">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="171f8-328">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="171f8-329">Para habilitar el registro de diagnóstico mejorado, realice cualquiera de las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="171f8-329">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="171f8-330">Siga las instrucciones de [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) para configurar la aplicación para un registro de diagnóstico mejorado.</span><span class="sxs-lookup"><span data-stu-id="171f8-330">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="171f8-331">Vuelva a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-331">Redeploy the app.</span></span>
   * <span data-ttu-id="171f8-332">Agregue la `<handlerSettings>` que se muestra en [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) al archivo *web.config* de la aplicación activa mediante la consola de Kudu:</span><span class="sxs-lookup"><span data-stu-id="171f8-332">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="171f8-333">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="171f8-333">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="171f8-334">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="171f8-334">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="171f8-335">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-335">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="171f8-336">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="171f8-336">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="171f8-337">Abra las carpetas para la ruta de acceso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="171f8-337">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="171f8-338">Seleccione el icono de lápiz para editar el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="171f8-338">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="171f8-339">Agregue la sección `<handlerSettings>` como se muestra en [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="171f8-339">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="171f8-340">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="171f8-340">Select the **Save** button.</span></span>
1. <span data-ttu-id="171f8-341">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="171f8-341">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="171f8-342">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="171f8-342">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="171f8-343">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-343">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="171f8-344">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="171f8-344">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="171f8-345">Abra las carpetas para la ruta de acceso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="171f8-345">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="171f8-346">Si no ha proporcionado una ruta de acceso para el archivo *aspnetcore debug.log*, el archivo aparece en la lista.</span><span class="sxs-lookup"><span data-stu-id="171f8-346">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="171f8-347">Si ha proporcionado una ruta de acceso, vaya a la ubicación del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="171f8-347">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="171f8-348">Abra el archivo de registro con el botón de lápiz situado junto al nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="171f8-348">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="171f8-349">Deshabilite el registro de depuración una vez haya solucionado los problemas:</span><span class="sxs-lookup"><span data-stu-id="171f8-349">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="171f8-350">Para deshabilitar el registro de depuración mejorado, realice cualquiera de las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="171f8-350">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="171f8-351">Quite localmente la `<handlerSettings>` del archivo *web.config* y vuelva a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-351">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="171f8-352">Use la consola de Kudu para editar el archivo *web.config* y quite la sección `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="171f8-352">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="171f8-353">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="171f8-353">Save the file.</span></span>

<span data-ttu-id="171f8-354">Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="171f8-354">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="171f8-355">Si no deshabilita el registro de depuración, es posible que se produzcan errores en la aplicación o el servidor.</span><span class="sxs-lookup"><span data-stu-id="171f8-355">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="171f8-356">No hay ningún límite para el tamaño del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="171f8-356">There's no limit on log file size.</span></span> <span data-ttu-id="171f8-357">Use únicamente el registro de depuración para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-357">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="171f8-358">Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="171f8-358">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="171f8-359">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="171f8-359">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="171f8-360">Aplicación lenta o bloqueada (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="171f8-360">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="171f8-361">Cuando una aplicación responde con lentitud o se bloquea en una solicitud, consulte los artículos siguientes:</span><span class="sxs-lookup"><span data-stu-id="171f8-361">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="171f8-362">Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="171f8-362">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="171f8-363">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) (Uso de la extensión de sitio de diagnóstico de bloqueo para capturar el volcado de memoria para problemas de excepciones intermitentes o problemas de rendimiento en Azure Web Apps)</span><span class="sxs-lookup"><span data-stu-id="171f8-363">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="171f8-364">Hojas de supervisión</span><span class="sxs-lookup"><span data-stu-id="171f8-364">Monitoring blades</span></span>

<span data-ttu-id="171f8-365">Las hojas de supervisión proporcionan una alternativa a la experiencia de solución de problemas de los métodos descritos anteriormente en el tema.</span><span class="sxs-lookup"><span data-stu-id="171f8-365">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="171f8-366">Estas hojas se pueden usar para diagnosticar errores de la serie 500.</span><span class="sxs-lookup"><span data-stu-id="171f8-366">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="171f8-367">Confirme que están instaladas las extensiones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="171f8-367">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="171f8-368">Si no lo están, instálelas manualmente:</span><span class="sxs-lookup"><span data-stu-id="171f8-368">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="171f8-369">En la sección de la hoja **HERRAMIENTAS DE DESARROLLO**, seleccione la hoja **Extensiones**.</span><span class="sxs-lookup"><span data-stu-id="171f8-369">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="171f8-370">Aparecerán en la lista las **extensiones de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="171f8-370">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="171f8-371">Si las extensiones no están instaladas, seleccione el botón **Add** (Agregar).</span><span class="sxs-lookup"><span data-stu-id="171f8-371">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="171f8-372">Elija las **extensiones de ASP.NET Core** de la lista.</span><span class="sxs-lookup"><span data-stu-id="171f8-372">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="171f8-373">Seleccione **Aceptar** para aceptar los términos legales.</span><span class="sxs-lookup"><span data-stu-id="171f8-373">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="171f8-374">Seleccione **Aceptar** en la hoja **Agregar extensión**.</span><span class="sxs-lookup"><span data-stu-id="171f8-374">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="171f8-375">Un mensaje emergente informativo indica si las extensiones se han instalado correctamente.</span><span class="sxs-lookup"><span data-stu-id="171f8-375">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="171f8-376">Si el registro de stdout no está habilitado, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="171f8-376">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="171f8-377">En Azure Portal, seleccione la hoja **Herramientas avanzadas** en el área **HERRAMIENTAS DE DESARROLLO**.</span><span class="sxs-lookup"><span data-stu-id="171f8-377">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="171f8-378">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="171f8-378">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="171f8-379">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="171f8-379">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="171f8-380">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="171f8-380">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="171f8-381">Abra las carpetas a la ruta de acceso **sitio** > **wwwroot** y desplácese hacia abajo para mostrar el archivo *web.config* en la parte inferior de la lista.</span><span class="sxs-lookup"><span data-stu-id="171f8-381">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="171f8-382">Haga clic en el icono de lápiz junto al archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="171f8-382">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="171f8-383">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** a: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="171f8-383">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="171f8-384">Seleccione **Save** (Guardar) para guardar el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="171f8-384">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="171f8-385">Continúe para activar el registro de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="171f8-385">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="171f8-386">En Azure Portal, seleccione la hoja **Registros de diagnóstico**.</span><span class="sxs-lookup"><span data-stu-id="171f8-386">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="171f8-387">Seleccione el conmutador **Activado** en **Registro de la aplicación (sistema de archivos)** y **Mensajes de error detallados**.</span><span class="sxs-lookup"><span data-stu-id="171f8-387">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="171f8-388">Seleccione el botón **Guardar** en la parte superior de la hoja.</span><span class="sxs-lookup"><span data-stu-id="171f8-388">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="171f8-389">Para incluir el seguimiento de solicitudes con error, también conocido como almacenamiento en búfer de eventos de solicitudes con error (FREB), seleccione el conmutador **Activado** en **Seguimiento de solicitudes con error**.</span><span class="sxs-lookup"><span data-stu-id="171f8-389">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="171f8-390">Seleccione la hoja **Secuencia de registro**, que aparece inmediatamente bajo la hoja **Registros de diagnóstico** en el portal.</span><span class="sxs-lookup"><span data-stu-id="171f8-390">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="171f8-391">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-391">Make a request to the app.</span></span>
1. <span data-ttu-id="171f8-392">Dentro de los datos de la secuencia de registro, se indica la causa del error.</span><span class="sxs-lookup"><span data-stu-id="171f8-392">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="171f8-393">No olvide deshabilitar el registro de stdout cuando finalice la solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="171f8-393">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="171f8-394">Para ver los registros de seguimiento de solicitudes con error (registros FREB):</span><span class="sxs-lookup"><span data-stu-id="171f8-394">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="171f8-395">Vaya a la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="171f8-395">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="171f8-396">Seleccione **Failed Request Tracing Logs** (Registros de seguimiento de solicitudes con error) en el área **SUPPORT TOOLS** (HERRAMIENTAS DE SOPORTE TÉCNICO) de la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="171f8-396">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="171f8-397">Para más información, consulte la [sección sobre los seguimientos de solicitudes con error del tema Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) y el artículo [Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure: ¿Cómo se activa el seguimiento de solicitudes con error?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).</span><span class="sxs-lookup"><span data-stu-id="171f8-397">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="171f8-398">Para más información, consulte [Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="171f8-398">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="171f8-399">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="171f8-399">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="171f8-400">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="171f8-400">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="171f8-401">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="171f8-401">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="171f8-402">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="171f8-402">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="171f8-403">Solucionar problemas en IIS</span><span class="sxs-lookup"><span data-stu-id="171f8-403">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="171f8-404">Registro de eventos de aplicación (IIS)</span><span class="sxs-lookup"><span data-stu-id="171f8-404">Application Event Log (IIS)</span></span>

<span data-ttu-id="171f8-405">Acceda al registro de eventos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="171f8-405">Access the Application Event Log:</span></span>

1. <span data-ttu-id="171f8-406">Abra el menú Inicio, busque **Visor de eventos** y luego seleccione la aplicación **Visor de eventos**.</span><span class="sxs-lookup"><span data-stu-id="171f8-406">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="171f8-407">En **Visor de eventos**, abra el nodo **Registros de Windows**.</span><span class="sxs-lookup"><span data-stu-id="171f8-407">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="171f8-408">Seleccione **Aplicación** para abrir el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-408">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="171f8-409">Busque los errores asociados a la aplicación objeto del error.</span><span class="sxs-lookup"><span data-stu-id="171f8-409">Search for errors associated with the failing app.</span></span> <span data-ttu-id="171f8-410">Los errores tienen un valor de *Módulo AspNetCore de IIS* o *Módulo AspNetCore de IIS Express* en la columna *Origen*.</span><span class="sxs-lookup"><span data-stu-id="171f8-410">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="171f8-411">Ejecución de la aplicación en un símbolo del sistema</span><span class="sxs-lookup"><span data-stu-id="171f8-411">Run the app at a command prompt</span></span>

<span data-ttu-id="171f8-412">Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-412">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="171f8-413">La causa de algunos errores se puede encontrar mediante la ejecución de la aplicación en un símbolo del sistema en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="171f8-413">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="171f8-414">Implementación dependiente de marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="171f8-414">Framework-dependent deployment</span></span>

<span data-ttu-id="171f8-415">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="171f8-415">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="171f8-416">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute la aplicación mediante la ejecución del ensamblado de la aplicación con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="171f8-416">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="171f8-417">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="171f8-417">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="171f8-418">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="171f8-418">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="171f8-419">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="171f8-419">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="171f8-420">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="171f8-420">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="171f8-421">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-421">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="171f8-422">Implementación autocontenida</span><span class="sxs-lookup"><span data-stu-id="171f8-422">Self-contained deployment</span></span>

<span data-ttu-id="171f8-423">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="171f8-423">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="171f8-424">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute el archivo ejecutable de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-424">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="171f8-425">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="171f8-425">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="171f8-426">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="171f8-426">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="171f8-427">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="171f8-427">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="171f8-428">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="171f8-428">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="171f8-429">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-429">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="171f8-430">Registro de ASP.NET Core del módulo stdout (IIS)</span><span class="sxs-lookup"><span data-stu-id="171f8-430">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="171f8-431">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="171f8-431">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="171f8-432">Vaya a la carpeta de implementación del sitio en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="171f8-432">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="171f8-433">Si la carpeta *Logs* no existe, cree la carpeta.</span><span class="sxs-lookup"><span data-stu-id="171f8-433">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="171f8-434">Para obtener instrucciones sobre cómo habilitar MSBuild para crear la carpeta *logs* automáticamente en la implementación, consulte el tema [Estructura de directorios](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="171f8-434">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="171f8-435">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="171f8-435">Edit the *web.config* file.</span></span> <span data-ttu-id="171f8-436">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** para que apunte a la carpeta *logs* (por ejemplo, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="171f8-436">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="171f8-437">`stdout` en la ruta de acceso es el prefijo del nombre del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="171f8-437">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="171f8-438">Cuando se crea el registro, se agregan automáticamente una marca de tiempo, un identificador de proceso y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="171f8-438">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="171f8-439">Cuando se usa `stdout` como prefijo para el nombre de archivo, un archivo de registro se llama normalmente *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="171f8-439">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="171f8-440">Asegúrese de que la identidad del grupo de aplicaciones tiene permisos de escritura en la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="171f8-440">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="171f8-441">Guarde el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="171f8-441">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="171f8-442">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-442">Make a request to the app.</span></span>
1. <span data-ttu-id="171f8-443">Vaya a la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="171f8-443">Navigate to the *logs* folder.</span></span> <span data-ttu-id="171f8-444">Busque y abra el registro más reciente de stdout.</span><span class="sxs-lookup"><span data-stu-id="171f8-444">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="171f8-445">Estudie el registro para ver los errores.</span><span class="sxs-lookup"><span data-stu-id="171f8-445">Study the log for errors.</span></span>

<span data-ttu-id="171f8-446">Deshabilite el registro de stdout una vez haya solucionado los problemas:</span><span class="sxs-lookup"><span data-stu-id="171f8-446">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="171f8-447">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="171f8-447">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="171f8-448">Establezca **stdoutLogEnabled** en `false`.</span><span class="sxs-lookup"><span data-stu-id="171f8-448">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="171f8-449">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="171f8-449">Save the file.</span></span>

<span data-ttu-id="171f8-450">Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="171f8-450">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="171f8-451">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="171f8-451">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="171f8-452">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="171f8-452">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="171f8-453">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="171f8-453">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="171f8-454">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="171f8-454">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="171f8-455">Registro de depuración del módulo de ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="171f8-455">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="171f8-456">Agregue la siguiente configuración de controlador al archivo *Web. config* de la aplicación para habilitar el registro de depuración del módulo de ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="171f8-456">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="171f8-457">Confirme que la ruta de acceso especificada para el registro exista y que la identidad del grupo de aplicaciones tenga permisos de escritura en la ubicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-457">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="171f8-458">Para obtener más información, consulta <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="171f8-458">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="171f8-459">Habilitación de la página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="171f8-459">Enable the Developer Exception Page</span></span>

<span data-ttu-id="171f8-460">La variable de entorno `ASPNETCORE_ENVIRONMENT` [ se puede agregar a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="171f8-460">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="171f8-461">Siempre y cuando el entorno no se invalide al inicio de la aplicación con `UseEnvironment` en el generador de host, la configuración de la variable de entorno permite que aparezca la [página de excepciones del desarrollador](xref:fundamentals/error-handling) cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-461">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="171f8-462">Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` cuando se use en servidores de ensayo o pruebas que no estén expuestos a Internet.</span><span class="sxs-lookup"><span data-stu-id="171f8-462">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="171f8-463">Quite la variable de entorno del archivo *web.config* cuando termine de solucionar los problemas.</span><span class="sxs-lookup"><span data-stu-id="171f8-463">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="171f8-464">Para información sobre la configuración de variables de entorno en *web.config*, consulte el [elemento secundario environmentVariables de aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="171f8-464">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="171f8-465">Obtención de datos de una aplicación</span><span class="sxs-lookup"><span data-stu-id="171f8-465">Obtain data from an app</span></span>

<span data-ttu-id="171f8-466">Si una aplicación es capaz de responder a solicitudes, obtenga solicitudes, conexiones y datos adicionales de la aplicación mediante el middleware en línea del terminal.</span><span class="sxs-lookup"><span data-stu-id="171f8-466">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="171f8-467">Para obtener más información y un código de ejemplo, vea <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="171f8-467">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="171f8-468">Aplicación lenta o bloqueada (IIS)</span><span class="sxs-lookup"><span data-stu-id="171f8-468">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="171f8-469">Un *volcado* de memoria es una instantánea de la memoria del sistema y puede ayudar a determinar la causa de un bloqueo de la aplicación, un error de inicio o una aplicación lenta.</span><span class="sxs-lookup"><span data-stu-id="171f8-469">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="171f8-470">Bloqueo o excepción de la aplicación</span><span class="sxs-lookup"><span data-stu-id="171f8-470">App crashes or encounters an exception</span></span>

<span data-ttu-id="171f8-471">Obtenga y analice un volcado de memoria en [Informe de errores de Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="171f8-471">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="171f8-472">Cree una carpeta para almacenar los archivos de volcado de memoria en `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="171f8-472">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="171f8-473">El grupo de aplicaciones debe tener acceso de escritura a la carpeta.</span><span class="sxs-lookup"><span data-stu-id="171f8-473">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="171f8-474">Ejecute el [script EnableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="171f8-474">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="171f8-475">Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="171f8-475">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="171f8-476">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="171f8-476">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="171f8-477">Ejecute la aplicación en las condiciones que hacen que se produzca el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="171f8-477">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="171f8-478">Una vez que se haya producido el bloqueo, ejecute el [script DisableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="171f8-478">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="171f8-479">Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="171f8-479">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="171f8-480">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="171f8-480">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="171f8-481">Después de que se bloquee la aplicación y se complete la recopilación del volcado de memoria, la aplicación puede finalizar con normalidad.</span><span class="sxs-lookup"><span data-stu-id="171f8-481">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="171f8-482">El script de PowerShell configura WER de modo que recopile un máximo de cinco volcados de memoria por aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-482">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="171f8-483">Los volcados de memoria pueden ocupar una gran cantidad de espacio en disco (hasta varios gigabytes cada uno).</span><span class="sxs-lookup"><span data-stu-id="171f8-483">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="171f8-484">La aplicación deja de responder, produce un error durante el inicio o se ejecuta con normalidad</span><span class="sxs-lookup"><span data-stu-id="171f8-484">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="171f8-485">Si una aplicación *deja de responder* (se detiene, pero no se bloquea), produce un error durante el inicio o se ejecuta con normalidad, vea [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Archivos de volcado de memoria en modo de usuario: selección de la mejor herramienta) para seleccionar una herramienta apropiada para generar el volcado de memoria.</span><span class="sxs-lookup"><span data-stu-id="171f8-485">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="171f8-486">Análisis del volcado de memoria</span><span class="sxs-lookup"><span data-stu-id="171f8-486">Analyze the dump</span></span>

<span data-ttu-id="171f8-487">El volcado de memoria se puede analizar de varias maneras.</span><span class="sxs-lookup"><span data-stu-id="171f8-487">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="171f8-488">Para obtener más información, vea [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Análisis de un archivo de volcado de memoria en modo de usuario).</span><span class="sxs-lookup"><span data-stu-id="171f8-488">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="171f8-489">Borrar cachés de paquetes</span><span class="sxs-lookup"><span data-stu-id="171f8-489">Clear package caches</span></span>

<span data-ttu-id="171f8-490">A veces se produce un error en una aplicación que funciona inmediatamente después de actualizar el SDK de .NET Core en el equipo de desarrollo o cambiar las versiones de paquete dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-490">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="171f8-491">En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes.</span><span class="sxs-lookup"><span data-stu-id="171f8-491">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="171f8-492">La mayoría de estos problemas puede corregirse siguiendo estas instrucciones:</span><span class="sxs-lookup"><span data-stu-id="171f8-492">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="171f8-493">Elimine las carpetas *bin* y *obj*.</span><span class="sxs-lookup"><span data-stu-id="171f8-493">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="171f8-494">Borre las memorias caché `dotnet nuget locals all --clear` de paquetes ejecutando desde un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="171f8-494">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="171f8-495">La eliminación de las memorias caché de paquetes también se puede realizar con la herramienta [Nuget. exe](https://www.nuget.org/downloads) y `nuget locals all -clear`ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="171f8-495">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="171f8-496">*nuget.exe* no es una instalación agrupada con el sistema operativo de escritorio de Windows y se debe obtener de forma independiente en el [sitio web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="171f8-496">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="171f8-497">Restaure el proyecto y vuelva a compilarlo.</span><span class="sxs-lookup"><span data-stu-id="171f8-497">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="171f8-498">Elimine todos los archivos de la carpeta de implementación en el servidor antes de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="171f8-498">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="171f8-499">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="171f8-499">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="171f8-500">Documentación de Azure</span><span class="sxs-lookup"><span data-stu-id="171f8-500">Azure documentation</span></span>

* [<span data-ttu-id="171f8-501">Application Insights para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="171f8-501">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="171f8-502">Sección de aplicaciones Web de depuración remota de solución de problemas de una aplicación web en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="171f8-502">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="171f8-503">Introducción a diagnósticos de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="171f8-503">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="171f8-504">Cómo: Supervisar aplicaciones en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="171f8-504">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="171f8-505">Solución de problemas de una aplicación web en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="171f8-505">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="171f8-506">Solucionar los errores HTTP de "502 Puerta de enlace no válida" y "503 Servicio no disponible" en las aplicaciones web de Azure</span><span class="sxs-lookup"><span data-stu-id="171f8-506">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="171f8-507">Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="171f8-507">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="171f8-508">Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure</span><span class="sxs-lookup"><span data-stu-id="171f8-508">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="171f8-509">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Espacio aislado de Azure Web App [limitaciones de ejecución del entono de tiempo de ejecución de App Service])</span><span class="sxs-lookup"><span data-stu-id="171f8-509">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="171f8-510">Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)</span><span class="sxs-lookup"><span data-stu-id="171f8-510">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="171f8-511">Documentación de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="171f8-511">Visual Studio documentation</span></span>

* [<span data-ttu-id="171f8-512">Depuración remota ASP.NET Core en IIS en Azure en Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="171f8-512">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="171f8-513">ASP.NET Core de depuración remota en un equipo remoto de IIS en Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="171f8-513">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="171f8-514">Información sobre cómo depurar con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="171f8-514">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="171f8-515">Visual Studio Code documentación</span><span class="sxs-lookup"><span data-stu-id="171f8-515">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="171f8-516">Depuración con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="171f8-516">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
