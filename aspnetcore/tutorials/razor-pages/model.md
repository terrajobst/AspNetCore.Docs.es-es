---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0934c94236b507f2f57200ded4344a71c483d356
ms.sourcegitcommit: 5fe17e54f7e4267a2fdecc6f9aa1d41166cecc34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737863"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="473be-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="473be-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="473be-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="473be-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="473be-105">En esta sección, se agregan clases para administrar películas en una [base de datos SQLite](https://www.sqlite.org/index.html) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="473be-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="473be-106">Las aplicaciones creadas a partir de una plantilla de ASP.NET Core usan una base de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="473be-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="473be-107">Las clases de modelo de la aplicación se usan con [Entity Framework Core (EF Core)](/ef/core) ([Proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite)) para trabajar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="473be-108">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="473be-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="473be-109">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="473be-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="473be-110">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="473be-111">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="473be-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-113">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="473be-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="473be-114">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-114">Name the folder *Models*.</span></span>

<span data-ttu-id="473be-115">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-115">Right click the *Models* folder.</span></span> <span data-ttu-id="473be-116">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="473be-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="473be-117">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="473be-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="473be-119">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="473be-120">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="473be-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="473be-122">En el Panel de solución, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Nueva carpeta...**. Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="473be-123">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo...**.</span><span class="sxs-lookup"><span data-stu-id="473be-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="473be-124">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="473be-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="473be-125">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="473be-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="473be-126">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="473be-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="473be-127">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="473be-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="473be-128">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="473be-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="473be-129">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="473be-129">Scaffold the movie model</span></span>

<span data-ttu-id="473be-130">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="473be-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="473be-131">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="473be-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-133">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="473be-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="473be-134">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="473be-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="473be-135">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="473be-135">Name the folder *Movies*</span></span>

<span data-ttu-id="473be-136">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="473be-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="473be-138">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="473be-140">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="473be-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="473be-141">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="473be-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="473be-142">En la fila **Clase de contexto de datos**, seleccione el signo **+** (más) y cambie el nombre generado de RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="473be-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="473be-143">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="473be-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="473be-144">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="473be-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="473be-145">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-145">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/3/arp.png)

<span data-ttu-id="473be-147">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="473be-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="473be-149">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="473be-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="473be-150">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="473be-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="473be-151">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="473be-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="473be-152">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="473be-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-153">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="473be-154">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="473be-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="473be-155">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="473be-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="473be-156">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="473be-156">Name the folder *Movies*</span></span>

<span data-ttu-id="473be-157">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo scaffolding...**.</span><span class="sxs-lookup"><span data-stu-id="473be-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/scaMac.png)

<span data-ttu-id="473be-159">En el cuadro de diálogo **Nuevo scaffolding**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="473be-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="473be-161">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="473be-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="473be-162">En la lista desplegable **Clase de modelo**, seleccione o escriba **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="473be-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="473be-163">En la fila **Clase de contexto de datos**, escriba el nombre de la nueva clase, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="473be-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="473be-164">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="473be-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="473be-165">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="473be-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="473be-166">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-166">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arpMac.png)

<span data-ttu-id="473be-168">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="473be-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="473be-169">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="473be-169">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-171">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="473be-171">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="473be-172">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="473be-172">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="473be-173">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="473be-173">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="473be-174">Actualizado</span><span class="sxs-lookup"><span data-stu-id="473be-174">Updated</span></span>

* <span data-ttu-id="473be-175">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="473be-175">*Startup.cs*</span></span>

<span data-ttu-id="473be-176">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="473be-176">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-177">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="473be-178">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="473be-178">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="473be-179">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="473be-179">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="473be-180">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="473be-180">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="473be-181">Actualizado</span><span class="sxs-lookup"><span data-stu-id="473be-181">Updated</span></span>

* <span data-ttu-id="473be-182">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="473be-182">*Startup.cs*</span></span>

