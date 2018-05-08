El elemento `<form method="post">` es una [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper). La aplicación auxiliar de etiquetas de formulario incluye automáticamente un [token antifalsificación](xref:security/anti-request-forgery).

El motor de scaffolding crea un marcado de Razor para cada campo del modelo (excepto el identificador) similar al siguiente:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

Las [aplicaciones auxiliares de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` y ` <span asp-validation-for`) muestran errores de validación. La validación se trata con más detalle en un punto posterior de esta serie.

La [aplicación auxiliar de etiquetas](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera el título de la etiqueta y el atributo `for` para la propiedad `Title`.

La [aplicación auxiliar de etiquetas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado del cliente.