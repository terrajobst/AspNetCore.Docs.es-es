---
title: Trabajar con varios entornos de ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de cómo ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos."
keywords: "Núcleo de ASP.NET, la configuración del entorno, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="e7661-104">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="e7661-104">Working with multiple environments</span></span>

<span data-ttu-id="e7661-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e7661-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e7661-106">ASP.NET Core proporciona compatibilidad para establecer el comportamiento de la aplicación en tiempo de ejecución con las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="e7661-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="e7661-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e7661-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="e7661-108">Entornos</span><span class="sxs-lookup"><span data-stu-id="e7661-108">Environments</span></span>

<span data-ttu-id="e7661-109">ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena ese valor en [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="e7661-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="e7661-110">`ASPNETCORE_ENVIRONMENT`se puede establecer en cualquier valor, pero [tres valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) son compatibles con el marco de trabajo: [desarrollo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [ensayo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), y [producción](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e7661-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="e7661-111">Si `ASPNETCORE_ENVIRONMENT` no se establece, el valor predeterminado será `Production`.</span><span class="sxs-lookup"><span data-stu-id="e7661-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="e7661-112">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="e7661-112">The preceding code:</span></span>

* <span data-ttu-id="e7661-113">Llamadas [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.</span><span class="sxs-lookup"><span data-stu-id="e7661-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="e7661-114">Llamadas [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando el valor de `ASPNETCORE_ENVIRONMENT` se establece uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="e7661-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="e7661-115">El [auxiliar de etiquetas de entorno ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utiliza el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:</span><span class="sxs-lookup"><span data-stu-id="e7661-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="e7661-116">Nota: En Windows y Mac OS, valores y variables de entorno no son distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="e7661-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="e7661-117">Las variables de entorno de Linux y los valores son **entre mayúsculas y minúsculas** de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e7661-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="e7661-118">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="e7661-118">Development</span></span>

<span data-ttu-id="e7661-119">El entorno de desarrollo puede habilitar las características que no deben exponerse en producción.</span><span class="sxs-lookup"><span data-stu-id="e7661-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="e7661-120">Por ejemplo, las plantillas de ASP.NET Core permiten la [página de excepción para desarrolladores](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="e7661-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="e7661-121">El entorno de desarrollo de equipo local se puede establecer el *Properties\launchSettings.json* archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e7661-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="e7661-122">Los valores de entorno establecer *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="e7661-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="e7661-123">El siguiente XML muestra tres perfiles de un *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="e7661-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="e7661-124">Cuando se inicia la aplicación con `dotnet run`, el primer perfil con `"commandName": "Project"` se usará.</span><span class="sxs-lookup"><span data-stu-id="e7661-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="e7661-125">El valor de `commandName` especifica el servidor web para iniciar.</span><span class="sxs-lookup"><span data-stu-id="e7661-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="e7661-126">`commandName`puede ser uno de:</span><span class="sxs-lookup"><span data-stu-id="e7661-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="e7661-127">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e7661-127">IIS Express</span></span>
* <span data-ttu-id="e7661-128">IIS</span><span class="sxs-lookup"><span data-stu-id="e7661-128">IIS</span></span>
* <span data-ttu-id="e7661-129">Proyecto (que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="e7661-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="e7661-130">Cuando se inicia una aplicación con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="e7661-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="e7661-131">*launchSettings.json* se lee si está disponible.</span><span class="sxs-lookup"><span data-stu-id="e7661-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="e7661-132">`environmentVariables`configuración de *launchSettings.json* reemplazan a las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="e7661-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="e7661-133">Se muestra el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="e7661-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="e7661-134">La siguiente salida muestra una aplicación que se inició con `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="e7661-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="e7661-135">Visual Studio **depurar** ficha proporciona una interfaz gráfica de usuario para editar la *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="e7661-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables de entorno de configuración de propiedades de proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="e7661-137">Los cambios realizados en los perfiles de los proyectos podrán no surtan efecto hasta que se reinicie el servidor web.</span><span class="sxs-lookup"><span data-stu-id="e7661-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="e7661-138">Kestrel debe reiniciarse para que se detectarán los cambios realizados en su entorno.</span><span class="sxs-lookup"><span data-stu-id="e7661-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="e7661-139">*launchSettings.json* no debería almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="e7661-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="e7661-140">El [herramienta Administrador de secreto](xref:security/app-secrets) puede usarse para almacenar secretos de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="e7661-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="e7661-141">Producción</span><span class="sxs-lookup"><span data-stu-id="e7661-141">Production</span></span>

<span data-ttu-id="e7661-142">El entorno de producción debe configurarse para maximizar la seguridad, rendimiento y eficacia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e7661-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="e7661-143">Algunas configuraciones comunes que podría tener un entorno de producción que serían diferentes entornos de desarrollo incluyen:</span><span class="sxs-lookup"><span data-stu-id="e7661-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="e7661-144">Almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="e7661-144">Caching.</span></span>
* <span data-ttu-id="e7661-145">Recursos del cliente están agrupados, reducidos y potencialmente provienen de una CDN.</span><span class="sxs-lookup"><span data-stu-id="e7661-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="e7661-146">Páginas de error de diagnóstico deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="e7661-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="e7661-147">Páginas de error descriptivo habilitadas.</span><span class="sxs-lookup"><span data-stu-id="e7661-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="e7661-148">El registro de producción y supervisión habilitada.</span><span class="sxs-lookup"><span data-stu-id="e7661-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="e7661-149">Por ejemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="e7661-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="e7661-150">Configurar el entorno</span><span class="sxs-lookup"><span data-stu-id="e7661-150">Setting the environment</span></span>

<span data-ttu-id="e7661-151">A menudo resulta útil establecer un entorno de pruebas específico.</span><span class="sxs-lookup"><span data-stu-id="e7661-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="e7661-152">Si no está configurado el entorno, serán `Production` que deshabilita la mayoría de características de depuración.</span><span class="sxs-lookup"><span data-stu-id="e7661-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="e7661-153">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="e7661-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="e7661-154">Azure</span><span class="sxs-lookup"><span data-stu-id="e7661-154">Azure</span></span>

<span data-ttu-id="e7661-155">Para el servicio de aplicaciones de Azure:</span><span class="sxs-lookup"><span data-stu-id="e7661-155">For Azure app service:</span></span>

* <span data-ttu-id="e7661-156">Seleccione el **configuración de la aplicación** hoja.</span><span class="sxs-lookup"><span data-stu-id="e7661-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="e7661-157">Agregar la clave y el valor de **configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="e7661-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="e7661-158">Windows</span><span class="sxs-lookup"><span data-stu-id="e7661-158">Windows</span></span>
<span data-ttu-id="e7661-159">Para establecer el `ASPNETCORE_ENVIRONMENT` para la sesión actual, si la aplicación se inicia con `dotnet run`, se utilizan los siguientes comandos</span><span class="sxs-lookup"><span data-stu-id="e7661-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="e7661-160">**Línea de comandos**</span><span class="sxs-lookup"><span data-stu-id="e7661-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="e7661-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="e7661-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="e7661-162">Estos comandos sólo tendrán efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="e7661-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="e7661-163">Cuando se cierra la ventana, la configuración de ASPNETCORE_ENVIRONMENT vuelve a la configuración predeterminada o el valor de la máquina.</span><span class="sxs-lookup"><span data-stu-id="e7661-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="e7661-164">Para establecer el valor global en ventanas abiertas del **el Panel de Control** > **System** > **configuración avanzada del sistema** y agregar o editar la `ASPNETCORE_ENVIRONMENT` valor.</span><span class="sxs-lookup"><span data-stu-id="e7661-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="e7661-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="e7661-167">**web.config**</span></span>

<span data-ttu-id="e7661-168">Consulte la *establecer variables de entorno* sección de la [referencia de configuración de ASP.NET Core módulo](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tema.</span><span class="sxs-lookup"><span data-stu-id="e7661-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="e7661-169">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="e7661-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="e7661-170">Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aisladas (se admite en IIS 10.0 +), consulte el *comandos AppCmd.exe* sección de la [Variables de entorno \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tema.</span><span class="sxs-lookup"><span data-stu-id="e7661-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="e7661-171">macOS</span><span class="sxs-lookup"><span data-stu-id="e7661-171">macOS</span></span>
<span data-ttu-id="e7661-172">Configurar el entorno actual para macOS puede realizarse en línea cuando se ejecuta la aplicación;</span><span class="sxs-lookup"><span data-stu-id="e7661-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="e7661-173">o con `export` establecer antes de ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e7661-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="e7661-174">Las variables de entorno de nivel de equipo se establecen en los *.bashrc* o *. bash_profile* archivo.</span><span class="sxs-lookup"><span data-stu-id="e7661-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="e7661-175">Edite el archivo con cualquier editor de texto y agregue la siguiente instrucción.</span><span class="sxs-lookup"><span data-stu-id="e7661-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="e7661-176">Linux</span><span class="sxs-lookup"><span data-stu-id="e7661-176">Linux</span></span>
<span data-ttu-id="e7661-177">Para distribuciones de Linux, use la `export` comando en la línea de comandos para la configuración de la variable basado en sesiones y *bash_profile* archivo de configuración del entorno de nivel de máquina.</span><span class="sxs-lookup"><span data-stu-id="e7661-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="e7661-178">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="e7661-178">Configuration by environment</span></span>

<span data-ttu-id="e7661-179">Vea [configuración entorno](xref:fundamentals/configuration/index#configuration-by-environment) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="e7661-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="e7661-180">Entorno en función de métodos y la clase de inicio</span><span class="sxs-lookup"><span data-stu-id="e7661-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="e7661-181">Cuando se inicia una aplicación de ASP.NET Core, el [clase inicio](xref:fundamentals/startup) ejecuta un bootstrap la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e7661-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="e7661-182">Si una clase `Startup{EnvironmentName}` existe, que se llamará para esa clase `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="e7661-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="e7661-183">Nota: Al llamar a [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) invalida las secciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="e7661-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="e7661-184">[Configurar](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) compatible con versiones específicas del entorno del formulario `Configure{EnvironmentName}` y `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="e7661-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="e7661-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e7661-185">Additional Resources</span></span>

* [<span data-ttu-id="e7661-186">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e7661-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="e7661-187">Configuración</span><span class="sxs-lookup"><span data-stu-id="e7661-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="e7661-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e7661-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