<span data-ttu-id="473be-183">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="473be-183">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-184">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-184">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="473be-185">El proceso de scaffolding crea los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="473be-185">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="473be-186">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="473be-186">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="473be-187">Los archivos creados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="473be-187">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="473be-188">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="473be-188">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-190">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="473be-190">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="473be-191">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="473be-191">Add an initial migration.</span></span>
* <span data-ttu-id="473be-192">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="473be-192">Update the database with the initial migration.</span></span>

<span data-ttu-id="473be-193">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="473be-193">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="473be-195">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="473be-195">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-197">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="473be-198">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="473be-198">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="473be-199">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="473be-199">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="473be-200">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="473be-200">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="473be-201">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="473be-201">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="473be-202">El comando migrations genera código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="473be-202">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="473be-203">El esquema se basa en el modelo especificado en `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="473be-203">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="473be-204">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="473be-204">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="473be-205">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="473be-205">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="473be-206">El comando `update` ejecuta el método `Up` en las migraciones que no se han aplicado.</span><span class="sxs-lookup"><span data-stu-id="473be-206">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="473be-207">En este caso, `update` ejecuta el método `Up` en *Migrations/\<time-stamp>_InitialCreate.cs*, que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-207">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-208">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="473be-209">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="473be-209">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="473be-210">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="473be-210">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="473be-211">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="473be-211">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="473be-212">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="473be-212">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="473be-213">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="473be-213">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="473be-214">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="473be-214">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="473be-215">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="473be-215">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="473be-216">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="473be-216">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="473be-217">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="473be-217">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="473be-218">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="473be-218">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="473be-219">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-219">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="473be-220">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="473be-220">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="473be-221">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-221">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="473be-222">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="473be-222">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="473be-223">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="473be-223">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="473be-224">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="473be-224">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-225">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-225">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="473be-226">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="473be-226">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-227">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="473be-228">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="473be-228">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="473be-229">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="473be-229">Test the app</span></span>

* <span data-ttu-id="473be-230">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="473be-230">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="473be-231">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="473be-231">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="473be-232">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="473be-232">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="473be-233">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="473be-233">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="473be-235">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="473be-235">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="473be-236">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="473be-236">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="473be-237">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="473be-237">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="473be-238">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="473be-238">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="473be-239">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="473be-239">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="473be-240">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="473be-240">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="473be-241">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="473be-241">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="473be-242">En esta sección, se agregan clases para administrar películas en una [base de datos SQLite](https://www.sqlite.org/index.html) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="473be-242">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="473be-243">Las aplicaciones creadas a partir de una plantilla de ASP.NET Core usan una base de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="473be-243">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="473be-244">Las clases de modelo de la aplicación se usan con [Entity Framework Core (EF Core)](/ef/core) ([Proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite)) para trabajar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-244">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="473be-245">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="473be-245">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="473be-246">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="473be-246">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="473be-247">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-247">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="473be-248">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="473be-248">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-249">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-249">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-250">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="473be-250">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="473be-251">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-251">Name the folder *Models*.</span></span>

<span data-ttu-id="473be-252">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-252">Right click the *Models* folder.</span></span> <span data-ttu-id="473be-253">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="473be-253">Select **Add** > **Class**.</span></span> <span data-ttu-id="473be-254">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="473be-254">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-255">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-255">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="473be-256">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-256">Add a folder named *Models*.</span></span>
* <span data-ttu-id="473be-257">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="473be-257">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-258">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-258">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="473be-259">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="473be-259">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="473be-260">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="473be-260">Name the folder *Models*.</span></span>
* <span data-ttu-id="473be-261">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="473be-261">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="473be-262">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="473be-262">In the **New File** dialog:</span></span>

  * <span data-ttu-id="473be-263">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="473be-263">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="473be-264">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="473be-264">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="473be-265">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="473be-265">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="473be-266">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="473be-266">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="473be-267">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="473be-267">Scaffold the movie model</span></span>

<span data-ttu-id="473be-268">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="473be-268">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="473be-269">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="473be-269">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-270">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-270">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-271">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="473be-271">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="473be-272">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="473be-272">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="473be-273">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="473be-273">Name the folder *Movies*</span></span>

<span data-ttu-id="473be-274">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="473be-274">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="473be-276">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-276">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="473be-278">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="473be-278">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="473be-279">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="473be-279">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="473be-280">En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="473be-280">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="473be-281">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-281">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="473be-283">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="473be-283">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-284">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-284">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="473be-285">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="473be-285">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="473be-286">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="473be-286">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="473be-287">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="473be-287">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-288">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-288">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="473be-289">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="473be-289">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="473be-290">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="473be-290">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="473be-291">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="473be-291">Name the folder *Movies*</span></span>

<span data-ttu-id="473be-292">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="473be-292">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/scaMac.png)

