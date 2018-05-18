---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: Descubrir compatibilidad para depurar aplicaciones de ASP.NET Core cuando se ejecuta detrás de IIS en Windows Server.
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
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="055fd-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="055fd-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="055fd-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="055fd-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="055fd-105">Este artículo se describen [Visual Studio](https://www.visualstudio.com/vs/) la compatibilidad para depurar aplicaciones de ASP.NET Core que se ejecutan detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="055fd-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="055fd-106">Este tema le guía a través de habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="055fd-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="055fd-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="055fd-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="055fd-108">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="055fd-108">Enable IIS</span></span>

1. <span data-ttu-id="055fd-109">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="055fd-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="055fd-110">Seleccione el **Internet Information Services** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="055fd-110">Select the **Internet Information Services** check box.</span></span>

![Casilla de verificación de que muestra las características de Windows Internet Information Services activada como un cuadrado negro (no una marca de verificación) que indica que algunas de las características IIS están habilitados](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="055fd-112">La instalación de IIS puede requerir un reinicio del sistema.</span><span class="sxs-lookup"><span data-stu-id="055fd-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="055fd-113">Configurar IIS</span><span class="sxs-lookup"><span data-stu-id="055fd-113">Configure IIS</span></span>

<span data-ttu-id="055fd-114">IIS debe tener un sitio Web configurado con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="055fd-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="055fd-115">Nombre de host de un nombre de host que coincida con la dirección URL del perfil de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="055fd-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="055fd-116">Enlace para el puerto 443 con un certificado asignado.</span><span class="sxs-lookup"><span data-stu-id="055fd-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="055fd-117">Por ejemplo, el **nombre de Host** para un sitio Web de agregado se establece en "localhost" (el perfil de inicio también usarán "localhost" más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="055fd-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="055fd-118">El puerto se establece en "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="055fd-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="055fd-119">El **certificado de IIS Express desarrollo** se asigna para el sitio Web, pero cualquier certificado válido funciona:</span><span class="sxs-lookup"><span data-stu-id="055fd-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Agregar ventana del sitio Web en IIS que muestra el conjunto de enlace para el host local en el puerto 443 con un certificado asignado.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="055fd-121">Si la instalación de IIS ya tiene un **sitio Web predeterminado** con un nombre de host que coincida con el nombre de host de dirección URL de perfil de inicio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="055fd-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="055fd-122">Agregue un enlace de puerto para el puerto 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="055fd-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="055fd-123">Asignar un certificado válido para el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="055fd-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="055fd-124">Habilitar la compatibilidad IIS de tiempo de desarrollo en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="055fd-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="055fd-125">Inicie al instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="055fd-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="055fd-126">Seleccione el **IIS compatible con el tiempo de desarrollo** componente.</span><span class="sxs-lookup"><span data-stu-id="055fd-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="055fd-127">El componente se enumera como opcionales en la **resumen** panel para la **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="055fd-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="055fd-128">El componente instala el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module), que es un módulo nativo de IIS necesario para ejecutar aplicaciones de IIS de ASP.NET Core en una configuración de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="055fd-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="055fd-132">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="055fd-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="055fd-133">Redirección de HTTPS</span><span class="sxs-lookup"><span data-stu-id="055fd-133">HTTPS redirection</span></span>

<span data-ttu-id="055fd-134">Para un nuevo proyecto, active la casilla de verificación para **configurar para HTTPS** en el **nueva aplicación de ASP.NET Core Web** ventana:</span><span class="sxs-lookup"><span data-stu-id="055fd-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nueva ventana de aplicación Web de ASP.NET Core con la configuración para activada la casilla de verificación HTTPS.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="055fd-136">En un proyecto existente, use HTTPS redirección Middleware en `Startup.Configure` mediante una llamada a la [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) método de extensión:</span><span class="sxs-lookup"><span data-stu-id="055fd-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="055fd-137">Perfil de inicio de IIS</span><span class="sxs-lookup"><span data-stu-id="055fd-137">IIS launch profile</span></span>

<span data-ttu-id="055fd-138">Crear un nuevo perfil de inicio para agregar compatibilidad en tiempo de desarrollo IIS:</span><span class="sxs-lookup"><span data-stu-id="055fd-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="055fd-139">Para **perfil**, seleccione la **New** botón.</span><span class="sxs-lookup"><span data-stu-id="055fd-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="055fd-140">Nombre del perfil "IIS" en la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="055fd-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="055fd-141">Seleccione **Aceptar** para crear el perfil.</span><span class="sxs-lookup"><span data-stu-id="055fd-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="055fd-142">Para el **iniciar** configuración, seleccione **IIS** en la lista.</span><span class="sxs-lookup"><span data-stu-id="055fd-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="055fd-143">Active la casilla de verificación para **Iniciar explorador** y proporcione la dirección URL del extremo.</span><span class="sxs-lookup"><span data-stu-id="055fd-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="055fd-144">Use el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="055fd-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="055fd-145">Este ejemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="055fd-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="055fd-146">En el **variables de entorno** sección, seleccione la **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="055fd-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="055fd-147">Proporcionar una variable de entorno con una clave de `ASPNETCORE_ENVIRONMENT` y un valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="055fd-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="055fd-148">En el **configuración del servidor Web** área, establecer la **URL de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="055fd-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="055fd-149">Este ejemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="055fd-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="055fd-150">Guarde el perfil.</span><span class="sxs-lookup"><span data-stu-id="055fd-150">Save the profile.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="055fd-155">O bien, agregar manualmente un perfil de inicio para la [launchSettings.json](http://json.schemastore.org/launchsettings) archivo en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="055fd-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="055fd-156">Ejecute el proyecto</span><span class="sxs-lookup"><span data-stu-id="055fd-156">Run the project</span></span>

<span data-ttu-id="055fd-157">En la interfaz de usuario de VS, establezca el botón ejecutar en el **IIS** genera un perfil y seleccione el botón para iniciar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="055fd-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Botón de ejecución en la barra de herramientas de VS que se establece en el perfil "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="055fd-159">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="055fd-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="055fd-160">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="055fd-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="055fd-161">Si se utiliza un certificado de confianza de desarrollo, el explorador puede requerir crear una excepción para el certificado no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="055fd-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="055fd-162">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="055fd-162">Additional resources</span></span>

* [<span data-ttu-id="055fd-163">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="055fd-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="055fd-164">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="055fd-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="055fd-165">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="055fd-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="055fd-166">Aplicación de HTTPS</span><span class="sxs-lookup"><span data-stu-id="055fd-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
