---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimizar el rendimiento de la compilación para la solución
author: AngelosP
description: Optimizar el rendimiento de la compilación para la solución
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312146"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="bf10e-103">Optimizar el rendimiento de la compilación para la solución</span><span class="sxs-lookup"><span data-stu-id="bf10e-103">Optimize build performance for solution</span></span>

<span data-ttu-id="bf10e-104">15,8 de Visual Studio 2017 o versiones posteriores incluyen un elemento de menú: **compilar** > **compilación de ASP.NET** > **optimizar el rendimiento de compilación para la solución**.</span><span class="sxs-lookup"><span data-stu-id="bf10e-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![captura de pantalla del nuevo elemento de menú](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="bf10e-106">ASP.NET compila sus vistas en tiempo de ejecución, lo que significa que un proyecto de ASP.NET conlleva una copia del compilador.</span><span class="sxs-lookup"><span data-stu-id="bf10e-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="bf10e-107">Sin embargo en una máquina de desarrollo cuando la copia del compilador no coincide con la copia de Visual Studio, rendimiento de la compilación se ve afectado del orden de 1 a 3 segundos por compilación incremental.</span><span class="sxs-lookup"><span data-stu-id="bf10e-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="bf10e-108">Esta característica actualiza la copia del proyecto del compilador para que coincida con Visual Studio, lo que normalmente se acelera las compilaciones incrementales.</span><span class="sxs-lookup"><span data-stu-id="bf10e-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="bf10e-109">**Esto es aplicable al marco de ASP.NET 4.7.1 o posterior solo para proyectos, no se aplica a ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="bf10e-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
