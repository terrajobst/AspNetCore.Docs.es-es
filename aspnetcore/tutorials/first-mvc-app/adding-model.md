---
title: "Adición de un modelo a una aplicación MVC de ASP.NET"
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="f7de4-104">Nota: Las plantillas de ASP.NET Core 2.0 contienen la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="f7de4-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="f7de4-105">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **MvcMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="f7de4-106">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="f7de4-106">Name the folder *Models*.</span></span>

<span data-ttu-id="f7de4-107">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="f7de4-108">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="f7de4-108">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="f7de4-109">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="f7de4-109">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="f7de4-110">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="f7de4-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="f7de4-111">Ahora tiene un **m**odelo en la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="f7de4-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="f7de4-112">Scaffolding de un controlador</span><span class="sxs-lookup"><span data-stu-id="f7de4-112">Scaffolding a controller</span></span>

<span data-ttu-id="f7de4-113">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller.png)

<span data-ttu-id="f7de4-115">En el cuadro de diálogo **Agregar dependencias de MVC**, seleccione **Dependencias mínimas** y **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-115">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![vista del paso anterior](adding-model/_static/add_depend.png)

<span data-ttu-id="f7de4-117">Visual Studio agrega las dependencias necesarias para aplicar la técnica de scaffolding a un controlador, pero no se crea el controlador.</span><span class="sxs-lookup"><span data-stu-id="f7de4-117">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="f7de4-118">La siguiente invocación de **> Agregar > Controlador** crea el controlador.</span><span class="sxs-lookup"><span data-stu-id="f7de4-118">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="f7de4-119">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-119">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller.png)

<span data-ttu-id="f7de4-121">En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-121">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="f7de4-123">Rellene el cuadro de diálogo **Agregar controlador**:</span><span class="sxs-lookup"><span data-stu-id="f7de4-123">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="f7de4-124">**Clase de modelo:** *Movie (MvcMovie.Models)*.</span><span class="sxs-lookup"><span data-stu-id="f7de4-124">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="f7de4-125">**Clase de contexto de datos:** seleccione el icono **+** y agregue el valor predeterminado **MvcMovie.Models.MvcMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-125">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adición de contexto de datos](adding-model/_static/dc.png)

* <span data-ttu-id="f7de4-127">**Vistas:** conserve el valor predeterminado de cada opción activada.</span><span class="sxs-lookup"><span data-stu-id="f7de4-127">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="f7de4-128">**Nombre del controlador:** conserve el valor predeterminado *MoviesController*.</span><span class="sxs-lookup"><span data-stu-id="f7de4-128">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="f7de4-129">Pulse en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-129">Tap **Add**</span></span>

![Cuadro de diálogo Agregar controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="f7de4-131">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="f7de4-131">Visual Studio creates:</span></span>

* <span data-ttu-id="f7de4-132">Una [clase de contexto de base de datos](xref:data/ef-mvc/intro#create-the-database-context) de Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="f7de4-132">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="f7de4-133">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f7de4-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="f7de4-134">Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="f7de4-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="f7de4-135">La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="f7de4-135">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="f7de4-136">Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="f7de4-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="f7de4-137">Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7de4-137">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="f7de4-138">Debe crear la base de datos y para ello usar la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f7de4-138">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="f7de4-139">Las migraciones permiten crear una base de datos que coincide con el modelo de datos y actualizan el esquema de base de datos cuando cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f7de4-139">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="f7de4-140">Adición de herramientas de EF y migración inicial</span><span class="sxs-lookup"><span data-stu-id="f7de4-140">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="f7de4-141">En esta sección se usa la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="f7de4-141">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f7de4-142">Agregar el paquete de herramientas de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f7de4-142">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="f7de4-143">Este paquete es necesario para agregar migraciones y actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7de4-143">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="f7de4-144">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f7de4-144">Add an initial migration.</span></span>
* <span data-ttu-id="f7de4-145">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f7de4-145">Update the database with the initial migration.</span></span>

<span data-ttu-id="f7de4-146">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f7de4-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menú de PMC](adding-model/_static/pmc.png)

<span data-ttu-id="f7de4-148">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="f7de4-148">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f7de4-149">**Nota:** Si recibe un error con el comando `Install-Package`, abra el administrador de paquetes NuGet y busque el paquete `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f7de4-149">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="f7de4-150">De esta forma, podrá instalar el paquete o comprobar si ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="f7de4-150">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="f7de4-151">Como alternativa, vea el [enfoque de la CLI](#cli) si tiene problemas con la PMC.</span><span class="sxs-lookup"><span data-stu-id="f7de4-151">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="f7de4-152">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="f7de4-152">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="f7de4-153">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f7de4-153">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="f7de4-154">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="f7de4-154">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f7de4-155">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="f7de4-155">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f7de4-156">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f7de4-156">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f7de4-157">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7de4-157">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="f7de4-158"><a name="cli"></a> Puede realizar los pasos anteriores mediante la interfaz de línea de comandos (CLI) en lugar de la PMC:</span><span class="sxs-lookup"><span data-stu-id="f7de4-158"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="f7de4-159">Agregue las [herramientas de EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f7de4-159">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="f7de4-160">Ejecute los siguientes comandos desde la consola (en el directorio del proyecto):</span><span class="sxs-lookup"><span data-stu-id="f7de4-160">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menú contextual de IntelliSense en un elemento Modelo que enumera las propiedades disponibles para Identificador, Precio, Fecha de lanzamiento y Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="f7de4-162">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f7de4-162">Additional resources</span></span>

* [<span data-ttu-id="f7de4-163">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="f7de4-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f7de4-164">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="f7de4-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="f7de4-165">[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="f7de4-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
