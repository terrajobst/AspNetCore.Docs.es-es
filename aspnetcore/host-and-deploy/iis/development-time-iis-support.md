---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubrir compatibilidad para depurar aplicaciones de ASP.NET Core cuando se ejecuta detrás de IIS en Windows Server."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="4a754-103">Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a754-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="4a754-104">De: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="4a754-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="4a754-105">Este artículo se describen [Visual Studio](https://www.visualstudio.com/vs/) la compatibilidad para depurar aplicaciones de ASP.NET Core que se ejecutan detrás de IIS en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4a754-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="4a754-106">Este tema le guía a través de habilitar esta característica y configurar un proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a754-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a754-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4a754-107">Prerequisites</span></span>

* <span data-ttu-id="4a754-108">Visual Studio (2017, versión 15.3 o una posterior)</span><span class="sxs-lookup"><span data-stu-id="4a754-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="4a754-109">Carga de trabajo de desarrollo de ASP.NET y web *O BIEN* la carga de trabajo de desarrollo multiplataforma de .NET Core</span><span class="sxs-lookup"><span data-stu-id="4a754-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="4a754-110">Habilitar IIS</span><span class="sxs-lookup"><span data-stu-id="4a754-110">Enable IIS</span></span>

<span data-ttu-id="4a754-111">Habilitar IIS.</span><span class="sxs-lookup"><span data-stu-id="4a754-111">Enable IIS.</span></span> <span data-ttu-id="4a754-112">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="4a754-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="4a754-113">Seleccione la casilla **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="4a754-113">Select the **Internet Information Services** checkbox.</span></span>

![Características de Windows en las que se muestra la casilla Internet Information Services marcada con un cuadrado negro (no una marca de verificación) que indica que algunas de las características de IIS están habilitadas](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="4a754-115">Si la instalación de IIS requiere un reinicio, reinicie el sistema.</span><span class="sxs-lookup"><span data-stu-id="4a754-115">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="4a754-116">Habilitar compatibilidad con IIS en tiempo de desarrollo</span><span class="sxs-lookup"><span data-stu-id="4a754-116">Enable development-time IIS support</span></span>

<span data-ttu-id="4a754-117">Inicie al instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a754-117">Launch the Visual Studio installer.</span></span> <span data-ttu-id="4a754-118">Seleccione el **IIS compatible con el tiempo de desarrollo** componente.</span><span class="sxs-lookup"><span data-stu-id="4a754-118">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="4a754-119">El componente se enumera como opcionales en la **resumen** panel para la **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="4a754-119">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="4a754-120">Esto instala los [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module), que es un módulo nativo de IIS necesario para ejecutar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a754-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="4a754-124">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="4a754-124">Configure the project</span></span>

<span data-ttu-id="4a754-125">Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4a754-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="4a754-126">En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="4a754-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="4a754-127">Seleccione la pestaña **Depurar**. Seleccione **IIS** en el menú desplegable **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="4a754-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="4a754-128">Confirme que la característica **Iniciar explorador** esté habilitada con la URL correcta.</span><span class="sxs-lookup"><span data-stu-id="4a754-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="4a754-133">O bien, agregar manualmente un perfil de inicio para la [launchSettings.json](http://json.schemastore.org/launchsettings) archivo en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4a754-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="4a754-134">Visual Studio puede solicitar un reinicio si no se ejecuta como administrador.</span><span class="sxs-lookup"><span data-stu-id="4a754-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="4a754-135">Si es así, reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a754-135">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a754-136">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4a754-136">Additional resources</span></span>

* [<span data-ttu-id="4a754-137">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="4a754-137">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="4a754-138">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a754-138">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="4a754-139">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a754-139">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
