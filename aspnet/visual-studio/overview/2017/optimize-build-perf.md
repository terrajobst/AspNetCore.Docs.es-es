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
# <a name="optimize-build-performance-for-solution"></a>Optimizar el rendimiento de la compilación para la solución
15,8 de Visual Studio 2017 y más adelante se agrega un nuevo elemento de menú en **compilar > compilación de ASP.NET > optimizar el rendimiento de compilación para la solución**.

![captura de pantalla del nuevo elemento de menú](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compila sus vistas en tiempo de ejecución, lo que significa que el proyecto ASP.NET conlleva una copia del compilador. Sin embargo, en una máquina de desarrollo cuando la copia del compilador no coincide con la copia de Visual Studio, el rendimiento de la compilación se ve afectado del orden de 1 a 3 segundos por compilación incremental. Esta característica actualizará la copia del proyecto del compilador para que coincida con Visual Studio que se debe acelerar las compilaciones incrementales.

Esto es aplicable a los proyectos de ASP.NET Framework solo, no se aplica a ASP.NET Core.