<span data-ttu-id="473be-294">En el cuadro de diálogo **Agregar nuevos scaffolding**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-294">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="473be-296">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="473be-296">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="473be-297">En la lista desplegable **Clase de modelo**, seleccione o escriba **Movie**.</span><span class="sxs-lookup"><span data-stu-id="473be-297">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="473be-298">En la fila **Clase de contexto de datos**, escriba o seleccione **RazorPagesMovieContext**. Se creará una nueva clase de contexto de la base de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="473be-298">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="473be-299">En este caso, será **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="473be-299">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="473be-300">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="473be-300">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arpMac.png)

<span data-ttu-id="473be-302">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="473be-302">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="473be-303">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="473be-303">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="473be-304">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="473be-304">Files created</span></span>

* <span data-ttu-id="473be-305">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="473be-305">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="473be-306">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="473be-306">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="473be-307">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="473be-307">File updated</span></span>

* <span data-ttu-id="473be-308">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="473be-308">*Startup.cs*</span></span>

<span data-ttu-id="473be-309">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="473be-309">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="473be-310">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="473be-310">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="473be-312">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="473be-312">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="473be-313">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="473be-313">Add an initial migration.</span></span>
* <span data-ttu-id="473be-314">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="473be-314">Update the database with the initial migration.</span></span>

<span data-ttu-id="473be-315">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="473be-315">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="473be-317">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="473be-317">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="473be-318">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="473be-318">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="473be-319">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="473be-319">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="473be-320">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="473be-320">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="473be-321">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="473be-321">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="473be-322">Para obtener más información, vea <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="473be-322">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="473be-323">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="473be-323">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="473be-324">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-324">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-326">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-326">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="473be-327">Los comandos anteriores generan la advertencia siguiente: *No type was specified for the decimal column "Price" on entity type "Movie" (No se ha especificado ningún tipo en la columna decimal "Price" en el tipo de entidad "Movie"). This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using "HasColumnType()" (Especifique de forma explícita el tipo de columna de SQL Server que pueda acomodar todos los valores mediante "HasColumnType()").* Esta advertencia se puede omitir, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="473be-327">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="473be-328">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="473be-328">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="473be-329">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="473be-329">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="473be-330">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="473be-330">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="473be-331">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="473be-331">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="473be-332">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="473be-332">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="473be-333">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="473be-333">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="473be-334">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="473be-334">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="473be-335">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="473be-335">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="473be-336">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="473be-336">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="473be-337">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="473be-337">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="473be-338">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="473be-338">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="473be-339">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-339">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="473be-340">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="473be-340">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="473be-341">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="473be-341">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="473be-342">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="473be-342">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="473be-343">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="473be-343">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="473be-344">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="473be-344">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="473be-345">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="473be-345">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="473be-346">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="473be-346">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="473be-347">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="473be-347">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="473be-348">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="473be-348">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="473be-349">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="473be-349">Test the app</span></span>

* <span data-ttu-id="473be-350">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="473be-350">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="473be-351">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="473be-351">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="473be-352">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="473be-352">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="473be-353">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="473be-353">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="473be-355">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="473be-355">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="473be-356">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="473be-356">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="473be-357">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="473be-357">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="473be-358">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="473be-358">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="473be-359">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="473be-359">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="473be-360">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="473be-360">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="473be-361">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="473be-361">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
