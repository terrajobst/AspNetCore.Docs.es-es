---
title: Usar varios entornos en ASP.NET Core
author: rick-anderson
description: Aprenda a controlar el comportamiento de las aplicaciones en varios entornos en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: b0218b2c77c283c0849dca9491046534b88c5a77
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78645359"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="5ee57-103">Usar varios entornos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ee57-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5ee57-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5ee57-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5ee57-105">ASP.NET Core configura el comportamiento de las aplicaciones en función del entorno en tiempo de ejecución mediante una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="5ee57-106">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5ee57-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="5ee57-107">Entornos</span><span class="sxs-lookup"><span data-stu-id="5ee57-107">Environments</span></span>

<span data-ttu-id="5ee57-108">ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena el valor en [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="5ee57-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="5ee57-109">`ASPNETCORE_ENVIRONMENT` se puede establecer en cualquier valor, pero el marco proporciona tres valores:</span><span class="sxs-lookup"><span data-stu-id="5ee57-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="5ee57-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="5ee57-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="5ee57-111">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="5ee57-111">The preceding code:</span></span>

* <span data-ttu-id="5ee57-112">Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="5ee57-113">Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="5ee57-114">El [asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:</span><span class="sxs-lookup"><span data-stu-id="5ee57-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="5ee57-115">En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5ee57-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="5ee57-116">Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5ee57-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="5ee57-117">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="5ee57-117">Development</span></span>

<span data-ttu-id="5ee57-118">El entorno de desarrollo puede habilitar características que no deben exponerse en producción.</span><span class="sxs-lookup"><span data-stu-id="5ee57-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="5ee57-119">Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#developer-exception-page) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="5ee57-120">El entorno para el desarrollo del equipo local se puede establecer en el archivo *Properties\launchSettings.json* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5ee57-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="5ee57-121">Los valores de entorno establecidos en *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="5ee57-122">En el siguiente fragmento de JSON se muestran tres perfiles de un archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5ee57-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="5ee57-123">La propiedad `applicationUrl` en *launchSettings.json* puede especificar una lista de direcciones URL del servidor.</span><span class="sxs-lookup"><span data-stu-id="5ee57-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="5ee57-124">Use un punto y coma entre las direcciones URL de la lista:</span><span class="sxs-lookup"><span data-stu-id="5ee57-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="5ee57-125">Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usa el primer perfil con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="5ee57-126">El valor de `commandName` especifica el servidor web que se va a iniciar.</span><span class="sxs-lookup"><span data-stu-id="5ee57-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="5ee57-127">`commandName` puede ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="5ee57-128">`Project` (que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="5ee57-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="5ee57-129">Cuando una aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="5ee57-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="5ee57-130">Se lee *launchSettings.json*, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="5ee57-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="5ee57-131">La configuración de `environmentVariables` de *launchSettings.json* reemplaza las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="5ee57-132">Se muestra el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="5ee57-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="5ee57-133">En la siguiente salida se muestra una aplicación que se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="5ee57-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="5ee57-134">La pestaña **Depurar** de las propiedades de proyecto de Visual Studio proporciona una GUI para editar el archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5ee57-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables de entorno de configuración de las propiedades del proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="5ee57-136">Los cambios realizados en los perfiles de proyecto podrían no surtir efecto hasta que se reinicie el servidor web.</span><span class="sxs-lookup"><span data-stu-id="5ee57-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="5ee57-137">Es necesario reiniciar Kestrel para que pueda detectar los cambios realizados en su entorno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="5ee57-138">En *launchSettings.json* no se deben almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="5ee57-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="5ee57-139">Se puede usar la [herramienta Administrador de secretos](xref:security/app-secrets) a fin de almacenar secretos para el desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="5ee57-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="5ee57-140">Cuando se usa [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="5ee57-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="5ee57-141">En el siguiente ejemplo se establece el entorno en `Development`:</span><span class="sxs-lookup"><span data-stu-id="5ee57-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="5ee57-142">Un archivo *.vscode/launch.json* del proyecto no se lee al iniciar la aplicación con `dotnet run` en la misma manera que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5ee57-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="5ee57-143">Cuando se inicia una aplicación en desarrollo que no tiene un archivo *launchSettings.json*, establezca el valor del entorno con una variable de entorno o con un argumento de línea de comandos para el comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="5ee57-144">Producción</span><span class="sxs-lookup"><span data-stu-id="5ee57-144">Production</span></span>

<span data-ttu-id="5ee57-145">El entorno de producción debe configurarse para maximizar la seguridad, el rendimiento y la solidez de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="5ee57-146">Las opciones de configuración comunes que difieren de las del entorno de desarrollo son:</span><span class="sxs-lookup"><span data-stu-id="5ee57-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="5ee57-147">Almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="5ee57-147">Caching.</span></span>
* <span data-ttu-id="5ee57-148">Los recursos del cliente se agrupan, se reducen y se atienden potencialmente desde una CDN.</span><span class="sxs-lookup"><span data-stu-id="5ee57-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="5ee57-149">Las páginas de error de diagnóstico están deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="5ee57-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="5ee57-150">Las páginas de error descriptivas están habilitadas.</span><span class="sxs-lookup"><span data-stu-id="5ee57-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="5ee57-151">El registro y la supervisión de la producción están habilitados.</span><span class="sxs-lookup"><span data-stu-id="5ee57-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="5ee57-152">Por ejemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="5ee57-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="5ee57-153">Establecimiento del entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-153">Set the environment</span></span>

<span data-ttu-id="5ee57-154">A menudo resulta útil establecer un entorno específico para realizar las pruebas con una variable de entorno o una configuración de plataforma.</span><span class="sxs-lookup"><span data-stu-id="5ee57-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="5ee57-155">Si el entorno no está establecido, el valor predeterminado es `Production`, lo que deshabilita la mayoría de las características de depuración.</span><span class="sxs-lookup"><span data-stu-id="5ee57-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="5ee57-156">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="5ee57-157">Cuando se compila el host, la última configuración de entorno leída por la aplicación determina el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="5ee57-158">El entorno de la aplicación no se puede cambiar mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="5ee57-159">Configuración de plataforma o variable de entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="5ee57-160">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5ee57-160">Azure App Service</span></span>

<span data-ttu-id="5ee57-161">Para establecer el entorno en [Azure App Service](https://azure.microsoft.com/services/app-service/), realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="5ee57-162">Seleccione la aplicación desde la hoja **App Services**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="5ee57-163">En el grupo **Configuración**, seleccione la hoja **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-163">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="5ee57-164">En la pestaña **Configuración de aplicaciones**, seleccione **Nueva configuración de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-164">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="5ee57-165">En la ventana **Agregar o editar la configuración de la aplicación**, escriba `ASPNETCORE_ENVIRONMENT` para el **Nombre**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-165">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="5ee57-166">En **Valor**, proporcione el entorno (por ejemplo, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="5ee57-166">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="5ee57-167">Active la casilla **Configuración de ranura de implementación** si quiere que la configuración del entorno permanezca con la ranura actual cuando se intercambien las ranuras de implementación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-167">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="5ee57-168">Para más información, consulte [Configuración de entornos de ensayo en Azure App Service](/azure/app-service/web-sites-staged-publishing) en la documentación de Azure.</span><span class="sxs-lookup"><span data-stu-id="5ee57-168">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="5ee57-169">Seleccione **Aceptar** para cerrar la ventana **Agregar o editar la configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-169">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="5ee57-170">Seleccione **Guardar** en la parte superior de la hoja **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-170">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="5ee57-171">Azure App Service reinicia automáticamente la aplicación después de que se agregue, cambie o elimine una configuración de aplicación (variable de entorno) en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5ee57-171">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="5ee57-172">Windows</span><span class="sxs-lookup"><span data-stu-id="5ee57-172">Windows</span></span>

<span data-ttu-id="5ee57-173">Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, cuando la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-173">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="5ee57-174">**Símbolo del sistema**</span><span class="sxs-lookup"><span data-stu-id="5ee57-174">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="5ee57-175">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="5ee57-175">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="5ee57-176">Estos comandos solo tienen efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="5ee57-176">These commands only take effect for the current window.</span></span> <span data-ttu-id="5ee57-177">Cuando se cierre la ventana, la configuración de `ASPNETCORE_ENVIRONMENT` volverá a la configuración predeterminada o al valor del equipo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-177">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="5ee57-178">Para establecer el valor globalmente en Windows, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-178">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="5ee57-179">Abra el **Panel de control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="5ee57-179">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

  ![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="5ee57-182">Abra un símbolo del sistema con permisos de administrador y use el comando `setx` o abra un símbolo del sistema administrativo de PowerShell y use `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="5ee57-182">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="5ee57-183">**Símbolo del sistema**</span><span class="sxs-lookup"><span data-stu-id="5ee57-183">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="5ee57-184">El modificador `/M` indica que hay que establecer la variable de entorno en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-184">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="5ee57-185">Si no se usa el modificador `/M`, la variable de entorno se establece para la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="5ee57-185">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="5ee57-186">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="5ee57-186">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="5ee57-187">El valor de opción `Machine` indica que hay que establecer la variable de entorno en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-187">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="5ee57-188">Si el valor de opción se cambia a `User`, la variable de entorno se establece para la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="5ee57-188">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="5ee57-189">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece de forma global, se aplica a `dotnet run` en cualquier ventana de comandos abierta después del establecimiento del valor.</span><span class="sxs-lookup"><span data-stu-id="5ee57-189">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="5ee57-190">**web.config**</span><span class="sxs-lookup"><span data-stu-id="5ee57-190">**web.config**</span></span>

<span data-ttu-id="5ee57-191">Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` con *web.config*, vea la sección *Establecimiento de variables de entorno* de <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-191">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="5ee57-192">**Archivo del proyecto o perfil de publicación**</span><span class="sxs-lookup"><span data-stu-id="5ee57-192">**Project file or publish profile**</span></span>

<span data-ttu-id="5ee57-193">**Para las implementaciones de IIS de Windows:** Incluya la propiedad `<EnvironmentName>` del [perfil de publicación (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) o un archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="5ee57-193">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="5ee57-194">Este método establece el entorno en *web.config* cuando se publica el proyecto:</span><span class="sxs-lookup"><span data-stu-id="5ee57-194">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="5ee57-195">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="5ee57-195">**Per IIS Application Pool**</span></span>

<span data-ttu-id="5ee57-196">Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` para una aplicación que se ejecuta en un grupo de aplicaciones aislado (se admite en IIS 10.0 o posterior), vea la sección *AppCmd.exe* del tema [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno).</span><span class="sxs-lookup"><span data-stu-id="5ee57-196">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="5ee57-197">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece para un grupo de aplicaciones, su valor reemplaza a un valor en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-197">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5ee57-198">Cuando hospede una aplicación en IIS y agregue o cambie la variable de entorno `ASPNETCORE_ENVIRONMENT`, use cualquiera de los siguientes métodos para que las aplicaciones tomen el nuevo valor:</span><span class="sxs-lookup"><span data-stu-id="5ee57-198">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="5ee57-199">Ejecute `net stop was /y` seguido de `net start w3svc` en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-199">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="5ee57-200">Reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="5ee57-200">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="5ee57-201">macOS</span><span class="sxs-lookup"><span data-stu-id="5ee57-201">macOS</span></span>

<span data-ttu-id="5ee57-202">Para establecer el entorno actual para macOS, puede hacerlo en línea al ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5ee57-202">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="5ee57-203">De forma alternativa, defina el entorno con `export` antes de ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5ee57-203">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="5ee57-204">Las variables de entorno de nivel de equipo se establecen en el archivo *.bashrc* o *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="5ee57-204">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="5ee57-205">Edite el archivo con cualquier editor de texto.</span><span class="sxs-lookup"><span data-stu-id="5ee57-205">Edit the file using any text editor.</span></span> <span data-ttu-id="5ee57-206">Agregue la siguiente instrucción:</span><span class="sxs-lookup"><span data-stu-id="5ee57-206">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="5ee57-207">Linux</span><span class="sxs-lookup"><span data-stu-id="5ee57-207">Linux</span></span>

<span data-ttu-id="5ee57-208">Para distribuciones de Linux, use el comando `export` en un símbolo del sistema para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-208">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="5ee57-209">Establecimiento del entorno en el código</span><span class="sxs-lookup"><span data-stu-id="5ee57-209">Set the environment in code</span></span>

<span data-ttu-id="5ee57-210">Llame a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> al compilar el host.</span><span class="sxs-lookup"><span data-stu-id="5ee57-210">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="5ee57-211">Vea <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-211">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="5ee57-212">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-212">Configuration by environment</span></span>

<span data-ttu-id="5ee57-213">Para cargar la configuración por entorno, se recomienda lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5ee57-213">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="5ee57-214">Archivos *appsettings* (*appsettings.{Entorno}.json*).</span><span class="sxs-lookup"><span data-stu-id="5ee57-214">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="5ee57-215">Vea <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-215">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="5ee57-216">Variables de entorno (establecidas en todos los sistemas donde se hospede la aplicación).</span><span class="sxs-lookup"><span data-stu-id="5ee57-216">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="5ee57-217">Vea <xref:fundamentals/host/generic-host#environmentname> y <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-217">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="5ee57-218">Administrador de secretos (solo en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="5ee57-218">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="5ee57-219">Vea <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-219">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="5ee57-220">Métodos y clase Startup basados en entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-220">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="5ee57-221">Inserción de IWebHostEnvironment en Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="5ee57-221">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="5ee57-222">Inserte <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-222">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="5ee57-223">Este enfoque es útil cuando la aplicación solo requiere el ajuste de `Startup.Configure` para algunos entornos con diferencias de código mínimas en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-223">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="5ee57-224">Inserción de IWebHostEnvironment en la clase Startup</span><span class="sxs-lookup"><span data-stu-id="5ee57-224">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="5ee57-225">Inserte <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> en el constructor de `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-225">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="5ee57-226">Este enfoque es útil cuando la aplicación solo requiere la configuración de `Startup` para algunos entornos con diferencias de código mínimas en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-226">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="5ee57-227">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5ee57-227">In the following example:</span></span>

* <span data-ttu-id="5ee57-228">El entorno se mantiene en el campo `_env`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-228">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="5ee57-229">`_env` se usa en `ConfigureServices` y `Configure` para aplicar la configuración de inicio en función del entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-229">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```
### <a name="startup-class-conventions"></a><span data-ttu-id="5ee57-230">Convenciones de la clase Startup</span><span class="sxs-lookup"><span data-stu-id="5ee57-230">Startup class conventions</span></span>

<span data-ttu-id="5ee57-231">Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-231">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="5ee57-232">La aplicación puede definir clases `Startup` independientes para distintos entornos (por ejemplo, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="5ee57-232">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="5ee57-233">La clase `Startup` correspondiente se selecciona en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5ee57-233">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="5ee57-234">La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria.</span><span class="sxs-lookup"><span data-stu-id="5ee57-234">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="5ee57-235">Si no se encuentra una clase `Startup{EnvironmentName}` coincidente, se usa la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-235">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="5ee57-236">Este enfoque es útil cuando la aplicación requiere configurar el inicio para algunos entornos con muchas diferencias de código en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-236">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="5ee57-237">Para implementar clases `Startup` basadas en entornos, cree una clase `Startup{EnvironmentName}` para cada entorno en uso y una clase `Startup` de reserva:</span><span class="sxs-lookup"><span data-stu-id="5ee57-237">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="5ee57-238">Use la sobrecarga [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) que acepta un nombre de ensamblado:</span><span class="sxs-lookup"><span data-stu-id="5ee57-238">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="5ee57-239">Convenciones del método Startup</span><span class="sxs-lookup"><span data-stu-id="5ee57-239">Startup method conventions</span></span>

<span data-ttu-id="5ee57-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) son compatibles con versiones específicas del entorno con el formato `Configure<EnvironmentName>` y `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="5ee57-241">Este enfoque es útil cuando la aplicación requiere configurar el inicio para algunos entornos con muchas diferencias de código en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-241">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="5ee57-242">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5ee57-242">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5ee57-243">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5ee57-243">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5ee57-244">ASP.NET Core configura el comportamiento de las aplicaciones en función del entorno en tiempo de ejecución mediante una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-244">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="5ee57-245">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5ee57-245">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="5ee57-246">Entornos</span><span class="sxs-lookup"><span data-stu-id="5ee57-246">Environments</span></span>

<span data-ttu-id="5ee57-247">ASP.NET Core lee la variable de entorno `ASPNETCORE_ENVIRONMENT` al inicio de la aplicación y almacena el valor en [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="5ee57-247">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="5ee57-248">`ASPNETCORE_ENVIRONMENT` se puede establecer en cualquier valor, pero el marco proporciona tres valores:</span><span class="sxs-lookup"><span data-stu-id="5ee57-248">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="5ee57-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="5ee57-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="5ee57-250">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="5ee57-250">The preceding code:</span></span>

* <span data-ttu-id="5ee57-251">Llama a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) cuando `ASPNETCORE_ENVIRONMENT` está establecido en `Development`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-251">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="5ee57-252">Llama a [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) cuando el valor de `ASPNETCORE_ENVIRONMENT` está establecido en uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-252">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="5ee57-253">El [asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa el valor de `IHostingEnvironment.EnvironmentName` para incluir o excluir el marcado en el elemento:</span><span class="sxs-lookup"><span data-stu-id="5ee57-253">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="5ee57-254">En Windows y macOS, los valores y las variables de entorno no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5ee57-254">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="5ee57-255">Los valores y las variables de entorno de Linux **distinguen mayúsculas de minúsculas** de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5ee57-255">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="5ee57-256">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="5ee57-256">Development</span></span>

<span data-ttu-id="5ee57-257">El entorno de desarrollo puede habilitar características que no deben exponerse en producción.</span><span class="sxs-lookup"><span data-stu-id="5ee57-257">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="5ee57-258">Por ejemplo, las plantillas de ASP.NET Core habilitan la [página de excepciones para el desarrollador](xref:fundamentals/error-handling#developer-exception-page) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-258">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="5ee57-259">El entorno para el desarrollo del equipo local se puede establecer en el archivo *Properties\launchSettings.json* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5ee57-259">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="5ee57-260">Los valores de entorno establecidos en *launchSettings.json* invalidan los valores establecidos en el entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-260">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="5ee57-261">En el siguiente fragmento de JSON se muestran tres perfiles de un archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5ee57-261">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="5ee57-262">La propiedad `applicationUrl` en *launchSettings.json* puede especificar una lista de direcciones URL del servidor.</span><span class="sxs-lookup"><span data-stu-id="5ee57-262">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="5ee57-263">Use un punto y coma entre las direcciones URL de la lista:</span><span class="sxs-lookup"><span data-stu-id="5ee57-263">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="5ee57-264">Cuando la aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run), se usa el primer perfil con `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-264">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="5ee57-265">El valor de `commandName` especifica el servidor web que se va a iniciar.</span><span class="sxs-lookup"><span data-stu-id="5ee57-265">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="5ee57-266">`commandName` puede ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-266">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="5ee57-267">`Project` (que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="5ee57-267">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="5ee57-268">Cuando una aplicación se inicia con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="5ee57-268">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="5ee57-269">Se lee *launchSettings.json*, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="5ee57-269">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="5ee57-270">La configuración de `environmentVariables` de *launchSettings.json* reemplaza las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-270">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="5ee57-271">Se muestra el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="5ee57-271">The hosting environment is displayed.</span></span>

<span data-ttu-id="5ee57-272">En la siguiente salida se muestra una aplicación que se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="5ee57-272">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="5ee57-273">La pestaña **Depurar** de las propiedades de proyecto de Visual Studio proporciona una GUI para editar el archivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5ee57-273">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables de entorno de configuración de las propiedades del proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="5ee57-275">Los cambios realizados en los perfiles de proyecto podrían no surtir efecto hasta que se reinicie el servidor web.</span><span class="sxs-lookup"><span data-stu-id="5ee57-275">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="5ee57-276">Es necesario reiniciar Kestrel para que pueda detectar los cambios realizados en su entorno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-276">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="5ee57-277">En *launchSettings.json* no se deben almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="5ee57-277">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="5ee57-278">Se puede usar la [herramienta Administrador de secretos](xref:security/app-secrets) a fin de almacenar secretos para el desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="5ee57-278">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="5ee57-279">Cuando se usa [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="5ee57-279">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="5ee57-280">En el siguiente ejemplo se establece el entorno en `Development`:</span><span class="sxs-lookup"><span data-stu-id="5ee57-280">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="5ee57-281">Un archivo *.vscode/launch.json* del proyecto no se lee al iniciar la aplicación con `dotnet run` en la misma manera que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5ee57-281">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="5ee57-282">Cuando se inicia una aplicación en desarrollo que no tiene un archivo *launchSettings.json*, establezca el valor del entorno con una variable de entorno o con un argumento de línea de comandos para el comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-282">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="5ee57-283">Producción</span><span class="sxs-lookup"><span data-stu-id="5ee57-283">Production</span></span>

<span data-ttu-id="5ee57-284">El entorno de producción debe configurarse para maximizar la seguridad, el rendimiento y la solidez de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-284">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="5ee57-285">Las opciones de configuración comunes que difieren de las del entorno de desarrollo son:</span><span class="sxs-lookup"><span data-stu-id="5ee57-285">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="5ee57-286">Almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="5ee57-286">Caching.</span></span>
* <span data-ttu-id="5ee57-287">Los recursos del cliente se agrupan, se reducen y se atienden potencialmente desde una CDN.</span><span class="sxs-lookup"><span data-stu-id="5ee57-287">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="5ee57-288">Las páginas de error de diagnóstico están deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="5ee57-288">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="5ee57-289">Las páginas de error descriptivas están habilitadas.</span><span class="sxs-lookup"><span data-stu-id="5ee57-289">Friendly error pages enabled.</span></span>
* <span data-ttu-id="5ee57-290">El registro y la supervisión de la producción están habilitados.</span><span class="sxs-lookup"><span data-stu-id="5ee57-290">Production logging and monitoring enabled.</span></span> <span data-ttu-id="5ee57-291">Por ejemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="5ee57-291">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="5ee57-292">Establecimiento del entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-292">Set the environment</span></span>

<span data-ttu-id="5ee57-293">A menudo resulta útil establecer un entorno específico para realizar las pruebas con una variable de entorno o una configuración de plataforma.</span><span class="sxs-lookup"><span data-stu-id="5ee57-293">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="5ee57-294">Si el entorno no está establecido, el valor predeterminado es `Production`, lo que deshabilita la mayoría de las características de depuración.</span><span class="sxs-lookup"><span data-stu-id="5ee57-294">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="5ee57-295">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-295">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="5ee57-296">Cuando se compila el host, la última configuración de entorno leída por la aplicación determina el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-296">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="5ee57-297">El entorno de la aplicación no se puede cambiar mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-297">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="5ee57-298">Configuración de plataforma o variable de entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-298">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="5ee57-299">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5ee57-299">Azure App Service</span></span>

<span data-ttu-id="5ee57-300">Para establecer el entorno en [Azure App Service](https://azure.microsoft.com/services/app-service/), realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-300">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="5ee57-301">Seleccione la aplicación desde la hoja **App Services**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-301">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="5ee57-302">En el grupo **Configuración**, seleccione la hoja **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-302">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="5ee57-303">En la pestaña **Configuración de aplicaciones**, seleccione **Nueva configuración de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-303">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="5ee57-304">En la ventana **Agregar o editar la configuración de la aplicación**, escriba `ASPNETCORE_ENVIRONMENT` para el **Nombre**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-304">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="5ee57-305">En **Valor**, proporcione el entorno (por ejemplo, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="5ee57-305">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="5ee57-306">Active la casilla **Configuración de ranura de implementación** si quiere que la configuración del entorno permanezca con la ranura actual cuando se intercambien las ranuras de implementación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-306">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="5ee57-307">Para más información, consulte [Configuración de entornos de ensayo en Azure App Service](/azure/app-service/web-sites-staged-publishing) en la documentación de Azure.</span><span class="sxs-lookup"><span data-stu-id="5ee57-307">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="5ee57-308">Seleccione **Aceptar** para cerrar la ventana **Agregar o editar la configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-308">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="5ee57-309">Seleccione **Guardar** en la parte superior de la hoja **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="5ee57-309">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="5ee57-310">Azure App Service reinicia automáticamente la aplicación después de que se agregue, cambie o elimine una configuración de aplicación (variable de entorno) en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5ee57-310">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="5ee57-311">Windows</span><span class="sxs-lookup"><span data-stu-id="5ee57-311">Windows</span></span>

<span data-ttu-id="5ee57-312">Para establecer `ASPNETCORE_ENVIRONMENT` en la sesión actual, cuando la aplicación se ha iniciado con [dotnet run](/dotnet/core/tools/dotnet-run), use los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-312">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="5ee57-313">**Símbolo del sistema**</span><span class="sxs-lookup"><span data-stu-id="5ee57-313">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="5ee57-314">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="5ee57-314">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="5ee57-315">Estos comandos solo tienen efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="5ee57-315">These commands only take effect for the current window.</span></span> <span data-ttu-id="5ee57-316">Cuando se cierre la ventana, la configuración de `ASPNETCORE_ENVIRONMENT` volverá a la configuración predeterminada o al valor del equipo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-316">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="5ee57-317">Para establecer el valor globalmente en Windows, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ee57-317">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="5ee57-318">Abra el **Panel de control** > **Sistema** > **Configuración avanzada del sistema** y agregue o edite el valor `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="5ee57-318">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

  ![Variable de entorno de ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="5ee57-321">Abra un símbolo del sistema con permisos de administrador y use el comando `setx` o abra un símbolo del sistema administrativo de PowerShell y use `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="5ee57-321">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="5ee57-322">**Símbolo del sistema**</span><span class="sxs-lookup"><span data-stu-id="5ee57-322">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="5ee57-323">El modificador `/M` indica que hay que establecer la variable de entorno en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-323">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="5ee57-324">Si no se usa el modificador `/M`, la variable de entorno se establece para la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="5ee57-324">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="5ee57-325">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="5ee57-325">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="5ee57-326">El valor de opción `Machine` indica que hay que establecer la variable de entorno en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-326">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="5ee57-327">Si el valor de opción se cambia a `User`, la variable de entorno se establece para la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="5ee57-327">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="5ee57-328">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece de forma global, se aplica a `dotnet run` en cualquier ventana de comandos abierta después del establecimiento del valor.</span><span class="sxs-lookup"><span data-stu-id="5ee57-328">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="5ee57-329">**web.config**</span><span class="sxs-lookup"><span data-stu-id="5ee57-329">**web.config**</span></span>

<span data-ttu-id="5ee57-330">Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` con *web.config*, vea la sección *Establecimiento de variables de entorno* de <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-330">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="5ee57-331">**Archivo del proyecto o perfil de publicación**</span><span class="sxs-lookup"><span data-stu-id="5ee57-331">**Project file or publish profile**</span></span>

<span data-ttu-id="5ee57-332">**Para las implementaciones de IIS de Windows:** Incluya la propiedad `<EnvironmentName>` del [perfil de publicación (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) o un archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="5ee57-332">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="5ee57-333">Este método establece el entorno en *web.config* cuando se publica el proyecto:</span><span class="sxs-lookup"><span data-stu-id="5ee57-333">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="5ee57-334">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="5ee57-334">**Per IIS Application Pool**</span></span>

<span data-ttu-id="5ee57-335">Para establecer la variable de entorno `ASPNETCORE_ENVIRONMENT` para una aplicación que se ejecuta en un grupo de aplicaciones aislado (se admite en IIS 10.0 o posterior), vea la sección *AppCmd.exe* del tema [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno).</span><span class="sxs-lookup"><span data-stu-id="5ee57-335">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="5ee57-336">Cuando la variable de entorno `ASPNETCORE_ENVIRONMENT` se establece para un grupo de aplicaciones, su valor reemplaza a un valor en el nivel de sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-336">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5ee57-337">Cuando hospede una aplicación en IIS y agregue o cambie la variable de entorno `ASPNETCORE_ENVIRONMENT`, use cualquiera de los siguientes métodos para que las aplicaciones tomen el nuevo valor:</span><span class="sxs-lookup"><span data-stu-id="5ee57-337">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="5ee57-338">Ejecute `net stop was /y` seguido de `net start w3svc` en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="5ee57-338">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="5ee57-339">Reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="5ee57-339">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="5ee57-340">macOS</span><span class="sxs-lookup"><span data-stu-id="5ee57-340">macOS</span></span>

<span data-ttu-id="5ee57-341">Para establecer el entorno actual para macOS, puede hacerlo en línea al ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5ee57-341">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="5ee57-342">De forma alternativa, defina el entorno con `export` antes de ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5ee57-342">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="5ee57-343">Las variables de entorno de nivel de equipo se establecen en el archivo *.bashrc* o *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="5ee57-343">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="5ee57-344">Edite el archivo con cualquier editor de texto.</span><span class="sxs-lookup"><span data-stu-id="5ee57-344">Edit the file using any text editor.</span></span> <span data-ttu-id="5ee57-345">Agregue la siguiente instrucción:</span><span class="sxs-lookup"><span data-stu-id="5ee57-345">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="5ee57-346">Linux</span><span class="sxs-lookup"><span data-stu-id="5ee57-346">Linux</span></span>

<span data-ttu-id="5ee57-347">Para distribuciones de Linux, use el comando `export` en un símbolo del sistema para la configuración de variables basada en sesión y el archivo *bash_profile* para la configuración del entorno en el nivel de equipo.</span><span class="sxs-lookup"><span data-stu-id="5ee57-347">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="5ee57-348">Establecimiento del entorno en el código</span><span class="sxs-lookup"><span data-stu-id="5ee57-348">Set the environment in code</span></span>

<span data-ttu-id="5ee57-349">Llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> al compilar el host.</span><span class="sxs-lookup"><span data-stu-id="5ee57-349">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="5ee57-350">Vea <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-350">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="5ee57-351">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-351">Configuration by environment</span></span>

<span data-ttu-id="5ee57-352">Para cargar la configuración por entorno, se recomienda lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5ee57-352">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="5ee57-353">Archivos *appsettings* (*appsettings.{Entorno}.json*).</span><span class="sxs-lookup"><span data-stu-id="5ee57-353">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="5ee57-354">Vea <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-354">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="5ee57-355">Variables de entorno (establecidas en todos los sistemas donde se hospede la aplicación).</span><span class="sxs-lookup"><span data-stu-id="5ee57-355">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="5ee57-356">Vea <xref:fundamentals/host/web-host#environment> y <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-356">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="5ee57-357">Administrador de secretos (solo en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="5ee57-357">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="5ee57-358">Vea <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="5ee57-358">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="5ee57-359">Métodos y clase Startup basados en entorno</span><span class="sxs-lookup"><span data-stu-id="5ee57-359">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="5ee57-360">Inserción de IHostingEnvironment en Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="5ee57-360">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="5ee57-361">Inserte <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-361">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="5ee57-362">Este enfoque es útil cuando la aplicación solo requiere la configuración de `Startup.Configure` para algunos entornos con diferencias de código mínimas en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-362">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="5ee57-363">Inserción de IHostingEnvironment en la clase Startup</span><span class="sxs-lookup"><span data-stu-id="5ee57-363">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="5ee57-364">Inserte <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> en el constructor `Startup` y asigne el servicio a un campo para su uso en la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-364">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="5ee57-365">Este enfoque es útil cuando la aplicación solo requiere configurar el inicio para algunos entornos con diferencias de código mínimas en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-365">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="5ee57-366">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5ee57-366">In the following example:</span></span>

* <span data-ttu-id="5ee57-367">El entorno se mantiene en el campo `_env`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-367">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="5ee57-368">`_env` se usa en `ConfigureServices` y `Configure` para aplicar la configuración de inicio en función del entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-368">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a><span data-ttu-id="5ee57-369">Convenciones de la clase Startup</span><span class="sxs-lookup"><span data-stu-id="5ee57-369">Startup class conventions</span></span>

<span data-ttu-id="5ee57-370">Cuando se inicia una aplicación ASP.NET Core, la [clase Startup](xref:fundamentals/startup) arranca la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ee57-370">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="5ee57-371">La aplicación puede definir clases `Startup` independientes para distintos entornos (por ejemplo, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="5ee57-371">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="5ee57-372">La clase `Startup` correspondiente se selecciona en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5ee57-372">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="5ee57-373">La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria.</span><span class="sxs-lookup"><span data-stu-id="5ee57-373">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="5ee57-374">Si no se encuentra una clase `Startup{EnvironmentName}` coincidente, se usa la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-374">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="5ee57-375">Este enfoque es útil cuando la aplicación requiere configurar el inicio para algunos entornos con muchas diferencias de código en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-375">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="5ee57-376">Para implementar clases `Startup` basadas en entornos, cree una clase `Startup{EnvironmentName}` para cada entorno en uso y una clase `Startup` de reserva:</span><span class="sxs-lookup"><span data-stu-id="5ee57-376">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="5ee57-377">Use la sobrecarga [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) que acepta un nombre de ensamblado:</span><span class="sxs-lookup"><span data-stu-id="5ee57-377">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="5ee57-378">Convenciones del método Startup</span><span class="sxs-lookup"><span data-stu-id="5ee57-378">Startup method conventions</span></span>

<span data-ttu-id="5ee57-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) son compatibles con versiones específicas del entorno con el formato `Configure<EnvironmentName>` y `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="5ee57-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="5ee57-380">Este enfoque es útil cuando la aplicación requiere configurar el inicio para algunos entornos con muchas diferencias de código en cada uno.</span><span class="sxs-lookup"><span data-stu-id="5ee57-380">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="5ee57-381">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5ee57-381">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
