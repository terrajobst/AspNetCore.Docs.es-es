---
title: "Adición de un modelo a una aplicación MVC de ASP.NET"
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: c2cd3cc81221c146dec70e487a17b33360eb6112
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="f2f05-103">Nota: Las plantillas de ASP.NET Core 2.0 contienen la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="f2f05-103">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="f2f05-104">Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="f2f05-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="f2f05-105">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="f2f05-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="f2f05-106">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="f2f05-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="f2f05-107">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="f2f05-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="f2f05-108">Ahora tiene un **m**odelo en la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="f2f05-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="f2f05-109">Scaffolding de un controlador</span><span class="sxs-lookup"><span data-stu-id="f2f05-109">Scaffolding a controller</span></span>

<span data-ttu-id="f2f05-110">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="f2f05-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![vista del paso anterior](adding-model/_static/add_controller.png)

<span data-ttu-id="f2f05-112">Si aparece el cuadro de diálogo **Agregar dependencias de MVC**:</span><span class="sxs-lookup"><span data-stu-id="f2f05-112">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="f2f05-113">[Actualice Visual Studio a la última versión](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f2f05-113">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="f2f05-114">La versiones de Visual Studio anteriores a la 15.5 muestran este cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="f2f05-114">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="f2f05-115">Si no puede actualizar, seleccione **AGREGAR** y luego siga los pasos para agregar el controlador de nuevo.</span><span class="sxs-lookup"><span data-stu-id="f2f05-115">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="f2f05-116">En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f2f05-116">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="f2f05-118">Rellene el cuadro de diálogo **Agregar controlador**:</span><span class="sxs-lookup"><span data-stu-id="f2f05-118">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="f2f05-119">**Clase de modelo:** *Movie (MvcMovie.Models)*.</span><span class="sxs-lookup"><span data-stu-id="f2f05-119">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="f2f05-120">**Clase de contexto de datos:** seleccione el icono **+** y agregue el valor predeterminado **MvcMovie.Models.MvcMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="f2f05-120">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adición de contexto de datos](adding-model/_static/dc.png)

* <span data-ttu-id="f2f05-122">**Vistas:** conserve el valor predeterminado de cada opción activada.</span><span class="sxs-lookup"><span data-stu-id="f2f05-122">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="f2f05-123">**Nombre del controlador:** conserve el valor predeterminado *MoviesController*.</span><span class="sxs-lookup"><span data-stu-id="f2f05-123">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="f2f05-124">Pulse en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f2f05-124">Tap **Add**</span></span>

![Cuadro de diálogo Agregar controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="f2f05-126">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="f2f05-126">Visual Studio creates:</span></span>

* <span data-ttu-id="f2f05-127">Una [clase de contexto de base de datos](xref:data/ef-mvc/intro#create-the-database-context) de Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="f2f05-127">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="f2f05-128">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f2f05-128">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="f2f05-129">Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="f2f05-129">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="f2f05-130">La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="f2f05-130">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="f2f05-131">Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="f2f05-131">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="f2f05-132">Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2f05-132">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="f2f05-133">Debe crear la base de datos y para ello usar la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f2f05-133">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="f2f05-134">Las migraciones permiten crear una base de datos que coincide con el modelo de datos y actualizan el esquema de base de datos cuando cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f05-134">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="f2f05-135">Adición de herramientas de EF y migración inicial</span><span class="sxs-lookup"><span data-stu-id="f2f05-135">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="f2f05-136">En esta sección se usa la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="f2f05-136">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f2f05-137">Agregar el paquete de herramientas de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f2f05-137">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="f2f05-138">Este paquete es necesario para agregar migraciones y actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f05-138">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="f2f05-139">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f2f05-139">Add an initial migration.</span></span>
* <span data-ttu-id="f2f05-140">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f2f05-140">Update the database with the initial migration.</span></span>

<span data-ttu-id="f2f05-141">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f2f05-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menú de PMC](adding-model/_static/pmc.png)

<span data-ttu-id="f2f05-143">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="f2f05-143">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f2f05-144">**Nota:** Si recibe un error con el comando `Install-Package`, abra el administrador de paquetes NuGet y busque el paquete `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f2f05-144">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="f2f05-145">De esta forma, podrá instalar el paquete o comprobar si ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="f2f05-145">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="f2f05-146">Como alternativa, vea el [enfoque de la CLI](#cli) si tiene problemas con la PMC.</span><span class="sxs-lookup"><span data-stu-id="f2f05-146">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="f2f05-147">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="f2f05-147">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="f2f05-148">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f2f05-148">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="f2f05-149">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="f2f05-149">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f2f05-150">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="f2f05-150">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f2f05-151">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f2f05-151">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f2f05-152">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<marca_de_tiempo>_Initial.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f05-152">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="f2f05-153">Puede realizar los pasos anteriores mediante la interfaz de línea de comandos (CLI) en lugar de la PMC:</span><span class="sxs-lookup"><span data-stu-id="f2f05-153">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="f2f05-154">Agregue las [herramientas de EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f2f05-154">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="f2f05-155">Ejecute los siguientes comandos desde la consola (en el directorio del proyecto):</span><span class="sxs-lookup"><span data-stu-id="f2f05-155">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menú contextual de IntelliSense en un elemento Modelo que enumera las propiedades disponibles para Identificador, Precio, Fecha de lanzamiento y Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="f2f05-157">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f2f05-157">Additional resources</span></span>

* [<span data-ttu-id="f2f05-158">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="f2f05-158">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f2f05-159">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="f2f05-159">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="f2f05-160">[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="f2f05-160">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
