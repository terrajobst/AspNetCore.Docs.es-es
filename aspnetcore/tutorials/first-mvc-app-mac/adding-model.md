---
title: "Adición de un modelo a una aplicación de ASP.NET MVC Core"
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
keywords: ASP.NET Core,MVC,scaffold,scaffolding
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 0e2c47c7d9903d6d300fa293dd0530a2f65f77d8
ms.sourcegitcommit: e6bcd56a4b11e20ff55df004971f9ed384937342
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="bebef-104">Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**.</span><span class="sxs-lookup"><span data-stu-id="bebef-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="bebef-105">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="bebef-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="bebef-106">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="bebef-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="bebef-107">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="bebef-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="bebef-108">Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="bebef-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="bebef-109">Agregue las propiedades siguientes a la clase `Movie`:</span><span class="sxs-lookup"><span data-stu-id="bebef-109">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="bebef-110">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="bebef-110">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="bebef-111">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="bebef-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="bebef-112">Ahora tiene un **m**odelo en su aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="bebef-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="bebef-113">Preparar el proyecto para la técnica scaffolding</span><span class="sxs-lookup"><span data-stu-id="bebef-113">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="bebef-114">Haga clic con el botón derecho en el archivo del proyecto y, luego, seleccione **Herramientas > Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="bebef-114">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![vista del paso anterior](adding-model/_static/1.png)

- <span data-ttu-id="bebef-116">Agregue los siguientes paquetes de NuGet resaltados al archivo *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="bebef-116">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="bebef-117">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="bebef-117">Save the file.</span></span>

- <span data-ttu-id="bebef-118">Cree un archivo *Modelos/MvcMovieContext.cs* y agregue la siguiente clase `MvcMovieContext`:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="bebef-118">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="bebef-119">Abra el archivo *Startup.cs* y agregue dos instrucciones using:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="bebef-119">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="bebef-120">Agregue el contexto de base de datos al archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bebef-120">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="bebef-121">Esto indica a Entity Framework las clases de modelo que se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="bebef-121">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="bebef-122">Se va a definir un *conjunto de entidades* de objetos Movie, que se representarán en la base de datos como una tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="bebef-122">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="bebef-123">Compile el proyecto para comprobar que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="bebef-123">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="bebef-124">Aplicar la técnica scaffolding a MovieController</span><span class="sxs-lookup"><span data-stu-id="bebef-124">Scaffold the MovieController</span></span>

<span data-ttu-id="bebef-125">Abra una ventana de terminal en la carpeta del proyecto y ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="bebef-125">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="bebef-126">Si recibe el error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="bebef-126">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="bebef-127">Se encuentra en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bebef-127">You are in the project directory.</span></span> <span data-ttu-id="bebef-128">El directorio del proyecto tiene los archivos *Program.cs*, *Startup.cs* y *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bebef-128">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="bebef-129">La versión de dotnet es la 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="bebef-129">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="bebef-130">Ejecute `dotnet` para obtener la versión.</span><span class="sxs-lookup"><span data-stu-id="bebef-130">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="bebef-131">Ha agregado el elemento `<DotNetCliToolReference>` al archivo [MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="bebef-131">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="bebef-132">El motor de scaffolding crea lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bebef-132">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="bebef-133">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="bebef-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="bebef-134">Archivos de vistas de Razor para las páginas Crear, Eliminar, Detalles, Editar e Índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="bebef-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="bebef-135">La creación automática de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="bebef-135">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="bebef-136">Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="bebef-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="bebef-137">Agregar los archivos a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bebef-137">Add the files to Visual Studio</span></span>

* <span data-ttu-id="bebef-138">Agregue el archivo *MovieController.cs* al proyecto de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bebef-138">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="bebef-139">Haga clic con el botón derecho en la carpeta *Controllers* y seleccione **Agregar > Agregar archivos**.</span><span class="sxs-lookup"><span data-stu-id="bebef-139">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="bebef-140">Seleccione el archivo *MovieController.cs*.</span><span class="sxs-lookup"><span data-stu-id="bebef-140">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="bebef-141">Agregue las vistas y la carpeta *Películas*:</span><span class="sxs-lookup"><span data-stu-id="bebef-141">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="bebef-142">Haga clic con el botón derecho en la carpeta *Vistas* y seleccione **Agregar > Agregar carpeta existente**.</span><span class="sxs-lookup"><span data-stu-id="bebef-142">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="bebef-143">Vaya a la carpeta *Vistas*, seleccione *Vistas\Películas* y seleccione **Abrir**.</span><span class="sxs-lookup"><span data-stu-id="bebef-143">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="bebef-144">En el cuadro de diálogo **Seleccione los archivos que se deben agregar de Películas**, seleccione **Incluir todo** y **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bebef-144">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="bebef-145">Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="bebef-145">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="bebef-146">En el siguiente tutorial trabajaremos con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bebef-146">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bebef-147">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bebef-147">Additional resources</span></span>

* [<span data-ttu-id="bebef-148">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="bebef-148">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="bebef-149">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="bebef-149">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="bebef-150">[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="bebef-150">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
