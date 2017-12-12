---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Introducción a ASP.NET Web Pages: se eliminarán los datos de la base de datos | Documentos de Microsoft"
author: tfitzmac
description: "Este tutorial muestra cómo eliminar una entrada de la base de datos individuales. Supone que ha completado la serie a través de actualizar los datos de base de datos de ASP.NET Web PA..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: aef31b6170cc3bba2421eb8c2c41e83aadc129c5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="57d4c-104">Introducción a las páginas Web ASP.NET: se eliminarán los datos de la base de datos</span><span class="sxs-lookup"><span data-stu-id="57d4c-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="57d4c-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="57d4c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="57d4c-106">Este tutorial muestra cómo eliminar una entrada de la base de datos individuales.</span><span class="sxs-lookup"><span data-stu-id="57d4c-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="57d4c-107">Supone que ha completado la serie a través de [actualizar la base de datos en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251583).</span><span class="sxs-lookup"><span data-stu-id="57d4c-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251583).</span></span>
> 
> <span data-ttu-id="57d4c-108">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="57d4c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="57d4c-109">Cómo seleccionar un registro individual de una lista de registros.</span><span class="sxs-lookup"><span data-stu-id="57d4c-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="57d4c-110">Cómo eliminar un único registro de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="57d4c-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="57d4c-111">Cómo comprobar que se ha hecho clic en un botón específico en un formulario.</span><span class="sxs-lookup"><span data-stu-id="57d4c-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="57d4c-112">Características y tecnologías que se tratan:</span><span class="sxs-lookup"><span data-stu-id="57d4c-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="57d4c-113">El `WebGrid` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="57d4c-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="57d4c-114">La instrucción SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="57d4c-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="57d4c-115">El `Database.Execute` método para ejecutar una instancia de SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="57d4c-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="57d4c-116">Lo que vamos a compilar</span><span class="sxs-lookup"><span data-stu-id="57d4c-116">What You'll Build</span></span>

<span data-ttu-id="57d4c-117">En el tutorial anterior, aprendió a actualizar un registro de base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="57d4c-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="57d4c-118">Este tutorial es similar, salvo que en lugar de actualizar el registro, podrá eliminarla.</span><span class="sxs-lookup"><span data-stu-id="57d4c-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="57d4c-119">Los procesos son igual, salvo que eliminar es más sencillo, por lo que este tutorial son corto.</span><span class="sxs-lookup"><span data-stu-id="57d4c-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="57d4c-120">En el *películas* página, podrá actualizar la `WebGrid` auxiliar por lo que muestra un **eliminar** vínculo situado junto a cada película para acompañar la **editar** vínculo que agregó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="57d4c-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Página de películas que muestra un vínculo de eliminación para cada película](deleting-data/_static/image1.png)

<span data-ttu-id="57d4c-122">Al igual que con la edición, al hacer clic en el **eliminar** vínculo, le lleva a una página distinta, donde la información de películas ya está en un formulario:</span><span class="sxs-lookup"><span data-stu-id="57d4c-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Eliminar página de la película con una película muestra](deleting-data/_static/image2.png)

<span data-ttu-id="57d4c-124">A continuación, hacer clic en el botón para eliminar el registro de forma permanente.</span><span class="sxs-lookup"><span data-stu-id="57d4c-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="57d4c-125">Agregar un vínculo Eliminar a la lista de películas</span><span class="sxs-lookup"><span data-stu-id="57d4c-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="57d4c-126">Podrá empezar agregando una **eliminar** vincular a la `WebGrid` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="57d4c-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="57d4c-127">Este vínculo es similar a la **editar** vínculo que agregó en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="57d4c-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="57d4c-128">Abra la *Movies.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="57d4c-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="57d4c-129">Cambiar el `WebGrid` marcado en el cuerpo de la página mediante la adición de una columna.</span><span class="sxs-lookup"><span data-stu-id="57d4c-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="57d4c-130">Este es el marcado modificado:</span><span class="sxs-lookup"><span data-stu-id="57d4c-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="57d4c-131">Esta es la nueva columna:</span><span class="sxs-lookup"><span data-stu-id="57d4c-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="57d4c-132">La manera en que se configura la cuadrícula, la **editar** columna está en la cuadrícula de la parte izquierda y la **eliminar** columna es más a la derecha.</span><span class="sxs-lookup"><span data-stu-id="57d4c-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="57d4c-133">(Hay una coma después de la `Year` columna ahora, en caso de no tenga en cuenta que.) No hay nada especial acerca de dónde ir estas columnas de vínculo y tan fácilmente podría poner junto a la otra.</span><span class="sxs-lookup"><span data-stu-id="57d4c-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="57d4c-134">En este caso, son independientes para hacerlos más difíciles se mezclen.</span><span class="sxs-lookup"><span data-stu-id="57d4c-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Página de películas con vínculos de edición y detalles de la marca para indicar que no encuentra al lado de otra](deleting-data/_static/image3.png)

