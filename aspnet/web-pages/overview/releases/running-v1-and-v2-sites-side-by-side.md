---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Con distintas versiones de ASP.NET Web Pages (Razor) en paralelo | Microsoft Docs
author: Rick-Anderson
description: Este artículo explica cómo ejecutar sitios Web de ASP.NET Web Pages (Razor) en el mismo servidor o equipo cuando los sitios Web están configurados para usar diferentes versiones...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: e587398b430795c12a1dcee394852b4e2b8a0e44
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021184"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Ejecución en paralelo diferentes versiones de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo ejecutar sitios Web de ASP.NET Web Pages (Razor) en el mismo servidor o equipo cuando los sitios Web están configurados para usar diferentes versiones de ASP.NET Web Pages.
> 
> Lo que aprenderá:
> 
> - Lo que el comportamiento predeterminado es de ASP.NET cuando tenga sitios creados con ASP.NET Web Pages.
> - Cómo configurar un nuevo sitio se ejecute con una versión anterior de ASP.NET Web Pages.
>   
> 
> Esta es la característica ASP.NET que se introdujo en el artículo:
> 
> - El `webPages:Version` opción de configuración.
>   
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y las páginas Web de ASP.NET 1.0.


ASP.NET Web Pages admite la capacidad para ejecutar sitios Web en paralelo. Esto le permite continuar para ejecutar sus aplicaciones más antiguas de ASP.NET Web Pages, crear nuevas aplicaciones de ASP.NET Web Pages y ejecutar todas ellas en el mismo equipo.

Estas son algunas cosas que recordar cuando se instalación las páginas Web con WebMatrix:

- De forma predeterminada, las aplicaciones existentes de las páginas Web se ejecutarán como la versión más reciente en el equipo. (Los ensamblados se instalan en la caché global de ensamblados (GAC) y se usan automáticamente).
- Si desea ejecutar un sitio con una versión diferente de ASP.NET Web Pages, puede configurar el sitio para hacerlo. Si el sitio ya no tiene un *web.config* de archivos en la raíz del sitio, cree uno y copie el siguiente código XML en él, sobrescribiendo el contenido existente. Si el sitio ya contiene un *web.config* , agregue un `<appSettings>` elemento como el siguiente a la `<configuration>` sección.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Si no especifica ninguna versión en el *web.config* archivo, un sitio se implementa como la versión más reciente. (Los ensamblados se copian en el *bin* carpeta en el sitio implementado.)
- Las aplicaciones nuevas que cree mediante las plantillas de sitio en WebMatrix incluyen los ensamblados de la versión de las páginas Web en el sitio *bin* carpeta.

En general, siempre puede controlar qué versión de las páginas Web para usar con su sitio mediante el uso de NuGet para instalar los ensamblados correspondientes en la carpeta del sitio *bin* carpeta. Para buscar paquetes, visite [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Recursos adicionales

[Las principales características de ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
