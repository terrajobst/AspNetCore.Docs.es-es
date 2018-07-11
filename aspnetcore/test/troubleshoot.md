---
title: Solución de problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938399"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="1db61-103">Solución de problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1db61-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="1db61-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1db61-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1db61-105">Los vínculos siguientes proporcionan orientación para la solución:</span><span class="sxs-lookup"><span data-stu-id="1db61-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="1db61-106">Solución de problemas de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1db61-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="1db61-107">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="1db61-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="1db61-108">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1db61-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="1db61-109">Conferencia NDC (London, 2018): Diagnóstico de problemas en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1db61-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="1db61-110">Blog de ASP.NET: Solucionar problemas de rendimiento de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1db61-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="1db61-111">Advertencias de SDK de .NET core</span><span class="sxs-lookup"><span data-stu-id="1db61-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="1db61-112">Se instalan las versiones de 64 bits del SDK de .NET Core y 32 bits</span><span class="sxs-lookup"><span data-stu-id="1db61-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="1db61-113">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="1db61-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="1db61-114">Se instalan las versiones de 32 y 64 bits del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1db61-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="1db61-115">Sólo las plantillas de las versiones de 64 bits instaladas en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="1db61-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="1db61-117">Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [SDK de .NET Core](https://www.microsoft.com/net/download/all) están instalados.</span><span class="sxs-lookup"><span data-stu-id="1db61-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="1db61-118">Causas comunes que se pueden instalar ambas versiones se incluyen:</span><span class="sxs-lookup"><span data-stu-id="1db61-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="1db61-119">Descargar al instalador del SDK de .NET Core con un equipo de 32 bits pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="1db61-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="1db61-120">Se instaló el SDK de .NET Core de 32 bits por otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="1db61-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="1db61-121">Descargado e instalada la versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="1db61-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="1db61-122">Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="1db61-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="1db61-123">Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="1db61-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="1db61-124">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="1db61-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="1db61-125">El SDK de .NET Core está instalado en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="1db61-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="1db61-126">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="1db61-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="1db61-127">El SDK de .NET Core está instalado en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="1db61-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="1db61-128">Sólo las plantillas de los SDK instalados en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="1db61-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="1db61-130">Vea este mensaje cuando haya al menos una instalación de SDK de .NET Core en un directorio fuera de *C:\\archivos de programa\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="1db61-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="1db61-131">Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="1db61-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="1db61-132">Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="1db61-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="1db61-133">Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="1db61-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="1db61-134">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="1db61-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="1db61-135">Se han detectado ningún SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1db61-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="1db61-136">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="1db61-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="1db61-137">Se han detectado ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="1db61-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="1db61-139">Esta advertencia aparece cuando la variable de entorno `PATH` no apunta a ningún SDK de .NET Core en el equipo.</span><span class="sxs-lookup"><span data-stu-id="1db61-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="1db61-140">Para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="1db61-140">To resolve this problem:</span></span>

* <span data-ttu-id="1db61-141">Instalar o comprobar que está instalado el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1db61-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="1db61-142">Compruebe que el `PATH` variable de entorno se apunta a la ubicación donde está instalado el SDK.</span><span class="sxs-lookup"><span data-stu-id="1db61-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="1db61-143">El instalador se establece normalmente el `PATH`.</span><span class="sxs-lookup"><span data-stu-id="1db61-143">The installer normally sets the `PATH`.</span></span>