<span data-ttu-id="57d4c-136">La nueva columna muestra un vínculo (`<a>` elemento) cuyo texto dice "Delete".</span><span class="sxs-lookup"><span data-stu-id="57d4c-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="57d4c-137">El destino del vínculo (su `href` atributo) es el código que finalmente se resuelve como algo parecido a esta dirección URL, con el `id` valor diferente para cada película:</span><span class="sxs-lookup"><span data-stu-id="57d4c-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="57d4c-138">Este vínculo va a invocar una página denominada *DeleteMovie* y pase el Id. de la película que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="57d4c-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="57d4c-139">Este tutorial no incluyen detalles sobre cómo se crea este vínculo, porque es casi idéntico a la **editar** vínculo desde el tutorial anterior ([actualizar la base de datos en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251583)).</span><span class="sxs-lookup"><span data-stu-id="57d4c-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251583)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="57d4c-140">Creación de la página de borrado</span><span class="sxs-lookup"><span data-stu-id="57d4c-140">Creating the Delete Page</span></span>

<span data-ttu-id="57d4c-141">Ahora puede crear la página que será el destino para la **eliminar** vínculo en la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="57d4c-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="57d4c-142">**Importante** la técnica de seleccionando primero un registro para eliminar y, a continuación, con una página independiente y un botón para confirmar el proceso es muy importante para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="57d4c-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="57d4c-143">Tal y como se ha leído en tutoriales anteriores, realizar *cualquier* tipo de cambio en el sitio Web debe *siempre* realizarse mediante un formulario &mdash; es decir, usando una operación HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="57d4c-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="57d4c-144">Si ha realizado posible cambiar el sitio simplemente haciendo clic en un vínculo (es decir, con una operación GET), personas pudieron hacer solicitudes sencillas a su sitio y eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="57d4c-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="57d4c-145">Incluso un agente del motor de búsqueda que se está indizando el sitio podría eliminar accidentalmente datos por los vínculos siguientes.</span><span class="sxs-lookup"><span data-stu-id="57d4c-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="57d4c-146">Cuando la aplicación permite que personas cambia un registro, tendrá que presenta el registro al usuario para su edición de todos modos.</span><span class="sxs-lookup"><span data-stu-id="57d4c-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="57d4c-147">Pero podría verse tentado a omitir este paso para eliminar un registro.</span><span class="sxs-lookup"><span data-stu-id="57d4c-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="57d4c-148">No omita este paso, aunque.</span><span class="sxs-lookup"><span data-stu-id="57d4c-148">Don't skip that step, though.</span></span> <span data-ttu-id="57d4c-149">(También es útil para los usuarios vean el registro y confirme que está eliminando el registro que han diseñado).</span><span class="sxs-lookup"><span data-stu-id="57d4c-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="57d4c-150">En un conjunto de tutoriales posterior, verá cómo agregar funcionalidad de inicio de sesión para que un usuario tendría que iniciar sesión antes de eliminar un registro.</span><span class="sxs-lookup"><span data-stu-id="57d4c-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="57d4c-151">Crear una página denominada *DeleteMovie.cshtml* y sustituir lo que aparece en el archivo con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="57d4c-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="57d4c-152">Este marcado es similar a la *EditMovie* páginas, salvo que en lugar de usar los cuadros de texto (`<input type="text">`), incluye el marcado `<span>` elementos.</span><span class="sxs-lookup"><span data-stu-id="57d4c-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="57d4c-153">No hay nada aquí para editar.</span><span class="sxs-lookup"><span data-stu-id="57d4c-153">There's nothing here to edit.</span></span> <span data-ttu-id="57d4c-154">Todo lo que tiene que hacer es mostrar los detalles de la película para que los usuarios pueden asegurarse de que está eliminando la película derecho.</span><span class="sxs-lookup"><span data-stu-id="57d4c-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="57d4c-155">El marcado ya contiene un vínculo que permite al usuario volver a la página de lista de películas.</span><span class="sxs-lookup"><span data-stu-id="57d4c-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="57d4c-156">Como en el *EditMovie* página, el identificador de la película seleccionada se almacena en un campo oculto.</span><span class="sxs-lookup"><span data-stu-id="57d4c-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="57d4c-157">(Se pasa en la página en primer lugar como un valor de cadena de consulta.) Hay un `Html.ValidationSummary` llamada que se mostrará los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="57d4c-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="57d4c-158">En este caso, el error puede ser que se ha pasado ningún identificador de película a la página o el identificador de película no es válido.</span><span class="sxs-lookup"><span data-stu-id="57d4c-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="57d4c-159">Esta situación puede producirse si alguien ejecutó esta página sin activar primero una película en la *películas* página.</span><span class="sxs-lookup"><span data-stu-id="57d4c-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="57d4c-160">El título del botón es **eliminar película**, y su nombre de atributo se establece en `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="57d4c-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="57d4c-161">El `name` atributo se usará en el código para identificar el botón que envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="57d4c-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="57d4c-162">Tendrá que escribir código para 1) leer los detalles de la película cuando se muestra la página por primera vez y (2) realmente elimina la película cuando el usuario hace clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="57d4c-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="57d4c-163">Agregar código para leer una sola película</span><span class="sxs-lookup"><span data-stu-id="57d4c-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="57d4c-164">En la parte superior de la *DeleteMovie.cshtml* página, agregue el siguiente bloque de código:</span><span class="sxs-lookup"><span data-stu-id="57d4c-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="57d4c-165">Esta marca es el mismo que el código correspondiente en el *EditMovie* página.</span><span class="sxs-lookup"><span data-stu-id="57d4c-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="57d4c-166">Obtiene el identificador de película fuera de la cadena de consulta y utiliza el identificador para leer un registro de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="57d4c-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="57d4c-167">El código incluye la prueba de validación (`IsInt()` y `row != null`) para asegurarse de que el identificador de película que se pasa a la página es válido.</span><span class="sxs-lookup"><span data-stu-id="57d4c-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="57d4c-168">Recuerde que este código sólo debe ejecutar la primera vez que se ejecuta la página.</span><span class="sxs-lookup"><span data-stu-id="57d4c-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="57d4c-169">No desea volver a leer el registro de la película de la base de datos cuando el usuario hace clic en el **eliminar película** botón.</span><span class="sxs-lookup"><span data-stu-id="57d4c-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="57d4c-170">Por lo tanto, el código para leer la película esté dentro de una prueba que dice `if(!IsPost)` &mdash; es decir, *si la solicitud no es una operación post (envío del formulario)*.</span><span class="sxs-lookup"><span data-stu-id="57d4c-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="57d4c-171">Agregar código para eliminar la película seleccionada</span><span class="sxs-lookup"><span data-stu-id="57d4c-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="57d4c-172">Para eliminar la película cuando el usuario hace clic en el botón, agregue el código siguiente justo dentro de la llave de cierre de la `@` bloque:</span><span class="sxs-lookup"><span data-stu-id="57d4c-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="57d4c-173">Este código es similar al código para actualizar un registro existente, pero es más sencillo.</span><span class="sxs-lookup"><span data-stu-id="57d4c-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="57d4c-174">Básicamente, el código ejecuta una instancia de SQL `Delete` instrucción.</span><span class="sxs-lookup"><span data-stu-id="57d4c-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="57d4c-175">Como en el *EditMovie* página, el código está en un `if(IsPost)` bloque.</span><span class="sxs-lookup"><span data-stu-id="57d4c-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="57d4c-176">En esta ocasión, el `if()` condición es un poco más complicada:</span><span class="sxs-lookup"><span data-stu-id="57d4c-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="57d4c-177">Hay dos condiciones aquí.</span><span class="sxs-lookup"><span data-stu-id="57d4c-177">There are two conditions here.</span></span> <span data-ttu-id="57d4c-178">La primera es que se va a enviar la página, tal y como se ha visto antes &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="57d4c-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="57d4c-179">La segunda condición es `!Request["buttonDelete"].IsEmpty()`, lo que significa que la solicitud tiene un objeto denominado `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="57d4c-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="57d4c-180">Es verdad, es una forma indirecta de probar qué botón enviado el formulario.</span><span class="sxs-lookup"><span data-stu-id="57d4c-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="57d4c-181">Si un formulario contiene varios botones de envío, solo el nombre del botón que se ha hecho clic aparece en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="57d4c-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="57d4c-182">Por lo tanto, lógicamente, si el nombre de un botón determinado aparece en la solicitud &mdash; o como se indica en el código, si dicho botón no está vacío &mdash; que es el botón que envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="57d4c-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="57d4c-183">El `&&` operador significa "and" (AND lógico).</span><span class="sxs-lookup"><span data-stu-id="57d4c-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="57d4c-184">Por lo tanto, toda la matriz `if` condición es...</span><span class="sxs-lookup"><span data-stu-id="57d4c-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="57d4c-185">*Esta solicitud es una entrada de blog (no una solicitud de la primera vez)*</span><span class="sxs-lookup"><span data-stu-id="57d4c-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="57d4c-186">AND</span><span class="sxs-lookup"><span data-stu-id="57d4c-186">AND</span></span>  
  
