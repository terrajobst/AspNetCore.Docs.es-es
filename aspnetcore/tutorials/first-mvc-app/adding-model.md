---
title: Agregar un modelo a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Agregue un modelo a una aplicación sencilla de ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5d4251a2577111324aa2cfb715c41e3ecad5a9d1
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722807"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="96879-103">Agregar un modelo a una aplicación de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="96879-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="96879-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="96879-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="96879-105">En esta sección, agregará las clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="96879-106">Estas clases serán el elemento "**M**odel" de la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="96879-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="96879-107">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="96879-108">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos que se debe escribir.</span><span class="sxs-lookup"><span data-stu-id="96879-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="96879-109">Las clases de modelo que se crean se conocen como clases POCO (del inglés "**p**lain **O**ld **C**LR **O**bjects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="96879-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="96879-110">Simplemente definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="96879-111">En este tutorial, se escriben primero las clases del modelo y EF Core crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="96879-112">Existe un enfoque alternativo que no trataremos aquí que consiste en generar clases de modelo a partir de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="96879-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="96879-113">Para más información sobre este enfoque, vea [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (ASP.NET Core - base de datos existente).</span><span class="sxs-lookup"><span data-stu-id="96879-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="96879-114">Agregar una clase de modelo de datos</span><span class="sxs-lookup"><span data-stu-id="96879-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-116">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="96879-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="96879-117">Asigne el nombre *Movie.cs* al archivo.</span><span class="sxs-lookup"><span data-stu-id="96879-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96879-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96879-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="96879-119">Agregue un archivo denominado *Movie.cs* a la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="96879-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96879-120">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="96879-121">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Nueva clase** > **Clase vacía**.</span><span class="sxs-lookup"><span data-stu-id="96879-121">Right-click the *Models* folder > **Add** > **New Class** > **Empty Class**.</span></span> <span data-ttu-id="96879-122">Asigne el nombre *Movie.cs* al archivo.</span><span class="sxs-lookup"><span data-stu-id="96879-122">Name the file *Movie.cs*.</span></span>

---

<span data-ttu-id="96879-123">Actualice el archivo *Movie.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="96879-123">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="96879-124">La clase `Movie` contiene un campo `Id`, que la base de datos requiere para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="96879-124">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="96879-125">El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) de `ReleaseDate` especifica el tipo de los datos (`Date`).</span><span class="sxs-lookup"><span data-stu-id="96879-125">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="96879-126">Con este atributo:</span><span class="sxs-lookup"><span data-stu-id="96879-126">With this attribute:</span></span>

  * <span data-ttu-id="96879-127">El usuario no tiene que especificar información horaria en el campo de fecha.</span><span class="sxs-lookup"><span data-stu-id="96879-127">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="96879-128">Solo se muestra la fecha, no información horaria.</span><span class="sxs-lookup"><span data-stu-id="96879-128">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="96879-129">Los elementos [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) se tratan en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="96879-129">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="96879-130">Adición de paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="96879-130">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-132">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="96879-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menú de PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="96879-134">En la Consola del administrador de paquetes, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="96879-134">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="96879-135">El comando anterior agrega el proveedor de SQL Server de EF Core.</span><span class="sxs-lookup"><span data-stu-id="96879-135">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="96879-136">El paquete de proveedor instala el paquete de EF Core como una dependencia.</span><span class="sxs-lookup"><span data-stu-id="96879-136">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="96879-137">Los paquetes adicionales se instalan de forma automática en el paso de scaffolding más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="96879-137">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96879-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96879-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96879-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="96879-140">En el menú **Proyecto**, seleccione **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="96879-140">From the **Project** menu, select **Manage Nuget Packages**.</span></span>

<span data-ttu-id="96879-141">En el campo de **búsqueda** de la esquina superior derecha, escriba `Microsoft.EntityFrameworkCore.SQLite` y presione la tecla **Entrar** para realizar la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="96879-141">In the **Search** field in the upper right, enter `Microsoft.EntityFrameworkCore.SQLite` and press the **Return** key to search.</span></span> <span data-ttu-id="96879-142">Seleccione el paquete NuGet correspondiente y presione el botón **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="96879-142">Select the matching NuGet package and press the **Add Package** button.</span></span>

