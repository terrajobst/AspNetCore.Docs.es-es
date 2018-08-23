---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimizar el rendimiento de la compilación para la solución
author: tfitzmac
description: Optimizar el rendimiento de la compilación para la solución
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909891"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="9c648-103">Optimizar el rendimiento de la compilación para la solución</span><span class="sxs-lookup"><span data-stu-id="9c648-103">Optimize build performance for solution</span></span>
<span data-ttu-id="9c648-104">15,8 de Visual Studio 2017 y más adelante se agrega un nuevo elemento de menú en **compilar > compilación de ASP.NET > optimizar el rendimiento de compilación para la solución**.</span><span class="sxs-lookup"><span data-stu-id="9c648-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![captura de pantalla del nuevo elemento de menú](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="9c648-106">ASP.NET compila sus vistas en tiempo de ejecución, lo que significa que el proyecto ASP.NET conlleva una copia del compilador.</span><span class="sxs-lookup"><span data-stu-id="9c648-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="9c648-107">Sin embargo, en una máquina de desarrollo cuando la copia del compilador no coincide con la copia de Visual Studio, el rendimiento de la compilación se ve afectado del orden de 1 a 3 segundos por compilación incremental.</span><span class="sxs-lookup"><span data-stu-id="9c648-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="9c648-108">Esta característica actualizará la copia del proyecto del compilador para que coincida con Visual Studio que se debe acelerar las compilaciones incrementales.</span><span class="sxs-lookup"><span data-stu-id="9c648-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="9c648-109">Esto es aplicable a los proyectos de ASP.NET Framework solo, no se aplica a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c648-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
