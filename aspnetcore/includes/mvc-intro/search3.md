<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Ahora, cuando se envía una búsqueda, la URL contiene la cadena de consulta de búsqueda. La búsqueda también será dirigida al método de acción `HttpGet Index`, aunque tenga un método `HttpPost Index`.

![Ventana del explorador que muestra searchString=ghost en la URL y las películas que se devuelven, Ghostbusters y Ghostbusters 2, que contienen la palabra Ghost (Fantasma)](~/tutorials/first-mvc-app/search/_static/search_get.png)

El marcado siguiente muestra el cambio en la etiqueta `form`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Agregar búsqueda por género

Agregue la clase `MovieGenreViewModel` siguiente a la carpeta *Models*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

El modelo de vista de película y género contendrá:

   * Una lista de películas.
   * `SelectList`, que contiene la lista de géneros. Esto permite al usuario seleccionar un género de la lista.
   * `MovieGenre`, que contiene el género seleccionado.
   * `SearchString`, que contiene el texto que los usuarios escriben en el cuadro de texto de búsqueda.

Reemplace el método `Index` en `MoviesController.cs` por el código siguiente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

El código siguiente es una consulta `LINQ` que recupera todos los géneros de la base de datos.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

La `SelectList` de géneros se crea mediante la proyección de los distintos géneros (no queremos que nuestra lista de selección tenga géneros duplicados).

Cuando el usuario busca el elemento, se conserva el valor de búsqueda en el cuadro de búsqueda. Para conservar el valor de búsqueda, rellene la propiedad `SearchString` con el valor de búsqueda. El valor de búsqueda es el parámetro `searchString` para la acción del controlador `Index`.

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a>Agregar búsqueda por género a la vista de índice

Actualice `Index.cshtml` de la siguiente manera:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Examine la expresión lambda usada en el siguiente asistente de HTML:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
En el código anterior, el asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar. Puesto que la expresión lambda se inspecciona en lugar de evaluarse, no recibirá una infracción de acceso cuando `model`, `model.Movies` o `model.Movies[0]` sean `null` o estén vacíos. Cuando se evalúa la expresión lambda (por ejemplo, `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.

Pruebe la aplicación haciendo búsquedas por género, título de la película y ambos.
