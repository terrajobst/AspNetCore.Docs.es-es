---
title: "Actualización de las páginas generadas"
author: rick-anderson
description: "Actualización de las páginas generadas con una mejor visualización."
keywords: "ASP.NET Core,páginas de Razor"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: dfe8136dccb0e98a9fc6b1395161ccb442392c76
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="71efe-104">Actualización de las páginas generadas</span><span class="sxs-lookup"><span data-stu-id="71efe-104">Updating the generated pages</span></span>

<span data-ttu-id="71efe-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71efe-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71efe-106">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="71efe-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="71efe-107">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).</span><span class="sxs-lookup"><span data-stu-id="71efe-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="71efe-109">Actualización del código generado</span><span class="sxs-lookup"><span data-stu-id="71efe-109">Update the generated code</span></span>

<span data-ttu-id="71efe-110">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="71efe-110">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="71efe-111">Haga clic con el botón derecho en una línea ondulada roja > ** Acciones rápidas y refactorizaciones**.</span><span class="sxs-lookup"><span data-stu-id="71efe-111">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

<span data-ttu-id="71efe-113">Seleccione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="71efe-113">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  <span data-ttu-id="71efe-115">Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="71efe-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="71efe-116">En el tutorial siguiente se habla sobre [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6).</span><span class="sxs-lookup"><span data-stu-id="71efe-116">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="71efe-117">El atributo [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="71efe-117">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="71efe-118">El atributo [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de los datos (Date), así que la información de hora almacenada en el campo no se muestra.</span><span class="sxs-lookup"><span data-stu-id="71efe-118">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="71efe-119">Vaya a Pages/Movies y mantenga el mouse sobre un vínculo de **edición** para ver la dirección URL de destino.</span><span class="sxs-lookup"><span data-stu-id="71efe-119">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Ventana del explorador con el mouse sobre el vínculo de edición y aparece una dirección URL de vínculo http://localhost:1234/Movies/Edit/5](da1/edit7.png)

