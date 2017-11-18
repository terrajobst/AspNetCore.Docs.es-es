---
title: "Adición de un modelo a una aplicación de ASP.NET Core MVC."
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, API web, REST, Mac, Linux, HTTP, servicio, servicio HTTP, VS Code
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="0faad-104">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0faad-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="0faad-105">Agregue el código siguiente al archivo *Modelos/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="0faad-105">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="0faad-106">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="0faad-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="0faad-107">Compile la aplicación para comprobar que no tiene ningún error y que ha agregado un **m**odelo a la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="0faad-107">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="0faad-108">Preparar el proyecto para la técnica scaffolding</span><span class="sxs-lookup"><span data-stu-id="0faad-108">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="0faad-109">Agregue los siguientes paquetes de NuGet resaltados al archivo *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="0faad-109">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="0faad-110">Guarde el archivo y seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".</span><span class="sxs-lookup"><span data-stu-id="0faad-110">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="0faad-111">Cree un archivo *Modelos/MvcMovieContext.cs* y agregue la siguiente clase `MvcMovieContext`:</span><span class="sxs-lookup"><span data-stu-id="0faad-111">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="0faad-112">Abra el archivo *Startup.cs* y agregue dos instrucciones using:</span><span class="sxs-lookup"><span data-stu-id="0faad-112">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="0faad-113">Agregue el contexto de base de datos al archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0faad-113">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="0faad-114">Esto indica a Entity Framework las clases de modelo que se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="0faad-114">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="0faad-115">Se va a definir un *conjunto de entidades* de objetos Movie, que se representarán en la base de datos como una tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="0faad-115">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="0faad-116">Compile el proyecto para comprobar que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="0faad-116">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="0faad-117">Aplicar la técnica scaffolding a MovieController</span><span class="sxs-lookup"><span data-stu-id="0faad-117">Scaffold the MovieController</span></span>

<span data-ttu-id="0faad-118">Abra una ventana de terminal en la carpeta del proyecto y ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="0faad-118">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> <span data-ttu-id="0faad-119">Si recibe un error al ejecutar el comando de la técnica scaffolding, vea [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) (Error 444 en el repositorio de scaffolding) para encontrar una solución.</span><span class="sxs-lookup"><span data-stu-id="0faad-119">If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.</span></span>

<span data-ttu-id="0faad-120">El motor de scaffolding crea lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0faad-120">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="0faad-121">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="0faad-121">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="0faad-122">Archivos de vistas de Razor para las páginas Crear, Eliminar, Detalles, Editar e Índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="0faad-122">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="0faad-123">La creación automática de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="0faad-123">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="0faad-124">Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="0faad-124">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="0faad-125">Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="0faad-125">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="0faad-126">En el siguiente tutorial trabajaremos con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0faad-126">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="0faad-127">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0faad-127">Additional resources</span></span>

* [<span data-ttu-id="0faad-128">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="0faad-128">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="0faad-129">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="0faad-129">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="0faad-130">[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="0faad-130">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
