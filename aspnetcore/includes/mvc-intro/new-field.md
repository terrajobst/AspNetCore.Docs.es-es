<!-- This include not used by windows version -->
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="42f91-101">Agregar un campo nuevo a una aplicación de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="42f91-101">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="42f91-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42f91-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42f91-103">En este tutorial se agregará un nuevo campo a la tabla `Movies`.</span><span class="sxs-lookup"><span data-stu-id="42f91-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="42f91-104">Quitaremos la base de datos y crearemos una nueva cuando cambiemos el esquema (agregar un nuevo campo).</span><span class="sxs-lookup"><span data-stu-id="42f91-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="42f91-105">Este flujo de trabajo funciona bien al principio de desarrollo si no tenemos que conservar datos de producción.</span><span class="sxs-lookup"><span data-stu-id="42f91-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="42f91-106">Una vez que se haya implementado la aplicación y se tengan datos que se quieran conservar, no se podrá desconectar la base de datos cuando sea necesario cambiar el esquema.</span><span class="sxs-lookup"><span data-stu-id="42f91-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="42f91-107">[Migraciones de Entity Framework Code First](/ef/core/get-started/aspnetcore/new-db) permite actualizar el esquema y migrar la base de datos sin perder datos.</span><span class="sxs-lookup"><span data-stu-id="42f91-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="42f91-108">Migraciones es una característica muy usada con SQL Server, pero SQLite no admite muchas operaciones de esquema de migración, por lo que solo se pueden realizar migraciones muy sencillas.</span><span class="sxs-lookup"><span data-stu-id="42f91-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="42f91-109">Para más información, vea [SQLite EF Core Database Provider Limitations](/ef/core/providers/sqlite/limitations) (Limitaciones del proveedor de base de datos de SQLite EF Core).</span><span class="sxs-lookup"><span data-stu-id="42f91-109">See [SQLite Limitations](/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="42f91-110">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="42f91-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="42f91-111">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="42f91-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

<span data-ttu-id="42f91-112">Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la lista blanca de direcciones de enlace para que incluya esta nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="42f91-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="42f91-113">En *MoviesController.cs*, actualice el atributo `[Bind]` para que los métodos de acción `Create` y `Edit` incluyan la propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="42f91-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="42f91-114">También necesita actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="42f91-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="42f91-115">Edite el archivo */Views/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="42f91-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="42f91-116">Actualice */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="42f91-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="42f91-117">La aplicación no funcionará hasta que la base de datos se actualice para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="42f91-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="42f91-118">Si la ejecuta ahora, se producirá la siguiente `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="42f91-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="42f91-119">Este error se muestra porque la clase del modelo Movie actualizada es diferente del esquema de la tabla Movie de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="42f91-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="42f91-120">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="42f91-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="42f91-121">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="42f91-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="42f91-122">Desconecte la base de datos y haga que Entity Framework vuelva a crear automáticamente la base de datos según el nuevo esquema de clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="42f91-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="42f91-123">Con este enfoque se pierden los datos que tenga en la base de datos, así que no puede hacer esto con una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="42f91-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="42f91-124">A menudo, una forma productiva de desarrollar una aplicación consiste en usar un inicializador para propagar una base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="42f91-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="42f91-125">Modifique manualmente el esquema de la base de datos existente para que coincida con las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="42f91-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="42f91-126">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="42f91-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="42f91-127">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="42f91-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="42f91-128">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="42f91-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="42f91-129">Para este tutorial, se desconectará la base de datos y se volverá a crear cuando cambie el esquema.</span><span class="sxs-lookup"><span data-stu-id="42f91-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="42f91-130">Para desconectar la base de datos, ejecute este comando desde un terminal:</span><span class="sxs-lookup"><span data-stu-id="42f91-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="42f91-131">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="42f91-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="42f91-132">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizarlo con cada `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="42f91-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="42f91-133">Agregue el campo `Rating` a la vista `Edit`, `Details` y `Delete`.</span><span class="sxs-lookup"><span data-stu-id="42f91-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="42f91-134">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="42f91-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="42f91-135">plantillas.</span><span class="sxs-lookup"><span data-stu-id="42f91-135">templates.</span></span>
