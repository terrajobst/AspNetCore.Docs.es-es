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
# <a name="performance-diagnostic-tools"></a>Herramientas de diagnóstico de rendimiento

Por [Mike Rousos](https://github.com/mjrousos)

En este artículo se enumera las herramientas para diagnosticar problemas de rendimiento en ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Herramientas de diagnóstico de Visual Studio

El [herramientas de generación de perfiles y diagnóstico](/visualstudio/profiling) integradas en Visual Studio son un buen lugar para comenzar a investigar los problemas de rendimiento. Estas herramientas son eficaces y convenientes usar desde el entorno de desarrollo de Visual Studio. Permite que las herramientas de análisis de uso de CPU, uso de memoria y los eventos de rendimiento en aplicaciones ASP.NET Core.

Obtener más información está disponible en [documentación de Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) proporciona datos de rendimiento detallados para su aplicación. Application Insights recopila automáticamente datos sobre las tasas de respuesta, tasas de error, tiempos de respuesta de dependencia y más. Application Insights admite el registro de eventos personalizados y métricas específicas en la aplicación.

Application Insights pueden usarse en entornos de varios:

* Optimizado para trabajar en Azure.
* Funciona en producción, desarrollo y almacenamiento provisional.
* Funciona de forma local desde [Visual Studio](/azure/application-insights/app-insights-visual-studio) o en otros entornos de hospedaje.

Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) es una herramienta de análisis de rendimiento creada por el equipo .NET específicamente para diagnosticar problemas de rendimiento. NET. PerfView permite un análisis de CPU uso, memoria y GC comportamiento, los eventos de rendimiento y tiempo de reloj.

Puede aprender más sobre PerfView y cómo empezar a trabajar con [tutoriales en vídeo PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) o mediante la lectura de la Guía de usuario disponible en la herramienta o [en GitHub](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

Aunque PerfView es una herramienta de análisis de rendimiento útil para escenarios. NET, sólo se ejecuta en Windows que no se puede utilizar para recopilar seguimientos de aplicaciones de ASP.NET Core que se ejecutan en entornos Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) es un script de bash que usa las herramientas de generación de perfiles nativos de Linux ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) y [LTTng](https://lttng.org/)) para recopilar seguimientos en Linux que se pueda analizar con PerfView. PerfCollect es útil cuando se muestran problemas de rendimiento en entornos Linux donde PerfView no se puede usar directamente. En su lugar, PerfCollect puede recopilar seguimientos de aplicaciones .NET Core que, a continuación, se analizan en un equipo Windows con PerfView.

Encontrará más información acerca de cómo instalar y empezar a trabajar con PerfCollect [en GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
