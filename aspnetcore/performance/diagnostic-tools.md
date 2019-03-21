---
title: Herramientas de diagnóstico de rendimiento
author: mjrousos
description: Herramientas útiles para diagnosticar problemas de rendimiento en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 0fb9944d1082090f4f58f641a26ee96286920456
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210007"
---
# <a name="performance-diagnostic-tools"></a>Herramientas de diagnóstico de rendimiento

Por [Mike Rousos](https://github.com/mjrousos)

En este artículo se enumera las herramientas para diagnosticar problemas de rendimiento en ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Herramientas de diagnóstico de Visual Studio

El [herramientas de generación de perfiles y diagnóstico](/visualstudio/profiling) integradas en Visual Studio son un buen lugar para comenzar a investigar los problemas de rendimiento. Estas herramientas son eficaces y convenientes usar desde el entorno de desarrollo de Visual Studio. Permite que las herramientas de análisis de uso de CPU, uso de memoria y los eventos de rendimiento en aplicaciones ASP.NET Core. Está integrado hace que sea fácil de generación de perfiles en tiempo de desarrollo.

Obtener más información está disponible en [documentación de Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) proporciona datos de rendimiento detallados para su aplicación. Application Insights recopila automáticamente datos sobre las tasas de respuesta, tasas de error, tiempos de respuesta de dependencia y más. Application Insights admite el registro de eventos personalizados y métricas específicas en la aplicación.

Azure Application Insights proporciona varias maneras de ofrecer información en las aplicaciones supervisadas:

- [Mapa de aplicación](/azure/application-insights/app-insights-app-map) : ayuda a los cuellos de botella de rendimiento detectar o zonas activas de un error en todos los componentes de aplicaciones distribuidas.
- [Hoja de métricas en el portal de Application Insights](/azure/application-insights/app-insights-metrics-explorer?toc=/azure/azure-monitor/toc.json) muestra valores medidos y recuentos de eventos.
- [Hoja de rendimiento en el portal de Application Insights](/azure/application-insights/app-insights-tutorial-performance):

  - Muestra detalles de rendimiento de distintas operaciones en la aplicación supervisada.
  - Permite la obtención de detalles de una sola operación para comprobar todos los elementos o dependencias que contribuyen a un período prolongado.
  - Profiler puede invocarse desde aquí para recopilar seguimientos de rendimiento a petición.

- [De Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) permite regular o a petición de generación de perfiles de aplicaciones .NET.  Azure portal muestra habían capturado seguimientos de rendimiento con pilas de llamadas y rutas de acceso activas. También se pueden descargar los archivos de seguimiento para un análisis más profundo con PerfView.

Application Insights pueden usarse en entornos de varios:

- Optimizado para trabajar en Azure.
- Funciona en producción, desarrollo y almacenamiento provisional.
- Funciona de forma local desde [Visual Studio](/azure/application-insights/app-insights-visual-studio) o en otros entornos de hospedaje.

Para más información, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) es una herramienta de análisis de rendimiento creada por el equipo .NET específicamente para diagnosticar problemas de rendimiento. NET. PerfView permite un análisis de CPU uso, memoria y GC comportamiento, los eventos de rendimiento y tiempo de reloj.

Puede aprender más sobre PerfView y cómo empezar a trabajar con [tutoriales en vídeo PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) o mediante la lectura de la Guía de usuario disponible en la herramienta o [en GitHub](https://github.com/Microsoft/perfview).

## <a name="windows-performance-toolkit"></a>Kit de herramientas de rendimiento de Windows

[Windows Performance Toolkit](/windows-hardware/test/wpt/) (WPT) consta de dos componentes: Windows Performance Recorder (WPR) y Windows Performance Analyzer (WPA). Las herramientas generan perfiles detallados de rendimiento de aplicaciones y sistemas operativos de Windows. WPT tiene más formas de visualizar los datos, pero están menos eficaces que el de PerfView sus recopilación de datos.

## <a name="perfcollect"></a>PerfCollect

Aunque PerfView es una herramienta de análisis de rendimiento útil para escenarios. NET, sólo se ejecuta en Windows que no se puede utilizar para recopilar seguimientos de aplicaciones de ASP.NET Core que se ejecutan en entornos Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) es un script de bash que usa las herramientas de generación de perfiles nativos de Linux ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) y [LTTng](https://lttng.org/)) para recopilar seguimientos en Linux que se pueda analizar con PerfView. PerfCollect es útil cuando se muestran problemas de rendimiento en entornos Linux donde PerfView no se puede usar directamente. En su lugar, PerfCollect puede recopilar seguimientos de aplicaciones .NET Core que, a continuación, se analizan en un equipo Windows con PerfView.

Encontrará más información acerca de cómo instalar y empezar a trabajar con PerfCollect [en GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).

## <a name="other-third-party-performance-tools"></a>Otras herramientas de rendimiento de aplicaciones de terceros

A continuación enumeran algunas herramientas de rendimiento de aplicaciones de terceros que son útiles en investigación de rendimiento de aplicaciones de .NET Core.

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace y dotMemory de JetBrains
- VTune de Intel
