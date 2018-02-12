# <a name="working-with-sqlite-in-and-razor-pages"></a><span data-ttu-id="02321-101">Trabajar con SQLite y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="02321-101">Working with SQLite in and Razor Pages</span></span>

<span data-ttu-id="02321-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="02321-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="02321-103">El objeto `MovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="02321-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="02321-104">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02321-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="02321-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="02321-105">SQLite</span></span>

<span data-ttu-id="02321-106">Según la información del sitio web de [SQLite](https://www.sqlite.org/):</span><span class="sxs-lookup"><span data-stu-id="02321-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="02321-107">SQLite es un motor de base de datos SQL independiente, de alta confiabilidad, insertado, con características completas y dominio público.</span><span class="sxs-lookup"><span data-stu-id="02321-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="02321-108">SQLite es el motor de base de datos más usado en el mundo.</span><span class="sxs-lookup"><span data-stu-id="02321-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="02321-109">Existen muchas herramientas de terceros que se pueden descargar para administrar y ver una base de datos de SQLite.</span><span class="sxs-lookup"><span data-stu-id="02321-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="02321-110">La imagen de abajo es de [DB Browser for SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="02321-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="02321-111">Si tiene una herramienta favorita de SQLite, deje un comentario sobre lo que le gusta de ella.</span><span class="sxs-lookup"><span data-stu-id="02321-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser for SQLite mostrando una base de datos de películas](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="02321-113">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="02321-113">Seed the database</span></span>

<span data-ttu-id="02321-114">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="02321-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="02321-115">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="02321-115">Replace the generated code with the following:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="02321-116">Si hay alguna película en la base de datos, se devuelve el inicializador.</span><span class="sxs-lookup"><span data-stu-id="02321-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="02321-117">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="02321-117">Add the seed initializer</span></span>

<span data-ttu-id="02321-118">Agregue el inicializador al método `Main` en el archivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="02321-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a><span data-ttu-id="02321-119">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="02321-119">Test the app</span></span>

<span data-ttu-id="02321-120">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="02321-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="02321-121">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="02321-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="02321-122">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="02321-122">The app shows the seeded data.</span></span>