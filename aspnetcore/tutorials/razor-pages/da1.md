---
title: Actualizar las páginas generadas en una aplicación ASP.NET Core
author: rick-anderson
description: Sepa cómo actualizar las páginas generadas en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 396cb9b9eeaab2d3db6108feeba71dbc2bc8981d
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997206"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="c18d2-103">Actualizar las páginas generadas en una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c18d2-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="c18d2-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c18d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c18d2-105">La aplicación de películas con scaffolding pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="c18d2-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="c18d2-106">**FechaDeLanzamiento** debe ser **Fecha de lanzamiento** (tres palabras).</span><span class="sxs-lookup"><span data-stu-id="c18d2-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplicación Movie abierta en Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="c18d2-108">Actualización del código generado</span><span class="sxs-lookup"><span data-stu-id="c18d2-108">Update the generated code</span></span>

<span data-ttu-id="c18d2-109">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c18d2-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

<span data-ttu-id="c18d2-110">La anotación de datos `[Column(TypeName = "decimal(18, 2)")]` permite que Entity Framework Core asigne correctamente `Price` a la moneda en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c18d2-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="c18d2-111">Para más información, vea [Tipos de datos](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="c18d2-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="c18d2-112">En el próximo tutorial, hablaremos de [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6).</span><span class="sxs-lookup"><span data-stu-id="c18d2-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="c18d2-113">El atributo [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="c18d2-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="c18d2-114">El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de los datos (Date), así que la información de hora almacenada en el campo no se muestra.</span><span class="sxs-lookup"><span data-stu-id="c18d2-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="c18d2-115">Vaya a Pages/Movies y mantenga el mouse sobre un vínculo de **edición** para ver la dirección URL de destino.</span><span class="sxs-lookup"><span data-stu-id="c18d2-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Ventana del explorador con el mouse sobre el vínculo Edit (Editar) donde se muestra una dirección URL de vínculo http://localhost:1234/Movies/Edit/5](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="c18d2-117">Los vínculos **Edit**, **Details** y **Delete** son generados por el [asistente de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c18d2-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="c18d2-118">Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="c18d2-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="c18d2-119">En el código anterior, `AnchorTagHelper` genera de forma dinámica el valor del atributo `href` HTML desde la página de Razor (la ruta es relativa), el elemento `asp-page` y el identificador de ruta (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="c18d2-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="c18d2-120">Vea [Generación de direcciones URL para las páginas](xref:razor-pages/index#url-generation-for-pages) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c18d2-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="c18d2-121">Use **Ver código fuente** en su explorador preferido para examinar el marcado generado.</span><span class="sxs-lookup"><span data-stu-id="c18d2-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="c18d2-122">A continuación se muestra una parte del HTML generado:</span><span class="sxs-lookup"><span data-stu-id="c18d2-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="c18d2-123">Los vínculos generados de forma dinámica pasan el identificador de la película con una cadena de consulta (por ejemplo, el `?id=1` de `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="c18d2-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="c18d2-124">Actualice las páginas de edición, detalles y eliminación de Razor para usar la plantilla de ruta "{id:int}".</span><span class="sxs-lookup"><span data-stu-id="c18d2-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="c18d2-125">Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="c18d2-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="c18d2-126">Ejecute la aplicación y luego vea el origen.</span><span class="sxs-lookup"><span data-stu-id="c18d2-126">Run the app and then view source.</span></span> <span data-ttu-id="c18d2-127">El HTML generado agrega el identificador a la parte de la ruta de acceso de la dirección URL:</span><span class="sxs-lookup"><span data-stu-id="c18d2-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="c18d2-128">Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya el entero devolverá un error HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="c18d2-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="c18d2-129">Por ejemplo, `http://localhost:5000/Movies/Details` devolverá un error 404.</span><span class="sxs-lookup"><span data-stu-id="c18d2-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="c18d2-130">Para que el identificador sea opcional, anexe `?` a la restricción de ruta:</span><span class="sxs-lookup"><span data-stu-id="c18d2-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="c18d2-131">Para probar el comportamiento de `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="c18d2-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="c18d2-132">Establezca la directiva de página de *Pages/Movies/Details.cshtml* en `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="c18d2-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="c18d2-133">Establezca un punto de interrupción en `public async Task<IActionResult> OnGetAsync(int? id)` (en *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="c18d2-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="c18d2-134">Navegue a `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="c18d2-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="c18d2-135">Con la directiva `@page "{id:int}"`, el punto de interrupción nunca se alcanza.</span><span class="sxs-lookup"><span data-stu-id="c18d2-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="c18d2-136">El motor de enrutamiento devuelve HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="c18d2-136">The routing engine return HTTP 404.</span></span> <span data-ttu-id="c18d2-137">Con `@page "{id:int?}"`, el método `OnGetAsync` devuelve `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="c18d2-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

<span data-ttu-id="c18d2-138">Aunque no se recomienda, puede escribir el método `OnGetAsync` (en *Pages/Movies/Delete.cshtml.cs*) como:</span><span class="sxs-lookup"><span data-stu-id="c18d2-138">Although not recommended, you could write the `OnGetAsync` method (in *Pages/Movies/Delete.cshtml.cs*) as:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

<span data-ttu-id="c18d2-139">Pruebe el código anterior:</span><span class="sxs-lookup"><span data-stu-id="c18d2-139">Test the preceding code:</span></span>

* <span data-ttu-id="c18d2-140">Seleccione un vínculo **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="c18d2-140">Select a **Delete** link.</span></span>
* <span data-ttu-id="c18d2-141">Quite el identificador de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="c18d2-141">Remove the ID from the URL.</span></span> <span data-ttu-id="c18d2-142">Por ejemplo, cambie `https://localhost:5001/Movies/Delete/8` a `https://localhost:5001/Movies/Delete`.</span><span class="sxs-lookup"><span data-stu-id="c18d2-142">For example, change `https://localhost:5001/Movies/Delete/8` to `https://localhost:5001/Movies/Delete`.</span></span>
* <span data-ttu-id="c18d2-143">Ejecute paso a paso el código del depurador.</span><span class="sxs-lookup"><span data-stu-id="c18d2-143">Step through the code in the debugger.</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="c18d2-144">Revisión del control de excepciones de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="c18d2-144">Review concurrency exception handling</span></span>

<span data-ttu-id="c18d2-145">Revise el método `OnPostAsync` en el archivo *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c18d2-145">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="c18d2-146">El código anterior detecta las excepciones de simultaneidad cuando el cliente uno elimina la película y el otro cliente publica cambios en ella.</span><span class="sxs-lookup"><span data-stu-id="c18d2-146">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="c18d2-147">Para probar el bloque `catch`:</span><span class="sxs-lookup"><span data-stu-id="c18d2-147">To test the `catch` block:</span></span>

* <span data-ttu-id="c18d2-148">Establecer un punto de interrupción en `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="c18d2-148">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="c18d2-149">Seleccione **Editar** para una película y realice cambios, pero no seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="c18d2-149">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="c18d2-150">En otra ventana del explorador, seleccione el vínculo de **eliminación** de la misma película y luego elimínela.</span><span class="sxs-lookup"><span data-stu-id="c18d2-150">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="c18d2-151">En la ventana anterior del explorador, publique los cambios en la película.</span><span class="sxs-lookup"><span data-stu-id="c18d2-151">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="c18d2-152">Es posible que el código de producción quiera detectar conflictos de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="c18d2-152">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="c18d2-153">Vea [Administración de conflictos de simultaneidad](xref:data/ef-rp/concurrency) para más información.</span><span class="sxs-lookup"><span data-stu-id="c18d2-153">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="c18d2-154">Revisión de publicaciones y enlaces</span><span class="sxs-lookup"><span data-stu-id="c18d2-154">Posting and binding review</span></span>

<span data-ttu-id="c18d2-155">Examine el archivo *Pages/Movies/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c18d2-155">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="c18d2-156">Cuando se realiza una solicitud HTTP GET a la página Movies/Edit (por ejemplo, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="c18d2-156">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="c18d2-157">El método `OnGetAsync` obtiene la película en la base de datos y devuelve el método `Page`.</span><span class="sxs-lookup"><span data-stu-id="c18d2-157">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="c18d2-158">El método `Page` presenta la página de Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c18d2-158">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="c18d2-159">El archivo *Pages/Movies/Edit.cshtml* contiene la directiva de modelo (`@model RazorPagesMovie.Pages.Movies.EditModel`), que hace que el modelo de película esté disponible en la página.</span><span class="sxs-lookup"><span data-stu-id="c18d2-159">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="c18d2-160">Se abre el formulario de edición con los valores de la película.</span><span class="sxs-lookup"><span data-stu-id="c18d2-160">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="c18d2-161">Cuando se publica la página Movies/Edit:</span><span class="sxs-lookup"><span data-stu-id="c18d2-161">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="c18d2-162">Los valores del formulario de la página se enlazan a la propiedad `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c18d2-162">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="c18d2-163">El atributo `[BindProperty]` habilita el [enlace de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="c18d2-163">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="c18d2-164">Si hay errores en el estado del modelo (por ejemplo, `ReleaseDate` no se puede convertir en una fecha), el formulario se muestra con los valores enviados.</span><span class="sxs-lookup"><span data-stu-id="c18d2-164">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="c18d2-165">Si no hay ningún error en el modelo, se guarda la película.</span><span class="sxs-lookup"><span data-stu-id="c18d2-165">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="c18d2-166">Los métodos HTTP GET de las páginas de índice, creación y eliminación de Razor siguen un patrón similar.</span><span class="sxs-lookup"><span data-stu-id="c18d2-166">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="c18d2-167">El método HTTP POST `OnPostAsync` de la página de creación de Razor sigue un patrón similar al del método `OnPostAsync` de la página de edición de Razor.</span><span class="sxs-lookup"><span data-stu-id="c18d2-167">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="c18d2-168">La búsqueda se incluye en el tutorial siguiente.</span><span class="sxs-lookup"><span data-stu-id="c18d2-168">Search is added in the next tutorial.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c18d2-169">[Anterior: Trabajo con una base de datos](xref:tutorials/razor-pages/sql)
> [Siguiente: Adición de búsqueda](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="c18d2-169">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
