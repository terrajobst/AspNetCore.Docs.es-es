---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0915c525d5fb96a3d32f91fbd65a4e1f62ee28b8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577869"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="fae90-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fae90-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="fae90-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fae90-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="fae90-105">En esta sección, se agregan clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="fae90-106">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="fae90-107">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="fae90-108">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="fae90-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="fae90-109">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="fae90-110">[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="fae90-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="fae90-111">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="fae90-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fae90-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fae90-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fae90-113">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="fae90-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="fae90-114">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="fae90-114">Name the folder *Models*.</span></span>

<span data-ttu-id="fae90-115">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="fae90-115">Right click the *Models* folder.</span></span> <span data-ttu-id="fae90-116">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="fae90-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="fae90-117">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="fae90-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fae90-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fae90-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fae90-119">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="fae90-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fae90-120">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="fae90-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fae90-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="fae90-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fae90-122">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="fae90-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fae90-123">Asigne un nombre a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="fae90-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="fae90-124">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Archivo nuevo**.</span><span class="sxs-lookup"><span data-stu-id="fae90-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fae90-125">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="fae90-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fae90-126">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="fae90-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fae90-127">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="fae90-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="fae90-128">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="fae90-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="fae90-129">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="fae90-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="fae90-130">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="fae90-130">Scaffold the movie model</span></span>

<span data-ttu-id="fae90-131">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="fae90-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="fae90-132">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="fae90-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fae90-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fae90-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fae90-134">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="fae90-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="fae90-135">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="fae90-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="fae90-136">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="fae90-136">Name the folder *Movies*</span></span>

<span data-ttu-id="fae90-137">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="fae90-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="fae90-139">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="fae90-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="fae90-141">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="fae90-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="fae90-142">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="fae90-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="fae90-143">En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="fae90-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="fae90-144">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="fae90-144">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="fae90-146">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="fae90-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fae90-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fae90-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="fae90-148">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fae90-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fae90-149">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="fae90-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fae90-150">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="fae90-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fae90-151">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="fae90-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fae90-152">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="fae90-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fae90-153">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fae90-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fae90-154">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="fae90-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* <span data-ttu-id="fae90-155">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="fae90-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="fae90-156">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="fae90-156">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="fae90-157">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="fae90-157">Files created</span></span>

* <span data-ttu-id="fae90-158">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="fae90-158">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="fae90-159">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="fae90-159">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="fae90-160">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="fae90-160">File updated</span></span>

* <span data-ttu-id="fae90-161">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="fae90-161">*Startup.cs*</span></span>

<span data-ttu-id="fae90-162">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="fae90-162">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="fae90-163">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="fae90-163">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fae90-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fae90-164">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="fae90-165">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="fae90-165">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="fae90-166">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="fae90-166">Add an initial migration.</span></span>
* <span data-ttu-id="fae90-167">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="fae90-167">Update the database with the initial migration.</span></span>

<span data-ttu-id="fae90-168">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="fae90-168">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="fae90-170">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="fae90-170">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fae90-171">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fae90-171">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fae90-172">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="fae90-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="fae90-173">El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="fae90-173">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fae90-174">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="fae90-174">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fae90-175">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="fae90-175">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="fae90-176">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="fae90-176">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="fae90-177">El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="fae90-177">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="fae90-178">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-178">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fae90-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fae90-179">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="fae90-180">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="fae90-180">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="fae90-181">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fae90-181">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fae90-182">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fae90-182">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="fae90-183">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="fae90-183">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="fae90-184">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="fae90-184">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="fae90-185">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="fae90-185">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="fae90-186">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fae90-186">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fae90-187">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="fae90-187">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="fae90-188">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fae90-188">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="fae90-189">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="fae90-189">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="fae90-190">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-190">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="fae90-191">En el código anterior se crea una propiedad [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="fae90-191">The preceding code creates a [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="fae90-192">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-192">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="fae90-193">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="fae90-193">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="fae90-194">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="fae90-194">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="fae90-195">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fae90-195">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fae90-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fae90-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fae90-197">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="fae90-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="fae90-198">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="fae90-198">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fae90-199">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fae90-199">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fae90-200">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="fae90-200">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="fae90-201">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="fae90-201">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="fae90-202">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="fae90-202">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="fae90-203">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fae90-203">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="fae90-204">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="fae90-204">Test the app</span></span>

* <span data-ttu-id="fae90-205">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="fae90-205">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="fae90-206">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="fae90-206">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="fae90-207">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="fae90-207">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="fae90-208">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="fae90-208">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="fae90-210">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="fae90-210">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fae90-211">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="fae90-211">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="fae90-212">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="fae90-212">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="fae90-213">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="fae90-213">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="fae90-214">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="fae90-214">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fae90-215">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="fae90-215">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
