# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Páginas de Razor con scaffolding en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el tutorial anterior. 

[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) un ejemplo.

## <a name="the-create-delete-details-and-edit-pages"></a>Páginas Crear, Eliminar, Detalles y Editar.

Examine el modelo de página *Pages/Movies/Index.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Las páginas de Razor se derivan de `PageModel`. Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`. El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `MovieContext` a la página. Todas las páginas con scaffolding siguen este patrón. Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.

Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor. Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página. En este caso, `OnGetAsync` obtiene una lista de películas y las muestra.

Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución. Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return. Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> Examine la página de Razor *Pages/Movies/Index.cshtml*:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor. Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.

La directiva de Razor `@page` convierte el archivo en una acción &mdash; de MVC, lo que significa que puede controlar las solicitudes. `@page` debe ser la primera directiva de Razor de una página. `@page` es un ejemplo de la transición a un marcado específico de Razor. Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.

Examine la expresión lambda usada en el siguiente asistente de HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

El asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar. La expresión lambda se inspecciona, no se evalúa. Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos. Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.

<a name="md"></a>
### <a name="the-model-directive"></a>La directiva @model 

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor. En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor. El modelo se usa en los [asistentes de HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)`@Html.DisplayNameFor` y `@Html.DisplayFor` de la página.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### Propiedades ViewData y Layout

Observe el código siguiente:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

El código resaltado anterior es un ejemplo de Razor con una transición a C#. Los caracteres `{` y `}` delimitan un bloque de código de C#.

La clase base `PageModel` tiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos que quiera pasar a una vista. Puede agregar objetos al diccionario `ViewData` con un patrón de clave/valor. En el ejemplo anterior, se agrega la propiedad "Title" al diccionario `ViewData`. 

::: moniker range="= aspnetcore-2.0"

La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*. En el siguiente marcado se muestran las primeras líneas del archivo *Pages/Shared/_Layout.cshtml*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*. En el siguiente marcado se muestran las primeras líneas del archivo *_Layout.cshtml*.

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

La línea `@*Markup removed for brevity.*@` es un comentario de Razor. A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.

Ejecute la aplicación y pruebe los vínculos del proyecto (**Inicio**, **Acerca de**, **Contacto**, **Crear**, **Editar** y **Eliminar**). Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador. *Pages/Index.cshtml* y *Pages/Movies/Index.cshtml* actualmente tienen el mismo título, pero puede modificarlos para que tengan valores diferentes.

> [!NOTE]
> Es posible que no pueda escribir comas decimales en el campo `Price`. Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación. Consulte el [problema 4076 de GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obtener instrucciones sobre cómo agregar la coma decimal.

La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

El marcado anterior establece el archivo de diseño en *Pages/Shared/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*. Vea [Layout](xref:razor-pages/index#layout) (Diseño) para más información.

### <a name="update-the-layout"></a>Actualizar el diseño

Cambie el elemento `<title>` del archivo *Pages/Shared/_Layout.cshtml* para usar una cadena más corta.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Busque el siguiente elemento delimitador en el archivo *Pages/Shared/_Layout.cshtml*.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Reemplace el elemento anterior por el marcado siguiente.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

El elemento delimitador anterior es un [asistente de etiquetas](xref:mvc/views/tag-helpers/intro). En este caso, se trata de el [asistente de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). El atributo y valor del asistente de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`.

Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**. Consulte el archivo [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) en GitHub.

### <a name="the-create-page-model"></a>Modelo de página Crear

Examine el modelo de página *Pages/Movies/Create.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end


El método `OnGet` inicializa cualquier estado necesario para la página. La página Crear no tiene ningún estado que inicializar, de modo que se devuelve `Page`. Más adelante en el tutorial veremos el estado de inicialización del método `OnGet`. El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.

La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding). Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.

El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados. La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario. Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha. Más adelante en el tutorial volveremos a hablar de la validación del lado cliente y de la validación de modelos.

Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.

### <a name="the-create-razor-page"></a>Página de Razor Crear

Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
