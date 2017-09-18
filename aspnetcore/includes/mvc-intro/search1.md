# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Adición de una búsqueda en una aplicación de ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En esta sección agregará capacidad de búsqueda para el método de acción `Index` que permite buscar películas por *género* o *nombre*.

Actualice el método `Index` con el código siguiente:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

La primera línea del método de acción `Index` crea una consulta [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) para seleccionar las películas:

```csharp
var movies = from m in _context.Movie
             select m;
```

En este momento *solo* se define la consulta, **no** se ejecuta en la base de datos.

Si el parámetro `searchString` contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

El código `s => s.Title.Contains()` anterior es una [expresión Lambda](http://msdn.microsoft.com/library/bb397687.aspx). Las lambdas se usan en consultas [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) basadas en métodos como argumentos para métodos de operador de consulta estándar, tales como el método [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) o `Contains` (usado en el código anterior). Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican mediante una llamada a un método como `Where`, `Contains` o `OrderBy`. En su lugar, se aplaza la ejecución de la consulta.  Esto significa que la evaluación de una expresión se aplaza hasta que su valor realizado se repita realmente o se llame al método `ToListAsync`. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](http://msdn.microsoft.com/library/bb738633.aspx).

Nota: El método [Contains](http://msdn.microsoft.com/library/bb155125.aspx) se ejecuta en la base de datos, no en el código de c# que se muestra arriba. La distinción entre mayúsculas y minúsculas en la consulta depende de la base de datos y la intercalación. En SQL Server, [Contains](http://msdn.microsoft.com/library/bb155125.aspx) se asigna a [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx), que distingue entre mayúsculas y minúsculas. En SQLite, con la intercalación predeterminada, se distingue entre mayúsculas y minúsculas.

Navegue a `/Movies/Index`. Anexe una cadena de consulta como `?searchString=Ghost` a la dirección URL. Se muestran las películas filtradas.

![Vista de índice](../../tutorials/first-mvc-app/search/_static/ghost.png)

Si se cambia la firma del método `Index` para que tenga un parámetro con el nombre `id`, el parámetro `id` coincidirá con el marcador `{id}` opcional para el conjunto de rutas predeterminado en *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]