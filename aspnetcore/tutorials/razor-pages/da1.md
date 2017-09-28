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
ms.openlocfilehash: 290d752ea5f177348ff3e749cc125e946ae6e763
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="updating-the-generated-pages"></a>Actualización de las páginas generadas

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación de películas pinta bien, pero la presentación no es ideal. No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Actualización del código generado

Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Haga clic con el botón derecho en una línea ondulada roja > ** Acciones rápidas y refactorizaciones**.

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

Seleccione `using System.ComponentModel.DataAnnotations;`

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.

En el tutorial siguiente se habla sobre [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6). El atributo [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate"). El atributo [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de los datos (Date), así que la información de hora almacenada en el campo no se muestra.

Vaya a Pages/Movies y mantenga el mouse sobre un vínculo de **edición** para ver la dirección URL de destino.

![Ventana del explorador con el mouse sobre el vínculo de edición y aparece una dirección URL de vínculo http://localhost:1234/Movies/Edit/5](da1/edit7.png)

Los vínculos **Edit**, **Details** y **Delete** son generados por la [aplicación auxiliar de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Movies/Index.cshtml*.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) permiten al código de servidor participar en la creación y representación de elementos HTML en archivos de Razor. En el código anterior, `AnchorTagHelper` genera de forma dinámica el valor del atributo `href` HTML desde la página de Razor (la ruta es relativa), el elemento `asp-page` y el identificador de ruta (`asp-route-id`). Vea [Generación de direcciones URL para las páginas](xref:mvc/razor-pages/index#url-generation-for-pages) para obtener más información.

Use **Ver código fuente** en su explorador preferido para examinar el marcado generado. A continuación se muestra una parte del HTML generado:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Los vínculos generados de forma dinámica pasan el identificador de la película con una cadena de consulta (por ejemplo, `http://localhost:5000/Movies/Details?id=2`). 

Actualice las páginas de edición, detalles y eliminación de Razor para usar la plantilla de ruta "{id:int}". Cambie la directiva de página de cada una de estas páginas a `@page "{id:int}"`. Ejecute la aplicación y luego vea el origen. El HTML generado agrega el identificador a la parte de la ruta de acceso de la dirección URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya el entero devolverá un error HTTP 404 (no encontrado). Por ejemplo, `http://localhost:5000/Movies/Details` devolverá un error 404. Para que el identificador sea opcional, anexe `?` a la restricción de ruta:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Actualización del control de excepciones de simultaneidad

Actualice el método `OnPostAsync` en el archivo *Pages/Movies/Edit.cshtml.cs*. En el código resaltado siguiente se muestran los cambios:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

El código anterior solo detecta las excepciones de simultaneidad cuando el primer cliente simultáneo elimina la película y el segundo cliente simultáneo publica cambios en ella.

Para probar el bloque `catch`:

* Establecer un punto de interrupción en `catch (DbUpdateConcurrencyException)`
* Edite una película.
* En otra ventana del explorador, seleccione el vínculo de **eliminación** de la misma película y luego elimínela.
* En la ventana anterior del explorador, publique los cambios en la película.

El código de producción por lo general debería detectar conflictos de simultaneidad cuando dos o más clientes actualizan un registro de forma simultánea. Vea [Administración de conflictos de simultaneidad](xref:data/ef-mvc/concurrency) para más información.

### <a name="posting-and-binding-review"></a>Revisión de publicaciones y enlaces

Examine el archivo *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

Cuando se realiza una solicitud HTTP GET a la página Movies/Edit (por ejemplo, `http://localhost:5000/Movies/Edit/2`):

* El método `OnGetAsync` obtiene la película en la base de datos y devuelve el método `Page`. 
* El método `Page` presenta la página de Razor *Pages/Movies/Edit.cshtml*. El archivo *Pages/Movies/Edit.cshtml* contiene la directiva de modelo (`@model RazorPagesMovie.Pages.Movies.EditModel`), lo que hace que el modelo de película esté disponible en la página.
* Se abre el formulario de edición con los valores de la película.

Cuando se publica la página Movies/Edit:

* Los valores del formulario de la página se enlazan a la propiedad `Movie`. El atributo `[BindProperty]` habilita el [enlace de modelos](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Si hay errores en el estado del modelo (por ejemplo, `ReleaseDate` no se puede convertir en una fecha), el formulario se vuelve a publicar con los valores enviados.
* Si no hay ningún error en el modelo, se guarda la película.

Los métodos HTTP GET de las páginas de índice, creación y eliminación de Razor siguen un patrón similar. El método HTTP POST `OnPostAsync` de la página de creación de Razor sigue un patrón similar al del método `OnPostAsync` de la página de edición de Razor.

La búsqueda se incluye en el tutorial siguiente.

>[!div class="step-by-step"]
[Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adición de búsqueda](xref:tutorials/razor-pages/search)
