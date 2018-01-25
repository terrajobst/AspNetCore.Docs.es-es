---
title: Trabajar con varios entornos de ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de cómo ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos."
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="23c6d-103">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="23c6d-103">Working with multiple environments</span></span>

<span data-ttu-id="23c6d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23c6d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="23c6d-105">ASP.NET Core proporciona compatibilidad para establecer el comportamiento de la aplicación en tiempo de ejecución con las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="23c6d-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="23c6d-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23c6d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="23c6d-107">Entornos</span><span class="sxs-lookup"><span data-stu-id="23c6d-107">Environments</span></span>

<span data-ttu-id="23c6d-108">ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena ese valor en [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="23c6d-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="23c6d-109">`ASPNETCORE_ENVIRONMENT`se puede establecer en cualquier valor, pero [tres valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) son compatibles con el marco de trabajo: [desarrollo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [ensayo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), y [producción](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="23c6d-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="23c6d-110">Si `ASPNETCORE_ENVIRONMENT` no está establecido, el valor predeterminado será `Production`.</span><span class="sxs-lookup"><span data-stu-id="23c6d-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="23c6d-111">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="23c6d-111">The preceding code:</span></span>

* <span data-ttu-id="23c6d-112">Llamadas [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.</span><span class="sxs-lookup"><span data-stu-id="23c6d-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="23c6d-113">Llamadas [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando el valor de `ASPNETCORE_ENVIRONMENT` se establece uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="23c6d-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="23c6d-114">El [auxiliar de etiquetas de entorno ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utiliza el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:</span><span class="sxs-lookup"><span data-stu-id="23c6d-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="23c6d-115">Nota: En Windows y Mac OS, valores y variables de entorno no son distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="23c6d-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="23c6d-116">Las variables de entorno de Linux y los valores son **entre mayúsculas y minúsculas** de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="23c6d-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="23c6d-117">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="23c6d-117">Development</span></span>

<span data-ttu-id="23c6d-118">El entorno de desarrollo puede habilitar las características que no deben exponerse en producción.</span><span class="sxs-lookup"><span data-stu-id="23c6d-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="23c6d-119">Por ejemplo, las plantillas de ASP.NET Core permiten la [página de excepción para desarrolladores](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="23c6d-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="23c6d-120">El entorno de desarrollo de equipo local se puede establecer el *Properties\launchSettings.json* archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="23c6d-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="23c6d-121">Los valores de entorno establecer *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="23c6d-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="23c6d-122">El siguiente XML muestra tres perfiles de un *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="23c6d-122">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="23c6d-123">Cuando se inicia la aplicación con `dotnet run`, el primer perfil con `"commandName": "Project"` se usará.</span><span class="sxs-lookup"><span data-stu-id="23c6d-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="23c6d-124">El valor de `commandName` especifica el servidor web para iniciar.</span><span class="sxs-lookup"><span data-stu-id="23c6d-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="23c6d-125">`commandName`puede ser uno de:</span><span class="sxs-lookup"><span data-stu-id="23c6d-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="23c6d-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="23c6d-126">IIS Express</span></span>
* <span data-ttu-id="23c6d-127">IIS</span><span class="sxs-lookup"><span data-stu-id="23c6d-127">IIS</span></span>
* <span data-ttu-id="23c6d-128">Proyecto (que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="23c6d-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="23c6d-129">Cuando se inicia una aplicación con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="23c6d-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="23c6d-130">*launchSettings.json* se lee si está disponible.</span><span class="sxs-lookup"><span data-stu-id="23c6d-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="23c6d-131">`environmentVariables`configuración de *launchSettings.json* reemplazan a las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="23c6d-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="23c6d-132">Se muestra el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="23c6d-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="23c6d-133">La siguiente salida muestra una aplicación que se inició con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="23c6d-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="23c6d-134">Visual Studio **depurar** ficha proporciona una interfaz gráfica de usuario para editar la *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="23c6d-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables de entorno de configuración de propiedades de proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="23c6d-136">Los cambios realizados en los perfiles de los proyectos podrán no surtan efecto hasta que se reinicie el servidor web.</span><span class="sxs-lookup"><span data-stu-id="23c6d-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="23c6d-137">Kestrel debe reiniciarse para que se detectarán los cambios realizados en su entorno.</span><span class="sxs-lookup"><span data-stu-id="23c6d-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="23c6d-138">*launchSettings.json* no debería almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="23c6d-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="23c6d-139">El [herramienta Administrador de secreto](xref:security/app-secrets) puede usarse para almacenar secretos de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="23c6d-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="23c6d-140">Producción</span><span class="sxs-lookup"><span data-stu-id="23c6d-140">Production</span></span>

<span data-ttu-id="23c6d-141">El entorno de producción debe configurarse para maximizar la seguridad, rendimiento y eficacia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c6d-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="23c6d-142">Algunas configuraciones comunes que podría tener un entorno de producción que serían diferentes entornos de desarrollo incluyen:</span><span class="sxs-lookup"><span data-stu-id="23c6d-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="23c6d-143">Almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="23c6d-143">Caching.</span></span>
* <span data-ttu-id="23c6d-144">Recursos del cliente están agrupados, reducidos y potencialmente provienen de una CDN.</span><span class="sxs-lookup"><span data-stu-id="23c6d-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="23c6d-145">Páginas de error de diagnóstico deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="23c6d-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="23c6d-146">Páginas de error descriptivo habilitadas.</span><span class="sxs-lookup"><span data-stu-id="23c6d-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="23c6d-147">El registro de producción y supervisión habilitada.</span><span class="sxs-lookup"><span data-stu-id="23c6d-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="23c6d-148">Por ejemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="23c6d-148">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="23c6d-149">Configurar el entorno</span><span class="sxs-lookup"><span data-stu-id="23c6d-149">Setting the environment</span></span>

<span data-ttu-id="23c6d-150">A menudo resulta útil establecer un entorno de pruebas específico.</span><span class="sxs-lookup"><span data-stu-id="23c6d-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="23c6d-151">Si no está configurado el entorno, serán `Production` que deshabilita la mayoría de características de depuración.</span><span class="sxs-lookup"><span data-stu-id="23c6d-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="23c6d-152">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="23c6d-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="23c6d-153">Azure</span><span class="sxs-lookup"><span data-stu-id="23c6d-153">Azure</span></span>

<span data-ttu-id="23c6d-154">Para el servicio de aplicaciones de Azure:</span><span class="sxs-lookup"><span data-stu-id="23c6d-154">For Azure app service:</span></span>

* <span data-ttu-id="23c6d-155">Seleccione el **configuración de la aplicación** hoja.</span><span class="sxs-lookup"><span data-stu-id="23c6d-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="23c6d-156">Agregar la clave y el valor de **configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="23c6d-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="23c6d-157">Windows</span><span class="sxs-lookup"><span data-stu-id="23c6d-157">Windows</span></span>
<span data-ttu-id="23c6d-158">Para establecer el `ASPNETCORE_ENVIRONMENT` para la sesión actual, si la aplicación se inicia con `dotnet run`, se utilizan los siguientes comandos</span><span class="sxs-lookup"><span data-stu-id="23c6d-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="23c6d-159">**Línea de comandos**</span><span class="sxs-lookup"><span data-stu-id="23c6d-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="23c6d-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="23c6d-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="23c6d-161">Estos comandos sólo tendrán efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="23c6d-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="23c6d-162">Cuando se cierra la ventana, la configuración de ASPNETCORE_ENVIRONMENT vuelve a la configuración predeterminada o el valor de la máquina.</span><span class="sxs-lookup"><span data-stu-id="23c6d-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="23c6d-163">Para establecer el valor global en ventanas abiertas del **el Panel de Control** > **System** > **configuración avanzada del sistema** y agregar o editar la `ASPNETCORE_ENVIRONMENT` valor.</span><span class="sxs-lookup"><span data-stu-id="23c6d-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="23c6d-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="23c6d-166">**web.config**</span></span>

<span data-ttu-id="23c6d-167">Consulte la *establecer variables de entorno* sección de la [referencia de configuración de ASP.NET Core módulo](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tema.</span><span class="sxs-lookup"><span data-stu-id="23c6d-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="23c6d-168">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="23c6d-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="23c6d-169">Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aisladas (se admite en IIS 10.0 +), consulte el *comandos AppCmd.exe* sección de la [Variables de entorno \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tema.</span><span class="sxs-lookup"><span data-stu-id="23c6d-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="23c6d-170">macOS</span><span class="sxs-lookup"><span data-stu-id="23c6d-170">macOS</span></span>
<span data-ttu-id="23c6d-171">Configurar el entorno actual para macOS puede realizarse en línea cuando se ejecuta la aplicación;</span><span class="sxs-lookup"><span data-stu-id="23c6d-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="23c6d-172">o con `export` establecer antes de ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c6d-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="23c6d-173">Las variables de entorno de nivel de equipo se establecen en los *.bashrc* o *. bash_profile* archivo.</span><span class="sxs-lookup"><span data-stu-id="23c6d-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="23c6d-174">Edite el archivo con cualquier editor de texto y agregue la siguiente instrucción.</span><span class="sxs-lookup"><span data-stu-id="23c6d-174">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="23c6d-175">Linux</span><span class="sxs-lookup"><span data-stu-id="23c6d-175">Linux</span></span>
<span data-ttu-id="23c6d-176">Para distribuciones de Linux, use la `export` comando en la línea de comandos para la configuración de la variable basado en sesiones y *bash_profile* archivo de configuración del entorno de nivel de máquina.</span><span class="sxs-lookup"><span data-stu-id="23c6d-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="23c6d-177">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="23c6d-177">Configuration by environment</span></span>

<span data-ttu-id="23c6d-178">Vea [configuración entorno](xref:fundamentals/configuration/index#configuration-by-environment) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="23c6d-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="23c6d-179">Entorno en función de métodos y la clase de inicio</span><span class="sxs-lookup"><span data-stu-id="23c6d-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="23c6d-180">Cuando se inicia una aplicación de ASP.NET Core, el [clase inicio](xref:fundamentals/startup) ejecuta un bootstrap la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c6d-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="23c6d-181">Si una clase `Startup{EnvironmentName}` existe, que se llamará para esa clase `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="23c6d-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="23c6d-182">Nota: Al llamar a [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) invalida las secciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="23c6d-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="23c6d-183">[Configurar](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) compatible con versiones específicas del entorno del formulario `Configure{EnvironmentName}` y `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="23c6d-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="23c6d-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="23c6d-184">Additional Resources</span></span>

* [<span data-ttu-id="23c6d-185">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="23c6d-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="23c6d-186">Configuración</span><span class="sxs-lookup"><span data-stu-id="23c6d-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="23c6d-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="23c6d-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
