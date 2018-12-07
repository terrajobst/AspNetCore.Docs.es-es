---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0d33901805d6728fb8006f14d41090b874ab28b1
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549125"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="8658b-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8658b-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="8658b-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="8658b-104">Add a data model</span></span>

<span data-ttu-id="8658b-105">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8658b-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="8658b-106">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8658b-106">Name the folder *Models*.</span></span>

<span data-ttu-id="8658b-107">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="8658b-107">Right click the *Models* folder.</span></span> <span data-ttu-id="8658b-108">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="8658b-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="8658b-109">Cambie el nombre de la clase **Movie** y reemplace el contenido de la clase `Movie` con este código:</span><span class="sxs-lookup"><span data-stu-id="8658b-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="8658b-110">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="8658b-110">Scaffold the movie model</span></span>

<span data-ttu-id="8658b-111">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="8658b-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="8658b-112">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="8658b-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="8658b-113">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="8658b-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="8658b-114">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8658b-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="8658b-115">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="8658b-115">Name the folder *Movies*</span></span>

<span data-ttu-id="8658b-116">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scanffold**.</span><span class="sxs-lookup"><span data-stu-id="8658b-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="8658b-118">En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8658b-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="8658b-120">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="8658b-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="8658b-121">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="8658b-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="8658b-122">En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="8658b-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="8658b-123">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8658b-123">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<span data-ttu-id="8658b-125">El proceso de scaffolding crea y actualiza los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8658b-125">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="8658b-126">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="8658b-126">Files created</span></span>

* <span data-ttu-id="8658b-127">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="8658b-127">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="8658b-128">Estas páginas se detallan en el tutorial siguiente.</span><span class="sxs-lookup"><span data-stu-id="8658b-128">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="8658b-129">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="8658b-129">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="8658b-130">Archivo actualizado</span><span class="sxs-lookup"><span data-stu-id="8658b-130">File updated</span></span>

* <span data-ttu-id="8658b-131">*Startup.cs*: en la sección siguiente se detallan los cambios realizados en este archivo.</span><span class="sxs-lookup"><span data-stu-id="8658b-131">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="8658b-132">*appsettings.json*: se agrega la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="8658b-132">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="8658b-133">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8658b-133">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="8658b-134">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8658b-134">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8658b-135">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8658b-135">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="8658b-136">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="8658b-136">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="8658b-137">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="8658b-137">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="8658b-138">La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="8658b-138">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="8658b-139">Examine el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8658b-139">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8658b-140">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="8658b-140">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="8658b-141">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8658b-141">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="8658b-142">El contexto de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="8658b-142">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="8658b-143">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="8658b-143">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="8658b-144">En este proyecto, la clase se denomina `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="8658b-144">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="8658b-145">En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="8658b-145">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="8658b-146">En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8658b-146">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="8658b-147">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="8658b-147">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="8658b-148">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="8658b-148">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="8658b-149">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8658b-149">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="8658b-150">Realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="8658b-150">Perform initial migration</span></span>

<span data-ttu-id="8658b-151">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="8658b-151">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="8658b-152">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8658b-152">Add an initial migration.</span></span>
* <span data-ttu-id="8658b-153">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8658b-153">Update the database with the initial migration.</span></span>

<span data-ttu-id="8658b-154">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="8658b-154">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="8658b-156">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="8658b-156">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="8658b-157">Como alternativa, se pueden usar los siguientes comandos de la CLI de .NET Core de la carpeta de proyecto:</span><span class="sxs-lookup"><span data-stu-id="8658b-157">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="8658b-158">Ignore el siguiente mensaje de advertencia, ya que se corregirá en un tutorial posterior:</span><span class="sxs-lookup"><span data-stu-id="8658b-158">Ignore the following warning message, which you fix in a later tutorial:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

<span data-ttu-id="8658b-159">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="8658b-159">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8658b-160">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="8658b-160">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="8658b-161">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8658b-161">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="8658b-162">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="8658b-162">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="8658b-163">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8658b-163">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="8658b-164">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8658b-164">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="8658b-165">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="8658b-165">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="8658b-166">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="8658b-166">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="8658b-167">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="8658b-167">Add a data model</span></span>

<span data-ttu-id="8658b-168">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8658b-168">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="8658b-169">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="8658b-169">Name the folder *Models*.</span></span>

<span data-ttu-id="8658b-170">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="8658b-170">Right click the *Models* folder.</span></span> <span data-ttu-id="8658b-171">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="8658b-171">Select **Add** > **Class**.</span></span> <span data-ttu-id="8658b-172">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="8658b-172">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="8658b-173">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="8658b-173">Add a database connection string</span></span>

<span data-ttu-id="8658b-174">Agregue una cadena de conexión al archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8658b-174">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="8658b-175">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="8658b-175">Register the database context</span></span>

<span data-ttu-id="8658b-176">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el [método ConfigureServices de la clase Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="8658b-176">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="8658b-177">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="8658b-177">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="8658b-178">Agregar herramientas de scaffolding y realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="8658b-178">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="8658b-179">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="8658b-179">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="8658b-180">Agregar el paquete de generación de código de Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="8658b-180">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="8658b-181">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8658b-181">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="8658b-182">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8658b-182">Add an initial migration.</span></span>
* <span data-ttu-id="8658b-183">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="8658b-183">Update the database with the initial migration.</span></span>

<span data-ttu-id="8658b-184">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="8658b-184">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="8658b-186">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="8658b-186">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="8658b-187">Se pueden usar los siguientes comandos de la CLI de .NET Core de forma alternativa:</span><span class="sxs-lookup"><span data-stu-id="8658b-187">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="8658b-188">Pase por alto el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="8658b-188">Ignore the following message:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

<span data-ttu-id="8658b-189">Lo subsanaremos en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="8658b-189">You fix that in the next tutorial.</span></span>

<span data-ttu-id="8658b-190">El comando `Install-Package` instala las herramientas necesarias para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8658b-190">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="8658b-191">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="8658b-191">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8658b-192">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="8658b-192">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="8658b-193">El argumento `Initial` se usa para asignar un nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8658b-193">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="8658b-194">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="8658b-194">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="8658b-195">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8658b-195">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="8658b-196">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8658b-196">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="8658b-197">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="8658b-197">Test the app</span></span>

* <span data-ttu-id="8658b-198">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="8658b-198">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="8658b-199">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8658b-199">Test the **Create** link.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8658b-200">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="8658b-200">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8658b-201">Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8658b-201">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, you must globalize your app.</span></span> <span data-ttu-id="8658b-202">Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="8658b-202">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

  ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="8658b-204">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="8658b-204">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="8658b-205">Si obtiene una excepción SQL, confirme que ha ejecutado las migraciones y que la base de datos está actualizada.</span><span class="sxs-lookup"><span data-stu-id="8658b-205">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="8658b-206">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8658b-206">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8658b-207">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="8658b-207">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
