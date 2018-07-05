---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Crear direcciones URL legibles en ASP.NET Web Pages (Razor) sitios | Microsoft Docs
author: tfitzmac
description: Este artículo describe el enrutamiento en un sitio Web de ASP.NET Web Pages (Razor), y cómo se puede usar direcciones URL que sean más legibles y mejor para SEO. Deberá...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d027be2924f24c1080deb4ee2deacc235bbd1001
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385753"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Crear direcciones URL legibles en sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe el enrutamiento en un sitio Web de ASP.NET Web Pages (Razor), y cómo se puede usar direcciones URL que sean más legibles y mejor para SEO.
> 
> Lo que aprenderá:
> 
> - Cómo ASP.NET usa el enrutamiento para que le permiten usar más legibles y que se puede buscar las direcciones URL.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


## <a name="about-routing"></a>Acerca del enrutamiento

Las direcciones URL para las páginas de su sitio pueden tener un impacto sobre cómo funciona el sitio. Una dirección URL que &quot;descriptivo&quot; puede ser más fácil para los usuarios a usar el sitio. También puede ayudar con la optimización de motor de búsqueda (SEO) para el sitio. Sitios Web ASP.NET incluyen la capacidad para usar direcciones URL descriptivas automáticamente.

ASP.NET permite crear direcciones URL significativas que describen las acciones del usuario en lugar de simplemente apunta a un archivo en el servidor. Tenga en cuenta estas direcciones URL para un blog ficticio:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Compare estas direcciones URL en las siguientes:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

En el primer par, un usuario tendría que saber que el blog se muestra utilizando el *blog.cshtml* página y, a continuación, tendría que construir una cadena de consulta que obtiene el intervalo derecho de la categoría o fecha. Es mucho más fácil de entender y crear el segundo conjunto de ejemplos.

Las direcciones URL para el primer ejemplo también señalar directamente a un archivo específico (*blog.cshtml*). Si por alguna razón el blog de se han movido a otra carpeta en el servidor, o si se reescribieron el blog para usar una página distinta, los vínculos sería incorrectos. El segundo conjunto de direcciones URL no apunta a una página específica, por lo que incluso si cambia la implementación de blog o la ubicación, las direcciones URL todavía sería válidas.

En ASP.NET Web Pages, puede crear direcciones URL más descriptivas, como aquellos en los ejemplos anteriores ya que ASP.NET utiliza *enrutamiento*. Enrutamiento crea la asignación lógica desde una dirección URL a una página (o páginas) que puede satisfacer la solicitud. Dado que la asignación es lógica (no físico, a un archivo específico), el enrutamiento proporciona gran flexibilidad en cómo definir las direcciones URL para el sitio.

## <a name="how-routing-works"></a>Cómo funciona el enrutamiento

Cuando ASP.NET procesa una solicitud, lee la dirección URL para determinar cómo enrutarlo. ASP.NET intenta hacer coincidir los segmentos individuales de la dirección URL a los archivos de disco, van de izquierda a derecha. Si hay una coincidencia, nada se queda en la dirección URL se pasa a la página como *información de ruta de acceso*.

Imagine que alguien realiza una solicitud con esta dirección URL:

`http://www.contoso.com/a/b/c`

La búsqueda es el siguiente:

1. ¿Hay un archivo con la ruta de acceso y el nombre de */a/b/c.cshtml*? Si es así, ejecute esa página y no pasar ninguna información a él. En caso contrario...
2. ¿Hay un archivo con la ruta de acceso y el nombre de */a/b.cshtml*? Si es así, ejecute esa página y pase el valor `c` a él. En caso contrario...
3. ¿Hay un archivo con la ruta de acceso y el nombre de */a.cshtml*? Si es así, ejecute esa página y pase el valor `b/c` a él.

Si coincide con la búsqueda encuentra no exacta para *.cshtml* archivos en sus carpetas especificadas, ASP.NET continúa buscando estos archivos a su vez:

1. */a/b/c/default.cshtml* (no hay información de ruta de acceso).
2. */a/b/c/index.cshtml* (no hay información de ruta de acceso).

> [!NOTE]
> Para ser claros, las solicitudes de páginas específicas (es decir, las solicitudes que incluyen la *.cshtml* extensión de nombre de archivo) funcionan tal como se esperaría. Una solicitud como `http://www.contoso.com/a/b.cshtml` ejecutará la página *b.cshtml* perfectamente.


Dentro de una página, puede obtener la información de ruta de acceso a través de la página `UrlData` propiedad, que es un diccionario. Imagine que tiene un archivo denominado *ViewCustomers.cshtml* y su sitio obtiene esta solicitud:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Como se describe en las reglas anteriores, la solicitud irá a la página. Dentro de la página, puede usar código similar al siguiente para obtener y mostrar la información de ruta de acceso (en este caso, el valor &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Dado que el enrutamiento no implica que los nombres de archivo completo, puede haber ambigüedad si tiene páginas que tienen el mismo nombre pero diferentes extensiones de nombre de archivo (por ejemplo, *MyPage.cshtml* y *MyPage.html*) . Con el fin de evitar problemas con el enrutamiento, es mejor asegurarse de que no tienen páginas de su sitio cuyos nombres difieren solo en su extensión.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[WebMatrix - direcciones URL, UrlData y enrutamiento de SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Este artículo blog de Mike Brind proporciona algunos detalles adicionales sobre cómo funciona el enrutamiento en ASP.NET Web Pages.
