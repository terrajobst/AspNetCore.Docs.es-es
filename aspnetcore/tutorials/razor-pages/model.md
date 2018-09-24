---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: de82738509bb009f030a02e28904e3155088fa6a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011371"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="c6183-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6183-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c6183-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c6183-104">Add a data model</span></span>

<span data-ttu-id="c6183-105">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c6183-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="c6183-106">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="c6183-106">Name the folder *Models*.</span></span>

<span data-ttu-id="c6183-107">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="c6183-107">Right click the *Models* folder.</span></span> <span data-ttu-id="c6183-108">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c6183-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="c6183-109">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="c6183-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="c6183-110">Reemplace el contenido de la clase `Movie` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="c6183-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="c6183-111">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="c6183-111">Scaffold the movie model</span></span>

<span data-ttu-id="c6183-112">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="c6183-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="c6183-113">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="c6183-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="c6183-114">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="c6183-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="c6183-115">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c6183-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="c6183-116">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="c6183-116">Name the folder *Movies*</span></span>

<span data-ttu-id="c6183-117">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scanffold**.</span><span class="sxs-lookup"><span data-stu-id="c6183-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="c6183-119">En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.</span><span class="sxs-lookup"><span data-stu-id="c6183-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="c6183-121">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="c6183-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="c6183-122">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="c6183-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="c6183-123">En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="c6183-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="c6183-124">En la lista desplegable **Clase de contexto de datos**, seleccione **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="c6183-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="c6183-125">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c6183-125">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="c6183-127">El proceso de scaffolding ha creado y cambiado los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c6183-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="c6183-128">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="c6183-128">Files created</span></span>

* <span data-ttu-id="c6183-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="c6183-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="c6183-130">Estas páginas se detallan en el tutorial siguiente.</span><span class="sxs-lookup"><span data-stu-id="c6183-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="c6183-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="c6183-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="c6183-132">Actualizaciones de archivos</span><span class="sxs-lookup"><span data-stu-id="c6183-132">Files updates</span></span>

* <span data-ttu-id="c6183-133">*Startup.cs*: en la sección siguiente se detallan los cambios realizados en este archivo.</span><span class="sxs-lookup"><span data-stu-id="c6183-133">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="c6183-134">*appsettings.json*: se agrega la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="c6183-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="c6183-135">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="c6183-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="c6183-136">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c6183-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c6183-137">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c6183-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c6183-138">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="c6183-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c6183-139">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="c6183-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c6183-140">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c6183-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="c6183-141">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c6183-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c6183-142">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="c6183-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="c6183-143">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c6183-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="c6183-144">El contexto de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c6183-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c6183-145">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c6183-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="c6183-146">En este proyecto, la clase se denomina `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="c6183-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="c6183-147">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="c6183-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="c6183-148">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c6183-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="c6183-149">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="c6183-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c6183-150">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c6183-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c6183-151">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c6183-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="c6183-152">Realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="c6183-152">Perform initial migration</span></span>

<span data-ttu-id="c6183-153">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="c6183-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="c6183-154">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c6183-154">Add an initial migration.</span></span>
* <span data-ttu-id="c6183-155">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c6183-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="c6183-156">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="c6183-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="c6183-158">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c6183-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="c6183-159">Como alternativa, se pueden usar los siguientes comandos de la CLI de .NET Core de la carpeta de proyecto:</span><span class="sxs-lookup"><span data-stu-id="c6183-159">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="c6183-160">Pase por alto el siguiente mensaje de advertencia, se corregirá en un próximo tutorial:</span><span class="sxs-lookup"><span data-stu-id="c6183-160">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="c6183-161">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="c6183-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="c6183-162">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c6183-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="c6183-163">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="c6183-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="c6183-164">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="c6183-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="c6183-165">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c6183-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="c6183-166">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c6183-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="c6183-167">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="c6183-167">If you get the error:</span></span>

<span data-ttu-id="c6183-168">SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-GUID" solicitada por el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="c6183-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="c6183-169">Error de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="c6183-169">The login failed.</span></span>
<span data-ttu-id="c6183-170">Error de inicio de sesión del usuario <nombre de usuario>.</span><span class="sxs-lookup"><span data-stu-id="c6183-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="c6183-171">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="c6183-171">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c6183-172">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c6183-172">Add a data model</span></span>

<span data-ttu-id="c6183-173">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c6183-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="c6183-174">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="c6183-174">Name the folder *Models*.</span></span>

<span data-ttu-id="c6183-175">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="c6183-175">Right click the *Models* folder.</span></span> <span data-ttu-id="c6183-176">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c6183-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="c6183-177">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="c6183-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="c6183-178">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="c6183-178">Add a database connection string</span></span>

<span data-ttu-id="c6183-179">Agregue una cadena de conexión al archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c6183-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="c6183-180">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="c6183-180">Register the database context</span></span>

<span data-ttu-id="c6183-181">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el [método ConfigureServices de la clase Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="c6183-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="c6183-182">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="c6183-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="c6183-183">Agregar herramientas de scaffolding y realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="c6183-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="c6183-184">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="c6183-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="c6183-185">Agregar el paquete de generación de código de Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="c6183-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="c6183-186">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c6183-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="c6183-187">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c6183-187">Add an initial migration.</span></span>
* <span data-ttu-id="c6183-188">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c6183-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="c6183-189">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="c6183-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="c6183-191">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c6183-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="c6183-192">Se pueden usar los siguientes comandos de la CLI de .NET Core de forma alternativa:</span><span class="sxs-lookup"><span data-stu-id="c6183-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="c6183-193">Pase por alto el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="c6183-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="c6183-194">Lo subsanaremos en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="c6183-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="c6183-195">El comando `Install-Package` instala las herramientas necesarias para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c6183-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="c6183-196">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="c6183-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="c6183-197">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c6183-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="c6183-198">El argumento `Initial` se usa para asignar un nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="c6183-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="c6183-199">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="c6183-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="c6183-200">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c6183-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="c6183-201">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c6183-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="c6183-202">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c6183-202">Test the app</span></span>

* <span data-ttu-id="c6183-203">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="c6183-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="c6183-204">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c6183-204">Test the **Create** link.</span></span>

  ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="c6183-206">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="c6183-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="c6183-207">Si obtiene una excepción SQL, confirme que ha ejecutado las migraciones y que la base de datos está actualizada.</span><span class="sxs-lookup"><span data-stu-id="c6183-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="c6183-208">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c6183-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c6183-209">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="c6183-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
