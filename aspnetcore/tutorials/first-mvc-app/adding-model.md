---
title: Agregar un modelo a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Agregue un modelo a una aplicación sencilla de ASP.NET Core.
ms.author: riande
ms.date: 02/12/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: da30c1c97cbf40a89d163b2116c8d5f9ad422b25
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159245"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="f7660-103">Agregar un modelo a una aplicación de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f7660-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="f7660-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f7660-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f7660-105">En esta sección, agregará las clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="f7660-106">Estas clases serán el elemento "**M**odel" de la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="f7660-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="f7660-107">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="f7660-108">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos que se debe escribir.</span><span class="sxs-lookup"><span data-stu-id="f7660-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="f7660-109">Las clases de modelo que se crean se conocen como clases POCO (del inglés "**p**lain **O**ld **C**LR **O**bjects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f7660-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="f7660-110">Simplemente definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="f7660-111">En este tutorial, se escriben primero las clases del modelo y EF Core crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="f7660-112">Existe un enfoque alternativo que no trataremos aquí que consiste en generar clases de modelo a partir de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="f7660-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="f7660-113">Para más información sobre este enfoque, vea [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (ASP.NET Core - base de datos existente).</span><span class="sxs-lookup"><span data-stu-id="f7660-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="f7660-114">Agregar una clase de modelo de datos</span><span class="sxs-lookup"><span data-stu-id="f7660-114">Add a data model class</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7660-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7660-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f7660-116">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="f7660-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="f7660-117">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="f7660-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f7660-118">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f7660-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f7660-119">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="f7660-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="f7660-120">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="f7660-120">Scaffold the movie model</span></span>

<span data-ttu-id="f7660-121">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="f7660-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="f7660-122">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="f7660-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7660-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7660-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f7660-124">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="f7660-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller21.png)

<span data-ttu-id="f7660-126">En el cuadro de diálogo **Agregar scaffold**, seleccione **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f7660-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="f7660-128">Rellene el cuadro de diálogo **Agregar controlador**:</span><span class="sxs-lookup"><span data-stu-id="f7660-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="f7660-129">**Clase de modelo**: *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="f7660-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="f7660-130">**Clase de contexto de datos**: seleccione el icono **+** y agregue el valor predeterminado **MvcMovie.Models.MvcMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="f7660-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adición de contexto de datos](adding-model/_static/dc.png)

* <span data-ttu-id="f7660-132">**Vistas**: conserve el valor predeterminado de cada opción activada.</span><span class="sxs-lookup"><span data-stu-id="f7660-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="f7660-133">**Nombre del controlador**: conserve el valor predeterminado *MoviesController*.</span><span class="sxs-lookup"><span data-stu-id="f7660-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="f7660-134">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f7660-134">Select **Add**</span></span>

