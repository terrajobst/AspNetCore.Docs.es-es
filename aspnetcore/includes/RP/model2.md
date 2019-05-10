<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="5e2df-101">Agregar una clase de contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="5e2df-101">Add a database context class</span></span>

<span data-ttu-id="5e2df-102">Agregue la clase `RazorPagesMovieContext` siguiente a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="5e2df-102">Add the following `RazorPagesMovieContext` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="5e2df-103">El código anterior crea una propiedad `DbSet` para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="5e2df-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="5e2df-104">En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="5e2df-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="5e2df-105">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="5e2df-105">Add a database connection string</span></span>

<span data-ttu-id="5e2df-106">Agregue una cadena de conexión al archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5e2df-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="5e2df-107">Adición de los paquetes NuGet necesarios</span><span class="sxs-lookup"><span data-stu-id="5e2df-107">Add required NuGet packages</span></span>

<span data-ttu-id="5e2df-108">Ejecute el comando siguiente de la CLI de .NET Core para agregar SQLite y CodeGeneration.Design al proyecto:</span><span class="sxs-lookup"><span data-stu-id="5e2df-108">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="5e2df-109">El paquete `Microsoft.VisualStudio.Web.CodeGeneration.Design` es necesario para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="5e2df-109">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="5e2df-110">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="5e2df-110">Register the database context</span></span>

<span data-ttu-id="5e2df-111">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5e2df-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="5e2df-112">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5e2df-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="5e2df-113">Compile el proyecto para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="5e2df-113">Build the project as a check for errors.</span></span>
