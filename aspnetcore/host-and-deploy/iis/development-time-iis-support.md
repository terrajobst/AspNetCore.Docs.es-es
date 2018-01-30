---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra la compatibilidad para depurar aplicaciones de ASP.NET Core al ejecutarlas detrás de IIS en Windows Server."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="ecbac-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecbac-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="ecbac-104">De: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="ecbac-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="ecbac-105">En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) para depurar aplicaciones de ASP.NET Core ejecutadas detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ecbac-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="ecbac-106">Este tema le guía a través de habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="ecbac-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ecbac-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ecbac-107">Prerequisites</span></span>

* <span data-ttu-id="ecbac-108">Visual Studio (2017, versión 15.3 o una posterior)</span><span class="sxs-lookup"><span data-stu-id="ecbac-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="ecbac-109">Carga de trabajo de desarrollo de ASP.NET y web *O BIEN* la carga de trabajo de desarrollo multiplataforma de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ecbac-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="ecbac-110">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="ecbac-110">Enable IIS</span></span>

<span data-ttu-id="ecbac-111">Habilitar IIS.</span><span class="sxs-lookup"><span data-stu-id="ecbac-111">Enable IIS.</span></span> <span data-ttu-id="ecbac-112">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="ecbac-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="ecbac-113">Seleccione la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="ecbac-113">Select the **Internet Information Services** checkbox.</span></span>

![Características de Windows en las que se muestra la casilla Internet Information Services marcada con un cuadrado negro (no una marca de verificación) que indica que algunas de las características de IIS están habilitadas](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="ecbac-115">Si la instalación de IIS requiere un reinicio, reinicie el sistema.</span><span class="sxs-lookup"><span data-stu-id="ecbac-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="ecbac-116">Habilitar compatibilidad con IIS en tiempo de desarrollo</span><span class="sxs-lookup"><span data-stu-id="ecbac-116">Enable development-time IIS support</span></span>

<span data-ttu-id="ecbac-117">Una vez instalado IIS, inicie el instalador de Visual Studio para modificar la instalación existente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ecbac-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="ecbac-118">En el instalador, seleccione el componente **Compatibilidad con IIS en tiempo de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="ecbac-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="ecbac-119">Aparece como componente opcional en el panel **Resumen** de la carga de trabajo **Desarrollo de ASP.NET y web**.</span><span class="sxs-lookup"><span data-stu-id="ecbac-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="ecbac-120">Se instalará el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que es un módulo de IIS nativo necesario para ejecutar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecbac-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="ecbac-124">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="ecbac-124">Configure the project</span></span>

<span data-ttu-id="ecbac-125">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ecbac-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="ecbac-126">En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="ecbac-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="ecbac-127">Seleccione la pestaña **Depurar**. Seleccione **IIS** en el menú desplegable **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="ecbac-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="ecbac-128">Confirme que la característica **Iniciar explorador** esté habilitada con la URL correcta.</span><span class="sxs-lookup"><span data-stu-id="ecbac-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="ecbac-133">O bien, agregar manualmente un perfil de inicio para la [launchSettings.json](http://json.schemastore.org/launchsettings) archivo en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ecbac-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="ecbac-134">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="ecbac-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="ecbac-135">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ecbac-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="ecbac-136">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="ecbac-136">Congratulations!</span></span> <span data-ttu-id="ecbac-137">En este momento, el proyecto está configurado para admitir IIS de tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ecbac-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ecbac-138">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ecbac-138">Additional resources</span></span>

* [<span data-ttu-id="ecbac-139">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="ecbac-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="ecbac-140">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecbac-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="ecbac-141">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecbac-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
