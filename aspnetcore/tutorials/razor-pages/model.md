---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327557"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="6f41f-103">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f41f-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6f41f-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="6f41f-104">Add a data model</span></span>

<span data-ttu-id="6f41f-105">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6f41f-106">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f41f-106">Name the folder *Models*.</span></span>

<span data-ttu-id="6f41f-107">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f41f-107">Right click the *Models* folder.</span></span> <span data-ttu-id="6f41f-108">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="6f41f-109">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f41f-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="6f41f-110">Reemplace el contenido de la clase `Movie` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="6f41f-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="6f41f-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="6f41f-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6f41f-112">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="6f41f-112">Scaffold the movie model</span></span>

<span data-ttu-id="6f41f-113">En esta sección se aplica scaffolding al modelo de película;</span><span class="sxs-lookup"><span data-stu-id="6f41f-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6f41f-114">es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.</span><span class="sxs-lookup"><span data-stu-id="6f41f-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="6f41f-115">Cree una carpeta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="6f41f-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="6f41f-116">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages* > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="6f41f-117">Asigne a la carpeta el nombre *Movies*.</span><span class="sxs-lookup"><span data-stu-id="6f41f-117">Name the folder *Movies*</span></span>

<span data-ttu-id="6f41f-118">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scanffold**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

<span data-ttu-id="6f41f-120">En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="6f41f-122">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="6f41f-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="6f41f-123">En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="6f41f-124">En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="6f41f-125">En la lista desplegable **Clase de contexto de datos**, seleccione **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="6f41f-126">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-126">Select **Add**.</span></span>

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="6f41f-128">Realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="6f41f-128">Perform initial migration</span></span>

<span data-ttu-id="6f41f-129">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="6f41f-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="6f41f-130">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-130">Add an initial migration.</span></span>
* <span data-ttu-id="6f41f-131">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="6f41f-132">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6f41f-134">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="6f41f-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6f41f-135">Se pueden usar los siguientes comandos de la CLI de .NET Core de forma alternativa:</span><span class="sxs-lookup"><span data-stu-id="6f41f-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="6f41f-136">Pase por alto el siguiente mensaje de advertencia, lo subsanaremos en el próximo tutorial:</span><span class="sxs-lookup"><span data-stu-id="6f41f-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="6f41f-137">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6f41f-138">El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="6f41f-138">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="6f41f-139">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="6f41f-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6f41f-140">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="6f41f-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="6f41f-141">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="6f41f-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6f41f-142">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f41f-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="6f41f-143">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="6f41f-143">If you get the error:</span></span>

<span data-ttu-id="6f41f-144">SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-GUID" solicitada por el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6f41f-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="6f41f-145">Error de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6f41f-145">The login failed.</span></span>
<span data-ttu-id="6f41f-146">Error de inicio de sesión del usuario <nombre de usuario>.</span><span class="sxs-lookup"><span data-stu-id="6f41f-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="6f41f-147">Quiere decir que falta el [paso de migraciones](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6f41f-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6f41f-148">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="6f41f-148">Add a data model</span></span>

<span data-ttu-id="6f41f-149">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6f41f-150">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f41f-150">Name the folder *Models*.</span></span>

<span data-ttu-id="6f41f-151">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="6f41f-151">Right click the *Models* folder.</span></span> <span data-ttu-id="6f41f-152">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="6f41f-153">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f41f-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="6f41f-154">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="6f41f-154">Add a database connection string</span></span>

<span data-ttu-id="6f41f-155">Agregue una cadena de conexión al archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6f41f-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="6f41f-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="6f41f-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="6f41f-157">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="6f41f-157">Register the database context</span></span>

<span data-ttu-id="6f41f-158">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el [método ConfigureServices de la clase Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="6f41f-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="6f41f-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="6f41f-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="6f41f-160">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="6f41f-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="6f41f-161">Agregar herramientas de scaffolding y realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="6f41f-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="6f41f-162">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="6f41f-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="6f41f-163">Agregar el paquete de generación de código de Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="6f41f-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="6f41f-164">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6f41f-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="6f41f-165">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-165">Add an initial migration.</span></span>
* <span data-ttu-id="6f41f-166">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="6f41f-167">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6f41f-169">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="6f41f-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6f41f-170">Se pueden usar los siguientes comandos de la CLI de .NET Core de forma alternativa:</span><span class="sxs-lookup"><span data-stu-id="6f41f-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="6f41f-171">Pase por alto el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="6f41f-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="6f41f-172">Lo subsanaremos en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="6f41f-173">El comando `Install-Package` instala las herramientas necesarias para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6f41f-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="6f41f-174">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="6f41f-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6f41f-175">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="6f41f-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="6f41f-176">El argumento `Initial` se usa para asignar un nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="6f41f-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6f41f-177">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="6f41f-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="6f41f-178">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="6f41f-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6f41f-179">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f41f-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6f41f-180">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6f41f-180">Test the app</span></span>

* <span data-ttu-id="6f41f-181">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6f41f-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="6f41f-182">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-182">Test the **Create** link.</span></span>

  ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="6f41f-184">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="6f41f-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6f41f-185">Si obtiene una excepción SQL, confirme que ha ejecutado las migraciones y que la base de datos está actualizada.</span><span class="sxs-lookup"><span data-stu-id="6f41f-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="6f41f-186">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6f41f-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f41f-187">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6f41f-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
