En el código resaltado anterior se muestra cómo se agrega el contexto de base de datos de películas al contenedor [inserción de dependencias](xref:fundamentals/dependency-injection). La línea siguiente a `services.AddDbContext<MvcMovieContext>(options =>` no se muestra (vea el código). Especifica la base de datos que se usará y la cadena de conexión. `=>` es un [operador lambda](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator).

Abra el archivo *Controllers/MoviesController.cs* y examine el constructor:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`MvcMovieContext `) en el controlador. El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.

<a name=strongly-typed-models-keyword-label></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fuertemente tipados y la palabra clave @model

Anteriormente en este tutorial, vimos cómo un controlador puede pasar datos u objetos a una vista mediante el diccionario `ViewData`. El diccionario `ViewData` es un objeto dinámico que proporciona una cómoda manera enlazada en tiempo de ejecución de pasar información a una vista.

MVC también ofrece la capacidad de pasar objetos de modelo fuertemente tipados a una vista. Este enfoque fuertemente tipado permite una mejor comprobación del código en tiempo de compilación. El mecanismo de scaffolding usó este enfoque (que consiste en pasar un modelo fuertemente tipado) con la clase `MoviesController` y las vistas cuando creó los métodos y las vistas.

Examine el método `Details` generado en el archivo *Controllers/MoviesController.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

El parámetro `id` suele pasarse como datos de ruta. Por ejemplo, `http://localhost:5000/movies/details/1` establece:

* El controlador en el controlador `movies` (el primer segmento de dirección URL).
* La acción en `details` (el segundo segmento de dirección URL).
* El identificador en 1 (el último segmento de dirección URL).

También puede pasar `id` con una cadena de consulta como se indica a continuación:

`http://localhost:1234/movies/details?id=1`

El parámetro `id` se define como un [tipo que acepta valores NULL](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) en caso de que no se proporcione un valor de identificador.

Se pasa una [expresión lambda](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) a `SingleOrDefaultAsync` para seleccionar entidades de película que coincidan con los datos de enrutamiento o el valor de consulta de cadena.

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

Si se encuentra una película, se pasa una instancia del modelo `Movie` a la vista `Details`:

```csharp
return View(movie);
   ```

Examine el contenido del archivo *Views/Movies/Details.cshtml*:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

Mediante la inclusión de una instrucción `@model` en la parte superior del archivo de vista, puede especificar el tipo de objeto que espera la vista. Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:

```HTML
@model MvcMovie.Models.Movie
   ```

Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado. Por ejemplo, en la vista *Details.cshtml*, el código pasa cada campo de película a las aplicaciones auxiliares HTML `DisplayNameFor` y `DisplayFor` con el objeto `Model` fuertemente tipado. Los métodos `Create` y `Edit` y las vistas también pasan un objeto de modelo `Movie`.

Examine la vista *Index.cshtml* y el método `Index` en el controlador Movies. Observe cómo el código crea un objeto `List` cuando llama al método `View`. El código pasa esta lista `Movies` desde el método de acción `Index` a la vista:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

Cuando se creó el controlador movies, el scaffolding incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Index.cshtml*:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

Esta directiva `@model` permite acceder a la lista de películas que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado. Por ejemplo, en la vista *Index.cshtml*, el código recorre en bucle las películas con una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Como el objeto `Model` es fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada elemento del bucle está tipado como `Movie`. Entre otras ventajas, esto significa que se obtiene una comprobación del código en tiempo de compilación:
