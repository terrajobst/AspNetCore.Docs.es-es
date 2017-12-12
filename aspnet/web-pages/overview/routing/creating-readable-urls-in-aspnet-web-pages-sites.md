---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Crear direcciones URL legibles en ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft
author: tfitzmac
description: "Este artículo describe el enrutamiento en un sitio Web de ASP.NET Web Pages (Razor), y cómo esto le permite usar las direcciones URL que son más legibles y mejor para motores de búsqueda. Deberá..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Crear direcciones URL legibles en sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe el enrutamiento en un sitio Web de ASP.NET Web Pages (Razor), y cómo esto le permite usar las direcciones URL que son más legibles y mejor para motores de búsqueda.
> 
> Lo que aprenderá:
> 
> - Cómo ASP.NET usa el enrutamiento para permitir el uso más direcciones URL legibles y que admiten búsquedas.
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

Las direcciones URL de las páginas de su sitio pueden tener un impacto en el grado en que funciona el sitio. Una dirección URL que &quot;descriptivo&quot; puede ser de ayuda para los usuarios puedan usar el sitio. También puede ayudar con la optimización de motor de búsqueda (SEO) para el sitio. Sitios Web ASP.NET incluye la capacidad para usar direcciones URL descriptivas automáticamente.

ASP.NET le permite crear direcciones URL significativas que describen las acciones del usuario en lugar de simplemente que apunta a un archivo en el servidor. Tenga en cuenta estas direcciones URL para un blog ficticio:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Compare estas direcciones URL a los siguientes:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

En el primer par, un usuario tendría que conocer que el blog se muestra mediante el *blog.cshtml* página y, a continuación, tendría que crear una cadena de consulta que obtiene el intervalo derecho de categoría o de fecha. Es mucho más fácil de entender y crear el segundo conjunto de ejemplos.

Las direcciones URL para el primer ejemplo también apuntan directamente a un archivo específico (*blog.cshtml*). Si por alguna razón el blog se han movido a otra carpeta en el servidor, o si el blog reescrito para usar una página distinta, los vínculos sería incorrectos. El segundo conjunto de direcciones URL no apunta a una página específica, por lo que incluso si cambia la implementación de blog o la ubicación, las direcciones URL se seguirán siendo válidas.

En ASP.NET Web Pages, puede crear direcciones URL más descriptivas similares a las de los ejemplos anteriores ya que ASP.NET usa *enrutamiento*. Enrutamiento crea la asignación lógica de una dirección URL a una página (o páginas) que puede satisfacer la solicitud. Dado que la asignación es lógica (no físico, en un archivo específico), el enrutamiento proporciona gran flexibilidad en cómo definir las direcciones URL para el sitio.

## <a name="how-routing-works"></a>Cómo funciona el enrutamiento

Cuando ASP.NET procesa una solicitud, lee la dirección URL para determinar cómo enrutarlo. ASP.NET intenta hacer coincidir los segmentos individuales de la dirección URL a los archivos de disco, van de izquierda a derecha. Si hay una coincidencia, cualquier elemento restante en la dirección URL se pasa a la página como *información de ruta de acceso*.

Imagine que un usuario realiza una solicitud con esta dirección URL:

`http://www.contoso.com/a/b/c`

La búsqueda se pasa como el siguiente:

1. ¿Hay un archivo con la ruta de acceso y el nombre de */a/b/c.cshtml*? Si es así, ejecute esa página y no pasándole ninguna información. En caso contrario...
2. ¿Hay un archivo con la ruta de acceso y el nombre de */a/b.cshtml*? En caso afirmativo, ejecute esa página y pase el valor `c` a él. En caso contrario...
3. ¿Hay un archivo con la ruta de acceso y el nombre de */a.cshtml*? En caso afirmativo, ejecute esa página y pase el valor `b/c` a él.

Si coincide con la búsqueda encuentra no exacta para *.cshtml* archivos en sus carpetas especificadas, ASP.NET continúa buscando estos archivos a su vez:

1. */a/b/c/default.cshtml* (no hay información de ruta de acceso).
2. */a/b/c/index.cshtml* (no hay información de ruta de acceso).

> [!NOTE]
> Para estar claro, las solicitudes de páginas específicas (es decir, las solicitudes que incluyen la *.cshtml* extensión de nombre de archivo) funcionan tal como se esperaría. Al igual que una solicitud `http://www.contoso.com/a/b.cshtml` se ejecutará la página *b.cshtml* correctamente.


Dentro de una página, puede obtener la información de ruta de acceso a través de la página `UrlData` propiedad, que es un diccionario. Imagine que tiene un archivo denominado *ViewCustomers.cshtml* y su sitio recibe esta solicitud:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Como se describe en las reglas anteriores, la solicitud irá a la página. Dentro de la página, puede utilizar código similar al siguiente para obtener y mostrar la información de ruta de acceso (en este caso, el valor &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Dado que el enrutamiento no implica nombres de archivo completos, puede haber ambigüedad si tiene páginas que tienen el mismo nombre pero diferentes extensiones de nombre de archivo (por ejemplo, *MyPage.cshtml* y *MyPage.html*) . Para evitar problemas con el enrutamiento, es mejor asegurarse de que no tienen páginas de su sitio cuyos nombres solo difieren en su extensión.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[WebMatrix - UrlData, las direcciones URL y el enrutamiento para motores de búsqueda](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Esta entrada de blog de Mike Brind proporciona algunos detalles adicionales sobre cómo funciona el enrutamiento de ASP.NET Web Pages.
