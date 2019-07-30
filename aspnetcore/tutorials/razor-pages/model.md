---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: b7f77cfa51f8d86504939e31eade0dfda8a6b1c9
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371908"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="e5a4b-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5a4b-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="e5a4b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5a4b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e5a4b-105">En esta sección, se agregan clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="e5a4b-106">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="e5a4b-107">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="e5a4b-108">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="e5a4b-109">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e5a4b-110">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="e5a4b-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5a4b-112">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e5a4b-113">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-113">Name the folder *Models*.</span></span>

<span data-ttu-id="e5a4b-114">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-114">Right click the *Models* folder.</span></span> <span data-ttu-id="e5a4b-115">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="e5a4b-116">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e5a4b-118">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="e5a4b-119">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-120">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e5a4b-121">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="e5a4b-122">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="e5a4b-123">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="e5a4b-124">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="e5a4b-125">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="e5a4b-126">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="e5a4b-127">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="e5a4b-128">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e5a4b-129">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="e5a4b-129">Scaffold the movie model</span></span>

<span data-ttu-id="e5a4b-130">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="e5a4b-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e5a4b-131">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5a4b-133">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="e5a4b-134">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="e5a4b-135">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-135">Name the folder *Movies*</span></span>

<span data-ttu-id="e5a4b-136">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="e5a4b-138">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="e5a4b-140">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="e5a4b-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="e5a4b-141">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="e5a4b-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="e5a4b-142">En la fila **Clase de contexto de datos**, seleccione el signo **+** (más) y cambie el nombre generado de RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="e5a4b-143">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="e5a4b-144">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="e5a4b-145">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-145">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/3/arp.png)

<span data-ttu-id="e5a4b-147">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="e5a4b-149">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e5a4b-150">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e5a4b-151">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="e5a4b-152">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-153">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e5a4b-154">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e5a4b-155">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e5a4b-156">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="e5a4b-157">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="e5a4b-158">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="e5a4b-158">Files created</span></span>

* <span data-ttu-id="e5a4b-159">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="e5a4b-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="e5a4b-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="e5a4b-161">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="e5a4b-161">File updated</span></span>

* <span data-ttu-id="e5a4b-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="e5a4b-162">*Startup.cs*</span></span>

<span data-ttu-id="e5a4b-163">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="e5a4b-164">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="e5a4b-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5a4b-166">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="e5a4b-167">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-167">Add an initial migration.</span></span>
* <span data-ttu-id="e5a4b-168">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="e5a4b-169">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e5a4b-171">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-173">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="e5a4b-174">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="e5a4b-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="e5a4b-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="e5a4b-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="e5a4b-177">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="e5a4b-178">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e5a4b-179">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e5a4b-180">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="e5a4b-181">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="e5a4b-182">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="e5a4b-183">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e5a4b-185">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="e5a4b-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e5a4b-186">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e5a4b-187">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e5a4b-188">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e5a4b-189">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="e5a4b-190">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="e5a4b-191">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e5a4b-192">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="e5a4b-193">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="e5a4b-194">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e5a4b-195">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="e5a4b-196">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e5a4b-197">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e5a4b-198">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e5a4b-199">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e5a4b-200">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e5a4b-202">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-203">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e5a4b-204">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="e5a4b-205">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e5a4b-206">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e5a4b-207">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e5a4b-208">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="e5a4b-209">Para más información, consulte <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="e5a4b-210">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e5a4b-211">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e5a4b-211">Test the app</span></span>

* <span data-ttu-id="e5a4b-212">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="e5a4b-213">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e5a4b-214">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="e5a4b-215">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-215">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="e5a4b-217">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e5a4b-218">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e5a4b-219">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e5a4b-220">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e5a4b-221">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5a4b-222">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e5a4b-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5a4b-223">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="e5a4b-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e5a4b-224">En esta sección, se agregan clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="e5a4b-225">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="e5a4b-226">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="e5a4b-227">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="e5a4b-228">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e5a4b-229">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="e5a4b-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5a4b-231">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e5a4b-232">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-232">Name the folder *Models*.</span></span>

<span data-ttu-id="e5a4b-233">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-233">Right click the *Models* folder.</span></span> <span data-ttu-id="e5a4b-234">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="e5a4b-235">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e5a4b-237">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="e5a4b-238">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-239">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e5a4b-240">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="e5a4b-241">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="e5a4b-242">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="e5a4b-243">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="e5a4b-244">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="e5a4b-245">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="e5a4b-246">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="e5a4b-247">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e5a4b-248">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="e5a4b-248">Scaffold the movie model</span></span>

<span data-ttu-id="e5a4b-249">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="e5a4b-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e5a4b-250">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5a4b-252">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="e5a4b-253">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="e5a4b-254">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-254">Name the folder *Movies*</span></span>

<span data-ttu-id="e5a4b-255">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="e5a4b-257">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="e5a4b-259">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="e5a4b-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="e5a4b-260">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="e5a4b-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="e5a4b-261">En la fila **Clase de contexto de datos**, seleccione el signo más **+** , inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="e5a4b-262">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-262">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="e5a4b-264">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="e5a4b-266">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e5a4b-267">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e5a4b-268">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="e5a4b-269">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-270">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e5a4b-271">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e5a4b-272">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e5a4b-273">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="e5a4b-274">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="e5a4b-275">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="e5a4b-275">Files created</span></span>

* <span data-ttu-id="e5a4b-276">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="e5a4b-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="e5a4b-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="e5a4b-278">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="e5a4b-278">File updated</span></span>

* <span data-ttu-id="e5a4b-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="e5a4b-279">*Startup.cs*</span></span>

<span data-ttu-id="e5a4b-280">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="e5a4b-281">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="e5a4b-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e5a4b-283">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="e5a4b-284">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-284">Add an initial migration.</span></span>
* <span data-ttu-id="e5a4b-285">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="e5a4b-286">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e5a4b-288">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-290">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="e5a4b-291">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="e5a4b-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="e5a4b-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="e5a4b-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="e5a4b-294">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="e5a4b-295">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e5a4b-296">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e5a4b-297">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="e5a4b-298">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="e5a4b-299">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="e5a4b-300">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e5a4b-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5a4b-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e5a4b-302">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="e5a4b-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e5a4b-303">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e5a4b-304">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e5a4b-305">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e5a4b-306">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="e5a4b-307">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="e5a4b-308">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e5a4b-309">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="e5a4b-310">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="e5a4b-311">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e5a4b-312">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="e5a4b-313">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e5a4b-314">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e5a4b-315">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e5a4b-316">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e5a4b-317">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e5a4b-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e5a4b-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e5a4b-319">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e5a4b-320">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e5a4b-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e5a4b-321">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="e5a4b-322">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e5a4b-323">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e5a4b-324">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e5a4b-325">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="e5a4b-326">Para más información, consulte <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="e5a4b-327">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e5a4b-328">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e5a4b-328">Test the app</span></span>

* <span data-ttu-id="e5a4b-329">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="e5a4b-330">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="e5a4b-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e5a4b-331">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="e5a4b-332">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-332">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="e5a4b-334">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e5a4b-335">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e5a4b-336">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="e5a4b-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e5a4b-337">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e5a4b-338">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e5a4b-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5a4b-339">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e5a4b-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5a4b-340">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="e5a4b-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end