![Adición del paquete NuGet de Entity Framework Core](~/tutorials/first-mvc-app-mac/adding-model/_static/add-nuget-packages.png)

<span data-ttu-id="96879-144">Se mostrará el cuadro de diálogo **Seleccionar los proyectos**, con el proyecto `MvcMovie` seleccionado.</span><span class="sxs-lookup"><span data-stu-id="96879-144">The **Select Projects** dialog will be displayed, with the `MvcMovie` project selected.</span></span> <span data-ttu-id="96879-145">Presione el botón **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="96879-145">Press the **Ok** button.</span></span>

<span data-ttu-id="96879-146">Se mostrará un cuadro de diálogo **Aceptación de licencia**.</span><span class="sxs-lookup"><span data-stu-id="96879-146">A **License Acceptance** dialog will be displayed.</span></span> <span data-ttu-id="96879-147">Revise las licencias como quiera y, a continuación, haga clic en el botón **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="96879-147">Review the licenses as desired, then click the **Accept** button.</span></span>

<span data-ttu-id="96879-148">Repita los pasos anteriores para instalar los paquetes NuGet siguientes:</span><span class="sxs-lookup"><span data-stu-id="96879-148">Repeat the above steps to install the following NuGet packages:</span></span>
 * `Microsoft.VisualStudio.Web.CodeGeneration.Design`
 * `Microsoft.EntityFrameworkCore.SqlServer`
 * `Microsoft.EntityFrameworkCore.Design`

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="96879-149">Creación de una clase de contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="96879-149">Create a database context class</span></span>

<span data-ttu-id="96879-150">Se necesita una clase de contexto de base de datos para coordinar la funcionalidad de EF Core (crear, leer, actualizar, eliminar) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="96879-150">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="96879-151">El contexto de base de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) y especifica las entidades que se van a incluir en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-151">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="96879-152">Cree una carpeta *Data*.</span><span class="sxs-lookup"><span data-stu-id="96879-152">Create a *Data* folder.</span></span>

<span data-ttu-id="96879-153">Agregue un archivo *Data/MvcMovieContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="96879-153">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="96879-154">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="96879-154">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="96879-155">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-155">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="96879-156">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="96879-156">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="96879-157">Registro del contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="96879-157">Register the database context</span></span>

<span data-ttu-id="96879-158">ASP.NET Core integra la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="96879-158">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="96879-159">Los servicios (como el contexto de base de datos de EF Core) se deben registrar con la inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96879-159">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="96879-160">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="96879-160">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="96879-161">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="96879-161">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="96879-162">En esta sección, se registra el contexto de base de datos con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="96879-162">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="96879-163">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="96879-163">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="96879-164">Agregue el código resaltado siguiente en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="96879-164">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-165">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-166">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="96879-167">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="96879-167">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="96879-168">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="96879-168">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="96879-169">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="96879-169">Add a database connection string</span></span>

<span data-ttu-id="96879-170">Agregue una cadena de conexión al archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96879-170">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-171">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-172">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-172">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="96879-173">Compile el proyecto para comprobar si hay errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="96879-173">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="96879-174">Scaffolding de las páginas de películas</span><span class="sxs-lookup"><span data-stu-id="96879-174">Scaffold movie pages</span></span>

<span data-ttu-id="96879-175">Use la herramienta de scaffolding para crear páginas de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="96879-175">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-177">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta \*Controladores\***> Agregar > Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="96879-177">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller21.png)

<span data-ttu-id="96879-179">En el cuadro de diálogo **Agregar scaffold**, seleccione **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="96879-179">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="96879-181">Rellene el cuadro de diálogo **Agregar controlador**:</span><span class="sxs-lookup"><span data-stu-id="96879-181">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="96879-182">**Clase de modelo**: *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="96879-182">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="96879-183">**Clase de contexto de datos**: *MvcMovieContext (MvcMovie.Data)*</span><span class="sxs-lookup"><span data-stu-id="96879-183">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Adición de contexto de datos](adding-model/_static/dc3.png)