<span data-ttu-id="57d4c-187">*El* `buttonDelete` *botón fue el botón que envía el formulario.*</span><span class="sxs-lookup"><span data-stu-id="57d4c-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="57d4c-188">Este formulario (de hecho, esta página) contiene un solo botón, por lo que la prueba adicional para `buttonDelete` técnicamente no es necesario.</span><span class="sxs-lookup"><span data-stu-id="57d4c-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="57d4c-189">No obstante, está a punto de realizar una operación que se quitará de forma permanente los datos.</span><span class="sxs-lookup"><span data-stu-id="57d4c-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="57d4c-190">Por lo que desea que estén tan seguro como sea posible que se va a realizar la operación solo cuando el usuario lo ha solicitado explícitamente.</span><span class="sxs-lookup"><span data-stu-id="57d4c-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="57d4c-191">Por ejemplo, suponga que expande esta página más adelante y agregarle otros botones.</span><span class="sxs-lookup"><span data-stu-id="57d4c-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="57d4c-192">Aún así, el código que elimina la película se ejecutará solo si el `buttonDelete` se hace clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="57d4c-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="57d4c-193">Como en el *EditMovie* página, obtener el identificador del campo oculto y, a continuación, ejecute el comando SQL.</span><span class="sxs-lookup"><span data-stu-id="57d4c-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="57d4c-194">La sintaxis de la `Delete` instrucción es:</span><span class="sxs-lookup"><span data-stu-id="57d4c-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="57d4c-195">Es esencial para incluir la `WHERE` cláusula y el identificador.</span><span class="sxs-lookup"><span data-stu-id="57d4c-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="57d4c-196">Si deja fuera de la cláusula WHERE, *se eliminarán todos los registros en la tabla*.</span><span class="sxs-lookup"><span data-stu-id="57d4c-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="57d4c-197">Como ha visto, pase el valor de identificador para el comando SQL mediante el uso de un marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="57d4c-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="57d4c-198">Probar el proceso de eliminación de película</span><span class="sxs-lookup"><span data-stu-id="57d4c-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="57d4c-199">Ahora puede probar.</span><span class="sxs-lookup"><span data-stu-id="57d4c-199">Now you can test.</span></span> <span data-ttu-id="57d4c-200">Ejecute el *películas* página y haga clic en **eliminar** situada junto a una película.</span><span class="sxs-lookup"><span data-stu-id="57d4c-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="57d4c-201">Cuando el *DeleteMovie* aparece en la página, haga clic en **eliminar película**.</span><span class="sxs-lookup"><span data-stu-id="57d4c-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Eliminar página de la película con el botón Eliminar película resaltado](deleting-data/_static/image4.png)

