---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: Descubra la compatibilidad con la depuración de aplicaciones ASP.NET Core cuando se ejecutan detrás de IIS en Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233084"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="bffe1-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffe1-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="bffe1-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bffe1-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bffe1-105">En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) con la depuración de aplicaciones ASP.NET Core que se ejecutan detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="bffe1-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="bffe1-106">En este tema se explica cómo habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="bffe1-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bffe1-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bffe1-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="bffe1-108">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="bffe1-108">Enable IIS</span></span>

1. <span data-ttu-id="bffe1-109">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="bffe1-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="bffe1-110">Active la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="bffe1-110">Select the **Internet Information Services** check box.</span></span>

![Características de Windows que muestran la casilla de Internet Information Services activada como un cuadrado negro (no una marca de verificación) para indicar que alguna de las características de IIS está habilitada](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="bffe1-112">La instalación de IIS puede requerir un reinicio del sistema.</span><span class="sxs-lookup"><span data-stu-id="bffe1-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="bffe1-113">Configurar IIS</span><span class="sxs-lookup"><span data-stu-id="bffe1-113">Configure IIS</span></span>

<span data-ttu-id="bffe1-114">IIS debe tener un sitio web configurado con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bffe1-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="bffe1-115">Un nombre de host que coincida con el nombre de host de la dirección URL del perfil de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bffe1-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="bffe1-116">Enlace al puerto 443 con un certificado asignado.</span><span class="sxs-lookup"><span data-stu-id="bffe1-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="bffe1-117">Por ejemplo, el **nombre de host** de un sitio web agregado se establece en "localhost" (el perfil de inicio también usará "localhost" más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="bffe1-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="bffe1-118">El puerto se establece en "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="bffe1-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="bffe1-119">El **certificado de desarrollo de IIS Express** se asigna al sitio web, pero cualquier certificado válido sirve:</span><span class="sxs-lookup"><span data-stu-id="bffe1-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Ventana Agregar sitio web de IIS que muestra el enlace establecido para localhost en el puerto 443 con un certificado asignado.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="bffe1-121">Si la instalación de IIS ya tiene un **sitio web predeterminado** con un nombre de host que coincide con el nombre de host de la dirección URL del perfil de inicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bffe1-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="bffe1-122">Agregue un enlace al puerto 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="bffe1-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="bffe1-123">Asigne un certificado válido al sitio web.</span><span class="sxs-lookup"><span data-stu-id="bffe1-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="bffe1-124">Habilitación de la compatibilidad con IIS en tiempo de desarrollo en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bffe1-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="bffe1-125">Inicie el instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bffe1-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="bffe1-126">Seleccione el componente **Compatibilidad con IIS en tiempo de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="bffe1-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="bffe1-127">El componente se muestra como opcional en el panel **Resumen** de la carga de trabajo **Desarrollo de ASP.NET y web**.</span><span class="sxs-lookup"><span data-stu-id="bffe1-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="bffe1-128">El componente instala el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que es un módulo nativo de IIS necesario para ejecutar aplicaciones ASP.NET Core detrás de IIS en una configuración de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="bffe1-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="bffe1-132">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="bffe1-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="bffe1-133">Redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="bffe1-133">HTTPS redirection</span></span>

<span data-ttu-id="bffe1-134">En un nuevo proyecto, active la casilla **Configure for HTTPS** (Configurar para HTTPS) en la ventana **Nueva aplicación web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="bffe1-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Ventana Nueva aplicación web ASP.NET Core con la casilla Configure for HTTPS (Configurar para HTTPS) activada](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="bffe1-136">En un proyecto existente, use el Middleware de redireccionamiento HTTPS en `Startup.Configure` mediante la llamada al método de extensión [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):</span><span class="sxs-lookup"><span data-stu-id="bffe1-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="bffe1-137">Perfil de inicio de IIS</span><span class="sxs-lookup"><span data-stu-id="bffe1-137">IIS launch profile</span></span>

<span data-ttu-id="bffe1-138">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="bffe1-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="bffe1-139">En **Perfil**, seleccione el botón **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="bffe1-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="bffe1-140">Asigne el perfil el nombre "IIS" en la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="bffe1-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="bffe1-141">Seleccione **Aceptar** para crear el perfil.</span><span class="sxs-lookup"><span data-stu-id="bffe1-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="bffe1-142">En **Iniciar**, seleccione **IIS** en la lista.</span><span class="sxs-lookup"><span data-stu-id="bffe1-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="bffe1-143">Active la casilla **Iniciar explorador** y proporcione la dirección URL del punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="bffe1-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="bffe1-144">Use el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bffe1-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="bffe1-145">Este ejemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="bffe1-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="bffe1-146">En la sección **Variables de entorno**, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="bffe1-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="bffe1-147">Proporcione una variable de entorno con una clave de `ASPNETCORE_ENVIRONMENT` y un valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="bffe1-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="bffe1-148">En el área **Configuración del servidor web**, establezca la **dirección URL de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="bffe1-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="bffe1-149">Este ejemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="bffe1-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="bffe1-150">Guarde el perfil.</span><span class="sxs-lookup"><span data-stu-id="bffe1-150">Save the profile.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="bffe1-155">Como alternativa, puede agregar manualmente un perfil de inicio al archivo [launchSettings.json](http://json.schemastore.org/launchsettings) en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bffe1-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="bffe1-156">Ejecución del proyecto</span><span class="sxs-lookup"><span data-stu-id="bffe1-156">Run the project</span></span>

<span data-ttu-id="bffe1-157">En la interfaz de usuario de VS, establezca el botón Ejecutar en el perfil **IIS** y seleccione el botón para iniciar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bffe1-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Botón Ejecutar en la barra de herramientas de VS establecido en el perfil "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="bffe1-159">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="bffe1-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="bffe1-160">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bffe1-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="bffe1-161">Si se usa un certificado de desarrollo que no es de confianza, el explorador puede pedirle que cree una excepción para un certificado de esta clase.</span><span class="sxs-lookup"><span data-stu-id="bffe1-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bffe1-162">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bffe1-162">Additional resources</span></span>

* [<span data-ttu-id="bffe1-163">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="bffe1-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="bffe1-164">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffe1-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="bffe1-165">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffe1-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="bffe1-166">Aplicación de HTTPS</span><span class="sxs-lookup"><span data-stu-id="bffe1-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
