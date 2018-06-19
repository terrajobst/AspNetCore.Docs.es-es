---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Con distintas versiones de ASP.NET Web Pages (Razor) en paralelo | Documentos de Microsoft
author: tfitzmac
description: Este artículo explica cómo ejecutar sitios Web de ASP.NET Web Pages (Razor) en el mismo servidor o equipo cuando los sitios Web están configurados para usar una versión diferente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898411"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Ejecutar en paralelo de distintas versiones de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo explica cómo ejecutar sitios Web de ASP.NET Web Pages (Razor) en el mismo servidor o equipo cuando los sitios Web están configurados para usar diferentes versiones de ASP.NET Web Pages.
> 
> Lo que aprenderá:
> 
> - Lo que el comportamiento predeterminado es en ASP.NET si tiene sitios creados con ASP.NET Web Pages.
> - Cómo configurar un sitio nuevo para que se ejecute con una versión anterior de ASP.NET Web Pages.
>   
> 
> Se trata de la característica ASP.NET que se introdujo en el artículo:
> 
> - El `webPages:Version` de configuración.
>   
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y páginas Web de ASP.NET 1.0.


ASP.NET Web Pages es compatible con la capacidad de ejecutar sitios Web en paralelo. Esto le permite continuar ejecutar sus aplicaciones de ASP.NET Web Pages más antiguas, crear nuevas aplicaciones de ASP.NET Web Pages y ejecutar todas ellas en el mismo equipo.

Estas son algunas cosas que recordar cuando se instalación las páginas Web con WebMatrix:

- De forma predeterminada, las aplicaciones existentes de páginas Web se ejecutarán como la versión más reciente en el equipo. (Los ensamblados se instalan en la caché de ensamblados global (GAC) y se usan automáticamente.)
- Si desea ejecutar un sitio con una versión diferente de las páginas Web ASP.NET, puede configurar el sitio para hacerlo. Si el sitio ya no tiene un *web.config* un archivo en la raíz del sitio, cree uno nuevo y copie el siguiente código XML en él, sobrescribiendo el contenido existente. Si el sitio ya contiene un *web.config* , agregue un `<appSettings>` elemento como el siguiente a la `<configuration>` sección.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Si no especifica ninguna versión en el *web.config* archivo, un sitio se implementa como la versión más reciente. (Los ensamblados se copian en el *bin* carpeta en el sitio implementado.)
- Las aplicaciones nuevas que cree mediante las plantillas de sitio en Web Matrix incluyen los ensamblados de la versión de las páginas Web en el sitio *bin* carpeta.

En general, siempre puede controlar qué versión de páginas Web para utilizar con su sitio mediante el uso de NuGet para instalar los ensamblados correspondientes en el sitio *bin* carpeta. Para buscar paquetes, visite [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Recursos adicionales

[Las principales características de ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
