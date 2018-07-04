---
title: Agregar un campo nuevo a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo usar Migraciones de Entity Framework Code First para agregar un nuevo campo al modelo y migrar ese cambio a una base de datos.
ms.author: riande
ms.date: 10/14/2016
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: eb98ebcde1086ad605127dddc055a18d4874c722
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961040"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="2cefa-103">Agregar un campo nuevo a una aplicación de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="2cefa-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="2cefa-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2cefa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2cefa-105">En esta sección se usa Migraciones de [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para agregar un nuevo campo al modelo y migrar ese cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cefa-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="2cefa-106">Cuando se usa EF Code First para crear una base de datos de forma automática, Code First agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado.</span><span class="sxs-lookup"><span data-stu-id="2cefa-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="2cefa-107">Si no está sincronizado, EF produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="2cefa-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="2cefa-108">Esto facilita la detección de problemas de código o base de datos incoherentes.</span><span class="sxs-lookup"><span data-stu-id="2cefa-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="2cefa-109">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="2cefa-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="2cefa-110">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="2cefa-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

<span data-ttu-id="2cefa-111">Compile la aplicación (Ctrl + Mayús + B).</span><span class="sxs-lookup"><span data-stu-id="2cefa-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="2cefa-112">Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la lista blanca de enlaces para que se incluya esta nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="2cefa-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="2cefa-113">En *MoviesController.cs*, actualice el atributo `[Bind]` de los métodos de acción `Create` y `Edit` para incluir la propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="2cefa-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="2cefa-114">También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="2cefa-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="2cefa-115">Edite el archivo */Views/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="2cefa-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="2cefa-116">Actualice */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="2cefa-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="2cefa-117">Puede copiar o pegar el elemento "form group" anterior y permitir que IntelliSense le ayude a actualizar los campos.</span><span class="sxs-lookup"><span data-stu-id="2cefa-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="2cefa-118">IntelliSense funciona con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="2cefa-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="2cefa-119">Nota: En la versión RTM de Visual Studio 2017 debe instalar [Servicios de lenguaje Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) para Razor IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="2cefa-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="2cefa-120">Esto se resolverá en la próxima versión.</span><span class="sxs-lookup"><span data-stu-id="2cefa-120">This will be fixed in the next release.</span></span>

![El desarrollador ha escrito la letra R para el valor del atributo de asp-for en el segundo elemento de etiqueta de la vista.](new-field/_static/cr.png)

<span data-ttu-id="2cefa-124">La aplicación no funcionará hasta que la base de datos se actualice para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="2cefa-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="2cefa-125">Si la ejecuta ahora, se producirá la siguiente `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="2cefa-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="2cefa-126">Este error aparece porque la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="2cefa-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="2cefa-127">(No hay ninguna columna Rating en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="2cefa-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="2cefa-128">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="2cefa-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="2cefa-129">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="2cefa-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="2cefa-130">Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos.</span><span class="sxs-lookup"><span data-stu-id="2cefa-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="2cefa-131">La desventaja es que se pierden los datos existentes en la base de datos, así que no use este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="2cefa-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="2cefa-132">Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cefa-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="2cefa-133">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="2cefa-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="2cefa-134">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="2cefa-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="2cefa-135">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cefa-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="2cefa-136">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2cefa-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="2cefa-137">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="2cefa-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="2cefa-138">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="2cefa-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="2cefa-139">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizarlo con cada `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="2cefa-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="2cefa-140">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="2cefa-140">Build the solution.</span></span>

<span data-ttu-id="2cefa-141">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="2cefa-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menú de PMC](adding-model/_static/pmc.png)

<span data-ttu-id="2cefa-143">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="2cefa-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="2cefa-144">El comando `Add-Migration` indica el marco de trabajo de migración para examinar el modelo `Movie` actual con el esquema de base de datos `Movie` actual y para crear el código con el que se migrará la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="2cefa-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="2cefa-145">El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="2cefa-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="2cefa-146">Resulta útil emplear un nombre descriptivo para el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="2cefa-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="2cefa-147">Si elimina todos los registros de la base de datos, el inicializador inicializa la base de datos e incluye el campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="2cefa-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="2cefa-148">Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.</span><span class="sxs-lookup"><span data-stu-id="2cefa-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="2cefa-149">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="2cefa-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="2cefa-150">También debe agregar el campo `Rating` a las plantillas de vista `Edit`, `Details` y `Delete`.</span><span class="sxs-lookup"><span data-stu-id="2cefa-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2cefa-151">[Anterior](search.md)
> [Siguiente](validation.md)</span><span class="sxs-lookup"><span data-stu-id="2cefa-151">[Previous](search.md)
[Next](validation.md)</span></span>  
