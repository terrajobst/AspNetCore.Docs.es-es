---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Mostrar mapas en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: Este artículo explica cómo mostrar mapas interactivos en las páginas de un sitio Web de ASP.NET Web Pages (Razor) según la asignación de servicios proporcionados por Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368840"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Mostrar mapas en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo mostrar mapas interactivos en las páginas de un sitio Web de ASP.NET Web Pages (Razor) según la asignación de servicios proporcionados por Bing, Google, Yahoo y MapQuest.
> 
> Lo que aprenderá:
> 
> - Cómo generar una asignación basada en una dirección.
> - Cómo generar un mapa en función de las coordenadas de latitud y longitud.
> - Cómo registrar una cuenta de desarrollador de Bing Maps y obtener una clave que se usará con Bing Maps.
> 
> Esta es la característica ASP.NET que se introdujo en el artículo:
> 
> - El `Maps` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con WebMatrix 3.


En las páginas Web, puede mostrar mapas en una página mediante `Maps` auxiliar. Puede generar asignaciones basadas en una dirección o en un conjunto de coordenadas de longitud y latitud. La `Maps` clase le permite llamar a los motores de mapa populares como Bing, Google, Yahoo y MapQuest.

Los pasos para agregar la asignación a una página son el mismo independientemente de cuál de los motores de mapa que llamar a. Solo tiene que agregar una referencia de archivo de JavaScript que hace que los métodos disponibles para mostrar el mapa y, a continuación, llamar a métodos de la `Maps` auxiliar.

Elija un servicio de asignación basado en el que `Maps` método auxiliar que se utiliza. Puede usar cualquiera de estos:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalación de los elementos que necesarios

Para mostrar los mapas, necesita estos elementos:

- El `Maps` auxiliar. Esta aplicación auxiliar es en la versión 2 de la biblioteca de aplicaciones auxiliares de Web de ASP.NET. Si aún no ha agregado la biblioteca, puede instalarlo en su sitio como un paquete de NuGet. Para obtener más información, consulte [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372). (En la galería, busque el `microsoft-web-helpers` paquete.)
- La biblioteca de jQuery. Varias de las plantillas de sitio de WebMatrix ya incluyen bibliotecas de jQuery en su *Script* carpetas. Si no tiene estas bibliotecas, puede descargar la biblioteca de jQuery más reciente directamente desde el [jQuery.org](http://jQuery.org) sitio. O bien puede crear un nuevo sitio mediante una plantilla (por ejemplo, el **Starter Site** plantilla) y, a continuación, copie los archivos de jQuery desde ese sitio a sitio actual.

Por último, si desea usar Bing maps, primero debe crear una cuenta (gratis) y obtener una clave. Para obtener una clave, siga estos pasos:

1. Crear una cuenta en el [cuenta para desarrolladores de Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). Debe tener también una cuenta de Microsoft (Windows Live ID).

    Puede especificar que desea usar la clave para **evaluación y pruebas**. Si va a probar la función de asignación en su propio equipo con WebMatrix y IIS Express, vaya el **sitio** área de trabajo y anote la dirección URL del sitio (por ejemplo, `http://localhost:50408`, aunque el número de puerto probablemente será diferente). Puede usar esto *localhost* dirección como el sitio al registrar.
2. Después de haber registrado una cuenta, vaya al centro de cuentas de Bing Maps y haga clic en **crear o ver claves**:

    ![asignación de 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Anote la clave que se crea de Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Creación de una asignación basada en una dirección (con Google)

El ejemplo siguiente muestra cómo crear una página que representa una asignación basada en una dirección. En este ejemplo se muestra cómo usar Google Maps.

1. Cree un archivo denominado *MapAddress.cshtml* en la raíz del sitio. Esta página generará una asignación basada en una dirección que se pasa a él.
2. Copie el código siguiente en el archivo, sobrescribiendo el contenido existente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Tenga en cuenta las siguientes características de la página:

    - El `<script>` elemento en el `<head>` elemento. En el ejemplo, el `<script>` referencias del elemento de la *jquery 1.6.4.min.js* archivo, que es una versión reducida (comprimida) de la biblioteca de jQuery, versión 1.6.4. Tenga en cuenta que la referencia se da por supuesto que el *.js* archivo se encuentra en la *Scripts* carpeta del sitio. 

        > [!NOTE]
        > Si usa una versión diferente de la biblioteca de jQuery, asegúrese de que se apunta a esa versión correctamente.
    - La llamada a la `@Maps.GetGoogleHtml` en el cuerpo de la página. Para asignar una dirección, debe pasar una cadena de dirección. Los métodos para los otros motores de mapa funcionan de manera similar (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Ejecute la página y escriba una dirección. La página muestra una asignación, en función de Google Maps, que muestra la ubicación que especificó.

     ![asignación de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Creación de una asignación basada en latitud y longitud coordina (con Bing)

En este ejemplo se muestra cómo crear un mapa en función de las coordenadas. En este ejemplo se muestra cómo usar mapas de Bing y cómo incluir la clave de Bing. (Puede crear un mapa en función de las coordenadas con otros motores de mapa también, sin usar una clave de Bing).

1. Cree un archivo denominado *MapCoordinates.cshtml* en la raíz del sitio y reemplace el contenido existente con el código y el marcado siguiente:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Reemplace `your-key-here` con la clave de Bing Maps que ha generado anteriormente.
3. Ejecute el *MapCoordinates.cshtml* página, escriba las coordenadas de latitud y longitud y, a continuación, haga clic en el **Map It!** . (Si no conoce las coordenadas, intente lo siguiente. Esta es una ubicación en el campus Redmond de Microsoft.)

   - Latitud: 47.6781005859375
   - Longitud:-122.158317565918

     Se abrirá la página utilizando las coordenadas que especificó.

     ![asignación de 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Referencia de la API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
