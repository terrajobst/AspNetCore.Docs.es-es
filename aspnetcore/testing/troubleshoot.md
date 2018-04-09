---
title: Solucionar problemas principales de ASP.NET
author: Rick-Anderson
description: Entender y solucionar problemas de advertencias y errores con los proyectos de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="402bd-103">Solucionar problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="402bd-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="402bd-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="402bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="402bd-105">Los vínculos siguientes proporcionan instrucciones para solucionar problemas:</span><span class="sxs-lookup"><span data-stu-id="402bd-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="402bd-106">Solución de problemas de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="402bd-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="402bd-107">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="402bd-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="402bd-108">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="402bd-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="402bd-109">Advertencias de .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="402bd-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="402bd-110">Ambas ediciones de 32 y 64 bits de .NET Core SDK se instalan las versiones</span><span class="sxs-lookup"><span data-stu-id="402bd-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="402bd-111">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la siguiente advertencia aparece en la parte superior:</span><span class="sxs-lookup"><span data-stu-id="402bd-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="402bd-113">Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [.NET Core SDK](https://www.microsoft.com/net/download/all) están instalados.</span><span class="sxs-lookup"><span data-stu-id="402bd-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="402bd-114">Los motivos comunes que se pueden instalar ambas versiones son:</span><span class="sxs-lookup"><span data-stu-id="402bd-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="402bd-115">Descargar al instalador de .NET Core SDK con un equipo de 32 bits, pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="402bd-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="402bd-116">El SDK de núcleo de .NET de 32 bits se instaló por otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="402bd-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="402bd-117">Se ha descargado e instalado una versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="402bd-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="402bd-118">Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="402bd-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="402bd-119">Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="402bd-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="402bd-120">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="402bd-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="402bd-121">.NET Core SDK está instalado en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="402bd-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="402bd-122">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core puede ver la siguiente advertencia aparece en la parte superior:</span><span class="sxs-lookup"><span data-stu-id="402bd-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="402bd-123">.NET Core SDK está instalado en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="402bd-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="402bd-124">Sola plantillas desde el SDK(s) instalado en ' C:\Program Files\dotnet\sdk\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="402bd-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="402bd-126">Está viendo este mensaje porque tiene al menos una instalación de .NET Core SDK en un directorio fuera de * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="402bd-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="402bd-127">Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="402bd-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="402bd-128">Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="402bd-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="402bd-129">Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="402bd-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="402bd-130">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="402bd-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
