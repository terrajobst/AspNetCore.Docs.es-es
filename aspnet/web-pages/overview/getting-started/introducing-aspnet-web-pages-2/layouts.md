---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: "Introducción a ASP.NET Web Pages: crear un diseño homogéneo | Documentos de Microsoft"
author: tfitzmac
description: "Este tutorial muestra cómo usar diseños para crear una apariencia coherente para las páginas en un sitio que usa ASP.NET Web Pages. Supone que ha completado el..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 692adc5a03892f27c91fe8868c8eab6ce08f49cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="cb903-104">Introducción a las páginas Web ASP.NET - crear un diseño coherente</span><span class="sxs-lookup"><span data-stu-id="cb903-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="cb903-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cb903-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cb903-106">Este tutorial muestra cómo usar *diseños* para crear una apariencia coherente para las páginas en un sitio que usa ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="cb903-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="cb903-107">Supone que ha completado la serie a través de [eliminar la base de datos en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="cb903-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="cb903-108">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="cb903-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="cb903-109">¿Qué una página de diseño es.</span><span class="sxs-lookup"><span data-stu-id="cb903-109">What a layout page is.</span></span>
> - <span data-ttu-id="cb903-110">Cómo combinar páginas de diseño con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="cb903-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="cb903-111">Cómo pasar valores a una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="cb903-112">Acerca de los diseños</span><span class="sxs-lookup"><span data-stu-id="cb903-112">About Layouts</span></span>

<span data-ttu-id="cb903-113">Las páginas que ha creado hasta ahora han sido todos haya finalizado, páginas independientes.</span><span class="sxs-lookup"><span data-stu-id="cb903-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="cb903-114">Todos los que pertenecen al mismo sitio, pero no tienen elementos comunes o un aspecto estándar.</span><span class="sxs-lookup"><span data-stu-id="cb903-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="cb903-115">Mayoría de los sitios tienen un diseño y una apariencia coherente.</span><span class="sxs-lookup"><span data-stu-id="cb903-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="cb903-116">Por ejemplo, si va a la [Web de Microsoft.com](https://www.microsoft.com/web/) de sitio y buscar, verá que las páginas de todos los adhieren a un diseño global a un tema visual:</span><span class="sxs-lookup"><span data-stu-id="cb903-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Página del sitio Web de Microsoft.com que muestra el diseño del encabezado, el área de navegación, el área de contenido y el pie de página](layouts/_static/image1.png)

<span data-ttu-id="cb903-118">Un *ineficaz* forma de crear este diseño sería definir un encabezado, una barra de navegación y un pie de página por separado en cada una de las páginas.</span><span class="sxs-lookup"><span data-stu-id="cb903-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="cb903-119">Se podría duplicar el marcado mismo cada vez.</span><span class="sxs-lookup"><span data-stu-id="cb903-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="cb903-120">Si desea cambiar algo (por ejemplo, actualizar el pie de página), tendría que cambiar cada página por separado.</span><span class="sxs-lookup"><span data-stu-id="cb903-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="cb903-121">Ahí es donde *páginas de diseño* vienen en.</span><span class="sxs-lookup"><span data-stu-id="cb903-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="cb903-122">En ASP.NET Web Pages, puede definir una página de diseño que proporciona un contenedor general de las páginas en el sitio.</span><span class="sxs-lookup"><span data-stu-id="cb903-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="cb903-123">Por ejemplo, la página de diseño puede contener el encabezado, el área de navegación y el pie de página.</span><span class="sxs-lookup"><span data-stu-id="cb903-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="cb903-124">La página de diseño incluye un marcador de posición dónde va el contenido principal.</span><span class="sxs-lookup"><span data-stu-id="cb903-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="cb903-125">A continuación, puede definir las páginas de contenido individuales que contienen el marcado y el código de sólo de esa página.</span><span class="sxs-lookup"><span data-stu-id="cb903-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="cb903-126">Páginas de contenido no tienen que estar páginas HTML completas; incluso no tienen por qué tener un `<body>` elemento.</span><span class="sxs-lookup"><span data-stu-id="cb903-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="cb903-127">También tienen una línea de código que indica qué página de diseño que desea mostrar el contenido ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cb903-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="cb903-128">Aquí se muestra una imagen que muestra que aproximadamente el funcionamiento de esta relación:</span><span class="sxs-lookup"><span data-stu-id="cb903-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagrama conceptual que muestra dos páginas de contenido y una página de diseño en la que se ajustan](layouts/_static/image2.png)

