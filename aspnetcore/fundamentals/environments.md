---
title: Usar varios entornos en ASP.NET Core
author: rick-anderson
description: Aprenda a controlar el comportamiento de las aplicaciones en varios entornos en aplicaciones ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: de3c3fd5a2f0e49366d9d5b4e992d0247bcab0e5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577528"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="37ba5-103">Usar varios entornos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37ba5-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="37ba5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37ba5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="37ba5-105">ASP.NET Core configura el comportamiento de las aplicaciones en función del entorno en tiempo de ejecución mediante una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="37ba5-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="37ba5-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37ba5-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="37ba5-107">Entornos</span><span class="sxs-lookup"><span data-stu-id="37ba5-107">Environments</span></span>

<span data-ttu-id="37ba5-108">ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena el valor en [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="37ba5-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="37ba5-109">Puede establecer `ASPNETCORE_ENVIRONMENT` en cualquier valor, pero el marco de trabajo admite [tres valores](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) y [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="37ba5-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="37ba5-110">Si no se establece `ASPNETCORE_ENVIRONMENT`, el valor predeterminado es `Production`.</span><span class="sxs-lookup"><span data-stu-id="37ba5-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="37ba5-111">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="37ba5-111">The preceding code:</span></span>

* <span data-ttu-id="37ba5-112">Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.</span><span class="sxs-lookup"><span data-stu-id="37ba5-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="37ba5-113">Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="37ba5-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="37ba5-114">El [asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:</span><span class="sxs-lookup"><span data-stu-id="37ba5-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="37ba5-115">En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="37ba5-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="37ba5-116">Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="37ba5-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="37ba5-117">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="37ba5-117">Development</span></span>

<span data-ttu-id="37ba5-118">El entorno de desarrollo puede habilitar características que no deben exponerse en producción.</span><span class="sxs-lookup"><span data-stu-id="37ba5-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="37ba5-119">Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#the-developer-exception-page) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="37ba5-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="37ba5-120">El entorno para el desarrollo del equipo local se puede establecer en el archivo *Properties\launchSettings.json* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="37ba5-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="37ba5-121">Los valores de entorno establecidos en *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="37ba5-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="37ba5-122">En el siguiente fragmento de JSON se muestran tres perfiles de un archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="37ba5-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="37ba5-123">La propiedad `applicationUrl` en *launchSettings.json* puede especificar una lista de direcciones URL del servidor.</span><span class="sxs-lookup"><span data-stu-id="37ba5-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="37ba5-124">Use un punto y coma entre las direcciones URL de la lista:</span><span class="sxs-lookup"><span data-stu-id="37ba5-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="37ba5-125">Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usa el primer perfil con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="37ba5-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="37ba5-126">El valor de `commandName` especifica el servidor web que se va a iniciar.</span><span class="sxs-lookup"><span data-stu-id="37ba5-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="37ba5-127">`commandName` puede ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="37ba5-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="37ba5-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="37ba5-128">IIS Express</span></span>
* <span data-ttu-id="37ba5-129">IIS</span><span class="sxs-lookup"><span data-stu-id="37ba5-129">IIS</span></span>
* <span data-ttu-id="37ba5-130">Project (que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="37ba5-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="37ba5-131">Cuando una aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="37ba5-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="37ba5-132">Se lee *launchSettings.json*, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="37ba5-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="37ba5-133">La configuración de `environmentVariables` de *launchSettings.json* reemplaza las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="37ba5-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="37ba5-134">Se muestra el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="37ba5-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="37ba5-135">En la siguiente salida se muestra una aplicación que se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="37ba5-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="37ba5-136">La pestaña **Depurar** de las propiedades de proyecto de Visual Studio proporciona una GUI para editar el archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="37ba5-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables de entorno de configuración de las propiedades del proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="37ba5-138">Los cambios realizados en los perfiles de proyecto podrían no surtir efecto hasta que se reinicie el servidor web.</span><span class="sxs-lookup"><span data-stu-id="37ba5-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="37ba5-139">Es necesario reiniciar Kestrel para que pueda detectar los cambios realizados en su entorno.</span><span class="sxs-lookup"><span data-stu-id="37ba5-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="37ba5-140">En *launchSettings.json* no se deben almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="37ba5-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="37ba5-141">Se puede usar la [herramienta Administrador de secretos](xref:security/app-secrets) a fin de almacenar secretos para el desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="37ba5-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="37ba5-142">Cuando se usa [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="37ba5-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="37ba5-143">En el siguiente ejemplo se establece el entorno en `Development`:</span><span class="sxs-lookup"><span data-stu-id="37ba5-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="37ba5-144">Un archivo *.vscode/launch.json* del proyecto no se lee al iniciar la aplicación con `dotnet run` en la misma manera que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="37ba5-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="37ba5-145">Cuando se inicia una aplicación en desarrollo que no tiene un archivo *launchSettings.json*, establezca el valor del entorno con una variable de entorno o con un argumento de línea de comandos para el comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="37ba5-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="37ba5-146">Producción</span><span class="sxs-lookup"><span data-stu-id="37ba5-146">Production</span></span>

<span data-ttu-id="37ba5-147">El entorno de producción debe configurarse para maximizar la seguridad, el rendimiento y la solidez de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37ba5-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="37ba5-148">Las opciones de configuración comunes que difieren de las del entorno de desarrollo son:</span><span class="sxs-lookup"><span data-stu-id="37ba5-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="37ba5-149">Almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="37ba5-149">Caching.</span></span>
* <span data-ttu-id="37ba5-150">Los recursos del cliente se agrupan, se reducen y se atienden potencialmente desde una CDN.</span><span class="sxs-lookup"><span data-stu-id="37ba5-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="37ba5-151">Las páginas de error de diagnóstico están deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="37ba5-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="37ba5-152">Las páginas de error descriptivas están habilitadas.</span><span class="sxs-lookup"><span data-stu-id="37ba5-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="37ba5-153">El registro y la supervisión de la producción están habilitados.</span><span class="sxs-lookup"><span data-stu-id="37ba5-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="37ba5-154">Por ejemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="37ba5-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="37ba5-155">Establecimiento del entorno</span><span class="sxs-lookup"><span data-stu-id="37ba5-155">Set the environment</span></span>

<span data-ttu-id="37ba5-156">A menudo resulta útil establecer un entorno específico para la realización de pruebas.</span><span class="sxs-lookup"><span data-stu-id="37ba5-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="37ba5-157">Si el entorno no está establecido, el valor predeterminado es `Production`, lo que deshabilita la mayoría de las características de depuración.</span><span class="sxs-lookup"><span data-stu-id="37ba5-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="37ba5-158">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="37ba5-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="37ba5-159">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="37ba5-159">Azure App Service</span></span>

<span data-ttu-id="37ba5-160">Para establecer el entorno en [Azure App Service](https://azure.microsoft.com/services/app-service/), realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="37ba5-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="37ba5-161">Seleccione la aplicación desde la hoja **App Services**.</span><span class="sxs-lookup"><span data-stu-id="37ba5-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="37ba5-162">En el grupo **CONFIGURACIÓN**, seleccione la hoja **Configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="37ba5-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="37ba5-163">En el área **Configuración de la aplicación**, haga clic en **Agregar nueva configuración**.</span><span class="sxs-lookup"><span data-stu-id="37ba5-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="37ba5-164">Para **Escriba un nombre**, proporcione `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="37ba5-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="37ba5-165">Para **Escriba un valor**, proporcione el entorno (por ejemplo, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="37ba5-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="37ba5-166">Active la casilla **Configuración de ranuras** si quiere que la configuración del entorno permanezca con la ranura actual cuando se intercambien las ranuras de implementación.</span><span class="sxs-lookup"><span data-stu-id="37ba5-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="37ba5-167">Para obtener más información, vea [Configuración de entornos de ensayo en Azure App Service](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="37ba5-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="37ba5-168">Haga clic en **Guardar** en la parte superior de la hoja.</span><span class="sxs-lookup"><span data-stu-id="37ba5-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="37ba5-169">Azure App Service reinicia automáticamente la aplicación después de que se agregue, cambie o elimine una configuración de aplicación (variable de entorno) en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="37ba5-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="37ba5-170">Windows</span><span class="sxs-lookup"><span data-stu-id="37ba5-170">Windows</span></span>

<span data-ttu-id="37ba5-171">Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, cuando la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="37ba5-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="37ba5-172">**Símbolo del sistema**</span><span class="sxs-lookup"><span data-stu-id="37ba5-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="37ba5-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="37ba5-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="37ba5-174">Estos comandos solo tienen efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="37ba5-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="37ba5-175">Cuando se cierre la ventana, la configuración de `ASPNETCORE_ENVIRONMENT` volverá a la configuración predeterminada o al valor del equipo.</span><span class="sxs-lookup"><span data-stu-id="37ba5-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="37ba5-176">Para establecer el valor globalmente en Windows, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="37ba5-176">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="37ba5-177">Abra **Panel de control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="37ba5-177">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

  ![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="37ba5-180">Abra un símbolo del sistema con permisos de administrador y use el comando `setx` o abra un símbolo del sistema administrativo de PowerShell y use `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="37ba5-180">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="37ba5-181">**Símbolo del sistema**</span><span class="sxs-lookup"><span data-stu-id="37ba5-181">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="37ba5-182">El modificador `/M` indica que hay que establecer la variable de entorno en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="37ba5-182">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="37ba5-183">Si no se usa el modificador `/M`, la variable de entorno se establece para la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="37ba5-183">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="37ba5-184">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="37ba5-184">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="37ba5-185">El valor de opción `Machine` indica que hay que establecer la variable de entorno en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="37ba5-185">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="37ba5-186">Si el valor de opción se cambia a `User`, la variable de entorno se establece para la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="37ba5-186">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="37ba5-187">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece de forma global, se aplica a `dotnet run` en cualquier ventana de comandos abierta después del establecimiento del valor.</span><span class="sxs-lookup"><span data-stu-id="37ba5-187">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="37ba5-188">**web.config**</span><span class="sxs-lookup"><span data-stu-id="37ba5-188">**web.config**</span></span>

<span data-ttu-id="37ba5-189">Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` con *web.config*, vea la sección *Establecimiento de variables de entorno* de <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="37ba5-189">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span> <span data-ttu-id="37ba5-190">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece con *web.config*, su valor reemplaza a un valor en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="37ba5-190">When the `ASPNETCORE_ENVIRONMENT` environment variable is set with *web.config*, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="37ba5-191">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="37ba5-191">**Per IIS Application Pool**</span></span>

<span data-ttu-id="37ba5-192">Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` para una aplicación que se ejecuta en un grupo de aplicaciones aislado (se admite en IIS 10.0 o posterior), vea la sección *AppCmd.exe* del tema [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno).</span><span class="sxs-lookup"><span data-stu-id="37ba5-192">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="37ba5-193">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece para un grupo de aplicaciones, su valor reemplaza a un valor en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="37ba5-193">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37ba5-194">Cuando hospede una aplicación en IIS y agregue o cambie la variable de entorno `ASPNETCORE_ENVIRONMENT`, use cualquiera de los siguientes métodos para que las aplicaciones tomen el nuevo valor:</span><span class="sxs-lookup"><span data-stu-id="37ba5-194">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="37ba5-195">Ejecute `net stop was /y` seguido de `net start w3svc` en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="37ba5-195">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="37ba5-196">Reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="37ba5-196">Restart the server.</span></span>

### <a name="macos"></a><span data-ttu-id="37ba5-197">macOS</span><span class="sxs-lookup"><span data-stu-id="37ba5-197">macOS</span></span>

<span data-ttu-id="37ba5-198">Para establecer el entorno actual para macOS, puede hacerlo en línea al ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="37ba5-198">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="37ba5-199">De forma alternativa, defina el entorno con `export` antes de ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="37ba5-199">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="37ba5-200">Las variables de entorno de nivel de equipo se establecen en el archivo *.bashrc* o *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="37ba5-200">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="37ba5-201">Edite el archivo con cualquier editor de texto.</span><span class="sxs-lookup"><span data-stu-id="37ba5-201">Edit the file using any text editor.</span></span> <span data-ttu-id="37ba5-202">Agregue la siguiente instrucción:</span><span class="sxs-lookup"><span data-stu-id="37ba5-202">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="37ba5-203">Linux</span><span class="sxs-lookup"><span data-stu-id="37ba5-203">Linux</span></span>

<span data-ttu-id="37ba5-204">Para distribuciones de Linux, use el comando `export` en un símbolo del sistema para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.</span><span class="sxs-lookup"><span data-stu-id="37ba5-204">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="37ba5-205">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="37ba5-205">Configuration by environment</span></span>

<span data-ttu-id="37ba5-206">Para cargar la configuración por entorno, se recomienda lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37ba5-206">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="37ba5-207">Archivos *appSettings* (\*appsettings.&lt;<Environment>&gt;.json).</span><span class="sxs-lookup"><span data-stu-id="37ba5-207">*appsettings* files (\*appsettings.&lt;<Environment>&gt;.json).</span></span> <span data-ttu-id="37ba5-208">Vea [Configuración: proveedor de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="37ba5-208">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider).</span></span>
* <span data-ttu-id="37ba5-209">Variables de entorno (establecidas en todos los sistemas donde se hospede la aplicación).</span><span class="sxs-lookup"><span data-stu-id="37ba5-209">environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="37ba5-210">Vea [Configuración: proveedor de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider) y [Almacenamiento seguro de secretos de aplicación en desarrollo: variables de entorno](xref:security/app-secrets#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="37ba5-210">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider) and [Safe storage of app secrets in development: Environment variables](xref:security/app-secrets#environment-variables).</span></span>
* <span data-ttu-id="37ba5-211">Administrador de secretos (solo en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="37ba5-211">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="37ba5-212">Vea <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="37ba5-212">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="37ba5-213">Métodos y clase Startup basados en entorno</span><span class="sxs-lookup"><span data-stu-id="37ba5-213">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="37ba5-214">Convenciones de la clase Startup</span><span class="sxs-lookup"><span data-stu-id="37ba5-214">Startup class conventions</span></span>

<span data-ttu-id="37ba5-215">Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37ba5-215">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="37ba5-216">La aplicación puede definir clases `Startup` independientes para distintos entornos (por ejemplo, `StartupDevelopment`) y la clase `Startup` correspondiente se selecciona en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="37ba5-216">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="37ba5-217">La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria.</span><span class="sxs-lookup"><span data-stu-id="37ba5-217">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="37ba5-218">Si no se encuentra una clase `Startup{EnvironmentName}` coincidente, se usa la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="37ba5-218">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="37ba5-219">Para implementar clases `Startup` basadas en entornos, cree una clase `Startup{EnvironmentName}` para cada entorno en uso y una clase `Startup` de reserva:</span><span class="sxs-lookup"><span data-stu-id="37ba5-219">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

<span data-ttu-id="37ba5-220">Use la sobrecarga [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) que acepta un nombre de ensamblado:</span><span class="sxs-lookup"><span data-stu-id="37ba5-220">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a><span data-ttu-id="37ba5-221">Convenciones del método Startup</span><span class="sxs-lookup"><span data-stu-id="37ba5-221">Startup method conventions</span></span>

<span data-ttu-id="37ba5-222">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) son compatibles con versiones específicas del entorno con el formato `Configure<EnvironmentName>` y `Configure<EnvironmentName>Services`:</span><span class="sxs-lookup"><span data-stu-id="37ba5-222">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="37ba5-223">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37ba5-223">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="37ba5-224">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="37ba5-224">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
