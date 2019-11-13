---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 312b3d4eb13eb04453bf0c3256fc362918157a45
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634182"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="6fb32-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6fb32-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="6fb32-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6fb32-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6fb32-105">En esta sección, se agregan clases para administrar películas en una [base de datos SQLite](https://www.sqlite.org/index.html) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="6fb32-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="6fb32-106">Las aplicaciones creadas a partir de una plantilla de ASP.NET Core usan una base de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="6fb32-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="6fb32-107">Las clases de modelo de la aplicación se usan con [Entity Framework Core (EF Core)](/ef/core) ([Proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite)) para trabajar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="6fb32-108">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="6fb32-109">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="6fb32-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6fb32-110">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6fb32-111">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="6fb32-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-113">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6fb32-114">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-114">Name the folder *Models*.</span></span>

<span data-ttu-id="6fb32-115">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-115">Right click the *Models* folder.</span></span> <span data-ttu-id="6fb32-116">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="6fb32-117">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6fb32-119">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6fb32-120">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6fb32-122">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="6fb32-123">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="6fb32-124">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="6fb32-125">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="6fb32-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="6fb32-126">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="6fb32-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="6fb32-127">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="6fb32-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="6fb32-128">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="6fb32-129">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="6fb32-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6fb32-130">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="6fb32-130">Scaffold the movie model</span></span>

<span data-ttu-id="6fb32-131">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="6fb32-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6fb32-132">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="6fb32-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-134">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="6fb32-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="6fb32-135">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="6fb32-136">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-136">Name the folder *Movies*</span></span>

<span data-ttu-id="6fb32-137">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="6fb32-139">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="6fb32-141">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="6fb32-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="6fb32-142">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="6fb32-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="6fb32-143">En la fila **Clase de contexto de datos**, seleccione el signo **+** (más) y cambie el nombre generado de RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="6fb32-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="6fb32-144">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="6fb32-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="6fb32-145">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="6fb32-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="6fb32-146">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-146">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/3/arp.png)

<span data-ttu-id="6fb32-148">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="6fb32-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6fb32-150">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="6fb32-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6fb32-151">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="6fb32-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6fb32-152">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6fb32-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6fb32-153">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6fb32-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-154">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6fb32-155">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="6fb32-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6fb32-156">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="6fb32-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6fb32-157">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6fb32-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="6fb32-158">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="6fb32-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-160">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6fb32-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="6fb32-161">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="6fb32-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="6fb32-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="6fb32-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="6fb32-163">Actualizado</span><span class="sxs-lookup"><span data-stu-id="6fb32-163">Updated</span></span>

* <span data-ttu-id="6fb32-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="6fb32-164">*Startup.cs*</span></span>

<span data-ttu-id="6fb32-165">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="6fb32-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6fb32-166">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6fb32-167">El proceso de scaffolding crea los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6fb32-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="6fb32-168">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="6fb32-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="6fb32-169">Los archivos creados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="6fb32-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6fb32-170">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="6fb32-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-172">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="6fb32-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="6fb32-173">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-173">Add an initial migration.</span></span>
* <span data-ttu-id="6fb32-174">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="6fb32-175">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6fb32-177">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="6fb32-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-179">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="6fb32-180">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="6fb32-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="6fb32-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="6fb32-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="6fb32-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="6fb32-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="6fb32-183">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="6fb32-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="6fb32-184">El comando migrations genera código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="6fb32-185">El esquema se basa en el modelo especificado en `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="6fb32-186">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="6fb32-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="6fb32-187">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="6fb32-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="6fb32-188">El comando `update` ejecuta el método `Up` en las migraciones que no se han aplicado.</span><span class="sxs-lookup"><span data-stu-id="6fb32-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="6fb32-189">En este caso, `update` ejecuta el método `Up` en *Migrations/\<time-stamp>_InitialCreate.cs*, que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6fb32-191">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="6fb32-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6fb32-192">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6fb32-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6fb32-193">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6fb32-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6fb32-194">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="6fb32-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6fb32-195">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6fb32-196">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6fb32-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="6fb32-197">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6fb32-198">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="6fb32-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="6fb32-199">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6fb32-200">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6fb32-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6fb32-201">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6fb32-202">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6fb32-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6fb32-203">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6fb32-204">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="6fb32-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6fb32-205">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="6fb32-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6fb32-206">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6fb32-208">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-209">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6fb32-210">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6fb32-211">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6fb32-211">Test the app</span></span>