<span data-ttu-id="cb903-130">Esta interacción es fácil de entender al verlo en acción.</span><span class="sxs-lookup"><span data-stu-id="cb903-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="cb903-131">En este tutorial, va a cambiar las páginas de películas para usar un diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="cb903-132">Agregar una página de diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-132">Adding a Layout Page</span></span>

<span data-ttu-id="cb903-133">Podrá empezar mediante la creación de una página de diseño que define un diseño de página típico con un encabezado, pie de página y un área para el contenido principal.</span><span class="sxs-lookup"><span data-stu-id="cb903-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="cb903-134">En el sitio WebPagesMovies, agregue una página CSHTML denominada  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cb903-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="cb903-135">El carácter de subrayado inicial ( `_` ) caracteres es significativo.</span><span class="sxs-lookup"><span data-stu-id="cb903-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="cb903-136">Si el nombre de la página comienza con un carácter de subrayado, ASP.NET no enviará directamente esa página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="cb903-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="cb903-137">Esta convención le permite definir las páginas que son necesarias para un sitio, pero que los usuarios no deberían poder solicitar directamente.</span><span class="sxs-lookup"><span data-stu-id="cb903-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="cb903-138">Reemplace el contenido de la página con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb903-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="cb903-139">Como puede ver, este marcado es simplemente código HTML que usa `<div>` elementos para definir tres secciones en la página más uno más `<div>` elemento que se va a contener las tres secciones.</span><span class="sxs-lookup"><span data-stu-id="cb903-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="cb903-140">El pie de página contiene un poco de código Razor: `@DateTime.Now.Year`, que representará el año actual en esa ubicación en la página.</span><span class="sxs-lookup"><span data-stu-id="cb903-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="cb903-141">Observe que hay un vínculo a una hoja de estilos denominada *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="cb903-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="cb903-142">La hoja de estilos es donde se definen los detalles de la distribución física de los elementos.</span><span class="sxs-lookup"><span data-stu-id="cb903-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="cb903-143">Se creará en un momento.</span><span class="sxs-lookup"><span data-stu-id="cb903-143">You'll create that in a moment.</span></span>

<span data-ttu-id="cb903-144">La característica solo inusual en este  *\_Layout.cshtml* página es la `@Render.Body()` línea.</span><span class="sxs-lookup"><span data-stu-id="cb903-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="cb903-145">Que es el marcador de posición donde irá el contenido cuando este diseño se combine con otra página.</span><span class="sxs-lookup"><span data-stu-id="cb903-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="cb903-146">Agregar un archivo .css</span><span class="sxs-lookup"><span data-stu-id="cb903-146">Adding a .css File</span></span>

<span data-ttu-id="cb903-147">La mejor manera de definir la organización real (es decir, apariencia) de los elementos de la página es usar las reglas en cascada (CSS) de la hoja de estilo.</span><span class="sxs-lookup"><span data-stu-id="cb903-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="cb903-148">Por lo que podrá crear un *.css* archivo con las reglas para el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="cb903-149">En WebMatrix, seleccione la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="cb903-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="cb903-150">A continuación, en la **archivos** ficha de la cinta de opciones, haga clic en la flecha situada debajo del **New** botón y, a continuación, haga clic en **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="cb903-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![La opción 'Nueva carpeta' debajo de nuevo en la cinta de opciones.](layouts/_static/image3.png)

<span data-ttu-id="cb903-152">Nombre de la nueva carpeta *estilos*.</span><span class="sxs-lookup"><span data-stu-id="cb903-152">Name the new folder *Styles*.</span></span>

![La nueva carpeta de nomenclatura 'Estilos'](layouts/_static/image4.png)

<span data-ttu-id="cb903-154">Dentro de la nueva *estilos* carpeta, cree un archivo denominado *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="cb903-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Crear un nuevo archivo Movies.css](layouts/_static/image5.png)

