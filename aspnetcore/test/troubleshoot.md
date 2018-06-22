---
title: Solucionar problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274598"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="884e4-103">Solucionar problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="884e4-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="884e4-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="884e4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="884e4-105">Los vínculos siguientes proporcionan instrucciones para solucionar problemas:</span><span class="sxs-lookup"><span data-stu-id="884e4-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="884e4-106">Solución de problemas de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="884e4-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="884e4-107">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="884e4-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="884e4-108">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="884e4-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="884e4-109">Conferencia NDC (Londres, 2018): Diagnosticar problemas en aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="884e4-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="884e4-110">Blog de ASP.NET: Solucionar problemas de rendimiento de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="884e4-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="884e4-111">Advertencias de .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="884e4-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="884e4-112">Se instalan las versiones de 64 bits de .NET Core SDK y 32 bits</span><span class="sxs-lookup"><span data-stu-id="884e4-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="884e4-113">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="884e4-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="884e4-114">Se instalan las versiones de 32 y 64 bits de .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="884e4-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="884e4-115">Sólo las plantillas de las versiones de 64 bits instaladas en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="884e4-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="884e4-117">Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [.NET Core SDK](https://www.microsoft.com/net/download/all) están instalados.</span><span class="sxs-lookup"><span data-stu-id="884e4-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="884e4-118">Ambas versiones pueden instalarse de motivos comunes son:</span><span class="sxs-lookup"><span data-stu-id="884e4-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="884e4-119">Descargar al instalador de .NET Core SDK con un equipo de 32 bits pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="884e4-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="884e4-120">El SDK de núcleo de .NET de 32 bits se instaló por otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="884e4-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="884e4-121">Se ha descargado e instalado una versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="884e4-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="884e4-122">Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="884e4-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="884e4-123">Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="884e4-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="884e4-124">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="884e4-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="884e4-125">.NET Core SDK está instalado en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="884e4-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="884e4-126">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="884e4-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="884e4-127">.NET Core SDK está instalado en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="884e4-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="884e4-128">Sólo las plantillas de la SDK(s) instalado en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="884e4-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="884e4-130">Verá este mensaje cuando se tiene al menos una instalación de .NET Core SDK en un directorio fuera de *C:\\archivos de programa\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="884e4-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="884e4-131">Normalmente esto sucede cuando se ha implementado el SDK de .NET Core en una máquina con copiar y pegar en lugar del instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="884e4-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="884e4-132">Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="884e4-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="884e4-133">Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="884e4-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="884e4-134">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="884e4-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="884e4-135">No se detectaron ningún SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="884e4-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="884e4-136">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="884e4-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="884e4-137">No se detectaron ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="884e4-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="884e4-139">Esta advertencia aparece cuando la variable de entorno `PATH` no apunta a los SDK de núcleo de .NET en el equipo.</span><span class="sxs-lookup"><span data-stu-id="884e4-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="884e4-140">Para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="884e4-140">To resolve this problem:</span></span>

* <span data-ttu-id="884e4-141">Instalar o comprobar que está instalado el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="884e4-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="884e4-142">Compruebe el `PATH` variable de entorno apunta a la ubicación que está instalado el SDK.</span><span class="sxs-lookup"><span data-stu-id="884e4-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="884e4-143">El instalador normalmente establece el `PATH`.</span><span class="sxs-lookup"><span data-stu-id="884e4-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="884e4-144">Uso de IHtmlHelper.Partial puede producir interbloqueos de aplicación</span><span class="sxs-lookup"><span data-stu-id="884e4-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="884e4-145">En ASP.NET Core 2.1 y versiones posteriores, al llamar a `Html.Partial` da como resultado una advertencia de analizador debido a la posibilidad de interbloqueos.</span><span class="sxs-lookup"><span data-stu-id="884e4-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="884e4-146">Es el mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="884e4-146">The warning message is:</span></span>

> <span data-ttu-id="884e4-147">Uso de IHtmlHelper.Partial puede producir interbloqueos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="884e4-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="884e4-148">Considere el uso de `<partial>` auxiliar de etiqueta o `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="884e4-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="884e4-149">Las llamadas a `@Html.Partial` deben reemplazarse por `@await Html.PartialAsync` o la aplicación auxiliar de la etiqueta parcial `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="884e4-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
