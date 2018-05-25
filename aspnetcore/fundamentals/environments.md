---
title: Usar varios entornos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la manera en que ASP.NET Core proporciona compatibilidad para controlar el comportamiento de las aplicaciones en varios entornos.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="b61ce-103">Usar varios entornos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b61ce-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="b61ce-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b61ce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b61ce-105">ASP.NET Core proporciona compatibilidad para establecer el comportamiento de las aplicaciones en tiempo de ejecución con variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b61ce-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="b61ce-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b61ce-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="b61ce-107">Entornos</span><span class="sxs-lookup"><span data-stu-id="b61ce-107">Environments</span></span>

<span data-ttu-id="b61ce-108">ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena ese valor en [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="b61ce-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="b61ce-109">`ASPNETCORE_ENVIRONMENT` se puede establecer en cualquier valor, pero el marco de trabajo admite [tres valores](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) y [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="b61ce-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="b61ce-110">Si no se ha establecido `ASPNETCORE_ENVIRONMENT`, el valor predeterminado será `Production`.</span><span class="sxs-lookup"><span data-stu-id="b61ce-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="b61ce-111">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="b61ce-111">The preceding code:</span></span>

* <span data-ttu-id="b61ce-112">Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.</span><span class="sxs-lookup"><span data-stu-id="b61ce-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="b61ce-113">Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="b61ce-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="b61ce-114">La [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:</span><span class="sxs-lookup"><span data-stu-id="b61ce-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="b61ce-115">Nota: En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="b61ce-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="b61ce-116">Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b61ce-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="b61ce-117">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="b61ce-117">Development</span></span>

<span data-ttu-id="b61ce-118">El entorno de desarrollo puede habilitar características que no deben exponerse en producción.</span><span class="sxs-lookup"><span data-stu-id="b61ce-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="b61ce-119">Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="b61ce-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="b61ce-120">El entorno para el desarrollo del equipo local se puede establecer en el archivo *Properties\launchSettings.json* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b61ce-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="b61ce-121">Los valores de entorno establecidos en *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="b61ce-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="b61ce-122">En el siguiente fragmento de JSON se muestran tres perfiles de un archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b61ce-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="b61ce-123">La propiedad `applicationUrl` en *launchSettings.json* puede especificar una lista de direcciones URL del servidor.</span><span class="sxs-lookup"><span data-stu-id="b61ce-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="b61ce-124">Use un punto y coma entre las direcciones URL de la lista:</span><span class="sxs-lookup"><span data-stu-id="b61ce-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="b61ce-125">Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usará el primer perfil con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="b61ce-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="b61ce-126">El valor de `commandName` especifica el servidor web que se va a iniciar.</span><span class="sxs-lookup"><span data-stu-id="b61ce-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="b61ce-127">`commandName` puede ser una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="b61ce-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="b61ce-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="b61ce-128">IIS Express</span></span>
* <span data-ttu-id="b61ce-129">IIS</span><span class="sxs-lookup"><span data-stu-id="b61ce-129">IIS</span></span>
* <span data-ttu-id="b61ce-130">Project (que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="b61ce-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="b61ce-131">Cuando una aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="b61ce-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="b61ce-132">Se lee *launchSettings.json*, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="b61ce-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="b61ce-133">La configuración de `environmentVariables` de *launchSettings.json* reemplaza las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b61ce-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="b61ce-134">Se muestra el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="b61ce-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="b61ce-135">En la siguiente salida se muestra una aplicación que se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="b61ce-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="b61ce-136">La pestaña **Depurar** de Visual Studio proporciona una GUI para editar el archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b61ce-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables de entorno de configuración de las propiedades del proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="b61ce-138">Los cambios realizados en los perfiles de proyecto podrían no surtir efecto hasta que se reinicie el servidor web.</span><span class="sxs-lookup"><span data-stu-id="b61ce-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="b61ce-139">Es necesario reiniciar Kestrel para que detecte los cambios realizados en su entorno.</span><span class="sxs-lookup"><span data-stu-id="b61ce-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="b61ce-140">En *launchSettings.json* no se deben almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="b61ce-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="b61ce-141">Se puede usar la [herramienta Administrador de secretos](xref:security/app-secrets) a fin de almacenar secretos para el desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="b61ce-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="b61ce-142">Producción</span><span class="sxs-lookup"><span data-stu-id="b61ce-142">Production</span></span>

<span data-ttu-id="b61ce-143">El entorno de producción debe configurarse para maximizar la seguridad, el rendimiento y la solidez de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b61ce-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="b61ce-144">Las opciones de configuración comunes que difieren de las del entorno de desarrollo son:</span><span class="sxs-lookup"><span data-stu-id="b61ce-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="b61ce-145">Almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="b61ce-145">Caching.</span></span>
* <span data-ttu-id="b61ce-146">Los recursos del cliente se agrupan, se reducen y se atienden potencialmente desde una CDN.</span><span class="sxs-lookup"><span data-stu-id="b61ce-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="b61ce-147">Las páginas de error de diagnóstico están deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="b61ce-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="b61ce-148">Las páginas de error descriptivas están habilitadas.</span><span class="sxs-lookup"><span data-stu-id="b61ce-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="b61ce-149">El registro y la supervisión de la producción están habilitados.</span><span class="sxs-lookup"><span data-stu-id="b61ce-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="b61ce-150">Por ejemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="b61ce-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="b61ce-151">Establecimiento del entorno</span><span class="sxs-lookup"><span data-stu-id="b61ce-151">Setting the environment</span></span>

<span data-ttu-id="b61ce-152">A menudo resulta útil establecer un entorno específico para la realización de pruebas.</span><span class="sxs-lookup"><span data-stu-id="b61ce-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="b61ce-153">Si el entorno no está establecido, el valor predeterminado será `Production`, lo que deshabilita la mayoría de las características de depuración.</span><span class="sxs-lookup"><span data-stu-id="b61ce-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="b61ce-154">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="b61ce-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="b61ce-155">Azure</span><span class="sxs-lookup"><span data-stu-id="b61ce-155">Azure</span></span>

<span data-ttu-id="b61ce-156">Para Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="b61ce-156">For Azure app service:</span></span>

* <span data-ttu-id="b61ce-157">Seleccione la hoja **Configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="b61ce-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="b61ce-158">Agregue la clave y el valor en **Configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="b61ce-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="b61ce-159">Windows</span><span class="sxs-lookup"><span data-stu-id="b61ce-159">Windows</span></span>
<span data-ttu-id="b61ce-160">Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, si la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b61ce-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="b61ce-161">**Línea de comandos**</span><span class="sxs-lookup"><span data-stu-id="b61ce-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="b61ce-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="b61ce-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="b61ce-163">Estos comandos solo tienen efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="b61ce-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="b61ce-164">Cuando se cierre la ventana, la configuración de ASPNETCORE_ENVIRONMENT volverá a la configuración predeterminada o al valor del equipo.</span><span class="sxs-lookup"><span data-stu-id="b61ce-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="b61ce-165">Para establecer el valor globalmente en Windows, abra **Panel de Control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="b61ce-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="b61ce-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="b61ce-168">**web.config**</span></span>

<span data-ttu-id="b61ce-169">Vea la sección *Establecer variables de entorno* del tema [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="b61ce-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="b61ce-170">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="b61ce-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="b61ce-171">Para establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *AppCmd.exe command* (Comando AppCmd.exe) del tema [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno <environmentVariables>).</span><span class="sxs-lookup"><span data-stu-id="b61ce-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="b61ce-172">macOS</span><span class="sxs-lookup"><span data-stu-id="b61ce-172">macOS</span></span>
<span data-ttu-id="b61ce-173">Para establecer el entorno actual para macOS, puede hacerlo en línea al ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b61ce-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="b61ce-174">También puede usar `export` para establecerlo antes de ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b61ce-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="b61ce-175">Las variables de entorno de nivel de equipo se establecen en el archivo *.bashrc* o *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="b61ce-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="b61ce-176">Para editar el archivo, use cualquier editor de texto y agregue la instrucción siguiente.</span><span class="sxs-lookup"><span data-stu-id="b61ce-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="b61ce-177">Linux</span><span class="sxs-lookup"><span data-stu-id="b61ce-177">Linux</span></span>
<span data-ttu-id="b61ce-178">Para distribuciones de Linux, use el comando `export` en la línea de comandos para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.</span><span class="sxs-lookup"><span data-stu-id="b61ce-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="b61ce-179">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="b61ce-179">Configuration by environment</span></span>

<span data-ttu-id="b61ce-180">Para más información, vea [Configuración de entorno](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="b61ce-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="b61ce-181">Métodos y clase Startup basados en entorno</span><span class="sxs-lookup"><span data-stu-id="b61ce-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="b61ce-182">Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b61ce-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="b61ce-183">Si existe una clase `Startup{EnvironmentName}`, se llamará para ese `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="b61ce-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="b61ce-184">Nota: Al llamar a [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) se invalidan las secciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="b61ce-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="b61ce-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) son compatibles con versiones específicas del entorno con el formato `Configure{EnvironmentName}` y `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="b61ce-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="b61ce-186">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b61ce-186">Additional resources</span></span>

* [<span data-ttu-id="b61ce-187">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="b61ce-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="b61ce-188">Configuración</span><span class="sxs-lookup"><span data-stu-id="b61ce-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="b61ce-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="b61ce-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
