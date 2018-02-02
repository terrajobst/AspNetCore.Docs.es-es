# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="be81d-101">Adición de búsqueda a la aplicación páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="be81d-101">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="be81d-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="be81d-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="be81d-103">En este documento se agrega a la página de índice una capacidad de búsqueda que permite buscar películas por *género* o *nombre*.</span><span class="sxs-lookup"><span data-stu-id="be81d-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="be81d-104">Actualice el método `OnGetAsync` de la página de índice con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="be81d-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="be81d-105">La primera línea del método `OnGetAsync` crea una consulta [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) para seleccionar las películas:</span><span class="sxs-lookup"><span data-stu-id="be81d-105">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="be81d-106">En este momento *solo* se define la consulta, **no** se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="be81d-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="be81d-107">Si el parámetro `searchString` contiene una cadena, la consulta de películas se modifica para filtrar según la cadena de búsqueda:</span><span class="sxs-lookup"><span data-stu-id="be81d-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="be81d-108">El código `s => s.Title.Contains()` es una [expresión lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="be81d-108">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="be81d-109">Las lambdas se usan en consultas [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) basadas en métodos como argumentos para métodos de operador de consulta estándar, como el método [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) o `Contains` (usado en el código anterior).</span><span class="sxs-lookup"><span data-stu-id="be81d-109">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="be81d-110">Las consultas LINQ no se ejecutan cuando se definen ni cuando se modifican mediante una llamada a un método (como `Where`, `Contains` u `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="be81d-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="be81d-111">En su lugar, se aplaza la ejecución de la consulta.</span><span class="sxs-lookup"><span data-stu-id="be81d-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="be81d-112">Esto significa que la evaluación de una expresión se aplaza hasta que su valor realizado se repite o se llama al método `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="be81d-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="be81d-113">Para más información, vea [Query Execution (Ejecución de consultas)](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="be81d-113">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="be81d-114">**Nota:** El método [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) se ejecuta en la base de datos, no en el código de C#.</span><span class="sxs-lookup"><span data-stu-id="be81d-114">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="be81d-115">La distinción entre mayúsculas y minúsculas en la consulta depende de la base de datos y la intercalación.</span><span class="sxs-lookup"><span data-stu-id="be81d-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="be81d-116">En SQL Server, `Contains` se asigna a [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), que distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="be81d-116">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="be81d-117">En SQLite, con la intercalación predeterminada, se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="be81d-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="be81d-118">Vaya a la página de películas y anexe una cadena de consulta como `?searchString=Ghost` a la dirección URL (por ejemplo, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="be81d-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="be81d-119">Se muestran las películas filtradas.</span><span class="sxs-lookup"><span data-stu-id="be81d-119">The filtered movies are displayed.</span></span>

![Vista de índice](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="be81d-121">Si se agrega la siguiente plantilla de ruta a la página de índice, la cadena de búsqueda se puede pasar como un segmento de dirección URL (por ejemplo, `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="be81d-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="be81d-122">La restricción de ruta anterior permite buscar el título como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="be81d-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="be81d-123">El elemento `?` de `"{searchString?}"` significa que se trata de un parámetro de ruta opcional.</span><span class="sxs-lookup"><span data-stu-id="be81d-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Vista de índice con la palabra Ghost (Fantasma) agregada a la dirección URL y una lista de películas devuelta con dos películas, Ghostbusters y Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="be81d-125">Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL para buscar una película.</span><span class="sxs-lookup"><span data-stu-id="be81d-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="be81d-126">En este paso, se agrega la interfaz de usuario para filtrar las películas.</span><span class="sxs-lookup"><span data-stu-id="be81d-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="be81d-127">Si ha agregado la restricción de ruta `"{searchString?}"`, quítela.</span><span class="sxs-lookup"><span data-stu-id="be81d-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="be81d-128">Abra el archivo *Pages/Movies/Index.cshtml* y agregue el marcado `<form>` resaltado en el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="be81d-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="be81d-129">La etiqueta HTML `<form>` usa la [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="be81d-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="be81d-130">Cuando se envía el formulario, la cadena de filtro se envía a la página *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="be81d-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="be81d-131">Guarde los cambios y pruebe el filtro.</span><span class="sxs-lookup"><span data-stu-id="be81d-131">Save the changes and test the filter.</span></span>

![Vista de índice con la palabra Ghost (Fantasma) escrita en el cuadro de texto del filtro de título](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="be81d-133">Búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="be81d-133">Search by genre</span></span>

<span data-ttu-id="be81d-134">Agregue las siguientes propiedades resaltadas a *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="be81d-134">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="be81d-135">El elemento `SelectList Genres` contiene la lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="be81d-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="be81d-136">Esto permite al usuario seleccionar un género de la lista.</span><span class="sxs-lookup"><span data-stu-id="be81d-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="be81d-137">La propiedad `MovieGenre` contiene el género concreto que selecciona el usuario (por ejemplo, "Western").</span><span class="sxs-lookup"><span data-stu-id="be81d-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="be81d-138">Actualice el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="be81d-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="be81d-139">El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="be81d-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="be81d-140">La `SelectList` de géneros se crea mediante la proyección de los distintos géneros.</span><span class="sxs-lookup"><span data-stu-id="be81d-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="be81d-141">Adición de búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="be81d-141">Adding search by genre</span></span>

<span data-ttu-id="be81d-142">Actualice *Index.cshtml* como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="be81d-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="be81d-143">Pruebe la aplicación al buscar por género, título de la película y ambos.</span><span class="sxs-lookup"><span data-stu-id="be81d-143">Test the app by searching by genre, by movie title, and by both.</span></span>