<span data-ttu-id="cb903-156">Reemplace el contenido del nuevo *.css* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb903-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="cb903-157">No decimos mucho sobre estas reglas CSS, salvo que tenga en cuenta dos cosas.</span><span class="sxs-lookup"><span data-stu-id="cb903-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="cb903-158">Una es que además de establecer las fuentes y tamaños, las reglas de utilizan posiciones absolutas para establecer la ubicación del encabezado, el pie de página y el área de contenido principal.</span><span class="sxs-lookup"><span data-stu-id="cb903-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="cb903-159">Si está familiarizado con la colocación en CSS, puede leer el [posicionamiento de CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial en el sitio W3Schools.</span><span class="sxs-lookup"><span data-stu-id="cb903-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="cb903-160">Otro lo tenga en cuenta que en la parte inferior, hemos copiado las reglas de estilo que estaban originalmente se define por separado en el *Movies.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="cb903-161">Estas reglas se usan en el [Introducción para mostrar datos mediante ASP.NET Web Pages de uso](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para realizar la `WebGrid` auxiliar representar el marcado que agrega secciona a la tabla.</span><span class="sxs-lookup"><span data-stu-id="cb903-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="cb903-162">(Si va a utilizar un *.css* archivo para definiciones de estilo, podría incluir también las reglas de estilo para la totalidad del sitio en él.)</span><span class="sxs-lookup"><span data-stu-id="cb903-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="cb903-163">Actualizando el archivo de películas para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="cb903-164">Ahora puede actualizar los archivos existentes en el sitio para usar el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="cb903-165">Abra la *Movies.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="cb903-166">En la parte superior, como la primera línea de código, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb903-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="cb903-167">Ahora inicia la página de este modo:</span><span class="sxs-lookup"><span data-stu-id="cb903-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="cb903-168">Esta línea de código indica a ASP.NET que, cuando la *películas* página se ejecuta, se deben combinar con la  *\_Layout.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="cb903-169">Puesto que la *Movies.cshtml* archivo ahora usa una página de diseño, puede quitar el marcado de la *Movies.cshtml* página que se encarga de la  *\_Layout.cshtml*archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="cb903-170">Retire el `<!DOCTYPE>`, `<html>`, y `<body>` abriendo y etiquetas de cierre.</span><span class="sxs-lookup"><span data-stu-id="cb903-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="cb903-171">Retire la totalidad `<head>` elemento y su contenido, que incluye las reglas de estilo de la cuadrícula, ya que ahora tienes esas reglas un *.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="cb903-172">Mientras se encuentra en él, cambiar existente `<h1>` elemento a una `<h2>` elemento; debe tener un `<h1>` elemento en la página de diseño ya.</span><span class="sxs-lookup"><span data-stu-id="cb903-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="cb903-173">Cambiar el `<h2>` texto a "Lista de películas".</span><span class="sxs-lookup"><span data-stu-id="cb903-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="cb903-174">Normalmente no es necesario realizar este tipo de cambios en una página de contenido.</span><span class="sxs-lookup"><span data-stu-id="cb903-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="cb903-175">Cuando empieza el sitio con una página de diseño, crear páginas de contenido sin todos estos elementos con el que comenzar.</span><span class="sxs-lookup"><span data-stu-id="cb903-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="cb903-176">Sin embargo, en este caso, se está convirtiendo una página independiente a uno que usa un diseño, por lo que es un poco de limpieza.</span><span class="sxs-lookup"><span data-stu-id="cb903-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="cb903-177">Cuando haya terminado, la *Movies.cshtml* página tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb903-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="cb903-178">Probar el diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-178">Testing the Layout</span></span>

<span data-ttu-id="cb903-179">Ahora puede ver el aspecto que tiene el diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="cb903-180">En WebMatrix, haga clic en el *Movies.cshtml* página y seleccione **iniciar en un explorador**.</span><span class="sxs-lookup"><span data-stu-id="cb903-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="cb903-181">Cuando el explorador muestra la página, parece que esta página:</span><span class="sxs-lookup"><span data-stu-id="cb903-181">When the browser displays the page, it looks like this page:</span></span>

![Representado mediante un diseño de página de películas](layouts/_static/image6.png)

<span data-ttu-id="cb903-183">ASP.NET se ha combinado el contenido de la página Movies.cshtml en la  *\_Layout.cshtml* página derecha donde el `RenderBody` es el método.</span><span class="sxs-lookup"><span data-stu-id="cb903-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="cb903-184">Y por supuesto el  *\_Layout.cshtml* página referencias un *.css* archivo que define la apariencia de la página.</span><span class="sxs-lookup"><span data-stu-id="cb903-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="cb903-185">Actualizar la página AddMovie para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="cb903-186">La ventaja real de los diseños es que puede usarlos para todas las páginas de su sitio.</span><span class="sxs-lookup"><span data-stu-id="cb903-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="cb903-187">Abra la *AddMovie.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="cb903-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="cb903-188">Es posible que recuerde que el *AddMovie.cshtml* página originalmente tenía algunas reglas CSS en él para definir la apariencia de los mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="cb903-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="cb903-189">Puesto que tiene un *.css* archivo para su sitio ahora, puede mover esas reglas a la *.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="cb903-190">Quitar el *AddMovie.cshtml* de archivos y agregarlos a la parte inferior de la *Movies.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb903-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="cb903-191">Va a mover las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="cb903-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="cb903-192">Ahora, realizar los mismos tipos de cambios en *AddMovie.cshtml* que siguió para *Movies.cshtml* : agregar `Layout="~/_Layout.cshtml;` y quite el marcado HTML que se encuentra ahora extraño.</span><span class="sxs-lookup"><span data-stu-id="cb903-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="cb903-193">Cambie el elemento `<h1>` por `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="cb903-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="cb903-194">Cuando haya terminado, la página tendrá un aspecto parecido a este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cb903-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="cb903-195">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="cb903-195">Run the page.</span></span> <span data-ttu-id="cb903-196">Ahora parece que esta ilustración:</span><span class="sxs-lookup"><span data-stu-id="cb903-196">Now it looks like this illustration:</span></span>

![Representado mediante un diseño de página 'Agregar películas'](layouts/_static/image7.png)

<span data-ttu-id="cb903-198">Desea realizar modificaciones similares a las páginas en el sitio: *EditMovie.cshtml* y *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cb903-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="cb903-199">Sin embargo, antes de hacerlo, puede realizar otro cambio en el diseño que resulta un poco más flexible.</span><span class="sxs-lookup"><span data-stu-id="cb903-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="cb903-200">Pasar información del título a la página de diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="cb903-201">El  *\_Layout.cshtml* página que ha creado tiene un `<title>` elemento que se establece en "Mi sitio de película".</span><span class="sxs-lookup"><span data-stu-id="cb903-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="cb903-202">Mayoría de los exploradores muestra el contenido de este elemento como el texto en una pestaña:</span><span class="sxs-lookup"><span data-stu-id="cb903-202">Most browsers display the content of this element as the text on a tab:</span></span>

![La página &lt;título&gt; elemento mostrado en una pestaña del explorador](layouts/_static/image8.png)

<span data-ttu-id="cb903-204">Esta información de título es genérica.</span><span class="sxs-lookup"><span data-stu-id="cb903-204">This title information is generic.</span></span> <span data-ttu-id="cb903-205">Suponga que desea que el texto de título para ser más específico en la página actual.</span><span class="sxs-lookup"><span data-stu-id="cb903-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="cb903-206">(El texto del título también utilizan motores de búsqueda para determinar cuál es la página acerca de.) Es posible pasar información de una página de contenido como *Movies.cshtml* o *AddMovie.cshtml* a la página de diseño y, a continuación, utilizar esa información para personalizar la página de diseño de qué representa.</span><span class="sxs-lookup"><span data-stu-id="cb903-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="cb903-207">Abra la *Movies.cshtml* página de nuevo.</span><span class="sxs-lookup"><span data-stu-id="cb903-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="cb903-208">En el código en la parte superior, agregue la siguiente línea:</span><span class="sxs-lookup"><span data-stu-id="cb903-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="cb903-209">El `Page` el objeto está disponible en todos los *.cshtml* páginas y es para este propósito, es decir, para compartir información entre una página y su diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="cb903-210">Abra la*\_Layout.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="cb903-210">Open the*\_Layout.cshtml* page.</span></span> <span data-ttu-id="cb903-211">Cambiar el `<title>` elemento para que aparezca como este marcado:</span><span class="sxs-lookup"><span data-stu-id="cb903-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="cb903-212">Este código representa lo que se encuentra en la `Page.Title` propiedad directamente en esa ubicación en la página.</span><span class="sxs-lookup"><span data-stu-id="cb903-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="cb903-213">Ejecute el *Movies.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="cb903-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="cb903-214">Este tiempo de la pestaña de explorador muestra la información que pasa como el valor de `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="cb903-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Una pestaña del explorador que muestra el título que se crea de forma dinámica](layouts/_static/image9.png)

