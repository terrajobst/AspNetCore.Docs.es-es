<!-- This include not used by windows version -->
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Agregar un campo nuevo a una aplicación de ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se agregará un nuevo campo a la tabla `Movies`. Quitaremos la base de datos y crearemos una nueva cuando cambiemos el esquema (agregar un nuevo campo). Este flujo de trabajo funciona bien al principio de desarrollo si no tenemos que conservar datos de producción.

Una vez que se haya implementado la aplicación y se tengan datos que se quieran conservar, no se podrá desconectar la base de datos cuando sea necesario cambiar el esquema. [Migraciones de Entity Framework Code First](/ef/core/get-started/aspnetcore/new-db) permite actualizar el esquema y migrar la base de datos sin perder datos. Migraciones es una característica muy usada con SQL Server, pero SQLite no admite muchas operaciones de esquema de migración, por lo que solo se pueden realizar migraciones muy sencillas. Para más información, vea [SQLite EF Core Database Provider Limitations](/ef/core/providers/sqlite/limitations) (Limitaciones del proveedor de base de datos de SQLite EF Core).

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la lista blanca de direcciones de enlace para que incluya esta nueva propiedad. En *MoviesController.cs*, actualice el atributo `[Bind]` para que los métodos de acción `Create` y `Edit` incluyan la propiedad `Rating`:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

También necesita actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.

Edite el archivo */Views/Movies/Index.cshtml* y agregue un campo `Rating`:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Actualice */Views/Movies/Create.cshtml* con un campo `Rating`.

La aplicación no funcionará hasta que la base de datos se actualice para incluir el nuevo campo. Si la ejecuta ahora, se producirá la siguiente `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Este error se muestra porque la clase del modelo Movie actualizada es diferente del esquema de la tabla Movie de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).

Este error se puede resolver de varias maneras:

1. Desconecte la base de datos y haga que Entity Framework vuelva a crear automáticamente la base de datos según el nuevo esquema de clase de modelo. Con este enfoque se pierden los datos que tenga en la base de datos, así que no puede hacer esto con una base de datos de producción. A menudo, una forma productiva de desarrollar una aplicación consiste en usar un inicializador para propagar una base de datos con datos de prueba.

2. Modifique manualmente el esquema de la base de datos existente para que coincida con las clases de modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

3. Use Migraciones de Code First para actualizar el esquema de la base de datos.

Para este tutorial, se desconectará la base de datos y se volverá a crear cuando cambie el esquema. Para desconectar la base de datos, ejecute este comando desde un terminal:

`dotnet ef database drop`

Actualice la clase `SeedData` para que proporcione un valor para la nueva columna. A continuación se muestra un cambio de ejemplo, aunque es conveniente realizarlo con cada `new Movie`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Agregue el campo `Rating` a la vista `Edit`, `Details` y `Delete`.

Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`. plantillas.
