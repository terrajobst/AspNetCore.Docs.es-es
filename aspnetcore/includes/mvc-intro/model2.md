::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="3102f-101">Cree una carpeta *Data*.</span><span class="sxs-lookup"><span data-stu-id="3102f-101">Create a *Data* folder.</span></span>

<span data-ttu-id="3102f-102">Agregue la clase `MvcMovieContext` siguiente a la carpeta *Data*:</span><span class="sxs-lookup"><span data-stu-id="3102f-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="3102f-103">El código anterior crea una propiedad `DbSet` para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3102f-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="3102f-104">En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="3102f-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="3102f-105">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="3102f-105">Add a database connection string</span></span>

<span data-ttu-id="3102f-106">Agregue una cadena de conexión al archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3102f-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="3102f-107">Agregue los paquetes NuGet y las herramientas EF.</span><span class="sxs-lookup"><span data-stu-id="3102f-107">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="3102f-108">Ejecute los siguientes comandos CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="3102f-108">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="3102f-109">Los comandos anteriores agregan las herramientas de Entity Framework Core para la CLI de .NET y varios paquetes al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3102f-109">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="3102f-110">El paquete `Microsoft.VisualStudio.Web.CodeGeneration.Design` es necesario para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="3102f-110">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="3102f-111">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="3102f-111">Register the database context</span></span>

<span data-ttu-id="3102f-112">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3102f-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="3102f-113">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3102f-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="3102f-114">Compile el proyecto para comprobar si hay errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="3102f-114">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3102f-115">Agregue la clase `MvcMovieContext` siguiente a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="3102f-115">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="3102f-116">El código anterior crea una propiedad `DbSet` para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3102f-116">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="3102f-117">En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="3102f-117">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="3102f-118">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="3102f-118">Add a database connection string</span></span>

<span data-ttu-id="3102f-119">Agregue una cadena de conexión al archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3102f-119">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="3102f-120">Adición de los paquetes NuGet necesarios</span><span class="sxs-lookup"><span data-stu-id="3102f-120">Add required NuGet packages</span></span>

<span data-ttu-id="3102f-121">Ejecute el comando siguiente de la CLI de .NET Core para agregar SQLite y CodeGeneration.Design al proyecto:</span><span class="sxs-lookup"><span data-stu-id="3102f-121">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="3102f-122">El paquete `Microsoft.VisualStudio.Web.CodeGeneration.Design` es necesario para realizar scaffolding.</span><span class="sxs-lookup"><span data-stu-id="3102f-122">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="3102f-123">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="3102f-123">Register the database context</span></span>

<span data-ttu-id="3102f-124">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3102f-124">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="3102f-125">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3102f-125">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="3102f-126">Compile el proyecto para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="3102f-126">Build the project as a check for errors.</span></span>
::: moniker-end