<span data-ttu-id="cb903-216">Si lo desea, puede ver el origen de la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="cb903-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="cb903-217">Puede ver que el `<title>` elemento se representa como `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="cb903-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="cb903-218">**El objeto de página**</span><span class="sxs-lookup"><span data-stu-id="cb903-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="cb903-219">Una característica útil de `Page` es que es un objeto dinámico, el `Title` propiedad no es un nombre reservado o fijo.</span><span class="sxs-lookup"><span data-stu-id="cb903-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="cb903-220">Puede usar *cualquier* nombre para un valor de la `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="cb903-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="cb903-221">Por ejemplo, podría tan fácilmente ha pasado el título mediante el uso de una propiedad denominada `Page.CurrentName` o `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="cb903-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="cb903-222">La única restricción es que el nombre tiene que seguir las reglas normales de qué propiedades pueden ser con nombre.</span><span class="sxs-lookup"><span data-stu-id="cb903-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="cb903-223">(Por ejemplo, el nombre no puede contener un espacio).</span><span class="sxs-lookup"><span data-stu-id="cb903-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="cb903-224">Puede pasar cualquier número de valores mediante la `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="cb903-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="cb903-225">Si desea pasar información de películas a la página de diseño, podría pasar valores mediante el uso de algo parecido a `Page.MovieTitle` y `Page.Genre` y `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="cb903-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="cb903-226">(O cualquier otro nombre que inventaron para almacenar la información.) El único requisito, que es probablemente obvio, es que se debe utilizar los mismos nombres en la página de contenido y la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="cb903-227">La información que se pasa mediante el `Page` objeto no se limita a simplemente texto para mostrarse en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="cb903-228">Puede pasar un valor a la página de diseño y, a continuación, el código en la página de diseño puede utilizar el valor para decidir si se muestra una sección de la página, lo que *.css* archivo para que utilice y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="cb903-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="cb903-229">Los valores pasados en el `Page` objeto son como cualquier otro valores que usa en el código.</span><span class="sxs-lookup"><span data-stu-id="cb903-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="cb903-230">Únicamente es que los valores se originan en la página de contenido y se pasan a la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="cb903-231">Abra la *AddMovie.cshtml* página y agregue una línea a la parte superior del código que proporciona un título para la *AddMovie.cshtml* página:</span><span class="sxs-lookup"><span data-stu-id="cb903-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="cb903-232">Ejecute el *AddMovie.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="cb903-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="cb903-233">Vea el nuevo título:</span><span class="sxs-lookup"><span data-stu-id="cb903-233">You see the new title there:</span></span>

![Una pestaña del explorador que muestra el título de 'Agregar películas' creado de forma dinámica](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="cb903-235">Actualización de las páginas restantes para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="cb903-236">Ahora puede finalizar el resto de páginas de su sitio para que usen el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="cb903-237">Abra *EditMovie.cshtml* y *DeleteMovie.cshtml* a su vez y realizar los mismos cambios en cada uno.</span><span class="sxs-lookup"><span data-stu-id="cb903-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="cb903-238">Agregue la línea de código que se vincula a la página de diseño:</span><span class="sxs-lookup"><span data-stu-id="cb903-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="cb903-239">Agregue una línea para establecer el título de la página:</span><span class="sxs-lookup"><span data-stu-id="cb903-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="cb903-240">O bien</span><span class="sxs-lookup"><span data-stu-id="cb903-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="cb903-241">Quite todo el marcado HTML extraño, básicamente, dejar solo los bits que están dentro de la `<body>` elemento (más el bloque de código en la parte superior).</span><span class="sxs-lookup"><span data-stu-id="cb903-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="cb903-242">Cambiar el `<h1>` elemento para que sea un `<h2>` elemento.</span><span class="sxs-lookup"><span data-stu-id="cb903-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="cb903-243">Cuando haya realizado estos cambios, pruebe cada una y asegúrese de que se muestra correctamente y que el título sea correcto.</span><span class="sxs-lookup"><span data-stu-id="cb903-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="cb903-244">Partición ideas acerca de las páginas de diseño</span><span class="sxs-lookup"><span data-stu-id="cb903-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="cb903-245">En este tutorial ha creado un  *\_Layout.cshtml* página y utiliza el `RenderBody` método para combinar el contenido de otra página.</span><span class="sxs-lookup"><span data-stu-id="cb903-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="cb903-246">Que es el patrón básico para el uso de diseños de páginas Web.</span><span class="sxs-lookup"><span data-stu-id="cb903-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="cb903-247">Páginas de diseño tienen características adicionales que no trataremos aquí.</span><span class="sxs-lookup"><span data-stu-id="cb903-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="cb903-248">Por ejemplo, puede anidar páginas de diseño, una página de diseño a su vez puede referencia a otro.</span><span class="sxs-lookup"><span data-stu-id="cb903-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="cb903-249">Los diseños anidados pueden ser útiles si está trabajando con subsecciones de un sitio que requieren diseños diferentes.</span><span class="sxs-lookup"><span data-stu-id="cb903-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="cb903-250">También puede utilizar métodos adicionales (por ejemplo, `RenderSection`) para configurar denominado secciones en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="cb903-251">La combinación de páginas de diseño y *.css* archivos es eficaz.</span><span class="sxs-lookup"><span data-stu-id="cb903-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="cb903-252">Como verá en la siguiente serie de tutoriales, en WebMatrix puede crear un sitio basado en un *plantilla*, que proporciona un sitio que tiene funcionalidad creada previamente en ella.</span><span class="sxs-lookup"><span data-stu-id="cb903-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="cb903-253">Las plantillas permiten buen uso de páginas de diseño y CSS para crear sitios que un aspecto excelente y que tienen características como los menús.</span><span class="sxs-lookup"><span data-stu-id="cb903-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="cb903-254">Esta es una captura de pantalla de la página principal de un sitio basado en una plantilla, que muestra las características que usan páginas de diseño y CSS:</span><span class="sxs-lookup"><span data-stu-id="cb903-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Diseño creado por la plantilla de sitio de WebMatrix que muestra el encabezado, área de navegación, área de contenido, sección opcional y los vínculos de inicio de sesión](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="cb903-256">Muestra la lista completa de página de la película (actualizada para utilizar una página de diseño)</span><span class="sxs-lookup"><span data-stu-id="cb903-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="cb903-257">Página completa lista para agregar la página de la película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="cb903-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="cb903-258">Página completa lista para eliminar página de la película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="cb903-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="cb903-259">Página completa lista para editar página de la película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="cb903-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="cb903-260">Próxima</span><span class="sxs-lookup"><span data-stu-id="cb903-260">Coming Up Next</span></span>

<span data-ttu-id="cb903-261">En el siguiente tutorial, aprenderá a publicar el sitio en Internet para que todo el mundo pueda verla.</span><span class="sxs-lookup"><span data-stu-id="cb903-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb903-262">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="cb903-262">Additional Resources</span></span>

- <span data-ttu-id="cb903-263">[Crear un aspecto coherente](https://go.microsoft.com/fwlink/?LinkID=202891) : un artículo que proporciona cierto nivel de detalle más sobre cómo trabajar con diseños.</span><span class="sxs-lookup"><span data-stu-id="cb903-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="cb903-264">También describe cómo pasar un valor a una página de diseño que muestra u oculta la parte del contenido.</span><span class="sxs-lookup"><span data-stu-id="cb903-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="cb903-265">[Páginas de diseño con Razor anidadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : blogs de Mike Brind un ejemplo de cómo anidar las páginas de diseño.</span><span class="sxs-lookup"><span data-stu-id="cb903-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="cb903-266">(Incluye una descarga de las páginas).</span><span class="sxs-lookup"><span data-stu-id="cb903-266">(Includes a download of the pages.)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="cb903-267">[Anterior](deleting-data.md)
[Siguiente](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="cb903-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