* <span data-ttu-id="96879-185">**Vistas**: conserve el valor predeterminado de cada opción activada.</span><span class="sxs-lookup"><span data-stu-id="96879-185">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="96879-186">**Nombre del controlador**: conserve el valor predeterminado *MoviesController*.</span><span class="sxs-lookup"><span data-stu-id="96879-186">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="96879-187">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="96879-187">Select **Add**</span></span>

<span data-ttu-id="96879-188">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="96879-188">Visual Studio creates:</span></span>

* <span data-ttu-id="96879-189">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="96879-189">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="96879-190">Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="96879-190">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="96879-191">La creación automática de estos archivos se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="96879-191">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96879-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96879-192">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="96879-193">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="96879-193">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="96879-194">En Linux, exporte la ruta de acceso de la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="96879-194">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="96879-195">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="96879-195">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96879-196">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="96879-197">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="96879-197">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="96879-198">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="96879-198">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="96879-199">Todavía no se pueden usar las páginas con scaffolding porque la base de datos no existe.</span><span class="sxs-lookup"><span data-stu-id="96879-199">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="96879-200">Si ejecuta la aplicación y hace clic en el vínculo **Movie App**, obtendrá un mensaje de error *Cannot open database* (No se puede abrir la base de datos) o *no such table: Movie* (no existe la tabla: Movie).</span><span class="sxs-lookup"><span data-stu-id="96879-200">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="96879-201">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="96879-201">Initial migration</span></span>

<span data-ttu-id="96879-202">Use la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-202">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="96879-203">Las migraciones son un conjunto de herramientas que permiten crear y actualizar una base de datos para que coincida con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-203">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-204">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-205">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="96879-205">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="96879-206">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="96879-206">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="96879-207">`Add-Migration InitialCreate`: Genera un archivo de migración *Migrations/{marca de tiempo}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="96879-207">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="96879-208">El argumento `InitialCreate` es el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-208">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="96879-209">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-209">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="96879-210">Como se trata de la primera migración, la clase generada contiene código para crear el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-210">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="96879-211">El esquema de la base de datos se basa en el modelo especificado en la clase `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="96879-211">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="96879-212">`Update-Database`: actualiza la base de datos a la migración más reciente, que ha creado el comando anterior.</span><span class="sxs-lookup"><span data-stu-id="96879-212">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="96879-213">El comando ejecuta el método `Up` en el archivo *Migrations/{marca de tiempo}_InitialCreate.cs*, que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-213">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="96879-214">El comando de actualización de la base de datos genera la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="96879-214">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="96879-215">No type was specified for the decimal column "Price" on entity type "Movie" (No se ha especificado ningún tipo en la columna decimal "Price" en el tipo de entidad "Movie").</span><span class="sxs-lookup"><span data-stu-id="96879-215">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="96879-216">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="96879-216">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="96879-217">Explicitly specify the SQL server column type that can accommodate all the values using "HasColumnType()" (Especifique de forma explícita el tipo de columna de SQL Server que pueda acomodar todos los valores mediante "HasColumnType()").</span><span class="sxs-lookup"><span data-stu-id="96879-217">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="96879-218">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="96879-218">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-219">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-219">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="96879-220">Ejecute los siguientes comandos CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="96879-220">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="96879-221">`ef migrations add InitialCreate`: Genera un archivo de migración *Migrations/{marca de tiempo}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="96879-221">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="96879-222">El argumento `InitialCreate` es el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-222">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="96879-223">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-223">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="96879-224">Como se trata de la primera migración, la clase generada contiene código para crear el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-224">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="96879-225">El esquema de la base de datos se basa en el modelo especificado en la clase `MvcMovieContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="96879-225">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="96879-226">`ef database update`: actualiza la base de datos a la migración más reciente, que ha creado el comando anterior.</span><span class="sxs-lookup"><span data-stu-id="96879-226">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="96879-227">El comando ejecuta el método `Up` en el archivo *Migrations/{marca de tiempo}_InitialCreate.cs*, que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-227">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="96879-228">La clase InitialCreate</span><span class="sxs-lookup"><span data-stu-id="96879-228">The InitialCreate class</span></span>

