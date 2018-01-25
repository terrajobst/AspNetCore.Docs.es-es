---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Mostrar mapas en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: "Este artículo explica cómo mostrar los mapas interactivos en páginas en un sitio Web de ASP.NET Web Pages (Razor) en función de asignación de servicios proporcionados por Bing, Google, Ma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 6f3e6a0cfb8c08cd971e88986d0f059dd8237aab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Mostrar mapas en un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo mostrar los mapas interactivos en páginas en un sitio Web de ASP.NET Web Pages (Razor) en función de asignación de servicios proporcionados por Bing, Google, MapQuest y Yahoo.
> 
> Lo que aprenderá:
> 
> - Cómo generar un mapa basándose en una dirección.
> - Cómo generar un mapa basado en coordenadas de latitud y longitud.
> - Cómo registrar una cuenta de desarrollador de mapas de Bing y obtener una clave para usarla con Bing Maps.
> 
> Se trata de la característica ASP.NET que se introdujo en el artículo:
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
> Este tutorial también funciona con 3 de WebMatrix.


En las páginas Web, puede mostrar los mapas en una página mediante el uso de `Maps` auxiliar. Puede generar las asignaciones basadas en una dirección o en un conjunto de coordenadas de longitud y latitud. La `Maps` clase permite llamar a motores de mapa populares como Bing, Google, MapQuest y Yahoo.

Los pasos para agregar la asignación a una página son los mismos independientemente de cuál de los motores de mapa se llama a. Solo tiene que agregar una referencia de archivo de JavaScript que habilita los métodos disponibles para mostrar el mapa y, a continuación, llamar a métodos de la `Maps` auxiliar.

Elija un servicio de mapa en función del `Maps` método auxiliar que se utiliza. Puede utilizar cualquiera de ellos:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalar los elementos que necesarios

Para mostrar los mapas, necesita estos fragmentos:

- El `Maps` auxiliar. Esta aplicación auxiliar es en la versión 2 de ASP.NET Web Helpers Library. Si aún no ha agregado la biblioteca, puede instalarlo en el sitio como un paquete de NuGet. Para obtener más información, consulte [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372). (En la galería, busque el `microsoft-web-helpers` paquete.)
- La biblioteca de jQuery. Varias de las plantillas de sitio de WebMatrix ya incluyen bibliotecas de jQuery en sus *Script* carpetas. Si no tiene estas bibliotecas, puede descargar la biblioteca de jQuery más reciente directamente desde el [jQuery.org](http://jQuery.org) sitio. O bien puede crear un sitio nuevo con una plantilla (por ejemplo, el **Starter Site** plantilla) y, a continuación, copie los archivos de jQuery desde ese sitio al sitio actual.

Por último, si desea usar Bing maps, primero debe crear una cuenta (gratis) y obtener una clave. Para obtener una clave, siga estos pasos:

1. Crear una cuenta en el [cuenta de desarrollador de mapas de Bing](https://www.microsoft.com/maps/developers/web.aspx). Debe tener también una cuenta de Microsoft (Windows Live ID).

    Puede especificar que desea usar la clave de **evaluación y pruebas**. Si va a probar la función de asignación en su propio equipo con WebMatrix y IIS Express, vaya el **sitio** área de trabajo y anote la dirección URL del sitio (por ejemplo, `http://localhost:50408`, aunque el número de puerto probablemente serán diferente). Puede utilizar esta *localhost* dirección que el sitio al registrar.
2. Después de haber registrado para una cuenta, vaya al centro de cuentas de mapas de Bing y haga clic en **permite crear o ver las claves**:

    ![asignación-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Registre la clave que se crea de Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Creación de un mapa basándose en una dirección (con Google)

En el ejemplo siguiente se muestra cómo crear una página que representa un mapa basándose en una dirección. Este ejemplo muestra cómo usar Google Maps.

1. Cree un archivo denominado *MapAddress.cshtml* en la raíz del sitio. Esta página generará un mapa basándose en una dirección que se pasa a él.
2. Copie el código siguiente en el archivo, sobrescribiendo el contenido existente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Tenga en cuenta las siguientes características de la página:

    - El `<script>` elemento en el `<head>` elemento. En el ejemplo, el `<script>` referencias del elemento de la *1.6.4.min.js jquery* archivo, que es una versión reducida (comprimida) de la biblioteca de jQuery, versión 1.6.4. Tenga en cuenta que la referencia se da por supuesto que el *.js* archivo se encuentra en la *Scripts* carpeta de su sitio. 

        > [!NOTE]
        > Si usa una versión diferente de la biblioteca de jQuery, asegúrese de que está señalando a esa versión correctamente.
    - La llamada a la `@Maps.GetGoogleHtml` en el cuerpo de la página. Para asignar una dirección, debe pasar una cadena de dirección. Los métodos para el resto de los motores de asignación de trabajo de una manera similar (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
- Ejecute la página e introduzca una dirección. La página muestra una asignación, en función de Google Maps, que muestra la ubicación que especificó.

    ![asignación-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Creación de un mapa en función de latitud y longitud coordina (con Bing)

En este ejemplo se muestra cómo crear un mapa en función de coordenadas. En este ejemplo se muestra cómo usar Bing maps y cómo incluir la clave de Bing. (Puede crear un mapa en función de coordenadas con el resto de los motores de mapa también, sin usar una clave de Bing.)

1. Cree un archivo denominado *MapCoordinates.cshtml* en la raíz del sitio y reemplazar el contenido existente con el código y el marcado siguiente:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Reemplace `your-key-here` con la clave de mapas de Bing que ha generado anteriormente.
3. Ejecute el *MapCoordinates.cshtml* página, escriba las coordenadas de latitud y longitud y, a continuación, haga clic en el **Map It!** . (Si no conoce las coordenadas, intente lo siguiente. Esta es una ubicación en la sede de Redmond de Microsoft.)

    - Latitud: 47.6781005859375
    - Longitud:-122.158317565918

    Se abrirá la página utilizando las coordenadas que especificó.

    ![asignación-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Referencia de la API de Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
