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
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="e9489-104">Introducción a las páginas Web ASP.NET - crear un diseño coherente</span><span class="sxs-lookup"><span data-stu-id="e9489-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="e9489-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e9489-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e9489-106">Este tutorial muestra cómo usar *diseños* para crear una apariencia coherente para las páginas en un sitio que usa ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="e9489-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="e9489-107">Supone que ha completado la serie a través de [eliminando la base de datos en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="e9489-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="e9489-108">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="e9489-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="e9489-109">Es lo que una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-109">What a layout page is.</span></span>
> - <span data-ttu-id="e9489-110">Cómo se combinan las páginas de diseño con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="e9489-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="e9489-111">Cómo pasar valores a una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="e9489-112">Acerca de los diseños</span><span class="sxs-lookup"><span data-stu-id="e9489-112">About Layouts</span></span>

<span data-ttu-id="e9489-113">Las páginas que ha creado hasta ahora han sido todo haya finalizado, páginas independientes.</span><span class="sxs-lookup"><span data-stu-id="e9489-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="e9489-114">Todas pertenecen al mismo sitio, pero no tienen un aspecto estándar o todos los elementos comunes.</span><span class="sxs-lookup"><span data-stu-id="e9489-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="e9489-115">Mayoría de los sitios tienen una apariencia coherente y un diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="e9489-116">Por ejemplo, si se dirige a la [Microsoft.com/Web](https://www.microsoft.com/web/) de sitio y mire, verá que las páginas de todos los adhieran a un diseño global a un tema visual:</span><span class="sxs-lookup"><span data-stu-id="e9489-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Página del sitio Microsoft.com/Web que muestra el diseño del encabezado, área de navegación, área de contenido y pie de página](layouts/_static/image1.png)

<span data-ttu-id="e9489-118">Un *ineficaz* forma para crear este diseño sería definir un encabezado, la barra de navegación y el pie de página por separado en cada una de las páginas.</span><span class="sxs-lookup"><span data-stu-id="e9489-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="e9489-119">Se podría duplicar el mismo marcado cada vez.</span><span class="sxs-lookup"><span data-stu-id="e9489-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="e9489-120">Si desea cambiar algo (por ejemplo, actualice el pie de página), tendría que cambiar por separado de cada página.</span><span class="sxs-lookup"><span data-stu-id="e9489-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="e9489-121">Es ahí donde *las páginas de diseño* venir.</span><span class="sxs-lookup"><span data-stu-id="e9489-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="e9489-122">En ASP.NET Web Pages, puede definir una página de diseño que proporciona un contenedor global para las páginas de su sitio.</span><span class="sxs-lookup"><span data-stu-id="e9489-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="e9489-123">Por ejemplo, la página de diseño puede contener el encabezado, el área de navegación y el pie de página.</span><span class="sxs-lookup"><span data-stu-id="e9489-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="e9489-124">La página de diseño incluye un marcador de posición donde se sitúa el contenido principal.</span><span class="sxs-lookup"><span data-stu-id="e9489-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="e9489-125">A continuación, puede definir las páginas de contenido individuales que contienen el marcado y el código solo para esa página.</span><span class="sxs-lookup"><span data-stu-id="e9489-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="e9489-126">Las páginas de contenido no tienen que estar páginas HTML completas; ni siquiera es necesario tener un `<body>` elemento.</span><span class="sxs-lookup"><span data-stu-id="e9489-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="e9489-127">También tienen una línea de código que indica qué página de diseño que desea mostrar el contenido ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e9489-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="e9489-128">Aquí es una imagen que muestra aproximadamente cómo funciona esta relación:</span><span class="sxs-lookup"><span data-stu-id="e9489-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagrama conceptual que se muestra dos páginas de contenido y una página de diseño en la que se ajustan](layouts/_static/image2.png)

<span data-ttu-id="e9489-130">Esta interacción es fácil de entender cuando lo vea en acción.</span><span class="sxs-lookup"><span data-stu-id="e9489-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="e9489-131">En este tutorial, cambiará las páginas de películas para usar un diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="e9489-132">Agregar una página de diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-132">Adding a Layout Page</span></span>

<span data-ttu-id="e9489-133">Comenzará creando una página de diseño que define un diseño de página típico con un encabezado, pie de página y un área para el contenido principal.</span><span class="sxs-lookup"><span data-stu-id="e9489-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="e9489-134">En el sitio WebPagesMovies, agregue una página CSHTML denominada  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e9489-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="e9489-135">El carácter de subrayado inicial ( `_` ) caracteres es significativo.</span><span class="sxs-lookup"><span data-stu-id="e9489-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="e9489-136">Si el nombre de la página se inicia con un carácter de subrayado, ASP.NET no enviar esa página directamente al explorador.</span><span class="sxs-lookup"><span data-stu-id="e9489-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="e9489-137">Esta convención permite definir las páginas que son necesarias para un sitio, pero que los usuarios no deberían poder solicitar directamente.</span><span class="sxs-lookup"><span data-stu-id="e9489-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="e9489-138">Reemplace el contenido de la página con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e9489-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="e9489-139">Como puede ver, este marcado es solo código HTML que usa `<div>` elementos para definir tres secciones en la página más uno más `<div>` elemento que contenga las tres secciones.</span><span class="sxs-lookup"><span data-stu-id="e9489-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="e9489-140">El pie de página contiene un poco de código Razor: `@DateTime.Now.Year`, que representará el año actual en esa ubicación en la página.</span><span class="sxs-lookup"><span data-stu-id="e9489-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="e9489-141">Tenga en cuenta que hay un vínculo a una hoja de estilos denominada *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="e9489-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="e9489-142">La hoja de estilos es donde se definirán los detalles de la distribución física de los elementos.</span><span class="sxs-lookup"><span data-stu-id="e9489-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="e9489-143">Va a crear en un momento.</span><span class="sxs-lookup"><span data-stu-id="e9489-143">You'll create that in a moment.</span></span>

<span data-ttu-id="e9489-144">La característica solo inusual en este  *\_Layout.cshtml* página es la `@Render.Body()` línea.</span><span class="sxs-lookup"><span data-stu-id="e9489-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="e9489-145">Que es el marcador de posición donde irá el contenido cuando este diseño se combina con otra página.</span><span class="sxs-lookup"><span data-stu-id="e9489-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="e9489-146">Agregar un archivo .css</span><span class="sxs-lookup"><span data-stu-id="e9489-146">Adding a .css File</span></span>

<span data-ttu-id="e9489-147">La mejor manera de definir la disposición real (es decir, apariencia) de los elementos de la página es usar las reglas en cascada (CSS) de la hoja de estilo.</span><span class="sxs-lookup"><span data-stu-id="e9489-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="e9489-148">Por lo que crearé un *.css* archivo con las reglas para el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="e9489-149">En WebMatrix, seleccione la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="e9489-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="e9489-150">A continuación, en el **archivos** pestaña de la cinta de opciones, haga clic en la flecha situada bajo la **New** botón y, a continuación, haga clic en **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e9489-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![La opción 'Nueva carpeta' debajo de nuevo en la cinta de opciones.](layouts/_static/image3.png)

<span data-ttu-id="e9489-152">Nombre de la nueva carpeta *estilos*.</span><span class="sxs-lookup"><span data-stu-id="e9489-152">Name the new folder *Styles*.</span></span>

!['La nueva carpeta los estilos de nombres '](layouts/_static/image4.png)

<span data-ttu-id="e9489-154">Dentro de la nueva *estilos* carpeta, cree un archivo denominado *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="e9489-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Crear un nuevo archivo Movies.css](layouts/_static/image5.png)

<span data-ttu-id="e9489-156">Reemplace el contenido del nuevo *.css* archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e9489-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="e9489-157">No se dice mucho acerca de estas reglas CSS, excepto para destacar dos cosas.</span><span class="sxs-lookup"><span data-stu-id="e9489-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="e9489-158">Una es que además de configurar las fuentes y tamaños, las reglas de usan el posicionamiento absoluto para establecer la ubicación del encabezado, pie de página y área de contenido principal.</span><span class="sxs-lookup"><span data-stu-id="e9489-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="e9489-159">Si está familiarizado con la colocación de CSS, puede leer el [posicionamiento de CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial en el sitio W3Schools.</span><span class="sxs-lookup"><span data-stu-id="e9489-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="e9489-160">La otra cosa a tener en cuenta que en la parte inferior, hemos copiado las reglas de estilo que estaban originalmente se define por separado en el *Movies.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="e9489-161">Estas reglas se usaron en el [Introducción a los datos mostrar por ASP.NET Web Pages de uso](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para realizar la `WebGrid` auxiliar representar el marcado que agrega franjas a la tabla.</span><span class="sxs-lookup"><span data-stu-id="e9489-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="e9489-162">(Si va a usar un *.css* archivo para definiciones de estilo, también puede poner las reglas de estilo para todo el sitio en él.)</span><span class="sxs-lookup"><span data-stu-id="e9489-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="e9489-163">Actualizando el archivo de películas para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="e9489-164">Ahora puede actualizar los archivos existentes en el sitio para usar el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="e9489-165">Abra el *Movies.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="e9489-166">En la parte superior, como la primera línea de código, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e9489-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="e9489-167">La página empieza ahora de esta manera:</span><span class="sxs-lookup"><span data-stu-id="e9489-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="e9489-168">Esta línea de código indica a ASP.NET que, cuando el *películas* página se ejecuta, se debe combinar con el  *\_Layout.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="e9489-169">Puesto que la *Movies.cshtml* archivo ahora usa una página de diseño, puede quitar el marcado de la *Movies.cshtml* página que se ha ocupado de por el  *\_Layout.cshtml*archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="e9489-170">Saque el `<!DOCTYPE>`, `<html>`, y `<body>` de apertura y cierre de las etiquetas.</span><span class="sxs-lookup"><span data-stu-id="e9489-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="e9489-171">Todo el contenido se lleve `<head>` elemento y su contenido, que incluye las reglas de estilo de la cuadrícula, ya que ahora tienes esas reglas un *.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="e9489-172">Mientras está en ello, cambie el existente `<h1>` elemento a una `<h2>` elemento; debe tener un `<h1>` elemento en la página de diseño ya.</span><span class="sxs-lookup"><span data-stu-id="e9489-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="e9489-173">Cambiar el `<h2>` texto a "Lista de películas".</span><span class="sxs-lookup"><span data-stu-id="e9489-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="e9489-174">Normalmente no tiene que realizar estos tipos de cambios en una página de contenido.</span><span class="sxs-lookup"><span data-stu-id="e9489-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="e9489-175">Al empezar su sitio con una página de diseño, que empiece por crear páginas de contenido sin todos estos elementos.</span><span class="sxs-lookup"><span data-stu-id="e9489-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="e9489-176">En este caso, no obstante, va a convertir una página independiente a otro que usa un diseño, por lo que es un poco de limpieza.</span><span class="sxs-lookup"><span data-stu-id="e9489-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="e9489-177">Cuando haya terminado, la *Movies.cshtml* página tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="e9489-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="e9489-178">Probar el diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-178">Testing the Layout</span></span>

<span data-ttu-id="e9489-179">Ahora puede ver el aspecto del diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="e9489-180">En WebMatrix, haga clic en el *Movies.cshtml* página y seleccione **iniciar en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="e9489-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="e9489-181">Cuando el explorador muestra la página, se parece a esta página:</span><span class="sxs-lookup"><span data-stu-id="e9489-181">When the browser displays the page, it looks like this page:</span></span>

![Representado mediante un diseño de página de películas](layouts/_static/image6.png)

<span data-ttu-id="e9489-183">ASP.NET se ha combinado el contenido de la página Movies.cshtml en la  *\_Layout.cshtml* página derecha donde el `RenderBody` es el método.</span><span class="sxs-lookup"><span data-stu-id="e9489-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="e9489-184">Y por supuesto el  *\_Layout.cshtml* página referencias una *.css* archivo que define el aspecto de la página.</span><span class="sxs-lookup"><span data-stu-id="e9489-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="e9489-185">Actualizar la página AddMovie para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="e9489-186">La verdadera ventaja de diseños es que puede usarlos para todas las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="e9489-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="e9489-187">Abra el *AddMovie.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="e9489-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="e9489-188">Es posible que recuerde que el *AddMovie.cshtml* página originalmente tenía algunas reglas CSS en él para definir la apariencia de los mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="e9489-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="e9489-189">Puesto que tiene un *.css* archivo para su sitio ahora, puede mover esas reglas para la *.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="e9489-190">Quitar el *AddMovie.cshtml* de archivos y agregarlos a la parte inferior de la *Movies.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9489-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="e9489-191">Va a mover las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="e9489-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="e9489-192">Ahora que las ordenaciones de cambios en el mismas *AddMovie.cshtml* que siguió para *Movies.cshtml* : agregar `Layout="~/_Layout.cshtml;` y quite la marca HTML que ahora está relacionada.</span><span class="sxs-lookup"><span data-stu-id="e9489-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="e9489-193">Cambie el elemento `<h1>` por `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="e9489-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="e9489-194">Cuando haya terminado, la página tendrá un aspecto similar a este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e9489-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="e9489-195">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="e9489-195">Run the page.</span></span> <span data-ttu-id="e9489-196">Parece ahora a esta ilustración:</span><span class="sxs-lookup"><span data-stu-id="e9489-196">Now it looks like this illustration:</span></span>

![Representado mediante un diseño de página 'Agregar películas'](layouts/_static/image7.png)

<span data-ttu-id="e9489-198">Desea realizar modificaciones similares a las páginas del sitio: *EditMovie.cshtml* y *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e9489-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="e9489-199">Sin embargo, antes de hacerlo, puede realizar otro cambio en el diseño de manera un poco más flexibles.</span><span class="sxs-lookup"><span data-stu-id="e9489-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="e9489-200">Pasar información del título a la página de diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="e9489-201">El  *\_Layout.cshtml* página que ha creado tiene un `<title>` elemento que se establece en "Mi sitio de película".</span><span class="sxs-lookup"><span data-stu-id="e9489-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="e9489-202">La mayoría de los exploradores mostrar el contenido de este elemento como el texto en una pestaña:</span><span class="sxs-lookup"><span data-stu-id="e9489-202">Most browsers display the content of this element as the text on a tab:</span></span>

![La página &lt;título&gt; elemento aparece en una pestaña del explorador](layouts/_static/image8.png)

<span data-ttu-id="e9489-204">Esta información de título es genérica.</span><span class="sxs-lookup"><span data-stu-id="e9489-204">This title information is generic.</span></span> <span data-ttu-id="e9489-205">Suponga que desea que el texto del título para ser más específico a la página actual.</span><span class="sxs-lookup"><span data-stu-id="e9489-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="e9489-206">(El texto del título es también usa los motores de búsqueda para determinar cuál es la página acerca de.) Puede pasar información de una página de contenido como *Movies.cshtml* o *AddMovie.cshtml* a la página de diseño y, a continuación, usar esa información para personalizar la página de diseño de qué representa.</span><span class="sxs-lookup"><span data-stu-id="e9489-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="e9489-207">Abra el *Movies.cshtml* página nuevo.</span><span class="sxs-lookup"><span data-stu-id="e9489-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="e9489-208">En el código en la parte superior, agregue la siguiente línea:</span><span class="sxs-lookup"><span data-stu-id="e9489-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="e9489-209">El `Page` está disponible en todos los objetos *.cshtml* páginas y es para este propósito, es decir, para compartir información entre una página y su diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="e9489-210">Abra el<em>\_Layout.cshtml</em> página.</span><span class="sxs-lookup"><span data-stu-id="e9489-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="e9489-211">Cambiar el `<title>` elemento para que quede como este marcado:</span><span class="sxs-lookup"><span data-stu-id="e9489-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="e9489-212">Este código representa cualquier cosa que esté en el `Page.Title` propiedad directamente en esa ubicación en la página.</span><span class="sxs-lookup"><span data-stu-id="e9489-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="e9489-213">Ejecute el *Movies.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="e9489-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="e9489-214">Esta vez la pestaña del explorador muestra la información que pasa como el valor de `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="e9489-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Una pestaña del explorador que muestra el título que se crean de forma dinámica](layouts/_static/image9.png)

<span data-ttu-id="e9489-216">Si lo desea, puede ver el origen de la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="e9489-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="e9489-217">Puede ver que el `<title>` elemento se representa como `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="e9489-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="e9489-218">**El objeto de página**</span><span class="sxs-lookup"><span data-stu-id="e9489-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="e9489-219">Una característica útil de `Page` es que es un objeto dinámico, el `Title` propiedad no es un nombre reservado o fijo.</span><span class="sxs-lookup"><span data-stu-id="e9489-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="e9489-220">Puede usar *cualquier* nombre para un valor de la `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="e9489-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="e9489-221">Por ejemplo, podría fácilmente ha pasado el título mediante el uso de una propiedad denominada `Page.CurrentName` o `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="e9489-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="e9489-222">La única restricción es que el nombre debe seguir las reglas normales para qué propiedades se pueden denominar.</span><span class="sxs-lookup"><span data-stu-id="e9489-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="e9489-223">(Por ejemplo, el nombre no puede contener un espacio).</span><span class="sxs-lookup"><span data-stu-id="e9489-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="e9489-224">Puede pasar cualquier número de valores mediante la `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="e9489-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="e9489-225">Si desea pasar información de películas a la página de diseño, podría pasar valores mediante el uso de algo parecido a `Page.MovieTitle` y `Page.Genre` y `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="e9489-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="e9489-226">(O cualquier otro nombre que inventaron para almacenar la información.) El único requisito, que es probablemente obvio, es que se debe usar los mismos nombres en la página de contenido y la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="e9489-227">La información que se pasa mediante el uso de la `Page` objeto no se limita a simplemente texto que se muestra en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="e9489-228">Puede pasar un valor a la página de diseño y, a continuación, el código en la página de diseño puede utilizar el valor para decidir si se muestra una sección de la página, lo que *.css* archivo para usar y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="e9489-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="e9489-229">Los valores pasados en el `Page` objeto son como cualquier otro los valores que se utiliza en código.</span><span class="sxs-lookup"><span data-stu-id="e9489-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="e9489-230">Simplemente es que los valores se originan en la página de contenido y se pasan a la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="e9489-231">Abra el *AddMovie.cshtml* página y agregue una línea a la parte superior del código que proporciona un título para el *AddMovie.cshtml* página:</span><span class="sxs-lookup"><span data-stu-id="e9489-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="e9489-232">Ejecute el *AddMovie.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="e9489-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="e9489-233">Consulte el nuevo título:</span><span class="sxs-lookup"><span data-stu-id="e9489-233">You see the new title there:</span></span>

![Una pestaña del explorador que muestra el título de 'Agregar películas' creado dinámicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="e9489-235">Actualización de las páginas restantes para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="e9489-236">Ahora puede finalizar el resto de páginas en el sitio para que usen el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="e9489-237">Abra *EditMovie.cshtml* y *DeleteMovie.cshtml* a su vez y realizar los mismos cambios en cada uno.</span><span class="sxs-lookup"><span data-stu-id="e9489-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="e9489-238">Agregue la línea de código que se vincula a la página de diseño:</span><span class="sxs-lookup"><span data-stu-id="e9489-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="e9489-239">Agregue una línea para establecer el título de la página:</span><span class="sxs-lookup"><span data-stu-id="e9489-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="e9489-240">O bien</span><span class="sxs-lookup"><span data-stu-id="e9489-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="e9489-241">Quitar todo el marcado HTML extraño, básicamente, deje sólo las partes que hay dentro de la `<body>` elemento (más en el bloque de código en la parte superior).</span><span class="sxs-lookup"><span data-stu-id="e9489-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="e9489-242">Cambiar el `<h1>` elemento para que sea un `<h2>` elemento.</span><span class="sxs-lookup"><span data-stu-id="e9489-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="e9489-243">Cuando haya realizado estos cambios, probar cada una y asegúrese de que se muestra correctamente y que el título es correcto.</span><span class="sxs-lookup"><span data-stu-id="e9489-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="e9489-244">Partición ideas acerca de las páginas de diseño</span><span class="sxs-lookup"><span data-stu-id="e9489-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="e9489-245">En este tutorial ha creado un  *\_Layout.cshtml* página y utiliza el `RenderBody` método para combinar el contenido de otra página.</span><span class="sxs-lookup"><span data-stu-id="e9489-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="e9489-246">Que es el patrón básico para el uso de los diseños de páginas Web.</span><span class="sxs-lookup"><span data-stu-id="e9489-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="e9489-247">Las páginas de diseño tienen características adicionales que no tratamos aquí.</span><span class="sxs-lookup"><span data-stu-id="e9489-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="e9489-248">Por ejemplo, puede anidar las páginas de diseño, una página de diseño a su vez puede hacerse referencia sí.</span><span class="sxs-lookup"><span data-stu-id="e9489-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="e9489-249">Los diseños anidados pueden ser útiles si está trabajando con las subsecciones de un sitio que requieren diseños diferentes.</span><span class="sxs-lookup"><span data-stu-id="e9489-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="e9489-250">También puede usar métodos adicionales (por ejemplo, `RenderSection`) para configurar denominado secciones en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="e9489-251">La combinación de las páginas de diseño y *.css* archivos es eficaz.</span><span class="sxs-lookup"><span data-stu-id="e9489-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="e9489-252">Como verá en la siguiente serie de tutoriales, en WebMatrix puede crear un sitio basado en un *plantilla*, que proporciona un sitio que tiene funciones predeterminadas en ella.</span><span class="sxs-lookup"><span data-stu-id="e9489-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="e9489-253">Las plantillas de hacen buen uso de CSS para crear sitios que muy bien y que tienen características como menús y páginas de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="e9489-254">Aquí es una captura de pantalla de la página principal de un sitio basado en una plantilla, que muestra las características que usan las páginas de diseño y CSS:</span><span class="sxs-lookup"><span data-stu-id="e9489-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Creado por la plantilla de sitio de WebMatrix que muestra el encabezado, el área de navegación, área de contenido, sección opcional y vínculos de inicio de sesión de diseño](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="e9489-256">Lista completa de la página de película (actualizada para usar una página de diseño)</span><span class="sxs-lookup"><span data-stu-id="e9489-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="e9489-257">Página completa lista para agregar página de la película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="e9489-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="e9489-258">Página completa lista para eliminar página de la película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="e9489-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="e9489-259">Página completa lista para la página de edición de película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="e9489-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="e9489-260">Próxima</span><span class="sxs-lookup"><span data-stu-id="e9489-260">Coming Up Next</span></span>

<span data-ttu-id="e9489-261">En el siguiente tutorial, obtendrá información sobre cómo publicar su sitio en Internet para que todo el mundo pueda verla.</span><span class="sxs-lookup"><span data-stu-id="e9489-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9489-262">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e9489-262">Additional Resources</span></span>

- <span data-ttu-id="e9489-263">[Creación de un aspecto coherente](https://go.microsoft.com/fwlink/?LinkID=202891) , un artículo que proporciona algunos detalles más sobre cómo trabajar con diseños.</span><span class="sxs-lookup"><span data-stu-id="e9489-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="e9489-264">También se describe cómo pasar un valor a una página de diseño que muestre u oculte parte del contenido.</span><span class="sxs-lookup"><span data-stu-id="e9489-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="e9489-265">[Páginas de diseño con Razor anidadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : blogs de Mike Brind un ejemplo de cómo anidar las páginas de diseño.</span><span class="sxs-lookup"><span data-stu-id="e9489-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="e9489-266">(Incluye la descarga de las páginas).</span><span class="sxs-lookup"><span data-stu-id="e9489-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e9489-267">[Anterior](deleting-data.md)
> [Siguiente](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="e9489-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
