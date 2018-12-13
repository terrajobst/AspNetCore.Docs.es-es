---
title: Herramientas de diagnóstico de rendimiento
author: mjrousos
description: Herramientas útiles para diagnosticar problemas de rendimiento en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329208"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="ac63c-103">Herramientas de diagnóstico de rendimiento</span><span class="sxs-lookup"><span data-stu-id="ac63c-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="ac63c-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="ac63c-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="ac63c-105">En este artículo se enumera las herramientas para diagnosticar problemas de rendimiento en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac63c-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="ac63c-106">Herramientas de diagnóstico de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac63c-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="ac63c-107">El [herramientas de generación de perfiles y diagnóstico](/visualstudio/profiling) integradas en Visual Studio son un buen lugar para comenzar a investigar los problemas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ac63c-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="ac63c-108">Estas herramientas son eficaces y convenientes usar desde el entorno de desarrollo de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac63c-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="ac63c-109">Permite que las herramientas de análisis de uso de CPU, uso de memoria y los eventos de rendimiento en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac63c-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="ac63c-110">Obtener más información está disponible en [documentación de Visual Studio](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="ac63c-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="ac63c-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="ac63c-111">Application Insights</span></span>

<span data-ttu-id="ac63c-112">[Application Insights](/azure/application-insights/app-insights-overview) proporciona datos de rendimiento detallados para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="ac63c-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="ac63c-113">Application Insights recopila automáticamente datos sobre las tasas de respuesta, tasas de error, tiempos de respuesta de dependencia y más.</span><span class="sxs-lookup"><span data-stu-id="ac63c-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="ac63c-114">Application Insights admite el registro de eventos personalizados y métricas específicas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ac63c-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="ac63c-115">Application Insights pueden usarse en entornos de varios:</span><span class="sxs-lookup"><span data-stu-id="ac63c-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="ac63c-116">Optimizado para trabajar en Azure.</span><span class="sxs-lookup"><span data-stu-id="ac63c-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="ac63c-117">Funciona en producción, desarrollo y almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="ac63c-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="ac63c-118">Funciona de forma local desde [Visual Studio](/azure/application-insights/app-insights-visual-studio) o en otros entornos de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ac63c-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="ac63c-119">Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="ac63c-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="ac63c-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="ac63c-120">PerfView</span></span>

<span data-ttu-id="ac63c-121">[PerfView](https://github.com/Microsoft/perfview) es una herramienta de análisis de rendimiento creada por el equipo .NET específicamente para diagnosticar problemas de rendimiento. NET.</span><span class="sxs-lookup"><span data-stu-id="ac63c-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="ac63c-122">PerfView permite un análisis de CPU uso, memoria y GC comportamiento, los eventos de rendimiento y tiempo de reloj.</span><span class="sxs-lookup"><span data-stu-id="ac63c-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="ac63c-123">Puede aprender más sobre PerfView y cómo empezar a trabajar con [tutoriales en vídeo PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) o mediante la lectura de la Guía de usuario disponible en la herramienta o [en GitHub](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="ac63c-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="ac63c-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="ac63c-124">PerfCollect</span></span>

<span data-ttu-id="ac63c-125">Aunque PerfView es una herramienta de análisis de rendimiento útil para escenarios. NET, sólo se ejecuta en Windows que no se puede utilizar para recopilar seguimientos de aplicaciones de ASP.NET Core que se ejecutan en entornos Linux.</span><span class="sxs-lookup"><span data-stu-id="ac63c-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="ac63c-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) es un script de bash que usa las herramientas de generación de perfiles nativos de Linux ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) y [LTTng](https://lttng.org/)) para recopilar seguimientos en Linux que se pueda analizar con PerfView.</span><span class="sxs-lookup"><span data-stu-id="ac63c-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="ac63c-127">PerfCollect es útil cuando se muestran problemas de rendimiento en entornos Linux donde PerfView no se puede usar directamente.</span><span class="sxs-lookup"><span data-stu-id="ac63c-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="ac63c-128">En su lugar, PerfCollect puede recopilar seguimientos de aplicaciones .NET Core que, a continuación, se analizan en un equipo Windows con PerfView.</span><span class="sxs-lookup"><span data-stu-id="ac63c-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="ac63c-129">Encontrará más información acerca de cómo instalar y empezar a trabajar con PerfCollect [en GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="ac63c-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