<span data-ttu-id="57d4c-203">Al hacer clic en el botón, el código elimina las películas y devuelve a la lista de películas.</span><span class="sxs-lookup"><span data-stu-id="57d4c-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="57d4c-204">Se puede buscar la película se eliminó y confirme que se ha eliminado.</span><span class="sxs-lookup"><span data-stu-id="57d4c-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="57d4c-205">Próxima</span><span class="sxs-lookup"><span data-stu-id="57d4c-205">Coming Up Next</span></span>

<span data-ttu-id="57d4c-206">El siguiente tutorial muestra cómo proporcionar a todas las páginas en su sitio un aspecto y un diseño común.</span><span class="sxs-lookup"><span data-stu-id="57d4c-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="57d4c-207">Muestra la lista completa de página de la película (actualizada con vínculos eliminar)</span><span class="sxs-lookup"><span data-stu-id="57d4c-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="57d4c-208">Muestra la lista completa de página DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="57d4c-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="57d4c-209">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="57d4c-209">Additional Resources</span></span>

- [<span data-ttu-id="57d4c-210">Introducción a la programación Web de ASP.NET mediante la sintaxis de Razor</span><span class="sxs-lookup"><span data-stu-id="57d4c-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="57d4c-211">[Instrucción DELETE de SQL](http://www.w3schools.com/sql/sql_delete.asp) en el sitio W3Schools</span><span class="sxs-lookup"><span data-stu-id="57d4c-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="57d4c-212">[Anterior](updating-data.md)
[Siguiente](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="57d4c-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
