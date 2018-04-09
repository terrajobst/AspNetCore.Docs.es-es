---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: Descubrir compatibilidad para depurar aplicaciones de ASP.NET Core cuando se ejecuta detrás de IIS en Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="237ef-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="237ef-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="237ef-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="237ef-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="237ef-105">Este artículo se describen [Visual Studio](https://www.visualstudio.com/vs/) la compatibilidad para depurar aplicaciones de ASP.NET Core que se ejecutan detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="237ef-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="237ef-106">Este tema le guía a través de habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="237ef-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="237ef-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="237ef-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="237ef-108">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="237ef-108">Enable IIS</span></span>

<span data-ttu-id="237ef-109">Habilitar IIS.</span><span class="sxs-lookup"><span data-stu-id="237ef-109">Enable IIS.</span></span> <span data-ttu-id="237ef-110">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="237ef-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="237ef-111">Seleccione la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="237ef-111">Select the **Internet Information Services** checkbox.</span></span>

![Características de Windows en las que se muestra la casilla Internet Information Services marcada con un cuadrado negro (no una marca de verificación) que indica que algunas de las características de IIS están habilitadas](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="237ef-113">Si la instalación de IIS requiere un reinicio, reinicie el sistema.</span><span class="sxs-lookup"><span data-stu-id="237ef-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="237ef-114">Habilitar compatibilidad con IIS en tiempo de desarrollo</span><span class="sxs-lookup"><span data-stu-id="237ef-114">Enable development-time IIS support</span></span>

<span data-ttu-id="237ef-115">Inicie al instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="237ef-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="237ef-116">Seleccione el **IIS compatible con el tiempo de desarrollo** componente.</span><span class="sxs-lookup"><span data-stu-id="237ef-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="237ef-117">El componente se enumera como opcionales en la **resumen** panel para la **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="237ef-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="237ef-118">Esto instala los [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module), que es un módulo nativo de IIS necesario para ejecutar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="237ef-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="237ef-122">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="237ef-122">Configure the project</span></span>

<span data-ttu-id="237ef-123">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="237ef-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="237ef-124">En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="237ef-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="237ef-125">Seleccione la pestaña **Depurar**. Seleccione **IIS** en el menú desplegable **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="237ef-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="237ef-126">Confirme que la característica **Iniciar explorador** esté habilitada con la URL correcta.</span><span class="sxs-lookup"><span data-stu-id="237ef-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="237ef-131">O bien, agregar manualmente un perfil de inicio para la [launchSettings.json](http://json.schemastore.org/launchsettings) archivo en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="237ef-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="237ef-132">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="237ef-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="237ef-133">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="237ef-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="237ef-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="237ef-134">Additional resources</span></span>

* [<span data-ttu-id="237ef-135">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="237ef-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="237ef-136">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="237ef-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="237ef-137">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="237ef-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
