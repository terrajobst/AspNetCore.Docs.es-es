---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Novedades de ASP.NET Web Pages 3.2 | Documentos de Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Novedades en las páginas Web ASP.NET 3.2
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe cuáles son las novedades de ASP.NET Web Pages 3.2, páginas Web 3.2.2 y [páginas Web 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>3.2 de ASP.NET Web Pages

Esta versión corrige un error e introduce una nueva característica.

## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [control de versiones semántico](http://semver.org/) especificación. El paquete de ASP.NET Web Pages 3.2 tiene la siguiente versión: &ldquo;3.2.0&rdquo;. Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). La versión también incluye paquetes localizados correspondientes en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nueva característica y corrección de errores

Se ha corregido un error y realiza una mejora de la función secundaria en esta versión. Puede encontrar la consulta para el mismo [aquí](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>3.2.2 de ASP.NET Web Pages

Esta versión acumula el cambio el [versión Beta de ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) que proporciona una mejora del rendimiento importante en representar páginas de razor grandes. Vea[Codeplex problema 585](https://aspnetwebstack.codeplex.com/workitem/585). Esta versión se alinea con MVC 5.2.2 paquetes que dependerán ahora esta versión.

Trabajamos con el equipo MSN representar páginas grandes. Cuando las páginas representar más de 80 Kilobytes de datos, se terminará con objetos del montón de objetos grandes. Cuando se utilizan varias capas de diseños se puede multiplicar este efecto.

El resultado en el servidor es el uso de CPU adicional, retención de más de memoria y aún mucho tiempo se pausa durante [Gen 2 limpieza](https://msdn.microsoft.com/en-us/library/ms973837.aspx) en el recolector de elementos no utilizados.

A continuación se muestra una tabla que muestra los resultados de analizar un [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) para una ejecución. La CPU se mantiene constante en aproximadamente un 68%, mientras se están representando páginas grandes. La tabla muestra que el número de recolecciones de generación 2 se ha eliminado casi por completo, y el resultado es mayor la tasa de solicitud y una reducción considerable en pausa debido a la colección de elementos no utilizados.

| **Área** | **Antes de (3.2)** | **Después de (3.2.1)** | **Delta %** |
| --- | --- | --- | --- |
| Número total de peticiones (recuento) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Duración de seguimiento (segundos) | 196.20 | 198.60 | 1.20% |
| Solicitud por segundo | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Carga de CPU | 68.80% | 68.50% |  -0.40% |
| Ejemplos de CPU de GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Asignaciones totales (recuento) | 55,357,146 | 57,222,949 | 3.40% |
| Total de GC de pausa (ejemplos) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| GC Gen0 (recuento) | 403 | 1,216 | 201.70% |
| GC Gen1 (recuento) | 290 | 367 | 26.60% |
| GC Gen2 (recuento) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / solicitar (ejemplos. / sol.) | 19.73 | 16.47 | -16.50% |

| Codificación en colores: | <font style="background-color: #00ff00">Mejora de núcleo</font> | <font style="background-color: #4bacc6">Impacto positivo en el rendimiento</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Esta versión contiene solo las correcciones de errores. Puede usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver la lista de problemas corregidos en esta versión.
