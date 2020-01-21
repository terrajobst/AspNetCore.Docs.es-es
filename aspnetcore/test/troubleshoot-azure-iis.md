---
title: Solucionar problemas de ASP.NET Core en Azure App Service e IIS
author: guardrex
description: Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service y Internet Information Services (IIS) de aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 071dba9e936351e201b7582b3d0667cd6fac54bb
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294614"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="f437e-103">Solucionar problemas de ASP.NET Core en Azure App Service e IIS</span><span class="sxs-lookup"><span data-stu-id="f437e-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="f437e-104">Por [Luke Latham](https://github.com/guardrex) y [Diego Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="f437e-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="f437e-105">En este artículo se proporciona información sobre los errores comunes de inicio de la aplicación e instrucciones sobre cómo diagnosticar errores cuando una aplicación se implementa en Azure App Service o IIS:</span><span class="sxs-lookup"><span data-stu-id="f437e-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="f437e-106">Errores de inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f437e-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="f437e-107">Explica escenarios comunes de código de Estado HTTP de inicio.</span><span class="sxs-lookup"><span data-stu-id="f437e-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="f437e-108">Solución de problemas en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f437e-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="f437e-109">Proporciona consejos para la solución de problemas de las aplicaciones implementadas en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f437e-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="f437e-110">Solución de problemas de IIS</span><span class="sxs-lookup"><span data-stu-id="f437e-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="f437e-111">Proporciona consejos para la solución de problemas de aplicaciones implementadas en IIS o que se ejecutan en IIS Express localmente.</span><span class="sxs-lookup"><span data-stu-id="f437e-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="f437e-112">La guía se aplica a las implementaciones de Windows Server y escritorio de Windows.</span><span class="sxs-lookup"><span data-stu-id="f437e-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="f437e-113">Borrar cachés de paquetes</span><span class="sxs-lookup"><span data-stu-id="f437e-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="f437e-114">Explica qué hacer cuando los paquetes incoherentes interrumpen una aplicación al realizar actualizaciones importantes o cambiar versiones de paquetes.</span><span class="sxs-lookup"><span data-stu-id="f437e-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="f437e-115">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f437e-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="f437e-116">Muestra temas de solución de problemas adicionales.</span><span class="sxs-lookup"><span data-stu-id="f437e-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="f437e-117">Errores de inicio de aplicación</span><span class="sxs-lookup"><span data-stu-id="f437e-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f437e-118">En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="f437e-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="f437e-119">Un error de *proceso de 502,5* o un *error de 500,30 inicio* que se produce cuando se puede diagnosticar localmente la depuración con los consejos de este tema.</span><span class="sxs-lookup"><span data-stu-id="f437e-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f437e-120">En Visual Studio, un proyecto de ASP.NET Core toma como predeterminado el hospedaje de [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante la depuración.</span><span class="sxs-lookup"><span data-stu-id="f437e-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="f437e-121">Un *error de proceso 502,5* que se produce cuando la depuración local se puede diagnosticar con los consejos de este tema.</span><span class="sxs-lookup"><span data-stu-id="f437e-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="f437e-122">403,14 prohibido</span><span class="sxs-lookup"><span data-stu-id="f437e-122">403.14 Forbidden</span></span>

<span data-ttu-id="f437e-123">No se puede iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-123">The app fails to start.</span></span> <span data-ttu-id="f437e-124">Se registra el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="f437e-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="f437e-125">Normalmente, el error se debe a una implementación interrumpida en el sistema de hospedaje, que incluye cualquiera de los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="f437e-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="f437e-126">La aplicación se implementa en la carpeta incorrecta en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="f437e-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="f437e-127">El proceso de implementación no pudo trasladar todos los archivos y carpetas de la aplicación a la carpeta de implementación en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="f437e-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="f437e-128">Falta el archivo *Web. config* de la implementación o el contenido del archivo *Web. config* no tiene el formato correcto.</span><span class="sxs-lookup"><span data-stu-id="f437e-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="f437e-129">Realice los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="f437e-129">Perform the following steps:</span></span>

1. <span data-ttu-id="f437e-130">Elimine todos los archivos y carpetas de la carpeta de implementación en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="f437e-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="f437e-131">Vuelva a implementar el contenido de la carpeta de *publicación* de la aplicación en el sistema de hospedaje con el método normal de implementación, como Visual Studio, PowerShell o la implementación manual:</span><span class="sxs-lookup"><span data-stu-id="f437e-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="f437e-132">Confirme que el archivo *Web. config* está presente en la implementación y que su contenido es correcto.</span><span class="sxs-lookup"><span data-stu-id="f437e-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="f437e-133">Cuando hospede en Azure App Service, confirme que la aplicación se implementa en la carpeta `D:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="f437e-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="f437e-134">Cuando la aplicación esté hospedada por IIS, confirme que la aplicación se implementa en la **ruta de acceso física** de IIS que se muestra en la **configuración básica**del **Administrador de IIS**.</span><span class="sxs-lookup"><span data-stu-id="f437e-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="f437e-135">Confirme que todos los archivos y carpetas de la aplicación se han implementado comparando la implementación en el sistema host con el contenido de la carpeta de *publicación* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f437e-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="f437e-136">Para obtener más información sobre el diseño de una aplicación ASP.NET Core publicada, consulte <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="f437e-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="f437e-137">Para obtener más información sobre el archivo *Web. config* , vea <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="f437e-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="f437e-138">500 Error interno del servidor</span><span class="sxs-lookup"><span data-stu-id="f437e-138">500 Internal Server Error</span></span>

<span data-ttu-id="f437e-139">La aplicación se inicia, pero un error impide que el servidor complete la solicitud.</span><span class="sxs-lookup"><span data-stu-id="f437e-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="f437e-140">Este error se produce dentro del código de la aplicación durante el inicio o mientras se crea una respuesta.</span><span class="sxs-lookup"><span data-stu-id="f437e-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="f437e-141">La respuesta no puede contener nada o puede aparecer como *500 Error interno del servidor* en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="f437e-142">El registro de eventos de la aplicación normalmente indica que la aplicación se ha iniciado normalmente.</span><span class="sxs-lookup"><span data-stu-id="f437e-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="f437e-143">Desde la perspectiva del servidor, eso es correcto.</span><span class="sxs-lookup"><span data-stu-id="f437e-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="f437e-144">La aplicación se inició, pero no puede generar una respuesta válida.</span><span class="sxs-lookup"><span data-stu-id="f437e-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="f437e-145">Ejecute la aplicación en un símbolo del sistema en el servidor o habilite el ASP.NET Core módulo stdout log para solucionar el problema.</span><span class="sxs-lookup"><span data-stu-id="f437e-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="f437e-146">500.0 Error de carga del controlador en proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="f437e-147">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-147">The worker process fails.</span></span> <span data-ttu-id="f437e-148">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-148">The app doesn't start.</span></span>

<span data-ttu-id="f437e-149">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) no puede encontrar el CLR de .net Core y encontrar el controlador de solicitud en proceso (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="f437e-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="f437e-150">Compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f437e-150">Check that:</span></span>

* <span data-ttu-id="f437e-151">Que la aplicación tiene como destino el paquete de NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f437e-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="f437e-152">Que la versión del marco compartido de ASP.NET Core que la aplicación tiene como destino esté instalada en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="f437e-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="f437e-153">500.0 Error de carga del controlador fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="f437e-154">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-154">The worker process fails.</span></span> <span data-ttu-id="f437e-155">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-155">The app doesn't start.</span></span>

<span data-ttu-id="f437e-156">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) no encuentra el controlador de solicitudes de hospedaje fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="f437e-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="f437e-157">Asegúrese de que el archivo *aspnetcorev2_outofprocess.dll* está presente en una subcarpeta junto a *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="f437e-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="f437e-158">500.0 Error de carga del controlador en proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="f437e-159">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-159">The worker process fails.</span></span> <span data-ttu-id="f437e-160">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-160">The app doesn't start.</span></span>

