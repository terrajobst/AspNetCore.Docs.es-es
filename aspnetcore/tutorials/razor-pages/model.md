---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 39e2a38e0b91b7dbecf05c084ca0be5e312dcb0d
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862868"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="8ff5c-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ff5c-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="8ff5c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ff5c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8ff5c-105">En esta sección, se agregan clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="8ff5c-106">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="8ff5c-107">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="8ff5c-108">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="8ff5c-109">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="8ff5c-110">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="8ff5c-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-112">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="8ff5c-113">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-113">Name the folder *Models*.</span></span>

<span data-ttu-id="8ff5c-114">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-114">Right click the *Models* folder.</span></span> <span data-ttu-id="8ff5c-115">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="8ff5c-116">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8ff5c-118">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="8ff5c-119">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-120">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8ff5c-121">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="8ff5c-122">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="8ff5c-123">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="8ff5c-124">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="8ff5c-125">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="8ff5c-126">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="8ff5c-127">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="8ff5c-128">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="8ff5c-129">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="8ff5c-129">Scaffold the movie model</span></span>

<span data-ttu-id="8ff5c-130">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="8ff5c-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="8ff5c-131">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-133">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="8ff5c-134">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="8ff5c-135">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-135">Name the folder *Movies*</span></span>

<span data-ttu-id="8ff5c-136">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="8ff5c-138">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="8ff5c-140">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="8ff5c-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="8ff5c-141">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="8ff5c-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="8ff5c-142">En la fila **Clase de contexto de datos**, seleccione el signo **+** (más) y cambie el nombre generado de RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="8ff5c-143">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="8ff5c-144">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="8ff5c-145">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-145">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/3/arp.png)

<span data-ttu-id="8ff5c-147">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="8ff5c-149">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8ff5c-150">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="8ff5c-151">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="8ff5c-152">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-153">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8ff5c-154">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8ff5c-155">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="8ff5c-156">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="8ff5c-157">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="8ff5c-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-159">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="8ff5c-160">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="8ff5c-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="8ff5c-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="8ff5c-162">Actualizado</span><span class="sxs-lookup"><span data-stu-id="8ff5c-162">Updated</span></span>

* <span data-ttu-id="8ff5c-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="8ff5c-163">*Startup.cs*</span></span>

<span data-ttu-id="8ff5c-164">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="8ff5c-165">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="8ff5c-166">El proceso de scaffolding crea los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="8ff5c-167">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="8ff5c-168">Los archivos creados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="8ff5c-169">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="8ff5c-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-171">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="8ff5c-172">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-172">Add an initial migration.</span></span>
* <span data-ttu-id="8ff5c-173">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="8ff5c-174">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="8ff5c-176">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-178">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="8ff5c-179">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="8ff5c-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="8ff5c-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="8ff5c-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="8ff5c-182">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="8ff5c-183">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8ff5c-184">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="8ff5c-185">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="8ff5c-186">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="8ff5c-187">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="8ff5c-188">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="8ff5c-190">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8ff5c-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="8ff5c-191">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8ff5c-192">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="8ff5c-193">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="8ff5c-194">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="8ff5c-195">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="8ff5c-196">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8ff5c-197">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="8ff5c-198">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="8ff5c-199">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="8ff5c-200">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="8ff5c-201">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="8ff5c-202">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="8ff5c-203">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="8ff5c-204">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="8ff5c-205">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8ff5c-207">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-208">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8ff5c-209">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="8ff5c-210">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8ff5c-211">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="8ff5c-212">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="8ff5c-213">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="8ff5c-214">Para más información, consulte <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="8ff5c-215">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="8ff5c-216">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="8ff5c-216">Test the app</span></span>

* <span data-ttu-id="8ff5c-217">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="8ff5c-218">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="8ff5c-219">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="8ff5c-220">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-220">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="8ff5c-222">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8ff5c-223">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="8ff5c-224">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="8ff5c-225">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="8ff5c-226">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ff5c-227">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8ff5c-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ff5c-228">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="8ff5c-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8ff5c-229">En esta sección, se agregan clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="8ff5c-230">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="8ff5c-231">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="8ff5c-232">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="8ff5c-233">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="8ff5c-234">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="8ff5c-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-236">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="8ff5c-237">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-237">Name the folder *Models*.</span></span>

<span data-ttu-id="8ff5c-238">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-238">Right click the *Models* folder.</span></span> <span data-ttu-id="8ff5c-239">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="8ff5c-240">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8ff5c-242">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="8ff5c-243">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-244">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8ff5c-245">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="8ff5c-246">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="8ff5c-247">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="8ff5c-248">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="8ff5c-249">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="8ff5c-250">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="8ff5c-251">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="8ff5c-252">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="8ff5c-253">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="8ff5c-253">Scaffold the movie model</span></span>

<span data-ttu-id="8ff5c-254">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="8ff5c-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="8ff5c-255">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-257">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="8ff5c-258">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="8ff5c-259">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-259">Name the folder *Movies*</span></span>

<span data-ttu-id="8ff5c-260">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="8ff5c-262">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="8ff5c-264">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="8ff5c-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="8ff5c-265">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="8ff5c-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="8ff5c-266">En la fila **Clase de contexto de datos**, seleccione el signo más **+** , inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="8ff5c-267">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-267">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="8ff5c-269">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="8ff5c-271">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8ff5c-272">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="8ff5c-273">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-273">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="8ff5c-274">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-274">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-275">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8ff5c-276">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8ff5c-277">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-277">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="8ff5c-278">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-278">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="8ff5c-279">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="8ff5c-280">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="8ff5c-280">Files created</span></span>

* <span data-ttu-id="8ff5c-281">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="8ff5c-282">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="8ff5c-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="8ff5c-283">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="8ff5c-283">File updated</span></span>

* <span data-ttu-id="8ff5c-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="8ff5c-284">*Startup.cs*</span></span>

<span data-ttu-id="8ff5c-285">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="8ff5c-286">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="8ff5c-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ff5c-288">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="8ff5c-289">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-289">Add an initial migration.</span></span>
* <span data-ttu-id="8ff5c-290">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="8ff5c-291">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="8ff5c-293">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-293">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-295">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="8ff5c-296">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="8ff5c-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="8ff5c-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="8ff5c-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="8ff5c-299">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="8ff5c-300">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8ff5c-301">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="8ff5c-302">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="8ff5c-303">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="8ff5c-304">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="8ff5c-305">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ff5c-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ff5c-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="8ff5c-307">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8ff5c-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="8ff5c-308">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8ff5c-309">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="8ff5c-310">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="8ff5c-311">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="8ff5c-312">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="8ff5c-313">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8ff5c-314">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="8ff5c-315">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="8ff5c-316">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="8ff5c-317">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="8ff5c-318">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="8ff5c-319">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="8ff5c-320">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="8ff5c-321">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="8ff5c-322">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ff5c-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ff5c-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8ff5c-324">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ff5c-325">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ff5c-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8ff5c-326">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="8ff5c-327">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8ff5c-328">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="8ff5c-329">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="8ff5c-330">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="8ff5c-331">Para más información, consulte <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="8ff5c-332">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="8ff5c-333">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="8ff5c-333">Test the app</span></span>

* <span data-ttu-id="8ff5c-334">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="8ff5c-335">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="8ff5c-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="8ff5c-336">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="8ff5c-337">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-337">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="8ff5c-339">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8ff5c-340">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="8ff5c-341">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="8ff5c-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="8ff5c-342">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="8ff5c-343">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8ff5c-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ff5c-344">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8ff5c-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ff5c-345">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="8ff5c-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
