<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="7a039-101">Agregar una clase de contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7a039-101">Add a database context class</span></span>

<span data-ttu-id="7a039-102">En el proyecto RazorPagesMovie, cree una carpeta denominada *Data*.</span><span class="sxs-lookup"><span data-stu-id="7a039-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="7a039-103">Agregue la clase `RazorPagesMovieContext` siguiente a la carpeta *Data*:</span><span class="sxs-lookup"><span data-stu-id="7a039-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="7a039-104">El código anterior crea una propiedad `DbSet` para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="7a039-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="7a039-105">En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="7a039-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="7a039-106">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="7a039-106">Add a database connection string</span></span>

<span data-ttu-id="7a039-107">Agregue una cadena de conexión al archivo *appsettings.json*, tal como se muestra en el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="7a039-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="7a039-108">Agregue los paquetes NuGet y las herramientas EF.</span><span class="sxs-lookup"><span data-stu-id="7a039-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="7a039-109">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7a039-109">Register the database context</span></span>

<span data-ttu-id="7a039-110">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a039-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="7a039-111">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7a039-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="7a039-112">Adición de los paquetes NuGet necesarios</span><span class="sxs-lookup"><span data-stu-id="7a039-112">Add required NuGet packages</span></span>

<span data-ttu-id="7a039-113">Ejecute el comando siguiente de la CLI de .NET Core para agregar SQLite y CodeGeneration.Design al proyecto:</span><span class="sxs-lookup"><span data-stu-id="7a039-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="7a039-114">El paquete `Microsoft.VisualStudio.Web.CodeGeneration.Design` es necesario para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7a039-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="7a039-115">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="7a039-115">Register the database context</span></span>

<span data-ttu-id="7a039-116">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a039-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="7a039-117">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7a039-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="7a039-118">Compile el proyecto para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="7a039-118">Build the project as a check for errors.</span></span>
::: moniker-end
