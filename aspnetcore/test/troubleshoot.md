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
uid: test/troubleshoot
ms.openlocfilehash: 64a353a9bdf0753c63f676f9d07f42ba45acdcab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217647"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="1a525-103">Solucionar problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a525-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="1a525-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a525-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a525-105">Los vínculos siguientes proporcionan instrucciones para solucionar problemas:</span><span class="sxs-lookup"><span data-stu-id="1a525-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="1a525-106">Solución de problemas de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1a525-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="1a525-107">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="1a525-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="1a525-108">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a525-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="1a525-109">YouTube: Diagnóstico de problemas en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a525-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="1a525-110">Advertencias de .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="1a525-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="1a525-111">Se instalan las versiones de 64 bits de .NET Core SDK y 32 bits</span><span class="sxs-lookup"><span data-stu-id="1a525-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="1a525-112">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="1a525-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="1a525-114">Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [.NET Core SDK](https://www.microsoft.com/net/download/all) están instalados.</span><span class="sxs-lookup"><span data-stu-id="1a525-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="1a525-115">Los motivos comunes que se pueden instalar ambas versiones son:</span><span class="sxs-lookup"><span data-stu-id="1a525-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="1a525-116">Descargar al instalador de .NET Core SDK con un equipo de 32 bits, pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="1a525-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="1a525-117">El SDK de núcleo de .NET de 32 bits se instaló por otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="1a525-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="1a525-118">Se ha descargado e instalado una versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="1a525-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="1a525-119">Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="1a525-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="1a525-120">Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="1a525-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="1a525-121">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="1a525-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="1a525-122">.NET Core SDK está instalado en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="1a525-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="1a525-123">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core verá la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="1a525-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="1a525-124">.NET Core SDK está instalado en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="1a525-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="1a525-125">Sola plantillas desde el SDK(s) instalado en ' C:\Program Files\dotnet\sdk\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="1a525-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="1a525-127">Verá este mensaje cuando se tiene al menos una instalación de .NET Core SDK en un directorio fuera de * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="1a525-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="1a525-128">Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="1a525-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="1a525-129">Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="1a525-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="1a525-130">Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="1a525-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="1a525-131">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="1a525-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="1a525-132">No se detectaron ningún SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1a525-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="1a525-133">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core verá la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="1a525-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="1a525-134">**No se detectaron ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno 'PATH'**</span><span class="sxs-lookup"><span data-stu-id="1a525-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="1a525-136">Esta advertencia aparece cuando la variable de entorno `PATH` no apunta a los SDK de núcleo de .NET en el equipo.</span><span class="sxs-lookup"><span data-stu-id="1a525-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="1a525-137">Para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="1a525-137">To resolve this problem:</span></span>

* <span data-ttu-id="1a525-138">Instalar o comprobar que está instalado el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a525-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="1a525-139">Compruebe el `PATH` variable de entorno apunta a la ubicación que está instalado el SDK.</span><span class="sxs-lookup"><span data-stu-id="1a525-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="1a525-140">El instalador normalmente establece el `PATH`.</span><span class="sxs-lookup"><span data-stu-id="1a525-140">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a><span data-ttu-id="1a525-141">Uso de IHtmlHelper.Partial puede producir interbloqueos de aplicación</span><span class="sxs-lookup"><span data-stu-id="1a525-141">Use of IHtmlHelper.Partial may result in application deadlocks</span></span>

<span data-ttu-id="1a525-142">En ASP.NET Core 2.1 y versiones posteriores, al llamar a `Html.Partial` da como resultado una advertencia de analizador debido a la posibilidad de interbloqueos.</span><span class="sxs-lookup"><span data-stu-id="1a525-142">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="1a525-143">Es el mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="1a525-143">The warning message is:</span></span>

<span data-ttu-id="1a525-144">*Uso de IHtmlHelper.Partial puede producir interbloqueos de la aplicación. Considere el uso de `<partial>` auxiliar de etiqueta o `IHtmlHelper.PartialAsync`.*</span><span class="sxs-lookup"><span data-stu-id="1a525-144">*Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.*</span></span>

<span data-ttu-id="1a525-145">Las llamadas a `@Html.Partial` deben reemplazarse por `@await Html.PartialAsync` o la aplicación auxiliar de la etiqueta parcial `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="1a525-145">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
