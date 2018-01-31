---
title: "Adición de un modelo a una aplicación de ASP.NET Core MVC."
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="f26a3-103">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="f26a3-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="f26a3-104">Agregue el código siguiente al archivo *Modelos/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="f26a3-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="f26a3-105">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="f26a3-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="f26a3-106">Compile la aplicación para comprobar que no tiene ningún error y que ha agregado un **m**odelo a la aplicación **M**VC.</span><span class="sxs-lookup"><span data-stu-id="f26a3-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="f26a3-107">Preparar el proyecto para la técnica scaffolding</span><span class="sxs-lookup"><span data-stu-id="f26a3-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="f26a3-108">Agregue los siguientes paquetes de NuGet resaltados al archivo *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="f26a3-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="f26a3-109">Guarde el archivo y seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".</span><span class="sxs-lookup"><span data-stu-id="f26a3-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="f26a3-110">Cree un archivo *Modelos/MvcMovieContext.cs* y agregue la siguiente clase `MvcMovieContext`:</span><span class="sxs-lookup"><span data-stu-id="f26a3-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="f26a3-111">Abra el archivo *Startup.cs* y agregue dos instrucciones using:</span><span class="sxs-lookup"><span data-stu-id="f26a3-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="f26a3-112">Agregue el contexto de base de datos al archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f26a3-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="f26a3-113">Esto indica a Entity Framework las clases de modelo que se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f26a3-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="f26a3-114">Se va a definir un *conjunto de entidades* de objetos Movie, que se representarán en la base de datos como una tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="f26a3-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="f26a3-115">Compile el proyecto para comprobar que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="f26a3-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="f26a3-116">Aplicar la técnica scaffolding a MovieController</span><span class="sxs-lookup"><span data-stu-id="f26a3-116">Scaffold the MovieController</span></span>

<span data-ttu-id="f26a3-117">Abra una ventana de terminal en la carpeta del proyecto y ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="f26a3-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="f26a3-118">El motor de scaffolding crea lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f26a3-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="f26a3-119">Un controlador de películas (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f26a3-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="f26a3-120">Archivos de vistas de Razor para las páginas Crear, Eliminar, Detalles, Editar e Índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="f26a3-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="f26a3-121">La creación automática de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="f26a3-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="f26a3-122">Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="f26a3-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="f26a3-123">Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="f26a3-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="f26a3-124">En el siguiente tutorial trabajaremos con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f26a3-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="f26a3-125">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f26a3-125">Additional resources</span></span>

* [<span data-ttu-id="f26a3-126">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="f26a3-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f26a3-127">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="f26a3-127">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="f26a3-128">[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="f26a3-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
