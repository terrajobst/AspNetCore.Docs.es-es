---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: guardrex
description: Descubra la compatibilidad con la depuración de aplicaciones ASP.NET Core cuando se ejecutan con IIS en Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 6f555858239b4432d252f8b3ac7add5c3e8bfe62
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59425106"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="84f5b-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84f5b-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="84f5b-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="84f5b-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="84f5b-105">En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) con la depuración de aplicaciones ASP.NET Core que se ejecutan con IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="84f5b-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="84f5b-106">En este tema se explica cómo habilitar este escenario y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="84f5b-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84f5b-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="84f5b-107">Prerequisites</span></span>

* <span data-ttu-id="84f5b-108">[Visual Studio para Windows](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="84f5b-108">[Visual Studio for Windows](https://visualstudio.microsoft.com/downloads/)</span></span>
* <span data-ttu-id="84f5b-109">Carga de trabajo de **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="84f5b-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="84f5b-110">Carga de trabajo **Desarrollo multiplataforma de .NET Core**</span><span class="sxs-lookup"><span data-stu-id="84f5b-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="84f5b-111">Certificado de seguridad X.509 (para compatibilidad con HTTPS)</span><span class="sxs-lookup"><span data-stu-id="84f5b-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="84f5b-112">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="84f5b-112">Enable IIS</span></span>

1. <span data-ttu-id="84f5b-113">En Windows, vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="84f5b-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="84f5b-114">Active la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="84f5b-115">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-115">Select **OK**.</span></span>

<span data-ttu-id="84f5b-116">La instalación de IIS puede requerir un reinicio del sistema.</span><span class="sxs-lookup"><span data-stu-id="84f5b-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="84f5b-117">Configurar IIS</span><span class="sxs-lookup"><span data-stu-id="84f5b-117">Configure IIS</span></span>

<span data-ttu-id="84f5b-118">IIS debe tener un sitio web configurado con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="84f5b-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="84f5b-119">**Nombre de host**: por lo general, el **Sitio web predeterminado** se usa con un **Nombre de host** de `localhost`.</span><span class="sxs-lookup"><span data-stu-id="84f5b-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="84f5b-120">Sin embargo, sirve cualquier sitio web de IIS válido con un nombre de host único.</span><span class="sxs-lookup"><span data-stu-id="84f5b-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="84f5b-121">**Enlace de sitio**</span><span class="sxs-lookup"><span data-stu-id="84f5b-121">**Site Binding**</span></span>
  * <span data-ttu-id="84f5b-122">Para las aplicaciones que requieran HTTPS, cree un enlace al puerto 443 con un certificado.</span><span class="sxs-lookup"><span data-stu-id="84f5b-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="84f5b-123">Por lo general, se usa el **certificado de desarrollo de IIS Express**, pero cualquier certificado válido sirve.</span><span class="sxs-lookup"><span data-stu-id="84f5b-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="84f5b-124">Para las aplicaciones que usan HTTP, confirme la existencia de un enlace al puerto 80 o cree un enlace a dicho puerto si se trata de un sitio nuevo.</span><span class="sxs-lookup"><span data-stu-id="84f5b-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="84f5b-125">Utilice un enlace único para HTTP o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="84f5b-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="84f5b-126">**No se admite el enlace al mismo tiempo a los puertos HTTP y HTTPS.**</span><span class="sxs-lookup"><span data-stu-id="84f5b-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="84f5b-127">Habilitación de la compatibilidad con IIS en tiempo de desarrollo en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84f5b-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="84f5b-128">Inicie el instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84f5b-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="84f5b-129">Seleccione **Modificar** para la instalación de Visual Studio que se pretende usar para la compatibilidad con IIS en tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="84f5b-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="84f5b-130">Para la carga de trabajo de **Desarrollo de ASP.NET y web** , busque e instale el componente **Compatibilidad con IIS en tiempo de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="84f5b-131">El componente se enumera en la sección **Opcional**, en **Compatibilidad con IIS en tiempo de desarrollo**, dentro del panel **Detalles de instalación** a la derecha de las cargas de trabajo.</span><span class="sxs-lookup"><span data-stu-id="84f5b-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="84f5b-132">El componente instala el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), que es un módulo de IIS nativo necesario para ejecutar aplicaciones de ASP.NET Core con IIS.</span><span class="sxs-lookup"><span data-stu-id="84f5b-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="84f5b-133">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="84f5b-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="84f5b-134">Redireccionamiento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="84f5b-134">HTTPS redirection</span></span>

<span data-ttu-id="84f5b-135">Para un nuevo proyecto que requiere HTTPS, seleccione la casilla **Configurar para HTTPS** en la ventana **Crear una aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="84f5b-136">Al seleccionar la casilla, se agrega [redireccionamiento HTTPS y middleware HSTS](xref:security/enforcing-ssl) a la aplicación cuando esta se crea.</span><span class="sxs-lookup"><span data-stu-id="84f5b-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="84f5b-137">Para un proyecto existente que requiere HTTPS, use el redireccionamiento HTTPS y el middleware HSTS en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="84f5b-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="84f5b-138">Para obtener más información, vea <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="84f5b-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="84f5b-139">Para un proyecto que usa HTTP, el [redireccionamiento HTTPS y el middleware HSTS](xref:security/enforcing-ssl) no se agregan a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84f5b-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="84f5b-140">No se requiere ninguna configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84f5b-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="84f5b-141">Perfil de inicio de IIS</span><span class="sxs-lookup"><span data-stu-id="84f5b-141">IIS launch profile</span></span>

<span data-ttu-id="84f5b-142">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="84f5b-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="84f5b-143">Haga clic con el botón derecho en el **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="84f5b-144">Haga clic en **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-144">Select **Properties**.</span></span> <span data-ttu-id="84f5b-145">Abra la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="84f5b-146">En **Perfil**, seleccione el botón **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="84f5b-147">Asigne el perfil el nombre "IIS" en la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="84f5b-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="84f5b-148">Seleccione **Aceptar** para crear el perfil.</span><span class="sxs-lookup"><span data-stu-id="84f5b-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="84f5b-149">En **Iniciar**, seleccione **IIS** en la lista.</span><span class="sxs-lookup"><span data-stu-id="84f5b-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="84f5b-150">Active la casilla **Iniciar explorador** y proporcione la dirección URL del punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="84f5b-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="84f5b-151">Si la aplicación requiere HTTPS, use un punto de conexión HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="84f5b-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="84f5b-152">Para HTTP, use un punto de conexión HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="84f5b-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="84f5b-153">Proporcione el mismo nombre de host y puerto especificados en la [configuración de IIS establecida en usos anteriores](#configure-iis), normalmente `localhost`.</span><span class="sxs-lookup"><span data-stu-id="84f5b-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="84f5b-154">Proporcione el nombre de la aplicación al final de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="84f5b-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="84f5b-155">Por ejemplo, `https://localhost/WebApplication1` (HTTPS) o `http://localhost/WebApplication1` (HTTP) son direcciones URL de punto de conexión válidas.</span><span class="sxs-lookup"><span data-stu-id="84f5b-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="84f5b-156">En la sección **Variables de entorno**, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="84f5b-157">Proporcione una variable de entorno con un **Nombre** de `ASPNETCORE_ENVIRONMENT` y un **Valor** de `Development`.</span><span class="sxs-lookup"><span data-stu-id="84f5b-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="84f5b-158">En el área **Configuración del servidor web**, defina la **Dirección URL de la aplicación** con el mismo valor utilizado para la dirección URL de punto de conexión de **Iniciar el explorador**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="84f5b-159">Para la configuración del **Modelo de hospedaje** de Visual Studio 2019 o posterior, seleccione **Predeterminado** para usar el modelo de hospedaje utilizado por el proyecto.</span><span class="sxs-lookup"><span data-stu-id="84f5b-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="84f5b-160">Si el proyecto establece la propiedad `<AspNetCoreHostingModel>` en su archivo del proyecto, se usa el valor de la propiedad (`InProcess` o `OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="84f5b-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="84f5b-161">Si la propiedad no existe, se usa el modelo de hospedaje predeterminado de la aplicación, que está en proceso.</span><span class="sxs-lookup"><span data-stu-id="84f5b-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="84f5b-162">Si la aplicación requiere una configuración del modelo de hospedaje distinta a la del modelo de hospedaje habitual de la aplicación, defina el **Modelo de hospedaje** como `In Process` o `Out Of Process`, según proceda.</span><span class="sxs-lookup"><span data-stu-id="84f5b-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="84f5b-163">Guarde el perfil.</span><span class="sxs-lookup"><span data-stu-id="84f5b-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="84f5b-164">Haga clic con el botón derecho en el **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="84f5b-165">Haga clic en **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-165">Select **Properties**.</span></span> <span data-ttu-id="84f5b-166">Abra la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="84f5b-167">En **Perfil**, seleccione el botón **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="84f5b-168">Asigne el perfil el nombre "IIS" en la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="84f5b-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="84f5b-169">Seleccione **Aceptar** para crear el perfil.</span><span class="sxs-lookup"><span data-stu-id="84f5b-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="84f5b-170">En **Iniciar**, seleccione **IIS** en la lista.</span><span class="sxs-lookup"><span data-stu-id="84f5b-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="84f5b-171">Active la casilla **Iniciar explorador** y proporcione la dirección URL del punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="84f5b-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="84f5b-172">Si la aplicación requiere HTTPS, use un punto de conexión HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="84f5b-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="84f5b-173">Para HTTP, use un punto de conexión HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="84f5b-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="84f5b-174">Proporcione el mismo nombre de host y puerto especificados en la [configuración de IIS establecida en usos anteriores](#configure-iis), normalmente `localhost`.</span><span class="sxs-lookup"><span data-stu-id="84f5b-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="84f5b-175">Proporcione el nombre de la aplicación al final de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="84f5b-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="84f5b-176">Por ejemplo, `https://localhost/WebApplication1` (HTTPS) o `http://localhost/WebApplication1` (HTTP) son direcciones URL de punto de conexión válidas.</span><span class="sxs-lookup"><span data-stu-id="84f5b-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="84f5b-177">En la sección **Variables de entorno**, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="84f5b-178">Proporcione una variable de entorno con un **Nombre** de `ASPNETCORE_ENVIRONMENT` y un **Valor** de `Development`.</span><span class="sxs-lookup"><span data-stu-id="84f5b-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="84f5b-179">En el área **Configuración del servidor web**, defina la **Dirección URL de la aplicación** con el mismo valor utilizado para la dirección URL de punto de conexión de **Iniciar el explorador**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="84f5b-180">Para la configuración del **Modelo de hospedaje** de Visual Studio 2019 o posterior, seleccione **Predeterminado** para usar el modelo de hospedaje utilizado por el proyecto.</span><span class="sxs-lookup"><span data-stu-id="84f5b-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="84f5b-181">Si el proyecto establece la propiedad `<AspNetCoreHostingModel>` en su archivo del proyecto, se usa el valor de la propiedad (`InProcess` o `OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="84f5b-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="84f5b-182">Si la propiedad no existe, se usa el modelo de hospedaje predeterminado de la aplicación, que está fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="84f5b-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="84f5b-183">Si la aplicación requiere una configuración del modelo de hospedaje distinta a la del modelo de hospedaje habitual de la aplicación, defina el **Modelo de hospedaje** como `In Process` o `Out Of Process`, según proceda.</span><span class="sxs-lookup"><span data-stu-id="84f5b-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="84f5b-184">Guarde el perfil.</span><span class="sxs-lookup"><span data-stu-id="84f5b-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="84f5b-185">Si no se usa Visual Studio, agregue manualmente un perfil de inicio al archivo [launchSettings.json](http://json.schemastore.org/launchsettings) en la carpeta *Propiedades*.</span><span class="sxs-lookup"><span data-stu-id="84f5b-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="84f5b-186">El siguiente ejemplo configura el perfil para usar el protocolo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="84f5b-186">The following example configures the profile to use the HTTPS protocol:</span></span>

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

<span data-ttu-id="84f5b-187">Confirme que los puntos de conexión `applicationUrl` y `launchUrl` coinciden y use el mismo protocolo que la configuración de enlace de IIS, es decir, HTTP o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="84f5b-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="84f5b-188">Ejecución del proyecto</span><span class="sxs-lookup"><span data-stu-id="84f5b-188">Run the project</span></span>

<span data-ttu-id="84f5b-189">Ejecute Visual Studio como administrador:</span><span class="sxs-lookup"><span data-stu-id="84f5b-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="84f5b-190">Confirme que la lista desplegable de configuración de compilación está configurada como **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="84f5b-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="84f5b-191">Establezca el botón Ejecutar en el perfil **IIS** y seleccione el botón para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84f5b-191">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="84f5b-192">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="84f5b-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="84f5b-193">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84f5b-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="84f5b-194">Si se usa un certificado de desarrollo que no es de confianza, el explorador puede pedirle que cree una excepción para un certificado de esta clase.</span><span class="sxs-lookup"><span data-stu-id="84f5b-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="84f5b-195">La depuración de una configuración de compilación de versión con [Solo mi código](/visualstudio/debugger/just-my-code) y las optimizaciones del compilador degradan el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="84f5b-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="84f5b-196">Por ejemplo, no se alcanzan los puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="84f5b-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84f5b-197">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="84f5b-197">Additional resources</span></span>

* [<span data-ttu-id="84f5b-198">Introducción al Administrador de IIS en IIS</span><span class="sxs-lookup"><span data-stu-id="84f5b-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="84f5b-199">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="84f5b-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="84f5b-200">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84f5b-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="84f5b-201">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84f5b-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="84f5b-202">Aplicación de HTTPS</span><span class="sxs-lookup"><span data-stu-id="84f5b-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