<span data-ttu-id="96879-229">Examine el archivo de migración *Migrations/{marca de tiempo}_InitialCreate.cs*:</span><span class="sxs-lookup"><span data-stu-id="96879-229">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="96879-230">El método `Up` crea la tabla Movie y configura `Id` como la clave principal.</span><span class="sxs-lookup"><span data-stu-id="96879-230">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="96879-231">El método `Down` revierte los cambios de esquema realizados por la migración `Up`.</span><span class="sxs-lookup"><span data-stu-id="96879-231">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="96879-232">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="96879-232">Test the app</span></span>

* <span data-ttu-id="96879-233">Ejecute la aplicación y haga clic en el vínculo **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="96879-233">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="96879-234">Si obtiene una excepción similar a una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="96879-234">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-235">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-236">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="96879-237">Probablemente haya omitido el [paso de migraciones](#migration).</span><span class="sxs-lookup"><span data-stu-id="96879-237">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="96879-238">Pruebe la página **Create**.</span><span class="sxs-lookup"><span data-stu-id="96879-238">Test the **Create** page.</span></span> <span data-ttu-id="96879-239">Escriba y envíe los datos.</span><span class="sxs-lookup"><span data-stu-id="96879-239">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="96879-240">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="96879-240">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="96879-241">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="96879-241">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="96879-242">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="96879-242">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="96879-243">Pruebe las páginas **Edit**, **Details** y **Delete**.</span><span class="sxs-lookup"><span data-stu-id="96879-243">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="96879-244">Inserción de dependencias en el controlador</span><span class="sxs-lookup"><span data-stu-id="96879-244">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-246">Abra el archivo *Controllers/MoviesController.cs* y examine el constructor:</span><span class="sxs-lookup"><span data-stu-id="96879-246">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="96879-247">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`MvcMovieContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="96879-247">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="96879-248">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="96879-248">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-249">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="96879-250">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`MvcMovieContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="96879-250">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="96879-251">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="96879-251">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="96879-252">Modelos fuertemente tipados y la palabra clave @model</span><span class="sxs-lookup"><span data-stu-id="96879-252">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="96879-253">Anteriormente en este tutorial, vimos cómo un controlador puede pasar datos u objetos a una vista mediante el diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="96879-253">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="96879-254">El diccionario `ViewData` es un objeto dinámico que proporciona una cómoda manera enlazada en tiempo de ejecución de pasar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="96879-254">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="96879-255">MVC también ofrece la capacidad de pasar objetos de modelo fuertemente tipados a una vista.</span><span class="sxs-lookup"><span data-stu-id="96879-255">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="96879-256">Este enfoque fuertemente tipado permite comprobar el código en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="96879-256">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="96879-257">En el mecanismo de scaffolding se ha usado este enfoque (que consiste en pasar un modelo fuertemente tipado) con la clase `MoviesController` y las vistas.</span><span class="sxs-lookup"><span data-stu-id="96879-257">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="96879-258">Examine el método `Details` generado en el archivo *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="96879-258">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="96879-259">El parámetro `id` suele pasarse como datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="96879-259">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="96879-260">Por ejemplo, `https://localhost:5001/movies/details/1` establece:</span><span class="sxs-lookup"><span data-stu-id="96879-260">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="96879-261">El controlador en el controlador `movies` (el primer segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="96879-261">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="96879-262">La acción en `details` (el segundo segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="96879-262">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="96879-263">El identificador en 1 (el último segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="96879-263">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="96879-264">También puede pasar `id` con una cadena de consulta como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="96879-264">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="96879-265">El parámetro `id` se define como un [tipo que acepta valores NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) en caso de que no se proporcione un valor de identificador.</span><span class="sxs-lookup"><span data-stu-id="96879-265">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="96879-266">Se pasa una [expresión lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) a `FirstOrDefaultAsync` para seleccionar entidades de película que coincidan con los datos de enrutamiento o el valor de consulta de cadena.</span><span class="sxs-lookup"><span data-stu-id="96879-266">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="96879-267">Si se encuentra una película, se pasa una instancia del modelo `Movie` a la vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="96879-267">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="96879-268">Examine el contenido del archivo *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96879-268">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="96879-269">La instrucción `@model` de la parte superior del archivo de vista especifica el tipo de objeto que espera la vista.</span><span class="sxs-lookup"><span data-stu-id="96879-269">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="96879-270">Cuando se ha creado el controlador de película, se ha incluido la siguiente instrucción `@model`:</span><span class="sxs-lookup"><span data-stu-id="96879-270">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="96879-271">Esta directiva `@model` permite el acceso a la película que el controlador ha pasado a la vista.</span><span class="sxs-lookup"><span data-stu-id="96879-271">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="96879-272">El objeto `Model` está fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="96879-272">The `Model` object is strongly typed.</span></span> <span data-ttu-id="96879-273">Por ejemplo, en la vista *Details.cshtml*, el código pasa cada campo de película a los asistentes de HTML `DisplayNameFor` y `DisplayFor` con el objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="96879-273">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="96879-274">Los métodos `Create` y `Edit` y las vistas también pasan un objeto de modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="96879-274">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="96879-275">Examine la vista *Index.cshtml* y el método `Index` en el controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="96879-275">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="96879-276">Observe cómo el código crea un objeto `List` cuando llama al método `View`.</span><span class="sxs-lookup"><span data-stu-id="96879-276">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="96879-277">El código pasa esta lista `Movies` desde el método de acción `Index` a la vista:</span><span class="sxs-lookup"><span data-stu-id="96879-277">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="96879-278">Cuando se ha creado el controlador de películas, el scaffolding ha incluido la siguiente instrucción `@model` en la parte superior del archivo *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96879-278">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="96879-279">Esta directiva `@model` permite acceder a la lista de películas que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="96879-279">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="96879-280">Por ejemplo, en la vista *Index.cshtml*, el código recorre en bucle las películas con una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:</span><span class="sxs-lookup"><span data-stu-id="96879-280">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="96879-281">Como el objeto `Model` es fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada elemento del bucle está tipado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="96879-281">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="96879-282">Entre otras ventajas, esto implica que se obtiene la comprobación del código en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="96879-282">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96879-283">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="96879-283">Additional resources</span></span>

* [<span data-ttu-id="96879-284">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="96879-284">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="96879-285">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="96879-285">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="96879-286">[Anterior: Agregar una vista](adding-view.md)
> [Siguiente: Trabajar con SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="96879-286">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="96879-287">Agregar una clase de modelo de datos</span><span class="sxs-lookup"><span data-stu-id="96879-287">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-288">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-288">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-289">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="96879-289">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="96879-290">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="96879-290">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-291">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-291">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="96879-292">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="96879-292">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="96879-293">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="96879-293">Scaffold the movie model</span></span>

<span data-ttu-id="96879-294">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="96879-294">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="96879-295">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="96879-295">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-297">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta \*Controladores\***> Agregar > Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="96879-297">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller21.png)

<span data-ttu-id="96879-299">En el cuadro de diálogo **Agregar scaffold**, seleccione **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="96879-299">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="96879-301">Rellene el cuadro de diálogo **Agregar controlador**:</span><span class="sxs-lookup"><span data-stu-id="96879-301">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="96879-302">**Clase de modelo**: *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="96879-302">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="96879-303">**Clase de contexto de datos**: seleccione el icono **+** y agregue el valor predeterminado **MvcMovie.Models.MvcMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="96879-303">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adición de contexto de datos](adding-model/_static/dc.png)

* <span data-ttu-id="96879-305">**Vistas**: conserve el valor predeterminado de cada opción activada.</span><span class="sxs-lookup"><span data-stu-id="96879-305">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="96879-306">**Nombre del controlador**: conserve el valor predeterminado *MoviesController*.</span><span class="sxs-lookup"><span data-stu-id="96879-306">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="96879-307">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="96879-307">Select **Add**</span></span>

![Cuadro de diálogo Agregar controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="96879-309">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="96879-309">Visual Studio creates:</span></span>

* <span data-ttu-id="96879-310">Una [clase de contexto de base de datos](xref:data/ef-mvc/intro#create-the-database-context) de Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="96879-310">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="96879-311">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="96879-311">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="96879-312">Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="96879-312">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="96879-313">La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="96879-313">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96879-314">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96879-314">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="96879-315">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="96879-315">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="96879-316">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="96879-316">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="96879-317">En Linux, exporte la ruta de acceso de la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="96879-317">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="96879-318">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="96879-318">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96879-319">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="96879-320">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="96879-320">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="96879-321">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="96879-321">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="96879-322">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="96879-322">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="96879-323">Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="96879-323">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-324">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-324">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-325">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="96879-326">Debe crear la base de datos y usar para ello la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core.</span><span class="sxs-lookup"><span data-stu-id="96879-326">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="96879-327">Las migraciones permiten crear una base de datos que coincide con el modelo de datos y actualizan el esquema de base de datos cuando cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-327">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="96879-328">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="96879-328">Initial migration</span></span>

<span data-ttu-id="96879-329">En esta sección, se completan las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="96879-329">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="96879-330">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="96879-330">Add an initial migration.</span></span>
* <span data-ttu-id="96879-331">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="96879-331">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-332">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="96879-333">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="96879-333">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menú de PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="96879-335">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="96879-335">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="96879-336">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="96879-336">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="96879-337">El esquema de la base de datos se basa en el modelo especificado en la clase `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="96879-337">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="96879-338">El argumento `Initial` es el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-338">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="96879-339">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-339">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="96879-340">Para obtener más información, vea <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="96879-340">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="96879-341">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-341">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-342">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-342">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="96879-343">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="96879-343">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="96879-344">El esquema de la base de datos se basa en el modelo especificado en la clase `MvcMovieContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="96879-344">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="96879-345">El argumento `InitialCreate` es el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-345">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="96879-346">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="96879-346">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="96879-347">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="96879-347">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="96879-348">ASP.NET Core integra la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="96879-348">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="96879-349">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96879-349">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="96879-350">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="96879-350">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="96879-351">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="96879-351">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96879-352">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96879-352">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96879-353">La herramienta de scaffolding creó de forma automática un contexto de base de datos y lo registró con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="96879-353">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="96879-354">Consulte el siguiente método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="96879-354">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="96879-355">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="96879-355">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="96879-356">El elemento `MvcMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="96879-356">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="96879-357">El contexto de datos (`MvcMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="96879-357">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="96879-358">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="96879-358">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="96879-359">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="96879-359">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="96879-360">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="96879-360">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="96879-361">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="96879-361">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="96879-362">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="96879-362">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="96879-363">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="96879-363">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="96879-364">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96879-364">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="96879-365">Ha creado un contexto de base de datos y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="96879-365">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="96879-366">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="96879-366">Test the app</span></span>

* <span data-ttu-id="96879-367">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="96879-367">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="96879-368">Si se produce una excepción de base de datos similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="96879-368">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="96879-369">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="96879-369">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="96879-370">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="96879-370">Test the **Create** link.</span></span> <span data-ttu-id="96879-371">Escriba y envíe los datos.</span><span class="sxs-lookup"><span data-stu-id="96879-371">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="96879-372">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="96879-372">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="96879-373">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="96879-373">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="96879-374">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="96879-374">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="96879-375">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="96879-375">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="96879-376">Examine la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="96879-376">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="96879-377">En el código resaltado anterior se muestra cómo se agrega el contexto de base de datos de películas al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="96879-377">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="96879-378">`services.AddDbContext<MvcMovieContext>(options =>` especifica la base de datos que se usará y la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="96879-378">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="96879-379">`=>` es un [operador lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="96879-379">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="96879-380">Abra el archivo *Controllers/MoviesController.cs* y examine el constructor:</span><span class="sxs-lookup"><span data-stu-id="96879-380">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="96879-381">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`MvcMovieContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="96879-381">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="96879-382">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="96879-382">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="96879-383">Modelos fuertemente tipados y la palabra clave @model</span><span class="sxs-lookup"><span data-stu-id="96879-383">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="96879-384">Anteriormente en este tutorial, vimos cómo un controlador puede pasar datos u objetos a una vista mediante el diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="96879-384">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="96879-385">El diccionario `ViewData` es un objeto dinámico que proporciona una cómoda manera enlazada en tiempo de ejecución de pasar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="96879-385">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="96879-386">MVC también ofrece la capacidad de pasar objetos de modelo fuertemente tipados a una vista.</span><span class="sxs-lookup"><span data-stu-id="96879-386">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="96879-387">Este enfoque fuertemente tipado permite una mejor comprobación del código en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="96879-387">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="96879-388">El mecanismo de scaffolding usó este enfoque (que consiste en pasar un modelo fuertemente tipado) con la clase `MoviesController` y las vistas cuando creó los métodos y las vistas.</span><span class="sxs-lookup"><span data-stu-id="96879-388">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="96879-389">Examine el método `Details` generado en el archivo *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="96879-389">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="96879-390">El parámetro `id` suele pasarse como datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="96879-390">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="96879-391">Por ejemplo, `https://localhost:5001/movies/details/1` establece:</span><span class="sxs-lookup"><span data-stu-id="96879-391">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="96879-392">El controlador en el controlador `movies` (el primer segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="96879-392">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="96879-393">La acción en `details` (el segundo segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="96879-393">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="96879-394">El identificador en 1 (el último segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="96879-394">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="96879-395">También puede pasar `id` con una cadena de consulta como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="96879-395">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="96879-396">El parámetro `id` se define como un [tipo que acepta valores NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) en caso de que no se proporcione un valor de identificador.</span><span class="sxs-lookup"><span data-stu-id="96879-396">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="96879-397">Se pasa una [expresión lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) a `FirstOrDefaultAsync` para seleccionar entidades de película que coincidan con los datos de enrutamiento o el valor de consulta de cadena.</span><span class="sxs-lookup"><span data-stu-id="96879-397">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="96879-398">Si se encuentra una película, se pasa una instancia del modelo `Movie` a la vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="96879-398">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="96879-399">Examine el contenido del archivo *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96879-399">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="96879-400">Mediante la inclusión de una instrucción `@model` en la parte superior del archivo de vista, puede especificar el tipo de objeto que espera la vista.</span><span class="sxs-lookup"><span data-stu-id="96879-400">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="96879-401">Al crear el controlador de película, se incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96879-401">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="96879-402">Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="96879-402">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="96879-403">Por ejemplo, en la vista *Details.cshtml*, el código pasa cada campo de película a los asistentes de HTML `DisplayNameFor` y `DisplayFor` con el objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="96879-403">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="96879-404">Los métodos `Create` y `Edit` y las vistas también pasan un objeto de modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="96879-404">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="96879-405">Examine la vista *Index.cshtml* y el método `Index` en el controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="96879-405">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="96879-406">Observe cómo el código crea un objeto `List` cuando llama al método `View`.</span><span class="sxs-lookup"><span data-stu-id="96879-406">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="96879-407">El código pasa esta lista `Movies` desde el método de acción `Index` a la vista:</span><span class="sxs-lookup"><span data-stu-id="96879-407">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="96879-408">Cuando se creó el controlador movies, el scaffolding incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="96879-408">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="96879-409">Esta directiva `@model` permite acceder a la lista de películas que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="96879-409">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="96879-410">Por ejemplo, en la vista *Index.cshtml*, el código recorre en bucle las películas con una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:</span><span class="sxs-lookup"><span data-stu-id="96879-410">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="96879-411">Como el objeto `Model` es fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada elemento del bucle está tipado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="96879-411">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="96879-412">Entre otras ventajas, esto implica que se obtiene una comprobación del código en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="96879-412">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96879-413">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="96879-413">Additional resources</span></span>

* [<span data-ttu-id="96879-414">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="96879-414">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="96879-415">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="96879-415">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="96879-416">[Anterior: Agregar una vista](adding-view.md)
> [Siguiente: Trabajar con una base de datos](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="96879-416">[Previous Adding a View](adding-view.md)
[Next Working with a database](working-with-sql.md)</span></span>

::: moniker-end