* <span data-ttu-id="6fb32-212">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="6fb32-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6fb32-213">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="6fb32-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6fb32-214">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6fb32-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6fb32-215">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-215">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="6fb32-217">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6fb32-218">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6fb32-219">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6fb32-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6fb32-220">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6fb32-221">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6fb32-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fb32-222">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6fb32-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6fb32-223">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6fb32-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6fb32-224">En esta sección, se agregan clases para administrar películas en una [base de datos SQLite](https://www.sqlite.org/index.html) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="6fb32-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="6fb32-225">Las aplicaciones creadas a partir de una plantilla de ASP.NET Core usan una base de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="6fb32-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="6fb32-226">Las clases de modelo de la aplicación se usan con [Entity Framework Core (EF Core)](/ef/core) ([Proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite)) para trabajar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="6fb32-227">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="6fb32-228">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="6fb32-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6fb32-229">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6fb32-230">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="6fb32-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-232">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6fb32-233">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-233">Name the folder *Models*.</span></span>

<span data-ttu-id="6fb32-234">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-234">Right click the *Models* folder.</span></span> <span data-ttu-id="6fb32-235">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="6fb32-236">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6fb32-238">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6fb32-239">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-240">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6fb32-241">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="6fb32-242">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="6fb32-243">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="6fb32-244">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="6fb32-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="6fb32-245">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="6fb32-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="6fb32-246">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="6fb32-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="6fb32-247">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="6fb32-248">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="6fb32-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6fb32-249">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="6fb32-249">Scaffold the movie model</span></span>

<span data-ttu-id="6fb32-250">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="6fb32-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6fb32-251">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="6fb32-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-253">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="6fb32-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="6fb32-254">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="6fb32-255">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-255">Name the folder *Movies*</span></span>

<span data-ttu-id="6fb32-256">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="6fb32-258">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="6fb32-260">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="6fb32-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="6fb32-261">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="6fb32-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="6fb32-262">En la fila **Clase de contexto de datos**, seleccione el signo más **+** , inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="6fb32-263">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-263">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="6fb32-265">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="6fb32-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6fb32-267">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="6fb32-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6fb32-268">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="6fb32-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6fb32-269">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6fb32-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6fb32-270">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6fb32-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-271">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6fb32-272">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="6fb32-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6fb32-273">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="6fb32-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6fb32-274">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6fb32-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="6fb32-275">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6fb32-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="6fb32-276">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="6fb32-276">Files created</span></span>

* <span data-ttu-id="6fb32-277">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="6fb32-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="6fb32-278">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="6fb32-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="6fb32-279">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="6fb32-279">File updated</span></span>

* <span data-ttu-id="6fb32-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="6fb32-280">*Startup.cs*</span></span>

<span data-ttu-id="6fb32-281">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="6fb32-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6fb32-282">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="6fb32-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fb32-284">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="6fb32-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="6fb32-285">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-285">Add an initial migration.</span></span>
* <span data-ttu-id="6fb32-286">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="6fb32-287">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6fb32-289">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="6fb32-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6fb32-290">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6fb32-291">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6fb32-292">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="6fb32-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="6fb32-293">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="6fb32-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="6fb32-294">Para más información, consulte <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="6fb32-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="6fb32-295">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="6fb32-296">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-298">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="6fb32-299">Los comandos anteriores generan la advertencia siguiente: *No type was specified for the decimal column "Price" on entity type "Movie" (No se ha especificado ningún tipo en la columna decimal "Price" en el tipo de entidad "Movie"). This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using "HasColumnType()" (Especifique de forma explícita el tipo de columna de SQL Server que pueda acomodar todos los valores mediante "HasColumnType()").* Esta advertencia se puede omitir, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="6fb32-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6fb32-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb32-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6fb32-301">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="6fb32-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6fb32-302">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6fb32-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6fb32-303">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6fb32-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6fb32-304">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="6fb32-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6fb32-305">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="6fb32-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6fb32-306">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6fb32-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="6fb32-307">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6fb32-308">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="6fb32-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="6fb32-309">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6fb32-310">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6fb32-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6fb32-311">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6fb32-312">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6fb32-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6fb32-313">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6fb32-314">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="6fb32-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6fb32-315">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="6fb32-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6fb32-316">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6fb32-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6fb32-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6fb32-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6fb32-318">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6fb32-319">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6fb32-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6fb32-320">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6fb32-321">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6fb32-321">Test the app</span></span>

* <span data-ttu-id="6fb32-322">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="6fb32-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6fb32-323">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="6fb32-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6fb32-324">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6fb32-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6fb32-325">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-325">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="6fb32-327">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="6fb32-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6fb32-328">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="6fb32-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6fb32-329">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6fb32-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6fb32-330">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="6fb32-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6fb32-331">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6fb32-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fb32-332">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6fb32-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6fb32-333">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6fb32-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