![Cuadro de diálogo Agregar controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="f7660-136">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="f7660-136">Visual Studio creates:</span></span>

* <span data-ttu-id="f7660-137">Una [clase de contexto de base de datos](xref:data/ef-mvc/intro#create-the-database-context) de Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="f7660-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="f7660-138">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f7660-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="f7660-139">Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="f7660-139">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="f7660-140">La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="f7660-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f7660-141">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f7660-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="f7660-142">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f7660-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f7660-143">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="f7660-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="f7660-144">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7660-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f7660-145">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f7660-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f7660-146">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f7660-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f7660-147">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="f7660-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="f7660-148">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7660-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="f7660-149">Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7660-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="f7660-150">Debe crear la base de datos y usar para ello la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f7660-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="f7660-151">Las migraciones permiten crear una base de datos que coincide con el modelo de datos y actualizan el esquema de base de datos cuando cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="f7660-152">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="f7660-152">Initial migration</span></span>

<span data-ttu-id="f7660-153">En esta sección, se completan las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="f7660-153">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="f7660-154">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f7660-154">Add an initial migration.</span></span>
* <span data-ttu-id="f7660-155">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f7660-155">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7660-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7660-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f7660-157">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="f7660-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menú de PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="f7660-159">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="f7660-159">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="f7660-160">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="f7660-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="f7660-161">El esquema de la base de datos se basa en el modelo especificado en la clase `MvcMovieContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f7660-161">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="f7660-162">El argumento `Initial` es el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="f7660-162">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="f7660-163">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="f7660-163">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="f7660-164">Para obtener más información, vea <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="f7660-164">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="f7660-165">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f7660-166">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f7660-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="f7660-167">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="f7660-167">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="f7660-168">El esquema de la base de datos se basa en el modelo especificado en la clase `MvcMovieContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f7660-168">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="f7660-169">El argumento `InitialCreate` es el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="f7660-169">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="f7660-170">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="f7660-170">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---  

<span data-ttu-id="f7660-171">Los comandos anteriores generan la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7660-171">The preceding commands generate the following warning:</span></span>

```text
No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.
```

<span data-ttu-id="f7660-172">Esa advertencia se puede omitir.</span><span class="sxs-lookup"><span data-stu-id="f7660-172">You can ignore that warning.</span></span> <span data-ttu-id="f7660-173">Se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="f7660-173">It will be fixed in a later tutorial.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="f7660-174">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="f7660-174">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="f7660-175">ASP.NET Core integra la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7660-175">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f7660-176">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7660-176">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="f7660-177">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="f7660-177">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f7660-178">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="f7660-178">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7660-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7660-179">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f7660-180">La herramienta de scaffolding creó de forma automática un contexto de base de datos y lo registró con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="f7660-180">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="f7660-181">Consulte el siguiente método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f7660-181">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f7660-182">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="f7660-182">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="f7660-183">El elemento `MvcMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f7660-183">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="f7660-184">El contexto de datos (`MvcMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="f7660-184">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="f7660-185">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="f7660-185">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="f7660-186">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="f7660-186">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="f7660-187">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7660-187">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="f7660-188">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="f7660-188">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f7660-189">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="f7660-189">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="f7660-190">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f7660-190">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f7660-191">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f7660-191">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f7660-192">Ha creado un contexto de base de datos y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="f7660-192">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="f7660-193">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f7660-193">Test the app</span></span>

* <span data-ttu-id="f7660-194">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="f7660-194">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="f7660-195">Si se produce una excepción de base de datos similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7660-195">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="f7660-196">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="f7660-196">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="f7660-197">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="f7660-197">Test the **Create** link.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f7660-198">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="f7660-198">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="f7660-199">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="f7660-199">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="f7660-200">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="f7660-200">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="f7660-201">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="f7660-201">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="f7660-202">Examine la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f7660-202">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="f7660-203">En el código resaltado anterior se muestra cómo se agrega el contexto de base de datos de películas al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="f7660-203">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="f7660-204">`services.AddDbContext<MvcMovieContext>(options =>` especifica la base de datos que se usará y la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="f7660-204">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="f7660-205">`=>` es un [operador lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="f7660-205">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="f7660-206">Abra el archivo *Controllers/MoviesController.cs* y examine el constructor:</span><span class="sxs-lookup"><span data-stu-id="f7660-206">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="f7660-207">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`MvcMovieContext `) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="f7660-207">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="f7660-208">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="f7660-208">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="f7660-209">Modelos fuertemente tipados y la palabra clave @model</span><span class="sxs-lookup"><span data-stu-id="f7660-209">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="f7660-210">Anteriormente en este tutorial, vimos cómo un controlador puede pasar datos u objetos a una vista mediante el diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="f7660-210">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="f7660-211">El diccionario `ViewData` es un objeto dinámico que proporciona una cómoda manera enlazada en tiempo de ejecución de pasar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="f7660-211">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="f7660-212">MVC también ofrece la capacidad de pasar objetos de modelo fuertemente tipados a una vista.</span><span class="sxs-lookup"><span data-stu-id="f7660-212">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="f7660-213">Este enfoque fuertemente tipado permite una mejor comprobación del código en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="f7660-213">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="f7660-214">El mecanismo de scaffolding usó este enfoque (que consiste en pasar un modelo fuertemente tipado) con la clase `MoviesController` y las vistas cuando creó los métodos y las vistas.</span><span class="sxs-lookup"><span data-stu-id="f7660-214">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="f7660-215">Examine el método `Details` generado en el archivo *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="f7660-215">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="f7660-216">El parámetro `id` suele pasarse como datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="f7660-216">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="f7660-217">Por ejemplo, `https://localhost:5001/movies/details/1` establece:</span><span class="sxs-lookup"><span data-stu-id="f7660-217">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="f7660-218">El controlador en el controlador `movies` (el primer segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="f7660-218">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="f7660-219">La acción en `details` (el segundo segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="f7660-219">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="f7660-220">El identificador en 1 (el último segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="f7660-220">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="f7660-221">También puede pasar `id` con una cadena de consulta como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="f7660-221">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="f7660-222">El parámetro `id` se define como un [tipo que acepta valores NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) en caso de que no se proporcione un valor de identificador.</span><span class="sxs-lookup"><span data-stu-id="f7660-222">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="f7660-223">Se pasa una [expresión lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) a `FirstOrDefaultAsync` para seleccionar entidades de película que coincidan con los datos de enrutamiento o el valor de consulta de cadena.</span><span class="sxs-lookup"><span data-stu-id="f7660-223">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="f7660-224">Si se encuentra una película, se pasa una instancia del modelo `Movie` a la vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="f7660-224">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="f7660-225">Examine el contenido del archivo *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f7660-225">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="f7660-226">Mediante la inclusión de una instrucción `@model` en la parte superior del archivo de vista, puede especificar el tipo de objeto que espera la vista.</span><span class="sxs-lookup"><span data-stu-id="f7660-226">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="f7660-227">Al crear el controlador de película, se incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f7660-227">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="f7660-228">Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="f7660-228">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="f7660-229">Por ejemplo, en la vista *Details.cshtml*, el código pasa cada campo de película a los asistentes de HTML `DisplayNameFor` y `DisplayFor` con el objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="f7660-229">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="f7660-230">Los métodos `Create` y `Edit` y las vistas también pasan un objeto de modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f7660-230">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="f7660-231">Examine la vista *Index.cshtml* y el método `Index` en el controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="f7660-231">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="f7660-232">Observe cómo el código crea un objeto `List` cuando llama al método `View`.</span><span class="sxs-lookup"><span data-stu-id="f7660-232">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="f7660-233">El código pasa esta lista `Movies` desde el método de acción `Index` a la vista:</span><span class="sxs-lookup"><span data-stu-id="f7660-233">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="f7660-234">Cuando se creó el controlador movies, el scaffolding incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f7660-234">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="f7660-235">Esta directiva `@model` permite acceder a la lista de películas que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="f7660-235">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="f7660-236">Por ejemplo, en la vista *Index.cshtml*, el código recorre en bucle las películas con una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:</span><span class="sxs-lookup"><span data-stu-id="f7660-236">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="f7660-237">Como el objeto `Model` es fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada elemento del bucle está tipado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f7660-237">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="f7660-238">Entre otras ventajas, esto implica que se obtiene una comprobación del código en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="f7660-238">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7660-239">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f7660-239">Additional resources</span></span>

* [<span data-ttu-id="f7660-240">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="f7660-240">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f7660-241">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="f7660-241">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="f7660-242">[Anterior: Agregar una vista](adding-view.md)
> [Siguiente: Trabajar con SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="f7660-242">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
