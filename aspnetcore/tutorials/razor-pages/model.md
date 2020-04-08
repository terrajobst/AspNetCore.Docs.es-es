---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78647177"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="dc939-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc939-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="dc939-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc939-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="dc939-105">En esta sección, se agregan clases para administrar películas en una [base de datos SQLite](https://www.sqlite.org/index.html) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="dc939-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="dc939-106">Las aplicaciones creadas a partir de una plantilla de ASP.NET Core usan una base de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="dc939-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="dc939-107">Las clases de modelo de la aplicación se usan con [Entity Framework Core (EF Core)](/ef/core) ([Proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite)) para trabajar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="dc939-108">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="dc939-109">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc939-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="dc939-110">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="dc939-111">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="dc939-111">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-113">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="dc939-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="dc939-114">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-114">Name the folder *Models*.</span></span>

<span data-ttu-id="dc939-115">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-115">Right click the *Models* folder.</span></span> <span data-ttu-id="dc939-116">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="dc939-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="dc939-117">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="dc939-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dc939-119">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="dc939-120">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc939-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dc939-122">En el Panel de solución, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Nueva carpeta...** . Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="dc939-123">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo...** .</span><span class="sxs-lookup"><span data-stu-id="dc939-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="dc939-124">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="dc939-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="dc939-125">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="dc939-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="dc939-126">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="dc939-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="dc939-127">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="dc939-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="dc939-128">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="dc939-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="dc939-129">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="dc939-129">Scaffold the movie model</span></span>

<span data-ttu-id="dc939-130">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="dc939-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="dc939-131">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="dc939-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-133">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="dc939-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="dc939-134">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="dc939-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="dc939-135">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="dc939-135">Name the folder *Movies*</span></span>

<span data-ttu-id="dc939-136">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="dc939-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="dc939-138">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="dc939-140">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="dc939-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="dc939-141">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="dc939-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="dc939-142">En la fila **Clase de contexto de datos**, seleccione el signo **+** (más) y cambie el nombre generado de RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="dc939-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="dc939-143">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="dc939-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="dc939-144">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="dc939-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="dc939-145">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-145">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/3/arp.png)

<span data-ttu-id="dc939-147">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="dc939-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="dc939-149">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="dc939-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="dc939-150">Instale la herramienta de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="dc939-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="dc939-151">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="dc939-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="dc939-152">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="dc939-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-153">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc939-154">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="dc939-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="dc939-155">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="dc939-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="dc939-156">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="dc939-156">Name the folder *Movies*</span></span>

<span data-ttu-id="dc939-157">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo scaffolding...** .</span><span class="sxs-lookup"><span data-stu-id="dc939-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/scaMac.png)

<span data-ttu-id="dc939-159">En el cuadro de diálogo **Nuevo scaffolding**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="dc939-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="dc939-161">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="dc939-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="dc939-162">En la lista desplegable **Clase de modelo**, seleccione o escriba **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="dc939-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="dc939-163">En la fila **Clase de contexto de datos**, escriba el nombre de la nueva clase, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="dc939-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="dc939-164">[Este cambio](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) no es necesario.</span><span class="sxs-lookup"><span data-stu-id="dc939-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="dc939-165">Crea la clase de contexto de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="dc939-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="dc939-166">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-166">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arpMac.png)

<span data-ttu-id="dc939-168">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="dc939-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="dc939-169">Incorporación de herramientas de EF</span><span class="sxs-lookup"><span data-stu-id="dc939-169">Add EF tools</span></span>

<span data-ttu-id="dc939-170">Ejecute el siguiente comando de la CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="dc939-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="dc939-171">El comando anterior incorpora las herramientas de Entity Framework Core para la CLI de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc939-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="dc939-172">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="dc939-172">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-174">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="dc939-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="dc939-175">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="dc939-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="dc939-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="dc939-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="dc939-177">Actualizado</span><span class="sxs-lookup"><span data-stu-id="dc939-177">Updated</span></span>

* <span data-ttu-id="dc939-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="dc939-178">*Startup.cs*</span></span>

<span data-ttu-id="dc939-179">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="dc939-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-180">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc939-181">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="dc939-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="dc939-182">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="dc939-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="dc939-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="dc939-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="dc939-184">Actualizado</span><span class="sxs-lookup"><span data-stu-id="dc939-184">Updated</span></span>

