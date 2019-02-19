---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 02/12/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: ab2bf372b305672a95d34879ad2c13a620d93c7a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159293"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="50fac-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50fac-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="50fac-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="50fac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="50fac-105">En esta sección, se agregan clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="50fac-106">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="50fac-107">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="50fac-108">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="50fac-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="50fac-109">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="50fac-110">[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="50fac-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="50fac-111">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="50fac-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="50fac-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50fac-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="50fac-113">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="50fac-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="50fac-114">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="50fac-114">Name the folder *Models*.</span></span>

<span data-ttu-id="50fac-115">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="50fac-115">Right click the *Models* folder.</span></span> <span data-ttu-id="50fac-116">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="50fac-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="50fac-117">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="50fac-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="50fac-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="50fac-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="50fac-119">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="50fac-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="50fac-120">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="50fac-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="50fac-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="50fac-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="50fac-122">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="50fac-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="50fac-123">Asigne un nombre a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="50fac-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="50fac-124">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Archivo nuevo**.</span><span class="sxs-lookup"><span data-stu-id="50fac-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="50fac-125">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="50fac-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="50fac-126">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="50fac-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="50fac-127">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="50fac-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="50fac-128">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="50fac-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="50fac-129">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="50fac-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="50fac-130">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="50fac-130">Scaffold the movie model</span></span>

<span data-ttu-id="50fac-131">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="50fac-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="50fac-132">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="50fac-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="50fac-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50fac-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="50fac-134">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="50fac-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="50fac-135">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="50fac-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="50fac-136">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="50fac-136">Name the folder *Movies*</span></span>

<span data-ttu-id="50fac-137">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="50fac-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="50fac-139">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="50fac-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="50fac-141">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="50fac-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="50fac-142">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="50fac-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="50fac-143">En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="50fac-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="50fac-144">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="50fac-144">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="50fac-146">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="50fac-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="50fac-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="50fac-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="50fac-148">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="50fac-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="50fac-149">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="50fac-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="50fac-150">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="50fac-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="50fac-151">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="50fac-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="50fac-152">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="50fac-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="50fac-153">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="50fac-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="50fac-154">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="50fac-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* <span data-ttu-id="50fac-155">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="50fac-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="50fac-156">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="50fac-156">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="50fac-157">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="50fac-157">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="50fac-158">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="50fac-158">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="50fac-159">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="50fac-159">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="50fac-160">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="50fac-160">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="50fac-161">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="50fac-161">Files created</span></span>

* <span data-ttu-id="50fac-162">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="50fac-162">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="50fac-163">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="50fac-163">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="50fac-164">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="50fac-164">File updated</span></span>

* <span data-ttu-id="50fac-165">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="50fac-165">*Startup.cs*</span></span>

<span data-ttu-id="50fac-166">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="50fac-166">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="50fac-167">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="50fac-167">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="50fac-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50fac-168">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="50fac-169">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="50fac-169">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="50fac-170">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="50fac-170">Add an initial migration.</span></span>
* <span data-ttu-id="50fac-171">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="50fac-171">Update the database with the initial migration.</span></span>

<span data-ttu-id="50fac-172">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="50fac-172">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="50fac-174">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="50fac-174">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="50fac-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="50fac-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="50fac-176">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="50fac-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="50fac-177">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="50fac-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="50fac-178">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="50fac-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="50fac-179">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="50fac-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="50fac-180">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="50fac-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="50fac-181">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="50fac-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="50fac-182">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-182">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="50fac-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50fac-183">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="50fac-184">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="50fac-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="50fac-185">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="50fac-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="50fac-186">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="50fac-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="50fac-187">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="50fac-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="50fac-188">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="50fac-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="50fac-189">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="50fac-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="50fac-190">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="50fac-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="50fac-191">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="50fac-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="50fac-192">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="50fac-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="50fac-193">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="50fac-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="50fac-194">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="50fac-195">El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="50fac-195">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="50fac-196">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="50fac-197">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="50fac-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="50fac-198">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="50fac-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="50fac-199">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="50fac-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="50fac-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="50fac-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="50fac-201">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="50fac-201">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="50fac-202">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="50fac-202">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="50fac-203">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="50fac-203">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="50fac-204">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="50fac-204">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="50fac-205">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="50fac-205">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="50fac-206">Para obtener más información, vea <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="50fac-206">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="50fac-207">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="50fac-207">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="50fac-208">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="50fac-208">Test the app</span></span>

* <span data-ttu-id="50fac-209">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="50fac-209">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="50fac-210">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="50fac-210">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="50fac-211">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="50fac-211">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="50fac-212">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="50fac-212">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="50fac-214">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="50fac-214">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="50fac-215">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="50fac-215">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="50fac-216">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="50fac-216">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="50fac-217">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="50fac-217">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="50fac-218">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="50fac-218">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="50fac-219">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="50fac-219">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
