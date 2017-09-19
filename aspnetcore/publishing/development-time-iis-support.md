---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra la compatibilidad para depurar aplicaciones de ASP.NET Core al ejecutarlas detrás de IIS en Windows Server."
keywords: "ASP.NET Core,internet information services,iis,Windows Server,módulo asp.net core,depuración"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: f531d90646b9d261c5fbbffcecd6ded9185ae292
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="424eb-104">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="424eb-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="424eb-105">De: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="424eb-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="424eb-106">En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) para depurar aplicaciones de ASP.NET Core ejecutadas detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="424eb-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="424eb-107">En este tema se explica cómo habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="424eb-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="424eb-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="424eb-108">Prerequisites</span></span>

* <span data-ttu-id="424eb-109">Visual Studio (2017, versión 15.3 o una posterior)</span><span class="sxs-lookup"><span data-stu-id="424eb-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="424eb-110">Carga de trabajo de desarrollo de ASP.NET y web *O BIEN* la carga de trabajo de desarrollo multiplataforma de .NET Core</span><span class="sxs-lookup"><span data-stu-id="424eb-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="424eb-111">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="424eb-111">Enable IIS</span></span>

<span data-ttu-id="424eb-112">Habilite IIS en su sistema.</span><span class="sxs-lookup"><span data-stu-id="424eb-112">Enable IIS on your system.</span></span> <span data-ttu-id="424eb-113">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="424eb-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="424eb-114">Seleccione la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="424eb-114">Select the **Internet Information Services** checkbox.</span></span>

![Características de Windows en las que se muestra la casilla Internet Information Services marcada con un cuadrado negro (no una marca de verificación) que indica que algunas de las características de IIS están habilitadas](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="424eb-116">Si la instalación de IIS lo requiere, reinicie el sistema.</span><span class="sxs-lookup"><span data-stu-id="424eb-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="424eb-117">Habilitar compatibilidad con IIS en tiempo de desarrollo</span><span class="sxs-lookup"><span data-stu-id="424eb-117">Enable development-time IIS support</span></span>

<span data-ttu-id="424eb-118">Una vez que haya instalado IIS, inicie el Instalador de Visual Studio para modificar la instalación de Visual Studio existente.</span><span class="sxs-lookup"><span data-stu-id="424eb-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="424eb-119">En el instalador, seleccione el componente **Compatibilidad con IIS en tiempo de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="424eb-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="424eb-120">Aparece como componente opcional en el panel **Resumen** de la carga de trabajo **Desarrollo de ASP.NET y web**.</span><span class="sxs-lookup"><span data-stu-id="424eb-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="424eb-121">Se instalará el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que es un módulo de IIS nativo necesario para ejecutar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="424eb-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="424eb-125">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="424eb-125">Configure the project</span></span>

<span data-ttu-id="424eb-126">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="424eb-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="424eb-127">En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="424eb-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="424eb-128">Seleccione la pestaña **Depurar**. Seleccione **IIS** en el menú desplegable **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="424eb-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="424eb-129">Confirme que la característica **Iniciar explorador** esté habilitada con la URL correcta.</span><span class="sxs-lookup"><span data-stu-id="424eb-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="424eb-134">Como alternativa, puede agregar manualmente un perfil de inicio para su archivo [launchSettings.json](http://json.schemastore.org/launchsettings) en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="424eb-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="424eb-135">Es posible que se le pida que reinicie Visual Studio si no lo estaba ejecutando como administrador.</span><span class="sxs-lookup"><span data-stu-id="424eb-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="424eb-136">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="424eb-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="424eb-137">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="424eb-137">Congratulations!</span></span> <span data-ttu-id="424eb-138">En este momento, el proyecto está configurado para la compatibilidad con IIS en tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="424eb-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="424eb-139">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="424eb-139">Additional resources</span></span>

* [<span data-ttu-id="424eb-140">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="424eb-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="424eb-141">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="424eb-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="424eb-142">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="424eb-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
