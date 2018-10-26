---
title: Solución de problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090116"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="999d7-103">Solución de problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="999d7-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="999d7-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="999d7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="999d7-105">Los vínculos siguientes proporcionan orientación para la solución:</span><span class="sxs-lookup"><span data-stu-id="999d7-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="999d7-106">Conferencia NDC (London, 2018): Diagnóstico de problemas en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="999d7-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="999d7-107">Blog de ASP.NET: Solucionar problemas de rendimiento de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="999d7-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="999d7-108">Advertencias de SDK de .NET core</span><span class="sxs-lookup"><span data-stu-id="999d7-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="999d7-109">Se instalan las versiones de 64 bits del SDK de .NET Core y 32 bits</span><span class="sxs-lookup"><span data-stu-id="999d7-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="999d7-110">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="999d7-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="999d7-111">Se instalan las versiones de 32 y 64 bits del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="999d7-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="999d7-112">Sólo las plantillas de las versiones de 64 bits instaladas en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="999d7-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="999d7-114">Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [SDK de .NET Core](https://www.microsoft.com/net/download/all) están instalados.</span><span class="sxs-lookup"><span data-stu-id="999d7-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="999d7-115">Causas comunes que se pueden instalar ambas versiones se incluyen:</span><span class="sxs-lookup"><span data-stu-id="999d7-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="999d7-116">Descargar al instalador del SDK de .NET Core con un equipo de 32 bits pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="999d7-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="999d7-117">Se instaló el SDK de .NET Core de 32 bits por otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="999d7-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="999d7-118">Descargado e instalada la versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="999d7-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="999d7-119">Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="999d7-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="999d7-120">Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="999d7-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="999d7-121">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="999d7-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="999d7-122">El SDK de .NET Core está instalado en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="999d7-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="999d7-123">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="999d7-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="999d7-124">El SDK de .NET Core está instalado en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="999d7-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="999d7-125">Sólo las plantillas de los SDK instalados en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="999d7-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="999d7-127">Vea este mensaje cuando haya al menos una instalación de SDK de .NET Core en un directorio fuera de *C:\\archivos de programa\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="999d7-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="999d7-128">Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="999d7-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="999d7-129">Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="999d7-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="999d7-130">Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="999d7-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="999d7-131">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="999d7-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="999d7-132">Se han detectado ningún SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="999d7-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="999d7-133">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="999d7-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="999d7-134">Se han detectado ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="999d7-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="999d7-136">Esta advertencia aparece cuando la variable de entorno `PATH` no apunta a ningún SDK de .NET Core en el equipo.</span><span class="sxs-lookup"><span data-stu-id="999d7-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="999d7-137">Para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="999d7-137">To resolve this problem:</span></span>

* <span data-ttu-id="999d7-138">Instalar o comprobar que está instalado el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="999d7-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="999d7-139">Compruebe que el `PATH` variable de entorno se apunta a la ubicación donde está instalado el SDK.</span><span class="sxs-lookup"><span data-stu-id="999d7-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="999d7-140">El instalador se establece normalmente el `PATH`.</span><span class="sxs-lookup"><span data-stu-id="999d7-140">The installer normally sets the `PATH`.</span></span>
