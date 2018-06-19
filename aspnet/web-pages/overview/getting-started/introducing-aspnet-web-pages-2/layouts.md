---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Introducción a ASP.NET Web Pages: crear un diseño homogéneo | Documentos de Microsoft'
author: tfitzmac
description: Este tutorial muestra cómo usar diseños para crear una apariencia coherente para las páginas en un sitio que usa ASP.NET Web Pages. Supone que ha completado el...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899819"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introducción a las páginas Web ASP.NET - crear un diseño coherente
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo usar *diseños* para crear una apariencia coherente para las páginas en un sitio que usa ASP.NET Web Pages. Supone que ha completado la serie a través de [eliminar la base de datos en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Lo que aprenderá:
> 
> - ¿Qué una página de diseño es.
> - Cómo combinar páginas de diseño con contenido dinámico.
> - Cómo pasar valores a una página de diseño.


## <a name="about-layouts"></a>Acerca de los diseños

Las páginas que ha creado hasta ahora han sido todos haya finalizado, páginas independientes. Todos los que pertenecen al mismo sitio, pero no tienen elementos comunes o un aspecto estándar.

Mayoría de los sitios tienen un diseño y una apariencia coherente. Por ejemplo, si va a la [Web de Microsoft.com](https://www.microsoft.com/web/) de sitio y buscar, verá que las páginas de todos los adhieren a un diseño global a un tema visual:

![Página del sitio Web de Microsoft.com que muestra el diseño del encabezado, el área de navegación, el área de contenido y el pie de página](layouts/_static/image1.png)

Un *ineficaz* forma de crear este diseño sería definir un encabezado, una barra de navegación y un pie de página por separado en cada una de las páginas. Se podría duplicar el marcado mismo cada vez. Si desea cambiar algo (por ejemplo, actualizar el pie de página), tendría que cambiar cada página por separado.

Ahí es donde *páginas de diseño* vienen en. En ASP.NET Web Pages, puede definir una página de diseño que proporciona un contenedor general de las páginas en el sitio. Por ejemplo, la página de diseño puede contener el encabezado, el área de navegación y el pie de página. La página de diseño incluye un marcador de posición dónde va el contenido principal.

A continuación, puede definir las páginas de contenido individuales que contienen el marcado y el código de sólo de esa página. Páginas de contenido no tienen que estar páginas HTML completas; incluso no tienen por qué tener un `<body>` elemento. También tienen una línea de código que indica qué página de diseño que desea mostrar el contenido ASP.NET. Aquí se muestra una imagen que muestra que aproximadamente el funcionamiento de esta relación:

![Diagrama conceptual que muestra dos páginas de contenido y una página de diseño en la que se ajustan](layouts/_static/image2.png)

Esta interacción es fácil de entender al verlo en acción. En este tutorial, va a cambiar las páginas de películas para usar un diseño.

## <a name="adding-a-layout-page"></a>Agregar una página de diseño

Podrá empezar mediante la creación de una página de diseño que define un diseño de página típico con un encabezado, pie de página y un área para el contenido principal. En el sitio WebPagesMovies, agregue una página CSHTML denominada  *\_Layout.cshtml*.

El carácter de subrayado inicial ( `_` ) caracteres es significativo. Si el nombre de la página comienza con un carácter de subrayado, ASP.NET no enviará directamente esa página en el explorador. Esta convención le permite definir las páginas que son necesarias para un sitio, pero que los usuarios no deberían poder solicitar directamente.

Reemplace el contenido de la página con lo siguiente:

[!code-html[Main](layouts/samples/sample1.html)]

Como puede ver, este marcado es simplemente código HTML que usa `<div>` elementos para definir tres secciones en la página más uno más `<div>` elemento que se va a contener las tres secciones. El pie de página contiene un poco de código Razor: `@DateTime.Now.Year`, que representará el año actual en esa ubicación en la página.

Observe que hay un vínculo a una hoja de estilos denominada *Movies.css*. La hoja de estilos es donde se definen los detalles de la distribución física de los elementos. Se creará en un momento.

La característica solo inusual en este  *\_Layout.cshtml* página es la `@Render.Body()` línea. Que es el marcador de posición donde irá el contenido cuando este diseño se combine con otra página.

## <a name="adding-a-css-file"></a>Agregar un archivo .css

La mejor manera de definir la organización real (es decir, apariencia) de los elementos de la página es usar las reglas en cascada (CSS) de la hoja de estilo. Por lo que podrá crear un *.css* archivo con las reglas para el nuevo diseño.

En WebMatrix, seleccione la raíz del sitio. A continuación, en la **archivos** ficha de la cinta de opciones, haga clic en la flecha situada debajo del **New** botón y, a continuación, haga clic en **nueva carpeta**.

![La opción 'Nueva carpeta' debajo de nuevo en la cinta de opciones.](layouts/_static/image3.png)

Nombre de la nueva carpeta *estilos*.

![La nueva carpeta de nomenclatura 'Estilos'](layouts/_static/image4.png)

Dentro de la nueva *estilos* carpeta, cree un archivo denominado *Movies.css*.

![Crear un nuevo archivo Movies.css](layouts/_static/image5.png)

Reemplace el contenido del nuevo *.css* archivo con lo siguiente:

[!code-css[Main](layouts/samples/sample2.css)]

No decimos mucho sobre estas reglas CSS, salvo que tenga en cuenta dos cosas. Una es que además de establecer las fuentes y tamaños, las reglas de utilizan posiciones absolutas para establecer la ubicación del encabezado, el pie de página y el área de contenido principal. Si está familiarizado con la colocación en CSS, puede leer el [posicionamiento de CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial en el sitio W3Schools.

Otro lo tenga en cuenta que en la parte inferior, hemos copiado las reglas de estilo que estaban originalmente se define por separado en el *Movies.cshtml* archivo. Estas reglas se usan en el [Introducción para mostrar datos mediante ASP.NET Web Pages de uso](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para realizar la `WebGrid` auxiliar representar el marcado que agrega secciona a la tabla. (Si va a utilizar un *.css* archivo para definiciones de estilo, podría incluir también las reglas de estilo para la totalidad del sitio en él.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Actualizando el archivo de películas para usar el diseño

Ahora puede actualizar los archivos existentes en el sitio para usar el nuevo diseño. Abra la *Movies.cshtml* archivo. En la parte superior, como la primera línea de código, agregue lo siguiente:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Ahora inicia la página de este modo:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Esta línea de código indica a ASP.NET que, cuando la *películas* página se ejecuta, se deben combinar con la  *\_Layout.cshtml* archivo.

Puesto que la *Movies.cshtml* archivo ahora usa una página de diseño, puede quitar el marcado de la *Movies.cshtml* página que se encarga de la  *\_Layout.cshtml*archivo. Retire el `<!DOCTYPE>`, `<html>`, y `<body>` abriendo y etiquetas de cierre. Retire la totalidad `<head>` elemento y su contenido, que incluye las reglas de estilo de la cuadrícula, ya que ahora tienes esas reglas un *.css* archivo. Mientras se encuentra en él, cambiar existente `<h1>` elemento a una `<h2>` elemento; debe tener un `<h1>` elemento en la página de diseño ya. Cambiar el `<h2>` texto a "Lista de películas".

Normalmente no es necesario realizar este tipo de cambios en una página de contenido. Cuando empieza el sitio con una página de diseño, crear páginas de contenido sin todos estos elementos con el que comenzar. Sin embargo, en este caso, se está convirtiendo una página independiente a uno que usa un diseño, por lo que es un poco de limpieza.

Cuando haya terminado, la *Movies.cshtml* página tendrá un aspecto similar al siguiente:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Probar el diseño

Ahora puede ver el aspecto que tiene el diseño. En WebMatrix, haga clic en el *Movies.cshtml* página y seleccione **iniciar en un explorador**. Cuando el explorador muestra la página, parece que esta página:

![Representado mediante un diseño de página de películas](layouts/_static/image6.png)

ASP.NET se ha combinado el contenido de la página Movies.cshtml en la  *\_Layout.cshtml* página derecha donde el `RenderBody` es el método. Y por supuesto el  *\_Layout.cshtml* página referencias un *.css* archivo que define la apariencia de la página.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Actualizar la página AddMovie para usar el diseño

La ventaja real de los diseños es que puede usarlos para todas las páginas de su sitio. Abra la *AddMovie.cshtml* página.

Es posible que recuerde que el *AddMovie.cshtml* página originalmente tenía algunas reglas CSS en él para definir la apariencia de los mensajes de error de validación. Puesto que tiene un *.css* archivo para su sitio ahora, puede mover esas reglas a la *.css* archivo. Quitar el *AddMovie.cshtml* de archivos y agregarlos a la parte inferior de la *Movies.css* archivo. Va a mover las siguientes reglas:

[!code-css[Main](layouts/samples/sample6.css)]

Ahora, realizar los mismos tipos de cambios en *AddMovie.cshtml* que siguió para *Movies.cshtml* : agregar `Layout="~/_Layout.cshtml;` y quite el marcado HTML que se encuentra ahora extraño. Cambie el elemento `<h1>` por `<h2>`. Cuando haya terminado, la página tendrá un aspecto parecido a este ejemplo:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Ejecute la página. Ahora parece que esta ilustración:

![Representado mediante un diseño de página 'Agregar películas'](layouts/_static/image7.png)

Desea realizar modificaciones similares a las páginas en el sitio: *EditMovie.cshtml* y *DeleteMovie.cshtml*. Sin embargo, antes de hacerlo, puede realizar otro cambio en el diseño que resulta un poco más flexible.

## <a name="passing-title-information-to-the-layout-page"></a>Pasar información del título a la página de diseño

El  *\_Layout.cshtml* página que ha creado tiene un `<title>` elemento que se establece en "Mi sitio de película". Mayoría de los exploradores muestra el contenido de este elemento como el texto en una pestaña:

![La página &lt;título&gt; elemento mostrado en una pestaña del explorador](layouts/_static/image8.png)

Esta información de título es genérica. Suponga que desea que el texto de título para ser más específico en la página actual. (El texto del título también utilizan motores de búsqueda para determinar cuál es la página acerca de.) Es posible pasar información de una página de contenido como *Movies.cshtml* o *AddMovie.cshtml* a la página de diseño y, a continuación, utilizar esa información para personalizar la página de diseño de qué representa.

Abra la *Movies.cshtml* página de nuevo. En el código en la parte superior, agregue la siguiente línea:

[!code-csharp[Main](layouts/samples/sample8.cs)]

El `Page` el objeto está disponible en todos los *.cshtml* páginas y es para este propósito, es decir, para compartir información entre una página y su diseño.

Abra la<em>\_Layout.cshtml</em> página. Cambiar el `<title>` elemento para que aparezca como este marcado:

[!code-html[Main](layouts/samples/sample9.html)]

Este código representa lo que se encuentra en la `Page.Title` propiedad directamente en esa ubicación en la página.

Ejecute el *Movies.cshtml* página. Este tiempo de la pestaña de explorador muestra la información que pasa como el valor de `Page.Title`:

![Una pestaña del explorador que muestra el título que se crea de forma dinámica](layouts/_static/image9.png)

Si lo desea, puede ver el origen de la página en el explorador. Puede ver que el `<title>` elemento se representa como `<title>List Movies</title>`.

> [!TIP] 
> 
> **El objeto de página**
> 
> Una característica útil de `Page` es que es un objeto dinámico, el `Title` propiedad no es un nombre reservado o fijo. Puede usar *cualquier* nombre para un valor de la `Page` objeto. Por ejemplo, podría tan fácilmente ha pasado el título mediante el uso de una propiedad denominada `Page.CurrentName` o `Page.MyPage`. La única restricción es que el nombre tiene que seguir las reglas normales de qué propiedades pueden ser con nombre. (Por ejemplo, el nombre no puede contener un espacio).
> 
> Puede pasar cualquier número de valores mediante la `Page` objeto. Si desea pasar información de películas a la página de diseño, podría pasar valores mediante el uso de algo parecido a `Page.MovieTitle` y `Page.Genre` y `Page.MovieYear`. (O cualquier otro nombre que inventaron para almacenar la información.) El único requisito, que es probablemente obvio, es que se debe utilizar los mismos nombres en la página de contenido y la página de diseño.
> 
> La información que se pasa mediante el `Page` objeto no se limita a simplemente texto para mostrarse en la página de diseño. Puede pasar un valor a la página de diseño y, a continuación, el código en la página de diseño puede utilizar el valor para decidir si se muestra una sección de la página, lo que *.css* archivo para que utilice y así sucesivamente. Los valores pasados en el `Page` objeto son como cualquier otro valores que usa en el código. Únicamente es que los valores se originan en la página de contenido y se pasan a la página de diseño.


Abra la *AddMovie.cshtml* página y agregue una línea a la parte superior del código que proporciona un título para la *AddMovie.cshtml* página:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Ejecute el *AddMovie.cshtml* página. Vea el nuevo título:

![Una pestaña del explorador que muestra el título de 'Agregar películas' creado de forma dinámica](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Actualización de las páginas restantes para usar el diseño

Ahora puede finalizar el resto de páginas de su sitio para que usen el nuevo diseño. Abra *EditMovie.cshtml* y *DeleteMovie.cshtml* a su vez y realizar los mismos cambios en cada uno.

Agregue la línea de código que se vincula a la página de diseño:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Agregue una línea para establecer el título de la página:

[!code-csharp[Main](layouts/samples/sample12.cs)]

O bien

[!code-csharp[Main](layouts/samples/sample13.cs)]

Quite todo el marcado HTML extraño, básicamente, dejar solo los bits que están dentro de la `<body>` elemento (más el bloque de código en la parte superior).

Cambiar el `<h1>` elemento para que sea un `<h2>` elemento.

Cuando haya realizado estos cambios, pruebe cada una y asegúrese de que se muestra correctamente y que el título sea correcto.

## <a name="parting-thoughts-about-layout-pages"></a>Partición ideas acerca de las páginas de diseño

En este tutorial ha creado un  *\_Layout.cshtml* página y utiliza el `RenderBody` método para combinar el contenido de otra página. Que es el patrón básico para el uso de diseños de páginas Web.

Páginas de diseño tienen características adicionales que no trataremos aquí. Por ejemplo, puede anidar páginas de diseño, una página de diseño a su vez puede referencia a otro. Los diseños anidados pueden ser útiles si está trabajando con subsecciones de un sitio que requieren diseños diferentes. También puede utilizar métodos adicionales (por ejemplo, `RenderSection`) para configurar denominado secciones en la página de diseño.

La combinación de páginas de diseño y *.css* archivos es eficaz. Como verá en la siguiente serie de tutoriales, en WebMatrix puede crear un sitio basado en un *plantilla*, que proporciona un sitio que tiene funcionalidad creada previamente en ella. Las plantillas permiten buen uso de páginas de diseño y CSS para crear sitios que un aspecto excelente y que tienen características como los menús. Esta es una captura de pantalla de la página principal de un sitio basado en una plantilla, que muestra las características que usan páginas de diseño y CSS:

![Diseño creado por la plantilla de sitio de WebMatrix que muestra el encabezado, área de navegación, área de contenido, sección opcional y los vínculos de inicio de sesión](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Muestra la lista completa de página de la película (actualizada para utilizar una página de diseño)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Página completa lista para agregar la página de la película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Página completa lista para eliminar página de la película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Página completa lista para editar página de la película (actualizada para el diseño)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, aprenderá a publicar el sitio en Internet para que todo el mundo pueda verla.

## <a name="additional-resources"></a>Recursos adicionales

- [Crear un aspecto coherente](https://go.microsoft.com/fwlink/?LinkID=202891) : un artículo que proporciona cierto nivel de detalle más sobre cómo trabajar con diseños. También describe cómo pasar un valor a una página de diseño que muestra u oculta la parte del contenido.
- [Páginas de diseño con Razor anidadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : blogs de Mike Brind un ejemplo de cómo anidar las páginas de diseño. (Incluye una descarga de las páginas).

> [!div class="step-by-step"]
> [Anterior](deleting-data.md)
> [Siguiente](publishing.md)
