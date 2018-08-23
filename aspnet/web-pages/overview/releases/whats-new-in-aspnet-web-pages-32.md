---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Novedades de ASP.NET Web Pages 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 31b462a3116b29770f534fb95b69a29ae665487c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828763"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Novedades de ASP.NET Web Pages 3.2
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe cuáles son las novedades para ASP.NET Web Pages 3.2, páginas Web 3.2.2 y [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Esta versión corrige un error y presenta una nueva característica.

## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [Versionamiento semántico](http://semver.org/) especificación. El paquete de ASP.NET Web Pages 3.2 tiene la siguiente versión: &ldquo;3.2.0&rdquo;. Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). La versión también incluye los correspondientes paquetes localizados en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nueva característica y corrección de errores

Que se ha corregido un error y realiza una mejora de las características secundarias de esta versión. Puede encontrar la consulta para el mismo [aquí](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>3.2.2 de ASP.NET Web Pages

Esta versión acumula el cambio el [versión Beta de ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) que proporciona una mejora significativa del rendimiento en las páginas de razor de gran tamaño de representación. Consulte[Codeplex problema 585](https://aspnetwebstack.codeplex.com/workitem/585). Esta versión se alinea con MVC 5.2.2 paquetes que ahora depende de esta versión.

Trabajamos con el equipo de MSN en representación de páginas grandes. Cuando las páginas representarán más de 80 Kilobytes de datos, terminamos con los objetos del montón de objetos grandes. Cuando se usan varias capas de diseños se puede multiplicar este efecto.

El resultado en el servidor es el uso de CPU adicional, tiempo de retención de la memoria y aún mucho tiempo se pausa durante [Gen 2 limpieza](https://msdn.microsoft.com/en-us/library/ms973837.aspx) en el recolector de elementos no utilizados.

A continuación es una tabla que muestra los resultados de análisis de un [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) para una ejecución. La CPU se mantiene constante en aproximadamente 68%, mientras se están representando páginas grandes. Se muestra en la tabla que se ha eliminado casi por completo el número de recolecciones de generación 2 y el resultado es mayor la tasa de solicitud y una reducción considerable en pausa debido a la recolección de elementos.

| **Área** | **Antes de (3.2)** | **Después de (3.2.1)** | **% De Delta** |
| --- | --- | --- | --- |
| Cantidad total de solicitudes (recuento) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Seguimiento de la duración (segundos) | 196.20 | 198.60 | 1.20% |
| Solicitud por segundo | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Carga de CPU | 68.80% | 68.50% |  -0.40% |
| Ejemplos de CPU de GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Total de asignaciones (recuento) | 55,357,146 | 57,222,949 | 3.40% |
| Total de pausa de GC (ejemplos) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| GC de Gen0 (recuento) | 403 | 1,216 | 201.70% |
| GC de Gen1 (recuento) | 290 | 367 | 26.60% |
| GC Gen2 (recuento) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / (ejemplos/req) de solicitud | 19.73 | 16.47 | -16.50% |

| Codificación en colores: | <font style="background-color: #00ff00">Mejora de Core</font> | <font style="background-color: #4bacc6">Impacto positivo en el rendimiento</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Esta versión contiene correcciones de errores solo. Puede usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver la lista de los problemas corregidos en esta versión.
