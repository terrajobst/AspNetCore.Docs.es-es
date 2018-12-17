---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: Descubra la compatibilidad con la depuración de aplicaciones ASP.NET Core cuando se ejecutan detrás de IIS en Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 51375e6a6bb25a469d467ca97a151abd305c1ece
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862387"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="2ff89-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ff89-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="2ff89-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2ff89-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2ff89-105">En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) con la depuración de aplicaciones ASP.NET Core que se ejecutan detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2ff89-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="2ff89-106">En este tema se explica cómo habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="2ff89-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ff89-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2ff89-107">Prerequisites</span></span>

* <span data-ttu-id="2ff89-108">[Visual Studio para Windows](https://www.microsoft.com/net/download/windows).</span><span class="sxs-lookup"><span data-stu-id="2ff89-108">[Visual Studio for Windows](https://www.microsoft.com/net/download/windows)</span></span>
* <span data-ttu-id="2ff89-109">Carga de trabajo de **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="2ff89-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="2ff89-110">Carga de trabajo **Desarrollo multiplataforma de .NET Core**</span><span class="sxs-lookup"><span data-stu-id="2ff89-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="2ff89-111">Certificado de seguridad X.509</span><span class="sxs-lookup"><span data-stu-id="2ff89-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="2ff89-112">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="2ff89-112">Enable IIS</span></span>

1. <span data-ttu-id="2ff89-113">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="2ff89-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="2ff89-114">Active la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-114">Select the **Internet Information Services** check box.</span></span>

![Características de Windows que muestran la casilla de Internet Information Services activada como un cuadrado negro (no una marca de verificación) para indicar que alguna de las características de IIS está habilitada](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="2ff89-116">La instalación de IIS puede requerir un reinicio del sistema.</span><span class="sxs-lookup"><span data-stu-id="2ff89-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="2ff89-117">Configurar IIS</span><span class="sxs-lookup"><span data-stu-id="2ff89-117">Configure IIS</span></span>

<span data-ttu-id="2ff89-118">IIS debe tener un sitio web configurado con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2ff89-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="2ff89-119">Un nombre de host que coincida con el nombre de host de la dirección URL del perfil de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ff89-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="2ff89-120">Enlace al puerto 443 con un certificado asignado.</span><span class="sxs-lookup"><span data-stu-id="2ff89-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="2ff89-121">Por ejemplo, el **nombre de host** de un sitio web agregado se establece en "localhost" (el perfil de inicio también usará "localhost" más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="2ff89-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="2ff89-122">El puerto se establece en "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2ff89-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="2ff89-123">El **certificado de desarrollo de IIS Express** se asigna al sitio web, pero cualquier certificado válido sirve:</span><span class="sxs-lookup"><span data-stu-id="2ff89-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Ventana Agregar sitio web de IIS que muestra el enlace establecido para localhost en el puerto 443 con un certificado asignado.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="2ff89-125">Si la instalación de IIS ya tiene un **sitio web predeterminado** con un nombre de host que coincide con el nombre de host de la dirección URL del perfil de inicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ff89-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="2ff89-126">Agregue un enlace al puerto 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2ff89-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="2ff89-127">Asigne un certificado válido al sitio web.</span><span class="sxs-lookup"><span data-stu-id="2ff89-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="2ff89-128">Habilitación de la compatibilidad con IIS en tiempo de desarrollo en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ff89-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="2ff89-129">Inicie el instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2ff89-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="2ff89-130">Seleccione el componente **Compatibilidad con IIS en tiempo de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="2ff89-131">El componente se muestra como opcional en el panel **Resumen** de la carga de trabajo **Desarrollo de ASP.NET y web**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="2ff89-132">El componente instala el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que es un módulo de IIS nativo necesario para ejecutar aplicaciones de ASP.NET Core con IIS.</span><span class="sxs-lookup"><span data-stu-id="2ff89-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="2ff89-136">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="2ff89-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="2ff89-137">Redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="2ff89-137">HTTPS redirection</span></span>

<span data-ttu-id="2ff89-138">En un nuevo proyecto, active la casilla **Configure for HTTPS** (Configurar para HTTPS) en la ventana **Nueva aplicación web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="2ff89-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Ventana Nueva aplicación web ASP.NET Core con la casilla Configure for HTTPS (Configurar para HTTPS) activada](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="2ff89-140">En un proyecto existente, use el Middleware de redireccionamiento HTTPS en `Startup.Configure` mediante la llamada al método de extensión [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):</span><span class="sxs-lookup"><span data-stu-id="2ff89-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="2ff89-141">Perfil de inicio de IIS</span><span class="sxs-lookup"><span data-stu-id="2ff89-141">IIS launch profile</span></span>

<span data-ttu-id="2ff89-142">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="2ff89-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="2ff89-143">En **Perfil**, seleccione el botón **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="2ff89-144">Asigne el perfil el nombre "IIS" en la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="2ff89-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="2ff89-145">Seleccione **Aceptar** para crear el perfil.</span><span class="sxs-lookup"><span data-stu-id="2ff89-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="2ff89-146">En **Iniciar**, seleccione **IIS** en la lista.</span><span class="sxs-lookup"><span data-stu-id="2ff89-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="2ff89-147">Active la casilla **Iniciar explorador** y proporcione la dirección URL del punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="2ff89-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="2ff89-148">Use el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ff89-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="2ff89-149">Este ejemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="2ff89-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="2ff89-150">En la sección **Variables de entorno**, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="2ff89-151">Proporcione una variable de entorno con una clave de `ASPNETCORE_ENVIRONMENT` y un valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="2ff89-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="2ff89-152">En el área **Configuración del servidor web**, establezca la **dirección URL de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="2ff89-153">Este ejemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="2ff89-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="2ff89-154">Guarde el perfil.</span><span class="sxs-lookup"><span data-stu-id="2ff89-154">Save the profile.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="2ff89-159">Como alternativa, puede agregar manualmente un perfil de inicio al archivo [launchSettings.json](http://json.schemastore.org/launchsettings) en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ff89-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a><span data-ttu-id="2ff89-160">Ejecución del proyecto</span><span class="sxs-lookup"><span data-stu-id="2ff89-160">Run the project</span></span>

<span data-ttu-id="2ff89-161">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2ff89-161">In Visual Studio:</span></span>

* <span data-ttu-id="2ff89-162">Confirme que la lista desplegable de configuración de compilación está configurada como **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="2ff89-162">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="2ff89-163">Establezca el botón Ejecutar en el perfil **IIS** y seleccione el botón para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ff89-163">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

![El botón Ejecutar de la barra de herramientas de VS se establece en el perfil IIS con la lista desplegable de configuración de compilación establecida en Versión.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="2ff89-165">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="2ff89-165">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="2ff89-166">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2ff89-166">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="2ff89-167">Si se usa un certificado de desarrollo que no es de confianza, el explorador puede pedirle que cree una excepción para un certificado de esta clase.</span><span class="sxs-lookup"><span data-stu-id="2ff89-167">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff89-168">La depuración de una configuración de compilación de versión con [Solo mi código](/visualstudio/debugger/just-my-code) y las optimizaciones del compilador degradan el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="2ff89-168">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="2ff89-169">Por ejemplo, no se alcanzan los puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="2ff89-169">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2ff89-170">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2ff89-170">Additional resources</span></span>

* [<span data-ttu-id="2ff89-171">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="2ff89-171">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="2ff89-172">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ff89-172">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="2ff89-173">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ff89-173">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="2ff89-174">Aplicación de HTTPS</span><span class="sxs-lookup"><span data-stu-id="2ff89-174">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
