<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="27a8d-101">Ahora, cuando se envía una búsqueda, la URL contiene la cadena de consulta de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="27a8d-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="27a8d-102">La búsqueda también será dirigida al método de acción `HttpGet Index`, aunque tenga un método `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="27a8d-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Ventana del explorador que muestra searchString=ghost en la URL y las películas que se devuelven, Ghostbusters y Ghostbusters 2, que contienen la palabra Ghost (Fantasma)](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="27a8d-104">El marcado siguiente muestra el cambio en la etiqueta `form`:</span><span class="sxs-lookup"><span data-stu-id="27a8d-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="27a8d-105">Agregar búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="27a8d-105">Adding Search by genre</span></span>

<span data-ttu-id="27a8d-106">Agregue la clase `MovieGenreViewModel` siguiente a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="27a8d-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="27a8d-107">El modelo de vista de película y género contendrá:</span><span class="sxs-lookup"><span data-stu-id="27a8d-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="27a8d-108">Una lista de películas.</span><span class="sxs-lookup"><span data-stu-id="27a8d-108">A list of movies.</span></span>
   * <span data-ttu-id="27a8d-109">`SelectList`, que contiene la lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="27a8d-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="27a8d-110">Esto permitirá al usuario seleccionar un género de la lista.</span><span class="sxs-lookup"><span data-stu-id="27a8d-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="27a8d-111">`movieGenre`, que contiene el género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="27a8d-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="27a8d-112">Reemplace el método `Index` en `MoviesController.cs` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="27a8d-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="27a8d-113">El código siguiente es una consulta `LINQ` que recupera todos los géneros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="27a8d-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="27a8d-114">La `SelectList` de géneros se crea mediante la proyección de los distintos géneros (no queremos que nuestra lista de selección tenga géneros duplicados).</span><span class="sxs-lookup"><span data-stu-id="27a8d-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="27a8d-115">Agregar búsqueda por género a la vista de índice</span><span class="sxs-lookup"><span data-stu-id="27a8d-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="27a8d-116">Actualice `Index.cshtml` de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="27a8d-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="27a8d-117">Examine la expresión lambda usada en la siguiente aplicación auxiliar HTML:</span><span class="sxs-lookup"><span data-stu-id="27a8d-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="27a8d-118">En el código anterior, la aplicación auxiliar HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar.</span><span class="sxs-lookup"><span data-stu-id="27a8d-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="27a8d-119">Puesto que la expresión lambda se inspecciona en lugar de evaluarse, no recibirá una infracción de acceso cuando `model`, `model.movies` o `model.movies[0]` sean `null` o estén vacíos.</span><span class="sxs-lookup"><span data-stu-id="27a8d-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="27a8d-120">Cuando se evalúa la expresión lambda (por ejemplo, `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="27a8d-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="27a8d-121">Pruebe la aplicación haciendo búsquedas por género, título de la película y ambos.</span><span class="sxs-lookup"><span data-stu-id="27a8d-121">Test the app by searching by genre, by movie title, and by both.</span></span>