<span data-ttu-id="71efe-121">Los vínculos **Edit**, **Details** y **Delete** son generados por la [aplicación auxiliar de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="71efe-121">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="71efe-122">Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) permiten al código de servidor participar en la creación y representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="71efe-122">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="71efe-123">En el código anterior, `AnchorTagHelper` genera de forma dinámica el valor del atributo `href` HTML desde la página de Razor (la ruta es relativa), el elemento `asp-page` y el identificador de ruta (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="71efe-123">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="71efe-124">Vea [Generación de direcciones URL para las páginas](xref:mvc/razor-pages/index#url-generation-for-pages) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="71efe-124">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="71efe-125">Use **Ver código fuente** en su explorador preferido para examinar el marcado generado.</span><span class="sxs-lookup"><span data-stu-id="71efe-125">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="71efe-126">A continuación se muestra una parte del HTML generado:</span><span class="sxs-lookup"><span data-stu-id="71efe-126">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="71efe-127">Los vínculos generados de forma dinámica pasan el identificador de la película con una cadena de consulta (por ejemplo, `http://localhost:5000/Movies/Details?id=2`).</span><span class="sxs-lookup"><span data-stu-id="71efe-127">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="71efe-128">Actualice las páginas de edición, detalles y eliminación de Razor para usar la plantilla de ruta "{id:int}".</span><span class="sxs-lookup"><span data-stu-id="71efe-128">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="71efe-129">Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="71efe-129">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="71efe-130">Ejecute la aplicación y luego vea el origen.</span><span class="sxs-lookup"><span data-stu-id="71efe-130">Run the app and then view source.</span></span> <span data-ttu-id="71efe-131">El HTML generado agrega el identificador a la parte de la ruta de acceso de la dirección URL:</span><span class="sxs-lookup"><span data-stu-id="71efe-131">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="71efe-132">Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya el entero devolverá un error HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="71efe-132">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="71efe-133">Por ejemplo, `http://localhost:5000/Movies/Details` devolverá un error 404.</span><span class="sxs-lookup"><span data-stu-id="71efe-133">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="71efe-134">Para que el identificador sea opcional, anexe `?` a la restricción de ruta:</span><span class="sxs-lookup"><span data-stu-id="71efe-134">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="71efe-135">Actualización del control de excepciones de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="71efe-135">Update concurrency exception handling</span></span>

<span data-ttu-id="71efe-136">Actualice el método `OnPostAsync` en el archivo *Pages/Movies/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="71efe-136">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="71efe-137">En el código resaltado siguiente se muestran los cambios:</span><span class="sxs-lookup"><span data-stu-id="71efe-137">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="71efe-138">El código anterior solo detecta las excepciones de simultaneidad cuando el primer cliente simultáneo elimina la película y el segundo cliente simultáneo publica cambios en ella.</span><span class="sxs-lookup"><span data-stu-id="71efe-138">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="71efe-139">Para probar el bloque `catch`:</span><span class="sxs-lookup"><span data-stu-id="71efe-139">To test the `catch` block:</span></span>

* <span data-ttu-id="71efe-140">Establecer un punto de interrupción en `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="71efe-140">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="71efe-141">Edite una película.</span><span class="sxs-lookup"><span data-stu-id="71efe-141">Edit a movie.</span></span>
* <span data-ttu-id="71efe-142">En otra ventana del explorador, seleccione el vínculo de **eliminación** de la misma película y luego elimínela.</span><span class="sxs-lookup"><span data-stu-id="71efe-142">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="71efe-143">En la ventana anterior del explorador, publique los cambios en la película.</span><span class="sxs-lookup"><span data-stu-id="71efe-143">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="71efe-144">El código de producción por lo general debería detectar conflictos de simultaneidad cuando dos o más clientes actualizan un registro de forma simultánea.</span><span class="sxs-lookup"><span data-stu-id="71efe-144">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="71efe-145">Vea [Administración de conflictos de simultaneidad](xref:data/ef-mvc/concurrency) para más información.</span><span class="sxs-lookup"><span data-stu-id="71efe-145">See [Handling concurrency conflicts](xref:data/ef-mvc/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="71efe-146">Revisión de publicaciones y enlaces</span><span class="sxs-lookup"><span data-stu-id="71efe-146">Posting and binding review</span></span>

<span data-ttu-id="71efe-147">Examine el archivo *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="71efe-147">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="71efe-148">Cuando se realiza una solicitud HTTP GET a la página Movies/Edit (por ejemplo, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="71efe-148">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="71efe-149">El método `OnGetAsync` obtiene la película en la base de datos y devuelve el método `Page`.</span><span class="sxs-lookup"><span data-stu-id="71efe-149">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="71efe-150">El método `Page` presenta la página de Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="71efe-150">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="71efe-151">El archivo *Pages/Movies/Edit.cshtml* contiene la directiva de modelo (`@model RazorPagesMovie.Pages.Movies.EditModel`), lo que hace que el modelo de película esté disponible en la página.</span><span class="sxs-lookup"><span data-stu-id="71efe-151">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the the movie model available on the page.</span></span>
* <span data-ttu-id="71efe-152">Se abre el formulario de edición con los valores de la película.</span><span class="sxs-lookup"><span data-stu-id="71efe-152">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="71efe-153">Cuando se publica la página Movies/Edit:</span><span class="sxs-lookup"><span data-stu-id="71efe-153">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="71efe-154">Los valores del formulario de la página se enlazan a la propiedad `Movie`.</span><span class="sxs-lookup"><span data-stu-id="71efe-154">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="71efe-155">El atributo `[BindProperty]` habilita el [enlace de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="71efe-155">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="71efe-156">Si hay errores en el estado del modelo (por ejemplo, `ReleaseDate` no se puede convertir en una fecha), el formulario se vuelve a publicar con los valores enviados.</span><span class="sxs-lookup"><span data-stu-id="71efe-156">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="71efe-157">Si no hay ningún error en el modelo, se guarda la película.</span><span class="sxs-lookup"><span data-stu-id="71efe-157">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="71efe-158">Los métodos HTTP GET de las páginas de índice, creación y eliminación de Razor siguen un patrón similar.</span><span class="sxs-lookup"><span data-stu-id="71efe-158">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="71efe-159">El método HTTP POST `OnPostAsync` de la página de creación de Razor sigue un patrón similar al del método `OnPostAsync` de la página de edición de Razor.</span><span class="sxs-lookup"><span data-stu-id="71efe-159">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="71efe-160">La búsqueda se incluye en el tutorial siguiente.</span><span class="sxs-lookup"><span data-stu-id="71efe-160">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="71efe-161">[Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adición de búsqueda](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="71efe-161">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