* <span data-ttu-id="dc939-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="dc939-185">*Startup.cs*</span></span>

<span data-ttu-id="dc939-186">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="dc939-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc939-188">El proceso de scaffolding crea los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="dc939-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="dc939-189">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="dc939-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="dc939-190">Los archivos creados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="dc939-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="dc939-191">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="dc939-191">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-193">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="dc939-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="dc939-194">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="dc939-194">Add an initial migration.</span></span>
* <span data-ttu-id="dc939-195">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="dc939-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="dc939-196">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="dc939-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="dc939-198">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="dc939-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-200">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="dc939-201">Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="dc939-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="dc939-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span><span class="sxs-lookup"><span data-stu-id="dc939-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="dc939-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")</span><span class="sxs-lookup"><span data-stu-id="dc939-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="dc939-204">Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="dc939-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="dc939-205">El comando migrations genera código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="dc939-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="dc939-206">El esquema se basa en el modelo especificado en `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="dc939-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="dc939-207">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="dc939-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="dc939-208">Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="dc939-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="dc939-209">El comando `update` ejecuta el método `Up` en las migraciones que no se han aplicado.</span><span class="sxs-lookup"><span data-stu-id="dc939-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="dc939-210">En este caso, `update` ejecuta el método `Up` en *Migrations/\<time-stamp>_InitialCreate.cs*, que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="dc939-212">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="dc939-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="dc939-213">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dc939-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="dc939-214">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dc939-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="dc939-215">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="dc939-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="dc939-216">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="dc939-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="dc939-217">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="dc939-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="dc939-218">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dc939-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="dc939-219">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="dc939-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="dc939-220">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="dc939-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="dc939-221">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="dc939-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="dc939-222">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="dc939-223">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="dc939-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="dc939-224">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="dc939-225">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="dc939-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="dc939-226">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="dc939-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="dc939-227">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dc939-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc939-229">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="dc939-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-230">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc939-231">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="dc939-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="dc939-232">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="dc939-232">Test the app</span></span>

* <span data-ttu-id="dc939-233">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="dc939-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="dc939-234">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="dc939-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="dc939-235">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="dc939-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="dc939-236">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="dc939-236">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="dc939-238">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="dc939-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="dc939-239">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="dc939-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="dc939-240">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="dc939-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="dc939-241">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="dc939-242">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="dc939-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc939-243">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dc939-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc939-244">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="dc939-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dc939-245">En esta sección, se agregan clases para administrar películas en una [base de datos SQLite](https://www.sqlite.org/index.html) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="dc939-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="dc939-246">Las aplicaciones creadas a partir de una plantilla de ASP.NET Core usan una base de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="dc939-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="dc939-247">Las clases de modelo de la aplicación se usan con [Entity Framework Core (EF Core)](/ef/core) ([Proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite)) para trabajar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="dc939-248">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="dc939-249">Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc939-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="dc939-250">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="dc939-251">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="dc939-251">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-253">Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="dc939-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="dc939-254">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-254">Name the folder *Models*.</span></span>

<span data-ttu-id="dc939-255">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-255">Right click the *Models* folder.</span></span> <span data-ttu-id="dc939-256">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="dc939-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="dc939-257">Asigne a la clase el nombre **Película**.</span><span class="sxs-lookup"><span data-stu-id="dc939-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dc939-259">Agregue una carpeta denominada *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="dc939-260">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc939-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-261">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dc939-262">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="dc939-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="dc939-263">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="dc939-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="dc939-264">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="dc939-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="dc939-265">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="dc939-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="dc939-266">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="dc939-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="dc939-267">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="dc939-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="dc939-268">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="dc939-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="dc939-269">Compile el proyecto para comprobar que no haya errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="dc939-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="dc939-270">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="dc939-270">Scaffold the movie model</span></span>

<span data-ttu-id="dc939-271">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="dc939-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="dc939-272">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="dc939-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-274">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="dc939-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="dc939-275">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="dc939-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="dc939-276">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="dc939-276">Name the folder *Movies*</span></span>

<span data-ttu-id="dc939-277">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="dc939-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="dc939-279">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="dc939-281">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="dc939-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="dc939-282">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="dc939-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="dc939-283">En la fila **Clase de contexto de datos**, seleccione el signo más **+** , inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="dc939-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="dc939-284">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-284">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="dc939-286">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="dc939-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="dc939-288">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="dc939-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="dc939-289">**En Windows**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="dc939-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="dc939-290">**En macOS y Linux**: Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="dc939-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-291">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc939-292">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="dc939-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="dc939-293">Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="dc939-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="dc939-294">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="dc939-294">Name the folder *Movies*</span></span>

<span data-ttu-id="dc939-295">Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="dc939-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/scaMac.png)

<span data-ttu-id="dc939-297">En el cuadro de diálogo **Agregar nuevos scaffolding**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="dc939-299">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="dc939-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="dc939-300">En la lista desplegable **Clase de modelo**, seleccione o escriba **Movie**.</span><span class="sxs-lookup"><span data-stu-id="dc939-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="dc939-301">En la fila **Clase de contexto de datos**, escriba o seleccione **RazorPagesMovieContext**. Se creará una nueva clase de contexto de la base de datos con el espacio de nombres correcto.</span><span class="sxs-lookup"><span data-stu-id="dc939-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="dc939-302">En este caso, será **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="dc939-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="dc939-303">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-303">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arpMac.png)

<span data-ttu-id="dc939-305">El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="dc939-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="dc939-306">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="dc939-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="dc939-307">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="dc939-307">Files created</span></span>

* <span data-ttu-id="dc939-308">*Pages/Movies*: Create, Delete, Details, Edit e Index.</span><span class="sxs-lookup"><span data-stu-id="dc939-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="dc939-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="dc939-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="dc939-310">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="dc939-310">File updated</span></span>

* <span data-ttu-id="dc939-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="dc939-311">*Startup.cs*</span></span>

<span data-ttu-id="dc939-312">Los archivos creados y actualizados se explican en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="dc939-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="dc939-313">Migración inicial</span><span class="sxs-lookup"><span data-stu-id="dc939-313">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc939-315">En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="dc939-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="dc939-316">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="dc939-316">Add an initial migration.</span></span>
* <span data-ttu-id="dc939-317">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="dc939-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="dc939-318">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="dc939-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="dc939-320">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="dc939-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="dc939-321">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="dc939-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="dc939-322">El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc939-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="dc939-323">El argumento `InitialCreate` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="dc939-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="dc939-324">Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="dc939-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="dc939-325">Para obtener más información, vea <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="dc939-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="dc939-326">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc939-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="dc939-327">El método `Up` crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-329">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="dc939-330">Los comandos anteriores generan la advertencia siguiente: *No type was specified for the decimal column "Price" on entity type "Movie" (No se ha especificado ningún tipo en la columna decimal "Price" en el tipo de entidad "Movie"). This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using "HasColumnType()" (Especifique de forma explícita el tipo de columna de SQL Server que pueda acomodar todos los valores mediante "HasColumnType()").* Esta advertencia se puede omitir, ya que se corregirá en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="dc939-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="dc939-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc939-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="dc939-332">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="dc939-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="dc939-333">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dc939-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="dc939-334">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dc939-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="dc939-335">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="dc939-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="dc939-336">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="dc939-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="dc939-337">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="dc939-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="dc939-338">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dc939-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="dc939-339">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="dc939-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="dc939-340">El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="dc939-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="dc939-341">El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="dc939-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="dc939-342">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="dc939-343">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="dc939-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="dc939-344">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc939-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="dc939-345">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="dc939-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="dc939-346">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="dc939-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="dc939-347">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dc939-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="dc939-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc939-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc939-349">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="dc939-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="dc939-350">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="dc939-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc939-351">Examine el método `Up`.</span><span class="sxs-lookup"><span data-stu-id="dc939-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="dc939-352">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="dc939-352">Test the app</span></span>

* <span data-ttu-id="dc939-353">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).</span><span class="sxs-lookup"><span data-stu-id="dc939-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="dc939-354">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="dc939-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="dc939-355">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="dc939-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="dc939-356">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="dc939-356">Test the **Create** link.</span></span>

  ![Página Crear](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="dc939-358">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="dc939-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="dc939-359">La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos.</span><span class="sxs-lookup"><span data-stu-id="dc939-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="dc939-360">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="dc939-360">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="dc939-361">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="dc939-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="dc939-362">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="dc939-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc939-363">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dc939-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc939-364">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="dc939-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
