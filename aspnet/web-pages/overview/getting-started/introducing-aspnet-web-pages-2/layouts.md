---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Introducción a ASP.NET Web Pages: crear un diseño coherente | Microsoft Docs'
author: tfitzmac
description: Este tutorial muestra cómo utilizar los diseños para crear una apariencia coherente para las páginas de un sitio que use ASP.NET Web Pages. Supone que ha completado la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 3368a3e3c9dc56b98ca0adddf4ffb181c7b34863
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379452"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introducción a las páginas Web ASP.NET - crear un diseño coherente
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo usar *diseños* para crear una apariencia coherente para las páginas en un sitio que usa ASP.NET Web Pages. Supone que ha completado la serie a través de [eliminando la base de datos en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Lo que aprenderá:
> 
> - Es lo que una página de diseño.
> - Cómo se combinan las páginas de diseño con contenido dinámico.
> - Cómo pasar valores a una página de diseño.


## <a name="about-layouts"></a>Acerca de los diseños

Las páginas que ha creado hasta ahora han sido todo haya finalizado, páginas independientes. Todas pertenecen al mismo sitio, pero no tienen un aspecto estándar o todos los elementos comunes.

Mayoría de los sitios tienen una apariencia coherente y un diseño. Por ejemplo, si se dirige a la [Microsoft.com/Web](https://www.microsoft.com/web/) de sitio y mire, verá que las páginas de todos los adhieran a un diseño global a un tema visual:

![Página del sitio Microsoft.com/Web que muestra el diseño del encabezado, área de navegación, área de contenido y pie de página](layouts/_static/image1.png)

Un *ineficaz* forma para crear este diseño sería definir un encabezado, la barra de navegación y el pie de página por separado en cada una de las páginas. Se podría duplicar el mismo marcado cada vez. Si desea cambiar algo (por ejemplo, actualice el pie de página), tendría que cambiar por separado de cada página.

Es ahí donde *las páginas de diseño* venir. En ASP.NET Web Pages, puede definir una página de diseño que proporciona un contenedor global para las páginas de su sitio. Por ejemplo, la página de diseño puede contener el encabezado, el área de navegación y el pie de página. La página de diseño incluye un marcador de posición donde se sitúa el contenido principal.

A continuación, puede definir las páginas de contenido individuales que contienen el marcado y el código solo para esa página. Las páginas de contenido no tienen que estar páginas HTML completas; ni siquiera es necesario tener un `<body>` elemento. También tienen una línea de código que indica qué página de diseño que desea mostrar el contenido ASP.NET. Aquí es una imagen que muestra aproximadamente cómo funciona esta relación:

![Diagrama conceptual que se muestra dos páginas de contenido y una página de diseño en la que se ajustan](layouts/_static/image2.png)

Esta interacción es fácil de entender cuando lo vea en acción. En este tutorial, cambiará las páginas de películas para usar un diseño.

## <a name="adding-a-layout-page"></a>Agregar una página de diseño

Comenzará creando una página de diseño que define un diseño de página típico con un encabezado, pie de página y un área para el contenido principal. En el sitio WebPagesMovies, agregue una página CSHTML denominada  *\_Layout.cshtml*.

El carácter de subrayado inicial ( `_` ) caracteres es significativo. Si el nombre de la página se inicia con un carácter de subrayado, ASP.NET no enviar esa página directamente al explorador. Esta convención permite definir las páginas que son necesarias para un sitio, pero que los usuarios no deberían poder solicitar directamente.

Reemplace el contenido de la página con lo siguiente:

[!code-html[Main](layouts/samples/sample1.html)]

Como puede ver, este marcado es solo código HTML que usa `<div>` elementos para definir tres secciones en la página más uno más `<div>` elemento que contenga las tres secciones. El pie de página contiene un poco de código Razor: `@DateTime.Now.Year`, que representará el año actual en esa ubicación en la página.

Tenga en cuenta que hay un vínculo a una hoja de estilos denominada *Movies.css*. La hoja de estilos es donde se definirán los detalles de la distribución física de los elementos. Va a crear en un momento.

La característica solo inusual en este  *\_Layout.cshtml* página es la `@Render.Body()` línea. Que es el marcador de posición donde irá el contenido cuando este diseño se combina con otra página.

## <a name="adding-a-css-file"></a>Agregar un archivo .css

La mejor manera de definir la disposición real (es decir, apariencia) de los elementos de la página es usar las reglas en cascada (CSS) de la hoja de estilo. Por lo que crearé un *.css* archivo con las reglas para el nuevo diseño.

En WebMatrix, seleccione la raíz del sitio. A continuación, en el **archivos** pestaña de la cinta de opciones, haga clic en la flecha situada bajo la **New** botón y, a continuación, haga clic en **nueva carpeta**.

![La opción 'Nueva carpeta' debajo de nuevo en la cinta de opciones.](layouts/_static/image3.png)

Nombre de la nueva carpeta *estilos*.

!['La nueva carpeta los estilos de nombres '](layouts/_static/image4.png)

Dentro de la nueva *estilos* carpeta, cree un archivo denominado *Movies.css*.

![Crear un nuevo archivo Movies.css](layouts/_static/image5.png)

Reemplace el contenido del nuevo *.css* archivo por lo siguiente:

[!code-css[Main](layouts/samples/sample2.css)]

No se dice mucho acerca de estas reglas CSS, excepto para destacar dos cosas. Una es que además de configurar las fuentes y tamaños, las reglas de usan el posicionamiento absoluto para establecer la ubicación del encabezado, pie de página y área de contenido principal. Si está familiarizado con la colocación de CSS, puede leer el [posicionamiento de CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial en el sitio W3Schools.

La otra cosa a tener en cuenta que en la parte inferior, hemos copiado las reglas de estilo que estaban originalmente se define por separado en el *Movies.cshtml* archivo. Estas reglas se usaron en el [Introducción a los datos mostrar por ASP.NET Web Pages de uso](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para realizar la `WebGrid` auxiliar representar el marcado que agrega franjas a la tabla. (Si va a usar un *.css* archivo para definiciones de estilo, también puede poner las reglas de estilo para todo el sitio en él.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Actualizando el archivo de películas para usar el diseño

Ahora puede actualizar los archivos existentes en el sitio para usar el nuevo diseño. Abra el *Movies.cshtml* archivo. En la parte superior, como la primera línea de código, agregue lo siguiente:

[!code-csharp[Main](layouts/samples/sample3.cs)]

La página empieza ahora de esta manera:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Esta línea de código indica a ASP.NET que, cuando el *películas* página se ejecuta, se debe combinar con el  *\_Layout.cshtml* archivo.

Puesto que la *Movies.cshtml* archivo ahora usa una página de diseño, puede quitar el marcado de la *Movies.cshtml* página que se ha ocupado de por el  *\_Layout.cshtml*archivo. Saque el `<!DOCTYPE>`, `<html>`, y `<body>` de apertura y cierre de las etiquetas. Todo el contenido se lleve `<head>` elemento y su contenido, que incluye las reglas de estilo de la cuadrícula, ya que ahora tienes esas reglas un *.css* archivo. Mientras está en ello, cambie el existente `<h1>` elemento a una `<h2>` elemento; debe tener un `<h1>` elemento en la página de diseño ya. Cambiar el `<h2>` texto a "Lista de películas".

Normalmente no tiene que realizar estos tipos de cambios en una página de contenido. Al empezar su sitio con una página de diseño, que empiece por crear páginas de contenido sin todos estos elementos. En este caso, no obstante, va a convertir una página independiente a otro que usa un diseño, por lo que es un poco de limpieza.

Cuando haya terminado, la *Movies.cshtml* página tendrá un aspecto similar al siguiente:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Probar el diseño

Ahora puede ver el aspecto del diseño. En WebMatrix, haga clic en el *Movies.cshtml* página y seleccione **iniciar en el explorador**. Cuando el explorador muestra la página, se parece a esta página:

![Representado mediante un diseño de página de películas](layouts/_static/image6.png)

ASP.NET se ha combinado el contenido de la página Movies.cshtml en la  *\_Layout.cshtml* página derecha donde el `RenderBody` es el método. Y por supuesto el  *\_Layout.cshtml* página referencias una *.css* archivo que define el aspecto de la página.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Actualizar la página AddMovie para usar el diseño

La verdadera ventaja de diseños es que puede usarlos para todas las páginas del sitio. Abra el *AddMovie.cshtml* página.

Es posible que recuerde que el *AddMovie.cshtml* página originalmente tenía algunas reglas CSS en él para definir la apariencia de los mensajes de error de validación. Puesto que tiene un *.css* archivo para su sitio ahora, puede mover esas reglas para la *.css* archivo. Quitar el *AddMovie.cshtml* de archivos y agregarlos a la parte inferior de la *Movies.css* archivo. Va a mover las siguientes reglas:

[!code-css[Main](layouts/samples/sample6.css)]

Ahora que las ordenaciones de cambios en el mismas *AddMovie.cshtml* que siguió para *Movies.cshtml* : agregar `Layout="~/_Layout.cshtml;` y quite la marca HTML que ahora está relacionada. Cambie el elemento `<h1>` por `<h2>`. Cuando haya terminado, la página tendrá un aspecto similar a este ejemplo:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Ejecute la página. Parece ahora a esta ilustración:

![Representado mediante un diseño de página 'Agregar películas'](layouts/_static/image7.png)

Desea realizar modificaciones similares a las páginas del sitio: *EditMovie.cshtml* y *DeleteMovie.cshtml*. Sin embargo, antes de hacerlo, puede realizar otro cambio en el diseño de manera un poco más flexibles.

## <a name="passing-title-information-to-the-layout-page"></a>Pasar información del título a la página de diseño

El  *\_Layout.cshtml* página que ha creado tiene un `<title>` elemento que se establece en "Mi sitio de película". La mayoría de los exploradores mostrar el contenido de este elemento como el texto en una pestaña:

![La página &lt;título&gt; elemento aparece en una pestaña del explorador](layouts/_static/image8.png)

Esta información de título es genérica. Suponga que desea que el texto del título para ser más específico a la página actual. (El texto del título es también usa los motores de búsqueda para determinar cuál es la página acerca de.) Puede pasar información de una página de contenido como *Movies.cshtml* o *AddMovie.cshtml* a la página de diseño y, a continuación, usar esa información para personalizar la página de diseño de qué representa.

Abra el *Movies.cshtml* página nuevo. En el código en la parte superior, agregue la siguiente línea:

[!code-csharp[Main](layouts/samples/sample8.cs)]

El `Page` está disponible en todos los objetos *.cshtml* páginas y es para este propósito, es decir, para compartir información entre una página y su diseño.

Abra el<em>\_Layout.cshtml</em> página. Cambiar el `<title>` elemento para que quede como este marcado:

[!code-html[Main](layouts/samples/sample9.html)]

Este código representa cualquier cosa que esté en el `Page.Title` propiedad directamente en esa ubicación en la página.

Ejecute el *Movies.cshtml* página. Esta vez la pestaña del explorador muestra la información que pasa como el valor de `Page.Title`:

![Una pestaña del explorador que muestra el título que se crean de forma dinámica](layouts/_static/image9.png)

Si lo desea, puede ver el origen de la página en el explorador. Puede ver que el `<title>` elemento se representa como `<title>List Movies</title>`.

> [!TIP] 
> 
> **El objeto de página**
> 
> Una característica útil de `Page` es que es un objeto dinámico, el `Title` propiedad no es un nombre reservado o fijo. Puede usar *cualquier* nombre para un valor de la `Page` objeto. Por ejemplo, podría fácilmente ha pasado el título mediante el uso de una propiedad denominada `Page.CurrentName` o `Page.MyPage`. La única restricción es que el nombre debe seguir las reglas normales para qué propiedades se pueden denominar. (Por ejemplo, el nombre no puede contener un espacio).
> 
> Puede pasar cualquier número de valores mediante la `Page` objeto. Si desea pasar información de películas a la página de diseño, podría pasar valores mediante el uso de algo parecido a `Page.MovieTitle` y `Page.Genre` y `Page.MovieYear`. (O cualquier otro nombre que inventaron para almacenar la información.) El único requisito, que es probablemente obvio, es que se debe usar los mismos nombres en la página de contenido y la página de diseño.
> 
> La información que se pasa mediante el uso de la `Page` objeto no se limita a simplemente texto que se muestra en la página de diseño. Puede pasar un valor a la página de diseño y, a continuación, el código en la página de diseño puede utilizar el valor para decidir si se muestra una sección de la página, lo que *.css* archivo para usar y así sucesivamente. Los valores pasados en el `Page` objeto son como cualquier otro los valores que se utiliza en código. Simplemente es que los valores se originan en la página de contenido y se pasan a la página de diseño.


Abra el *AddMovie.cshtml* página y agregue una línea a la parte superior del código que proporciona un título para el *AddMovie.cshtml* página:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Ejecute el *AddMovie.cshtml* página. Consulte el nuevo título:

![Una pestaña del explorador que muestra el título de 'Agregar películas' creado dinámicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Actualización de las páginas restantes para usar el diseño

Ahora puede finalizar el resto de páginas en el sitio para que usen el nuevo diseño. Abra *EditMovie.cshtml* y *DeleteMovie.cshtml* a su vez y realizar los mismos cambios en cada uno.

Agregue la línea de código que se vincula a la página de diseño:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Agregue una línea para establecer el título de la página:

[!code-csharp[Main](layouts/samples/sample12.cs)]

O bien

[!code-csharp[Main](layouts/samples/sample13.cs)]

Quitar todo el marcado HTML extraño, básicamente, deje sólo las partes que hay dentro de la `<body>` elemento (más en el bloque de código en la parte superior).

Cambiar el `<h1>` elemento para que sea un `<h2>` elemento.

Cuando haya realizado estos cambios, probar cada una y asegúrese de que se muestra correctamente y que el título es correcto.

## <a name="parting-thoughts-about-layout-pages"></a>Partición ideas acerca de las páginas de diseño

En este tutorial ha creado un  *\_Layout.cshtml* página y utiliza el `RenderBody` método para combinar el contenido de otra página. Que es el patrón básico para el uso de los diseños de páginas Web.

Las páginas de diseño tienen características adicionales que no tratamos aquí. Por ejemplo, puede anidar las páginas de diseño, una página de diseño a su vez puede hacerse referencia sí. Los diseños anidados pueden ser útiles si está trabajando con las subsecciones de un sitio que requieren diseños diferentes. También puede usar métodos adicionales (por ejemplo, `RenderSection`) para configurar denominado secciones en la página de diseño.

La combinación de las páginas de diseño y *.css* archivos es eficaz. Como verá en la siguiente serie de tutoriales, en WebMatrix puede crear un sitio basado en un *plantilla*, que proporciona un sitio que tiene funciones predeterminadas en ella. Las plantillas de hacen buen uso de CSS para crear sitios que muy bien y que tienen características como menús y páginas de diseño. Aquí es una captura de pantalla de la página principal de un sitio basado en una plantilla, que muestra las características que usan las páginas de diseño y CSS:

![Creado por la plantilla de sitio de WebMatrix que muestra el encabezado, el área de navegación, área de contenido, sección opcional y vínculos de inicio de sesión de diseño](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Lista completa de la página de película (actualizada para usar una página de diseño)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Página completa lista para agregar página de la película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Página completa lista para eliminar página de la película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Página completa lista para la página de edición de película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, obtendrá información sobre cómo publicar su sitio en Internet para que todo el mundo pueda verla.

## <a name="additional-resources"></a>Recursos adicionales

- [Creación de un aspecto coherente](https://go.microsoft.com/fwlink/?LinkID=202891) , un artículo que proporciona algunos detalles más sobre cómo trabajar con diseños. También se describe cómo pasar un valor a una página de diseño que muestre u oculte parte del contenido.
- [Páginas de diseño con Razor anidadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : blogs de Mike Brind un ejemplo de cómo anidar las páginas de diseño. (Incluye la descarga de las páginas).

> [!div class="step-by-step"]
> [Anterior](deleting-data.md)
> [Siguiente](publishing.md)