<span data-ttu-id="f437e-161">Error desconocido al cargar los componentes del [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="f437e-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="f437e-162">Realice una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="f437e-162">Take one of the following actions:</span></span>

* <span data-ttu-id="f437e-163">Póngase en contacto con el [equipo de soporte técnico de Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (seleccione **Herramientas de desarrollo** y, después, **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="f437e-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="f437e-164">Formule una pregunta en Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="f437e-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="f437e-165">Registre un problema en nuestro [repositorio de GitHub](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="f437e-165">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="f437e-166">500.30 Error de inicio en proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="f437e-167">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-167">The worker process fails.</span></span> <span data-ttu-id="f437e-168">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-168">The app doesn't start.</span></span>

<span data-ttu-id="f437e-169">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el CLR de .net Core en proceso, pero no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="f437e-170">Normalmente, la causa de un error de inicio del proceso se puede determinar a partir de las entradas del registro de eventos de aplicación y el registro de ASP.NET Core del módulo stdout.</span><span class="sxs-lookup"><span data-stu-id="f437e-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="f437e-171">Condiciones de error comunes:</span><span class="sxs-lookup"><span data-stu-id="f437e-171">Common failure conditions:</span></span>

* <span data-ttu-id="f437e-172">La aplicación no está configurada correctamente debido a que el destino es una versión del marco de ASP.NET Core compartido que no está presente.</span><span class="sxs-lookup"><span data-stu-id="f437e-172">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="f437e-173">Compruebe qué versiones del marco compartido de ASP.NET Core están instaladas en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="f437e-173">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="f437e-174">Con Azure Key Vault, faltan permisos para el Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f437e-174">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="f437e-175">Compruebe las directivas de acceso en el Key Vault de destino para asegurarse de que se concedan los permisos correctos.</span><span class="sxs-lookup"><span data-stu-id="f437e-175">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="f437e-176">500.31 ANMC no pudo encontrar las dependencias nativas</span><span class="sxs-lookup"><span data-stu-id="f437e-176">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="f437e-177">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-177">The worker process fails.</span></span> <span data-ttu-id="f437e-178">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-178">The app doesn't start.</span></span>

<span data-ttu-id="f437e-179">El [módulo ASP.net Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el tiempo de ejecución de .net Core en proceso, pero no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-179">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="f437e-180">Este error de inicio suele deberse a que el entorno de ejecución de `Microsoft.NETCore.App` o `Microsoft.AspNetCore.App` no está instalado.</span><span class="sxs-lookup"><span data-stu-id="f437e-180">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="f437e-181">Si la aplicación se implementa para ASP.NET Core 3.0 y esa versión no existe en el equipo, se produce este error.</span><span class="sxs-lookup"><span data-stu-id="f437e-181">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="f437e-182">Este es un mensaje de error de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f437e-182">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="f437e-183">El mensaje de error enumera todas las versiones instaladas de .NET Core y la versión solicitada por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-183">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="f437e-184">Para corregir este error, hay dos posibilidades:</span><span class="sxs-lookup"><span data-stu-id="f437e-184">To fix this error, either:</span></span>

* <span data-ttu-id="f437e-185">Instalar la versión adecuada de .NET Core en la máquina.</span><span class="sxs-lookup"><span data-stu-id="f437e-185">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="f437e-186">Cambiar el destino de la aplicación a una versión de .NET Core que exista en la máquina.</span><span class="sxs-lookup"><span data-stu-id="f437e-186">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="f437e-187">Publique la aplicación como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="f437e-187">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="f437e-188">Cuando se ejecuta en desarrollo (la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece en `Development`), el error específico se escribe en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f437e-188">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="f437e-189">La causa de un error de inicio del proceso también se encuentra en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-189">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="f437e-190">500.32 ANCM No se pudo cargar el archivo .dll</span><span class="sxs-lookup"><span data-stu-id="f437e-190">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="f437e-191">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-191">The worker process fails.</span></span> <span data-ttu-id="f437e-192">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-192">The app doesn't start.</span></span>

<span data-ttu-id="f437e-193">La causa más común de este error es que la aplicación se haya publicado para una arquitectura de procesador no compatible.</span><span class="sxs-lookup"><span data-stu-id="f437e-193">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="f437e-194">Si el proceso de trabajo se ejecuta como una aplicación de 32 bits y la aplicación se diseñó para 64 bits, se produce este error.</span><span class="sxs-lookup"><span data-stu-id="f437e-194">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="f437e-195">Para corregir este error, hay dos posibilidades:</span><span class="sxs-lookup"><span data-stu-id="f437e-195">To fix this error, either:</span></span>

* <span data-ttu-id="f437e-196">Volver a publicar la aplicación para la misma arquitectura de procesador que el proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="f437e-196">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="f437e-197">Publicar la aplicación como una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="f437e-197">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="f437e-198">500.33 Error de carga del controlador de solicitud</span><span class="sxs-lookup"><span data-stu-id="f437e-198">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="f437e-199">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-199">The worker process fails.</span></span> <span data-ttu-id="f437e-200">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-200">The app doesn't start.</span></span>

<span data-ttu-id="f437e-201">La aplicación no hizo referencia al marco `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="f437e-201">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="f437e-202">El [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module)solo puede hospedar las aplicaciones que tengan como destino el marco de `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="f437e-202">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="f437e-203">Para corregir este error, confirme que la aplicación tiene como destino el marco `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="f437e-203">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="f437e-204">Compruebe en `.runtimeconfig.json` cuál es el marco de destino de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-204">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="f437e-205">500.34 ANCM Modelos de hospedaje mixto no admitidos</span><span class="sxs-lookup"><span data-stu-id="f437e-205">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="f437e-206">El proceso de trabajo no puede ejecutar a la vez una aplicación en proceso y una aplicación fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="f437e-206">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="f437e-207">Para corregir este error, ejecute las aplicaciones en grupos de aplicaciones de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="f437e-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="f437e-208">500.35 ANCM Varias aplicaciones en proceso en el mismo proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-208">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="f437e-209">El proceso de trabajo no puede ejecutar varias aplicaciones en proceso en el mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="f437e-209">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="f437e-210">Para corregir este error, ejecute las aplicaciones en grupos de aplicaciones de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="f437e-210">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="f437e-211">500.36 Error de carga del controlador fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-211">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="f437e-212">El controlador de solicitudes de fuera de proceso, *aspnetcorev2_outofprocess.dll*, no está junto al archivo *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="f437e-212">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="f437e-213">Esto indica una instalación dañada del [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="f437e-213">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="f437e-214">Para corregir este error, repare la instalación del [conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (para IIS) o Visual Studio (para IIS Express).</span><span class="sxs-lookup"><span data-stu-id="f437e-214">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="f437e-215">500.37 ANCM no se pudo iniciar en el límite de tiempo de inicio</span><span class="sxs-lookup"><span data-stu-id="f437e-215">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="f437e-216">ANCM no se pudo iniciar dentro del límite de tiempo de inicio proporcionado.</span><span class="sxs-lookup"><span data-stu-id="f437e-216">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="f437e-217">De forma predeterminada, el tiempo de espera es de 120 segundos.</span><span class="sxs-lookup"><span data-stu-id="f437e-217">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="f437e-218">Este error puede producirse cuando se inicia un gran número de aplicaciones en el mismo equipo.</span><span class="sxs-lookup"><span data-stu-id="f437e-218">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="f437e-219">Busque picos de uso de CPU o memoria en el servidor durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="f437e-219">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="f437e-220">Es posible que deba escalonar el proceso de inicio de varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="f437e-220">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="f437e-221">502.5 Error de proceso</span><span class="sxs-lookup"><span data-stu-id="f437e-221">502.5 Process Failure</span></span>

<span data-ttu-id="f437e-222">El proceso de trabajo no funciona.</span><span class="sxs-lookup"><span data-stu-id="f437e-222">The worker process fails.</span></span> <span data-ttu-id="f437e-223">La aplicación no se inicia.</span><span class="sxs-lookup"><span data-stu-id="f437e-223">The app doesn't start.</span></span>

<span data-ttu-id="f437e-224">El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) intenta iniciar el proceso de trabajo, pero no lo consigue.</span><span class="sxs-lookup"><span data-stu-id="f437e-224">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="f437e-225">Normalmente, la causa de un error de inicio del proceso se puede determinar a partir de las entradas del registro de eventos de aplicación y el registro de ASP.NET Core del módulo stdout.</span><span class="sxs-lookup"><span data-stu-id="f437e-225">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="f437e-226">Una condición de error habitual es que la aplicación esté mal configurada porque tiene como destino una versión del marco compartido de ASP.NET Core que no está presente.</span><span class="sxs-lookup"><span data-stu-id="f437e-226">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="f437e-227">Compruebe qué versiones del marco compartido de ASP.NET Core están instaladas en el equipo de destino.</span><span class="sxs-lookup"><span data-stu-id="f437e-227">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="f437e-228">El *marco de trabajo compartido* es el conjunto de ensamblados (archivos *.dll*) que se instalan en la máquina, y se hace referencia a ellos mediante un metapaquete como `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="f437e-228">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="f437e-229">La referencia del metapaquete puede especificar una versión mínima necesaria.</span><span class="sxs-lookup"><span data-stu-id="f437e-229">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="f437e-230">Para más información, consulte este artículo sobre el [marco de trabajo compartido](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="f437e-230">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="f437e-231">La página de error *502.5 Error de proceso* se devuelve cuando el proceso de trabajo no se puede iniciar debido a un error de configuración de la aplicación o del hospedaje:</span><span class="sxs-lookup"><span data-stu-id="f437e-231">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="f437e-232">No se pudo iniciar la aplicación (código de error "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="f437e-232">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="f437e-233">Error al iniciar la aplicación porque el ensamblado de la aplicación ( *.dll*) no se ha podido cargar.</span><span class="sxs-lookup"><span data-stu-id="f437e-233">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="f437e-234">Este error se produce cuando hay un error de coincidencia del valor de bits entre la aplicación publicada y el proceso w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="f437e-234">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="f437e-235">Confirme que la opción de 32 bits del grupo de aplicaciones sea correcta:</span><span class="sxs-lookup"><span data-stu-id="f437e-235">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="f437e-236">Seleccione el grupo de aplicaciones en **Grupos de aplicaciones** del Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="f437e-236">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="f437e-237">Seleccione **Configuración avanzada** en **Modificar grupo de aplicaciones**, en el panel **Acciones**.</span><span class="sxs-lookup"><span data-stu-id="f437e-237">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="f437e-238">Establezca **Habilitar aplicaciones de 32 bits**:</span><span class="sxs-lookup"><span data-stu-id="f437e-238">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="f437e-239">Si implementa una aplicación de 32 bits (x86), establezca el valor en `True`.</span><span class="sxs-lookup"><span data-stu-id="f437e-239">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="f437e-240">Si implementa una aplicación de 64 bits (x64), establezca el valor en `False`.</span><span class="sxs-lookup"><span data-stu-id="f437e-240">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="f437e-241">Confirme que no hay un conflicto entre un `<Platform>` propiedad de MSBuild en el archivo de proyecto y el bits de la aplicación publicado.</span><span class="sxs-lookup"><span data-stu-id="f437e-241">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="f437e-242">Restablecimiento de la conexión</span><span class="sxs-lookup"><span data-stu-id="f437e-242">Connection reset</span></span>

<span data-ttu-id="f437e-243">Si se produce un error después de que se envían los encabezados, el servidor no tiene tiempo para enviar un mensaje **500 Error interno del servidor** cuando se produce un error.</span><span class="sxs-lookup"><span data-stu-id="f437e-243">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="f437e-244">Esto suele ocurrir cuando se produce un error durante la serialización de objetos complejos en una respuesta.</span><span class="sxs-lookup"><span data-stu-id="f437e-244">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="f437e-245">Este tipo de error aparece como un error de *restablecimiento de la conexión* en el cliente.</span><span class="sxs-lookup"><span data-stu-id="f437e-245">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="f437e-246">El [Registro de aplicaciones](xref:fundamentals/logging/index) puede ayudar a solucionar estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="f437e-246">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="f437e-247">Límites de inicio predeterminados</span><span class="sxs-lookup"><span data-stu-id="f437e-247">Default startup limits</span></span>

<span data-ttu-id="f437e-248">El [módulo de ASP.net Core](xref:host-and-deploy/aspnet-core-module) se configura con un valor predeterminado de *startupTimeLimit* de 120 segundos.</span><span class="sxs-lookup"><span data-stu-id="f437e-248">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="f437e-249">Cuando se deja en el valor predeterminado, una aplicación puede tardar hasta dos minutos en iniciarse antes de que el módulo registre un error de proceso.</span><span class="sxs-lookup"><span data-stu-id="f437e-249">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="f437e-250">Para información sobre la configuración del módulo, consulte [Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="f437e-250">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="f437e-251">Solución de problemas en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f437e-251">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="f437e-252">Registro de eventos de aplicación (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="f437e-252">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="f437e-253">Para acceder al registro de eventos de la aplicación, use la hoja **Diagnose and solve problems** (Diagnosticar y solucionar problemas) de Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="f437e-253">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="f437e-254">En Azure Portal, abra la aplicación en **App Services**.</span><span class="sxs-lookup"><span data-stu-id="f437e-254">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="f437e-255">Seleccione **Diagnosticar y solucionar problemas**.</span><span class="sxs-lookup"><span data-stu-id="f437e-255">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="f437e-256">Seleccione el título **Herramientas de diagnóstico**.</span><span class="sxs-lookup"><span data-stu-id="f437e-256">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="f437e-257">En **Herramientas de soporte técnico**, seleccione el botón **Eventos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="f437e-257">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="f437e-258">Examine el error más reciente que hayan proporcionado las entradas *IIS AspNetCoreModule* o *IIS AspNetCoreModule V2* en la columna **Origen**.</span><span class="sxs-lookup"><span data-stu-id="f437e-258">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="f437e-259">Una alternativa al uso de la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) es examinar el archivo de registro de eventos de la aplicación directamente mediante [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="f437e-259">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="f437e-260">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="f437e-260">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f437e-261">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="f437e-261">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f437e-262">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-262">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f437e-263">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f437e-263">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f437e-264">Abra la carpeta **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="f437e-264">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="f437e-265">Seleccione el icono de lápiz junto al archivo *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="f437e-265">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="f437e-266">Examine el registro.</span><span class="sxs-lookup"><span data-stu-id="f437e-266">Examine the log.</span></span> <span data-ttu-id="f437e-267">Desplácese al final del registro para ver los eventos más recientes.</span><span class="sxs-lookup"><span data-stu-id="f437e-267">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="f437e-268">Ejecución de la aplicación en la consola de Kudu</span><span class="sxs-lookup"><span data-stu-id="f437e-268">Run the app in the Kudu console</span></span>

<span data-ttu-id="f437e-269">Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-269">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="f437e-270">Puede ejecutar la aplicación en la consola de ejecución remota de [Kudu](https://github.com/projectkudu/kudu/wiki) para detectar el error:</span><span class="sxs-lookup"><span data-stu-id="f437e-270">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="f437e-271">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="f437e-271">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f437e-272">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="f437e-272">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f437e-273">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-273">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f437e-274">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f437e-274">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="f437e-275">Prueba de una aplicación de 32 bits (x86)</span><span class="sxs-lookup"><span data-stu-id="f437e-275">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="f437e-276">**Versión actual**</span><span class="sxs-lookup"><span data-stu-id="f437e-276">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="f437e-277">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f437e-277">Run the app:</span></span>
   * <span data-ttu-id="f437e-278">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="f437e-278">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="f437e-279">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="f437e-279">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="f437e-280">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="f437e-280">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="f437e-281">**Implementación dependiente del marco de trabajo que se ejecuta en una versión preliminar**</span><span class="sxs-lookup"><span data-stu-id="f437e-281">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="f437e-282">*Requiere la instalación de la extensión de sitio ASP.NET Core {VERSION} (x86) Runtime.*</span><span class="sxs-lookup"><span data-stu-id="f437e-282">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="f437e-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` es la versión del runtime)</span><span class="sxs-lookup"><span data-stu-id="f437e-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="f437e-284">Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="f437e-284">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="f437e-285">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="f437e-285">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="f437e-286">Prueba de una aplicación de 64 bits (x64)</span><span class="sxs-lookup"><span data-stu-id="f437e-286">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="f437e-287">**Versión actual**</span><span class="sxs-lookup"><span data-stu-id="f437e-287">**Current release**</span></span>

* <span data-ttu-id="f437e-288">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) de 64 bits (x64):</span><span class="sxs-lookup"><span data-stu-id="f437e-288">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="f437e-289">Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="f437e-289">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="f437e-290">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="f437e-290">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="f437e-291">Ejecute la aplicación: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="f437e-291">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="f437e-292">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="f437e-292">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="f437e-293">**Implementación dependiente del marco de trabajo que se ejecuta en una versión preliminar**</span><span class="sxs-lookup"><span data-stu-id="f437e-293">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="f437e-294">*Requiere la instalación de la extensión de sitio ASP.NET Core {VERSION} (x64) Runtime.*</span><span class="sxs-lookup"><span data-stu-id="f437e-294">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="f437e-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` es la versión del runtime)</span><span class="sxs-lookup"><span data-stu-id="f437e-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="f437e-296">Ejecute la aplicación: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="f437e-296">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="f437e-297">La salida de consola de la aplicación, que muestra los posibles errores, se canaliza a la consola de Kudu.</span><span class="sxs-lookup"><span data-stu-id="f437e-297">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="f437e-298">Registro de ASP.NET Core del módulo stdout (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="f437e-298">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="f437e-299">El registro stdout del módulo ASP.NET Core con frecuencia registra mensajes de error útiles que no se encuentran en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-299">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="f437e-300">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="f437e-300">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="f437e-301">Vaya a la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f437e-301">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="f437e-302">En **Seleccione una categoría de problema**, seleccione el botón abajo **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="f437e-302">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="f437e-303">En **Suggested Solutions** >  (Soluciones sugeridas) **Enable Stdout Log Redirection** (Habilitar el redireccionamiento de registros stdout), seleccione el botón para **abrir la consola de Kudu y editar web.config**.</span><span class="sxs-lookup"><span data-stu-id="f437e-303">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="f437e-304">En la **consola de diagnóstico** de Kudu, abra las carpetas para la ruta de acceso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="f437e-304">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="f437e-305">Desplácese hacia abajo para mostrar el archivo *web.config* en la parte inferior de la lista.</span><span class="sxs-lookup"><span data-stu-id="f437e-305">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="f437e-306">Haga clic en el icono de lápiz junto al archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="f437e-306">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="f437e-307">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** a: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="f437e-307">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="f437e-308">Seleccione **Save** (Guardar) para guardar el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="f437e-308">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="f437e-309">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-309">Make a request to the app.</span></span>
1. <span data-ttu-id="f437e-310">Vuelva a Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f437e-310">Return to the Azure portal.</span></span> <span data-ttu-id="f437e-311">Seleccione la hoja **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="f437e-311">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="f437e-312">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="f437e-312">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f437e-313">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-313">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f437e-314">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f437e-314">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f437e-315">Seleccione la carpeta **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="f437e-315">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="f437e-316">Inspeccione la columna **Modificado** y seleccione el icono de lápiz para editar el registro de stdout con la última fecha de modificación.</span><span class="sxs-lookup"><span data-stu-id="f437e-316">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="f437e-317">Cuando se abre el archivo de registro, se muestra el error.</span><span class="sxs-lookup"><span data-stu-id="f437e-317">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="f437e-318">Deshabilite el registro de stdout una vez haya solucionado los problemas:</span><span class="sxs-lookup"><span data-stu-id="f437e-318">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="f437e-319">En la **consola de diagnóstico** de Kudu, vuelva a la ruta de acceso **site** > **wwwroot** para mostrar el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="f437e-319">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="f437e-320">Seleccione el icono de lápiz para abrir de nuevo el archivo **web.config**.</span><span class="sxs-lookup"><span data-stu-id="f437e-320">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="f437e-321">Establezca **stdoutLogEnabled** en `false`.</span><span class="sxs-lookup"><span data-stu-id="f437e-321">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="f437e-322">Seleccione **Save** (Guardar) para guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="f437e-322">Select **Save** to save the file.</span></span>

<span data-ttu-id="f437e-323">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="f437e-323">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="f437e-324">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="f437e-324">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="f437e-325">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="f437e-325">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="f437e-326">Use únicamente el registro de stdout para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-326">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="f437e-327">Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="f437e-327">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f437e-328">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="f437e-328">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="f437e-329">Registro de depuración del módulo de ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="f437e-329">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="f437e-330">El registro de depuración del módulo de ASP.NET Core ofrece un registro adicional, más profundo, del módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f437e-330">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="f437e-331">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="f437e-331">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="f437e-332">Para habilitar el registro de diagnóstico mejorado, realice cualquiera de las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="f437e-332">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="f437e-333">Siga las instrucciones de [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) para configurar la aplicación para un registro de diagnóstico mejorado.</span><span class="sxs-lookup"><span data-stu-id="f437e-333">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="f437e-334">Vuelva a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-334">Redeploy the app.</span></span>
   * <span data-ttu-id="f437e-335">Agregue la `<handlerSettings>` que se muestra en [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) al archivo *web.config* de la aplicación activa mediante la consola de Kudu:</span><span class="sxs-lookup"><span data-stu-id="f437e-335">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="f437e-336">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="f437e-336">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f437e-337">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="f437e-337">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f437e-338">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-338">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="f437e-339">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f437e-339">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="f437e-340">Abra las carpetas para la ruta de acceso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="f437e-340">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="f437e-341">Seleccione el icono de lápiz para editar el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="f437e-341">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="f437e-342">Agregue la sección `<handlerSettings>` como se muestra en [Registros de diagnóstico mejorados](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="f437e-342">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="f437e-343">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="f437e-343">Select the **Save** button.</span></span>
1. <span data-ttu-id="f437e-344">Abra **Herramientas avanzadas** en el área **Herramientas de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="f437e-344">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f437e-345">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="f437e-345">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f437e-346">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-346">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f437e-347">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f437e-347">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f437e-348">Abra las carpetas para la ruta de acceso **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="f437e-348">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="f437e-349">Si no ha proporcionado una ruta de acceso para el archivo *aspnetcore debug.log*, el archivo aparece en la lista.</span><span class="sxs-lookup"><span data-stu-id="f437e-349">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="f437e-350">Si ha proporcionado una ruta de acceso, vaya a la ubicación del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="f437e-350">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="f437e-351">Abra el archivo de registro con el botón de lápiz situado junto al nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="f437e-351">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="f437e-352">Deshabilite el registro de depuración una vez haya solucionado los problemas:</span><span class="sxs-lookup"><span data-stu-id="f437e-352">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="f437e-353">Para deshabilitar el registro de depuración mejorado, realice cualquiera de las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="f437e-353">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="f437e-354">Quite localmente la `<handlerSettings>` del archivo *web.config* y vuelva a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-354">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="f437e-355">Use la consola de Kudu para editar el archivo *web.config* y quite la sección `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="f437e-355">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="f437e-356">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="f437e-356">Save the file.</span></span>

<span data-ttu-id="f437e-357">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="f437e-357">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="f437e-358">Si no deshabilita el registro de depuración, es posible que se produzcan errores en la aplicación o el servidor.</span><span class="sxs-lookup"><span data-stu-id="f437e-358">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="f437e-359">No hay ningún límite para el tamaño del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="f437e-359">There's no limit on log file size.</span></span> <span data-ttu-id="f437e-360">Use únicamente el registro de depuración para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-360">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="f437e-361">Para el registro general en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="f437e-361">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f437e-362">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="f437e-362">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="f437e-363">Aplicación lenta o bloqueada (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="f437e-363">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="f437e-364">Cuando una aplicación responde con lentitud o se bloquea en una solicitud, consulte los artículos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f437e-364">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="f437e-365">Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f437e-365">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="f437e-366">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) (Uso de la extensión de sitio de diagnóstico de bloqueo para capturar el volcado de memoria para problemas de excepciones intermitentes o problemas de rendimiento en Azure Web Apps)</span><span class="sxs-lookup"><span data-stu-id="f437e-366">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="f437e-367">Hojas de supervisión</span><span class="sxs-lookup"><span data-stu-id="f437e-367">Monitoring blades</span></span>

<span data-ttu-id="f437e-368">Las hojas de supervisión proporcionan una alternativa a la experiencia de solución de problemas de los métodos descritos anteriormente en el tema.</span><span class="sxs-lookup"><span data-stu-id="f437e-368">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="f437e-369">Estas hojas se pueden usar para diagnosticar errores de la serie 500.</span><span class="sxs-lookup"><span data-stu-id="f437e-369">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="f437e-370">Confirme que están instaladas las extensiones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f437e-370">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="f437e-371">Si no lo están, instálelas manualmente:</span><span class="sxs-lookup"><span data-stu-id="f437e-371">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="f437e-372">En la sección de la hoja **HERRAMIENTAS DE DESARROLLO**, seleccione la hoja **Extensiones**.</span><span class="sxs-lookup"><span data-stu-id="f437e-372">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="f437e-373">Aparecerán en la lista las **extensiones de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f437e-373">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="f437e-374">Si las extensiones no están instaladas, seleccione el botón **Add** (Agregar).</span><span class="sxs-lookup"><span data-stu-id="f437e-374">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="f437e-375">Elija las **extensiones de ASP.NET Core** de la lista.</span><span class="sxs-lookup"><span data-stu-id="f437e-375">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="f437e-376">Seleccione **Aceptar** para aceptar los términos legales.</span><span class="sxs-lookup"><span data-stu-id="f437e-376">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="f437e-377">Seleccione **Aceptar** en la hoja **Agregar extensión**.</span><span class="sxs-lookup"><span data-stu-id="f437e-377">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="f437e-378">Un mensaje emergente informativo indica si las extensiones se han instalado correctamente.</span><span class="sxs-lookup"><span data-stu-id="f437e-378">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="f437e-379">Si el registro de stdout no está habilitado, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="f437e-379">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="f437e-380">En Azure Portal, seleccione la hoja **Herramientas avanzadas** en el área **HERRAMIENTAS DE DESARROLLO**.</span><span class="sxs-lookup"><span data-stu-id="f437e-380">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="f437e-381">Seleccione el botón **Ir&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="f437e-381">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f437e-382">Se abre la consola de Kudu en una nueva pestaña o ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f437e-382">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f437e-383">Mediante la barra de navegación de la parte superior de la página, abra la **consola de depuración** y seleccione **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f437e-383">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f437e-384">Abra las carpetas a la ruta de acceso **sitio** > **wwwroot** y desplácese hacia abajo para mostrar el archivo *web.config* en la parte inferior de la lista.</span><span class="sxs-lookup"><span data-stu-id="f437e-384">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="f437e-385">Haga clic en el icono de lápiz junto al archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="f437e-385">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="f437e-386">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** a: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="f437e-386">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="f437e-387">Seleccione **Save** (Guardar) para guardar el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="f437e-387">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="f437e-388">Continúe para activar el registro de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="f437e-388">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="f437e-389">En Azure Portal, seleccione la hoja **Registros de diagnóstico**.</span><span class="sxs-lookup"><span data-stu-id="f437e-389">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="f437e-390">Seleccione el conmutador **Activado** en **Registro de la aplicación (sistema de archivos)** y **Mensajes de error detallados**.</span><span class="sxs-lookup"><span data-stu-id="f437e-390">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="f437e-391">Seleccione el botón **Guardar** en la parte superior de la hoja.</span><span class="sxs-lookup"><span data-stu-id="f437e-391">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="f437e-392">Para incluir el seguimiento de solicitudes con error, también conocido como almacenamiento en búfer de eventos de solicitudes con error (FREB), seleccione el conmutador **Activado** en **Seguimiento de solicitudes con error**.</span><span class="sxs-lookup"><span data-stu-id="f437e-392">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="f437e-393">Seleccione la hoja **Secuencia de registro**, que aparece inmediatamente bajo la hoja **Registros de diagnóstico** en el portal.</span><span class="sxs-lookup"><span data-stu-id="f437e-393">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="f437e-394">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-394">Make a request to the app.</span></span>
1. <span data-ttu-id="f437e-395">Dentro de los datos de la secuencia de registro, se indica la causa del error.</span><span class="sxs-lookup"><span data-stu-id="f437e-395">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="f437e-396">No olvide deshabilitar el registro de stdout cuando finalice la solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="f437e-396">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="f437e-397">Para ver los registros de seguimiento de solicitudes con error (registros FREB):</span><span class="sxs-lookup"><span data-stu-id="f437e-397">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="f437e-398">Vaya a la hoja **Diagnose and solve problems** (Diagnosticar y resolver problemas) de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f437e-398">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="f437e-399">Seleccione **Failed Request Tracing Logs** (Registros de seguimiento de solicitudes con error) en el área **SUPPORT TOOLS** (HERRAMIENTAS DE SOPORTE TÉCNICO) de la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="f437e-399">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="f437e-400">Para más información, consulte la [sección sobre los seguimientos de solicitudes con error del tema Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) y el artículo [Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure: ¿Cómo se activa el seguimiento de solicitudes con error?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).</span><span class="sxs-lookup"><span data-stu-id="f437e-400">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="f437e-401">Para más información, consulte [Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="f437e-401">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="f437e-402">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="f437e-402">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="f437e-403">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="f437e-403">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="f437e-404">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="f437e-404">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f437e-405">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="f437e-405">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="f437e-406">Solucionar problemas en IIS</span><span class="sxs-lookup"><span data-stu-id="f437e-406">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="f437e-407">Registro de eventos de aplicación (IIS)</span><span class="sxs-lookup"><span data-stu-id="f437e-407">Application Event Log (IIS)</span></span>

<span data-ttu-id="f437e-408">Acceda al registro de eventos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f437e-408">Access the Application Event Log:</span></span>

1. <span data-ttu-id="f437e-409">Abra el menú Inicio, busque *visor de eventos*y seleccione la aplicación **visor de eventos** .</span><span class="sxs-lookup"><span data-stu-id="f437e-409">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="f437e-410">En **Visor de eventos**, abra el nodo **Registros de Windows**.</span><span class="sxs-lookup"><span data-stu-id="f437e-410">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="f437e-411">Seleccione **Aplicación** para abrir el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-411">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="f437e-412">Busque los errores asociados a la aplicación objeto del error.</span><span class="sxs-lookup"><span data-stu-id="f437e-412">Search for errors associated with the failing app.</span></span> <span data-ttu-id="f437e-413">Los errores tienen un valor de *Módulo AspNetCore de IIS* o *Módulo AspNetCore de IIS Express* en la columna *Origen*.</span><span class="sxs-lookup"><span data-stu-id="f437e-413">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="f437e-414">Ejecución de la aplicación en un símbolo del sistema</span><span class="sxs-lookup"><span data-stu-id="f437e-414">Run the app at a command prompt</span></span>

<span data-ttu-id="f437e-415">Muchos errores de inicio no generan información útil en el registro de eventos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-415">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="f437e-416">La causa de algunos errores se puede encontrar mediante la ejecución de la aplicación en un símbolo del sistema en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="f437e-416">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="f437e-417">Implementación dependiente de marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="f437e-417">Framework-dependent deployment</span></span>

<span data-ttu-id="f437e-418">Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="f437e-418">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="f437e-419">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute la aplicación mediante la ejecución del ensamblado de la aplicación con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="f437e-419">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="f437e-420">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="f437e-420">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="f437e-421">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="f437e-421">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="f437e-422">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f437e-422">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="f437e-423">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="f437e-423">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="f437e-424">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-424">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="f437e-425">Implementación autocontenida</span><span class="sxs-lookup"><span data-stu-id="f437e-425">Self-contained deployment</span></span>

<span data-ttu-id="f437e-426">Si la aplicación es una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="f437e-426">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="f437e-427">En un símbolo del sistema, vaya a la carpeta de implementación y ejecute el archivo ejecutable de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-427">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="f437e-428">En el siguiente comando, sustituya el nombre del ensamblado de la aplicación por \<nombre_de_ensamblado>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="f437e-428">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="f437e-429">La salida de consola de la aplicación, que muestra los posibles errores, se escribe en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="f437e-429">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="f437e-430">Si los errores se producen al realizar una solicitud a la aplicación, realice una solicitud al host y el puerto donde escucha Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f437e-430">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="f437e-431">Con el host y el puerto predeterminados, realizar una solicitud a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="f437e-431">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="f437e-432">Si la aplicación responde normalmente en la dirección del punto de conexión de Kestrel, es más probable que el problema esté relacionado con la configuración de hospedaje que con la propia aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-432">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="f437e-433">Registro de ASP.NET Core del módulo stdout (IIS)</span><span class="sxs-lookup"><span data-stu-id="f437e-433">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="f437e-434">Para habilitar y ver los registros de stdout:</span><span class="sxs-lookup"><span data-stu-id="f437e-434">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="f437e-435">Vaya a la carpeta de implementación del sitio en el sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="f437e-435">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="f437e-436">Si la carpeta *Logs* no existe, cree la carpeta.</span><span class="sxs-lookup"><span data-stu-id="f437e-436">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="f437e-437">Para obtener instrucciones sobre cómo habilitar MSBuild para crear la carpeta *logs* automáticamente en la implementación, consulte el tema [Estructura de directorios](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="f437e-437">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="f437e-438">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="f437e-438">Edit the *web.config* file.</span></span> <span data-ttu-id="f437e-439">Establezca **stdoutLogEnabled** en `true` y cambie la ruta de acceso de **stdoutLogFile** para que apunte a la carpeta *logs* (por ejemplo, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="f437e-439">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="f437e-440">`stdout` en la ruta de acceso es el prefijo del nombre del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="f437e-440">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="f437e-441">Cuando se crea el registro, se agregan automáticamente una marca de tiempo, un identificador de proceso y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="f437e-441">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="f437e-442">Cuando se usa `stdout` como prefijo para el nombre de archivo, un archivo de registro se llama normalmente *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="f437e-442">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="f437e-443">Asegúrese de que la identidad del grupo de aplicaciones tiene permisos de escritura en la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="f437e-443">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="f437e-444">Guarde el archivo *web.config* actualizado.</span><span class="sxs-lookup"><span data-stu-id="f437e-444">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="f437e-445">Realice una solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-445">Make a request to the app.</span></span>
1. <span data-ttu-id="f437e-446">Vaya a la carpeta *logs*.</span><span class="sxs-lookup"><span data-stu-id="f437e-446">Navigate to the *logs* folder.</span></span> <span data-ttu-id="f437e-447">Busque y abra el registro más reciente de stdout.</span><span class="sxs-lookup"><span data-stu-id="f437e-447">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="f437e-448">Estudie el registro para ver los errores.</span><span class="sxs-lookup"><span data-stu-id="f437e-448">Study the log for errors.</span></span>

<span data-ttu-id="f437e-449">Deshabilite el registro de stdout una vez haya solucionado los problemas:</span><span class="sxs-lookup"><span data-stu-id="f437e-449">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="f437e-450">Edite el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="f437e-450">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="f437e-451">Establezca **stdoutLogEnabled** en `false`.</span><span class="sxs-lookup"><span data-stu-id="f437e-451">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="f437e-452">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="f437e-452">Save the file.</span></span>

<span data-ttu-id="f437e-453">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="f437e-453">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="f437e-454">La imposibilidad de deshabilitar el registro de stdout puede dar lugar a un error de la aplicación o del servidor.</span><span class="sxs-lookup"><span data-stu-id="f437e-454">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="f437e-455">No hay ningún límite en el tamaño del archivo de registro ni en el número de archivos de registro creados.</span><span class="sxs-lookup"><span data-stu-id="f437e-455">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="f437e-456">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="f437e-456">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f437e-457">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="f437e-457">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="f437e-458">Registro de depuración del módulo de ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="f437e-458">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="f437e-459">Agregue la siguiente configuración de controlador al archivo *Web. config* de la aplicación para habilitar el registro de depuración del módulo de ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="f437e-459">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="f437e-460">Confirme que la ruta de acceso especificada para el registro exista y que la identidad del grupo de aplicaciones tenga permisos de escritura en la ubicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-460">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="f437e-461">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="f437e-461">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="f437e-462">Habilitación de la página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="f437e-462">Enable the Developer Exception Page</span></span>

<span data-ttu-id="f437e-463">[Se puede Agregar la variable de entorno `ASPNETCORE_ENVIRONMENT` a Web. config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f437e-463">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="f437e-464">Siempre y cuando el entorno no se invalide al inicio de la aplicación con `UseEnvironment` en el generador de host, la configuración de la variable de entorno permite que aparezca la [página de excepciones del desarrollador](xref:fundamentals/error-handling) cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-464">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="f437e-465">Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` cuando se use en servidores de ensayo o pruebas que no estén expuestos a Internet.</span><span class="sxs-lookup"><span data-stu-id="f437e-465">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="f437e-466">Quite la variable de entorno del archivo *web.config* cuando termine de solucionar los problemas.</span><span class="sxs-lookup"><span data-stu-id="f437e-466">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="f437e-467">Para información sobre la configuración de variables de entorno en *web.config*, consulte el [elemento secundario environmentVariables de aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="f437e-467">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="f437e-468">Obtención de datos de una aplicación</span><span class="sxs-lookup"><span data-stu-id="f437e-468">Obtain data from an app</span></span>

<span data-ttu-id="f437e-469">Si una aplicación es capaz de responder a solicitudes, obtenga solicitudes, conexiones y datos adicionales de la aplicación mediante el middleware en línea del terminal.</span><span class="sxs-lookup"><span data-stu-id="f437e-469">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="f437e-470">Para obtener más información y un código de ejemplo, vea <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="f437e-470">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="f437e-471">Aplicación lenta o bloqueada (IIS)</span><span class="sxs-lookup"><span data-stu-id="f437e-471">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="f437e-472">Un *volcado* de memoria es una instantánea de la memoria del sistema y puede ayudar a determinar la causa de un bloqueo de la aplicación, un error de inicio o una aplicación lenta.</span><span class="sxs-lookup"><span data-stu-id="f437e-472">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="f437e-473">Bloqueo o excepción de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f437e-473">App crashes or encounters an exception</span></span>

<span data-ttu-id="f437e-474">Obtenga y analice un volcado de memoria en [Informe de errores de Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="f437e-474">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="f437e-475">Cree una carpeta para almacenar los archivos de volcado de memoria en `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="f437e-475">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="f437e-476">El grupo de aplicaciones debe tener acceso de escritura a la carpeta.</span><span class="sxs-lookup"><span data-stu-id="f437e-476">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="f437e-477">Ejecute el [script EnableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="f437e-477">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="f437e-478">Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="f437e-478">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="f437e-479">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="f437e-479">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="f437e-480">Ejecute la aplicación en las condiciones que hacen que se produzca el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="f437e-480">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="f437e-481">Una vez que se haya producido el bloqueo, ejecute el [script DisableDumps de PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="f437e-481">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="f437e-482">Si la aplicación usa el [modelo de hospedaje en proceso](xref:host-and-deploy/iis/index#in-process-hosting-model), ejecute el script *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="f437e-482">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="f437e-483">Si la aplicación usa el [modelo de hospedaje fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model), ejecute el script *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="f437e-483">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="f437e-484">Después de que se bloquee la aplicación y se complete la recopilación del volcado de memoria, la aplicación puede finalizar con normalidad.</span><span class="sxs-lookup"><span data-stu-id="f437e-484">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="f437e-485">El script de PowerShell configura WER de modo que recopile un máximo de cinco volcados de memoria por aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-485">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="f437e-486">Los volcados de memoria pueden ocupar una gran cantidad de espacio en disco (hasta varios gigabytes cada uno).</span><span class="sxs-lookup"><span data-stu-id="f437e-486">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="f437e-487">La aplicación deja de responder, produce un error durante el inicio o se ejecuta con normalidad</span><span class="sxs-lookup"><span data-stu-id="f437e-487">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="f437e-488">Cuando una aplicación se *bloquea* (deja de responder pero no se bloquea), se produce un error durante el inicio o se ejecuta con normalidad, consulte [archivos de volcado de modo de usuario: elegir la mejor herramienta](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) para seleccionar una herramienta adecuada para generar el volcado de memoria.</span><span class="sxs-lookup"><span data-stu-id="f437e-488">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="f437e-489">Análisis del volcado de memoria</span><span class="sxs-lookup"><span data-stu-id="f437e-489">Analyze the dump</span></span>

<span data-ttu-id="f437e-490">El volcado de memoria se puede analizar de varias maneras.</span><span class="sxs-lookup"><span data-stu-id="f437e-490">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="f437e-491">Para obtener más información, vea [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Análisis de un archivo de volcado de memoria en modo de usuario).</span><span class="sxs-lookup"><span data-stu-id="f437e-491">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="f437e-492">Borrar cachés de paquetes</span><span class="sxs-lookup"><span data-stu-id="f437e-492">Clear package caches</span></span>

<span data-ttu-id="f437e-493">Una aplicación en funcionamiento puede producir un error inmediatamente después de actualizar el SDK de .NET Core en el equipo de desarrollo o de cambiar las versiones de paquete dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-493">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="f437e-494">En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes.</span><span class="sxs-lookup"><span data-stu-id="f437e-494">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="f437e-495">La mayoría de estos problemas puede corregirse siguiendo estas instrucciones:</span><span class="sxs-lookup"><span data-stu-id="f437e-495">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="f437e-496">Elimine las carpetas *bin* y *obj*.</span><span class="sxs-lookup"><span data-stu-id="f437e-496">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="f437e-497">Borre las memorias caché de paquetes mediante la ejecución de [dotnet Nuget locals todo--claro](/dotnet/core/tools/dotnet-nuget-locals) desde un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="f437e-497">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="f437e-498">La desactivación de las memorias caché de paquetes también se puede realizar con la herramienta [Nuget. exe](https://www.nuget.org/downloads) y ejecutar el `nuget locals all -clear`de comandos.</span><span class="sxs-lookup"><span data-stu-id="f437e-498">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="f437e-499">*nuget.exe* no es una instalación agrupada con el sistema operativo de escritorio de Windows y se debe obtener de forma independiente en el [sitio web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="f437e-499">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="f437e-500">Restaure el proyecto y vuelva a compilarlo.</span><span class="sxs-lookup"><span data-stu-id="f437e-500">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="f437e-501">Elimine todos los archivos de la carpeta de implementación en el servidor antes de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f437e-501">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f437e-502">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f437e-502">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="f437e-503">Documentación de Azure</span><span class="sxs-lookup"><span data-stu-id="f437e-503">Azure documentation</span></span>

* [<span data-ttu-id="f437e-504">Application Insights para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f437e-504">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="f437e-505">Sección de aplicaciones Web de depuración remota de solución de problemas de una aplicación web en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f437e-505">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="f437e-506">Introducción a diagnósticos de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f437e-506">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="f437e-507">Cómo: Supervisar aplicaciones en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f437e-507">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="f437e-508">Solución de problemas de una aplicación web en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f437e-508">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="f437e-509">Solucionar los errores HTTP de "502 Puerta de enlace no válida" y "503 Servicio no disponible" en las aplicaciones web de Azure</span><span class="sxs-lookup"><span data-stu-id="f437e-509">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="f437e-510">Solucionar los problemas de rendimiento reducido de aplicaciones web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f437e-510">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="f437e-511">Preguntas más frecuentes sobre el rendimiento de aplicaciones para Web Apps de Azure</span><span class="sxs-lookup"><span data-stu-id="f437e-511">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="f437e-512">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Espacio aislado de Azure Web App [limitaciones de ejecución del entono de tiempo de ejecución de App Service])</span><span class="sxs-lookup"><span data-stu-id="f437e-512">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="f437e-513">Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)</span><span class="sxs-lookup"><span data-stu-id="f437e-513">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="f437e-514">Documentación de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f437e-514">Visual Studio documentation</span></span>

* [<span data-ttu-id="f437e-515">Depuración remota ASP.NET Core en IIS en Azure en Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f437e-515">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="f437e-516">ASP.NET Core de depuración remota en un equipo remoto de IIS en Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f437e-516">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="f437e-517">Información sobre cómo depurar con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f437e-517">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="f437e-518">Documentación de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f437e-518">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="f437e-519">Depuración con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f437e-519">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
