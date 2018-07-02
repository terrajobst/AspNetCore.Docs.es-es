---
title: Agregar un modelo a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Agregue un modelo a una aplicación sencilla de ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 28a63498bc1a3c7b6ad6be038209dacdb49e44ee
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960673"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="c0c15-103">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-103">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="c0c15-104">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="c0c15-104">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="c0c15-105">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="c0c15-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="c0c15-106">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="c0c15-106">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="c0c15-107">Ahora tiene un **m**odelo en la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="c0c15-107">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="c0c15-108">Scaffolding de un controlador</span><span class="sxs-lookup"><span data-stu-id="c0c15-108">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c0c15-109">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-109">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller21.png)

<span data-ttu-id="c0c15-111">En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-111">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c0c15-113">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller.png)

<span data-ttu-id="c0c15-115">Si aparece el cuadro de diálogo **Agregar dependencias de MVC**:</span><span class="sxs-lookup"><span data-stu-id="c0c15-115">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="c0c15-116">[Actualice Visual Studio a la última versión](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c0c15-116">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="c0c15-117">La versiones de Visual Studio anteriores a la 15.5 muestran este cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="c0c15-117">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="c0c15-118">Si no puede actualizar, seleccione **AGREGAR** y luego siga los pasos para agregar el controlador de nuevo.</span><span class="sxs-lookup"><span data-stu-id="c0c15-118">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="c0c15-119">En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-119">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="c0c15-121">Rellene el cuadro de diálogo **Agregar controlador**:</span><span class="sxs-lookup"><span data-stu-id="c0c15-121">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="c0c15-122">**Clase de modelo:** *Movie (MvcMovie.Models)*.</span><span class="sxs-lookup"><span data-stu-id="c0c15-122">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="c0c15-123">**Clase de contexto de datos:** seleccione el icono **+** y agregue el valor predeterminado **MvcMovie.Models.MvcMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-123">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adición de contexto de datos](adding-model/_static/dc.png)

* <span data-ttu-id="c0c15-125">**Vistas:** conserve el valor predeterminado de cada opción activada.</span><span class="sxs-lookup"><span data-stu-id="c0c15-125">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="c0c15-126">**Nombre del controlador:** conserve el valor predeterminado *MoviesController*.</span><span class="sxs-lookup"><span data-stu-id="c0c15-126">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="c0c15-127">Pulse en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-127">Tap **Add**</span></span>

![Cuadro de diálogo Agregar controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="c0c15-129">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="c0c15-129">Visual Studio creates:</span></span>

* <span data-ttu-id="c0c15-130">Una [clase de contexto de base de datos](xref:data/ef-mvc/intro#create-the-database-context) de Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="c0c15-130">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="c0c15-131">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="c0c15-131">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="c0c15-132">Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="c0c15-132">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="c0c15-133">La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="c0c15-133">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="c0c15-134">Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="c0c15-134">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="c0c15-135">Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0c15-135">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="c0c15-136">Debe crear la base de datos y para ello usar la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c0c15-136">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="c0c15-137">Las migraciones permiten crear una base de datos que coincide con el modelo de datos y actualizan el esquema de base de datos cuando cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c0c15-137">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="c0c15-138">Adición de herramientas de EF y migración inicial</span><span class="sxs-lookup"><span data-stu-id="c0c15-138">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="c0c15-139">En esta sección se usa la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="c0c15-139">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="c0c15-140">Agregar el paquete de herramientas de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c0c15-140">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="c0c15-141">Este paquete es necesario para agregar migraciones y actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c0c15-141">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="c0c15-142">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c0c15-142">Add an initial migration.</span></span>
* <span data-ttu-id="c0c15-143">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c0c15-143">Update the database with the initial migration.</span></span>

<span data-ttu-id="c0c15-144">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="c0c15-144">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<span data-ttu-id="c0c15-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![Menú de PMC](adding-model/_static/pmc.png)</span><span class="sxs-lookup"><span data-stu-id="c0c15-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)</span></span>

<span data-ttu-id="c0c15-146">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c0c15-146">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="c0c15-147">Pase por alto el siguiente mensaje de error, lo subsanaremos en el próximo tutorial:</span><span class="sxs-lookup"><span data-stu-id="c0c15-147">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="c0c15-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="c0c15-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="c0c15-149">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.* (No se ha especificado ningún tipo en la columna decimal "Price" en el tipo de entidad "Movie". Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando "ForHasColumnType()". Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados.)</span><span class="sxs-lookup"><span data-stu-id="c0c15-149">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="c0c15-150">**Nota:** Si recibe un error con el comando `Install-Package`, abra el administrador de paquetes NuGet y busque el paquete `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="c0c15-150">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="c0c15-151">De esta forma, podrá instalar el paquete o comprobar si ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="c0c15-151">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="c0c15-152">Como alternativa, vea el [enfoque de la CLI](#cli) si tiene problemas con la PMC.</span><span class="sxs-lookup"><span data-stu-id="c0c15-152">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="c0c15-153">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="c0c15-153">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="c0c15-154">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c0c15-154">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="c0c15-155">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="c0c15-155">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="c0c15-156">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="c0c15-156">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="c0c15-157">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c0c15-157">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="c0c15-158">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<marca_de_tiempo>_Initial.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c0c15-158">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="c0c15-159">Puede realizar los pasos anteriores mediante la interfaz de línea de comandos (CLI) en lugar de la PMC:</span><span class="sxs-lookup"><span data-stu-id="c0c15-159">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="c0c15-160">Agregue las [herramientas de EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c0c15-160">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="c0c15-161">Ejecute los siguientes comandos desde la consola (en el directorio del proyecto):</span><span class="sxs-lookup"><span data-stu-id="c0c15-161">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="c0c15-162">Si ejecuta la aplicación y aparece el error:</span><span class="sxs-lookup"><span data-stu-id="c0c15-162">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="c0c15-163">Probablemente se deba a que no ha ejecutado `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="c0c15-163">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menú contextual de IntelliSense en un elemento Modelo que enumera las propiedades disponibles para Identificador, Precio, Fecha de lanzamiento y Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="c0c15-165">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c0c15-165">Additional resources</span></span>

* [<span data-ttu-id="c0c15-166">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="c0c15-166">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="c0c15-167">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="c0c15-167">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="c0c15-168">[Anterior: Agregar una vista](adding-view.md)
> [Siguiente: Trabajar con SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="c0c15-168">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
