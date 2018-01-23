---
title: "Páginas de Razor con scaffolding en ASP.NET Core"
author: rick-anderson
description: "Explica las páginas de Razor generadas por la técnica scaffolding."
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad2a2b48beb31dddcfd78a8aab79ac58ccda28f3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Páginas de Razor con scaffolding en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el tema de tutorial anterior [Adición de un modelo](xref:tutorials/razor-pages/model#scaffold-the-movie-model). 

[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) un ejemplo.

## <a name="the-create-delete-details-and-edit-pages"></a>Páginas Crear, Eliminar, Detalles y Editar.

Examine el modelo de página *Pages/Movies/Index.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Las páginas de Razor se derivan de `PageModel`. Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`. El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `MovieContext` a la página. Todas las páginas con scaffolding siguen este patrón. Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.

Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor. Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página. En este caso, `OnGetAsync` obtiene una lista de películas y las muestra. 

Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución. Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return. Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:

<!-- TODO - replace with snippet
[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
Examine la página de Razor *Pages/Movies/Index.cshtml*:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor. Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.

La directiva de Razor `@page` convierte el archivo en una acción &mdash; de MVC, lo que significa que puede controlar las solicitudes. `@page` debe ser la primera directiva de Razor de una página. `@page` es un ejemplo de la transición a un marcado específico de Razor. Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.

Examine la expresión lambda usada en la siguiente aplicación auxiliar HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

La aplicación auxiliar HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar. La expresión lambda se inspecciona, no se evalúa. Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos. Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.

<a name="md"></a>
### <a name="the-model-directive"></a>La directiva @model 

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor. En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor. El modelo se usa en las [aplicaciones auxiliares HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` y `@Html.DisplayName` de la página.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### Propiedades ViewData y Layout

Observe el código siguiente:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

El código resaltado anterior es un ejemplo de Razor con una transición a C#. Los caracteres `{` y `}` delimitan un bloque de código de C#.

La clase base `PageModel` tiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos que quiera pasar a una vista. Puede agregar objetos al diccionario `ViewData` con un patrón de clave/valor. En el ejemplo anterior, se agrega la propiedad "Title" al diccionario `ViewData`. La propiedad "Title" se usa en el archivo *Pages/_Layout.cshtml*. En el siguiente marcado se muestran las primeras líneas del archivo *Pages/_Layout.cshtml*.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

La línea `@*Markup removed for brevity.*@` es un comentario de Razor. A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.

Ejecute la aplicación y pruebe los vínculos del proyecto (**Inicio**, **Acerca de**, **Contacto**, **Crear**, **Editar** y **Eliminar**). Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador. *Pages/Index.cshtml* y *Pages/Movies/Index.cshtml* actualmente tienen el mismo título, pero puede modificarlos para que tengan valores diferentes.

La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

El marcado anterior establece el archivo de diseño en *Pages/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*. Vea [Layout](xref:mvc/razor-pages/index#layout) (Diseño) para más información.

### <a name="update-the-layout"></a>Actualizar el diseño

Cambie el elemento `<title>` del archivo *Pages/_Layout.cshtml* para usar una cadena más corta.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Busque el siguiente elemento delimitador en el archivo *Pages/_Layout.cshtml*.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Reemplace el elemento anterior por el marcado siguiente.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

El elemento delimitador anterior es una [aplicación auxiliar de etiquetas](xref:mvc/views/tag-helpers/intro). En este caso, se trata de la [aplicación auxiliar de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). El atributo y valor de la aplicación auxiliar de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`.

Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**. Consulte el archivo [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) en GitHub.

### <a name="the-create-code-behind-page"></a>Página de código subyacente Crear

Examine el archivo de código subyacente *Pages/Movies/Create.cshtml.cs*:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

El método `OnGet` inicializa cualquier estado necesario para la página. La página Crear no tiene ningún estado para inicializar. El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.

La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding). Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.

El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados. La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario. Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha. Más adelante en el tutorial volveremos a hablar de la validación del lado cliente y de la validación de modelos.

Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.

### <a name="the-create-razor-page"></a>Página de Razor Crear

Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

Visual Studio muestra la etiqueta `<form method="post">` con una fuente diferenciada que se aplica a las aplicaciones auxiliares de etiquetas. El elemento `<form method="post">` es una [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper). La aplicación auxiliar de etiquetas de formulario incluye automáticamente un [token antifalsificación](xref:security/anti-request-forgery).

![Vista de VS17 de la página Create.cshtml](page/_static/th.png)

El motor de scaffolding crea un marcado de Razor para cada campo del modelo (excepto el identificador) similar al siguiente:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

Las [aplicaciones auxiliares de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` y ` <span asp-validation-for`) muestran errores de validación. La validación se trata con más detalle en un punto posterior de esta serie.

La [aplicación auxiliar de etiquetas](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera el título de la etiqueta y el atributo `for` para la propiedad `Title`.

La [aplicación auxiliar de etiquetas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa los atributos [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado del cliente.

En el siguiente tutorial se describe SQL Server LocalDB y la propagación de la base de datos.

>[!div class="step-by-step"]
[Anterior: Adición de un modelo](xref:tutorials/razor-pages/model)
[Siguiente: SQL Server LocalDB](xref:tutorials/razor-pages/sql)
