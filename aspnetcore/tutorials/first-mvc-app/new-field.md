---
title: "Adición de un nuevo campo"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 046e16b1a9581edb2be9a2315cf7f2677684747b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-new-field"></a><span data-ttu-id="33076-102">Adición de un nuevo campo</span><span class="sxs-lookup"><span data-stu-id="33076-102">Adding a New Field</span></span>

<span data-ttu-id="33076-103">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33076-103">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33076-104">En esta sección se usa Migraciones de [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para agregar un nuevo campo al modelo y migrar ese cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="33076-104">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="33076-105">Cuando se usa EF Code First para crear una base de datos de forma automática, Code First agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado.</span><span class="sxs-lookup"><span data-stu-id="33076-105">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="33076-106">Si no está sincronizado, EF produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="33076-106">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="33076-107">Esto facilita la detección de problemas de código o base de datos incoherentes.</span><span class="sxs-lookup"><span data-stu-id="33076-107">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="33076-108">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="33076-108">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="33076-109">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="33076-109">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="33076-110">Compile la aplicación (Ctrl + Mayús + B).</span><span class="sxs-lookup"><span data-stu-id="33076-110">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="33076-111">Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la lista blanca de enlaces para que se incluya esta nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="33076-111">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="33076-112">En *MoviesController.cs*, actualice el atributo `[Bind]` de los métodos de acción `Create` y `Edit` para incluir la propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="33076-112">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="33076-113">También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="33076-113">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="33076-114">Edite el archivo */Views/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="33076-114">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="33076-115">Actualice */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="33076-115">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="33076-116">Puede copiar o pegar el elemento "form group" anterior y permitir que IntelliSense le ayude a actualizar los campos.</span><span class="sxs-lookup"><span data-stu-id="33076-116">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="33076-117">IntelliSense funciona con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="33076-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="33076-118">Nota: En la versión RTM de Visual Studio 2017 debe instalar [Servicios de lenguaje Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) para Razor IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="33076-118">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="33076-119">Esto se resolverá en la próxima versión.</span><span class="sxs-lookup"><span data-stu-id="33076-119">This will be fixed in the next release.</span></span>

![El desarrollador ha escrito la letra R para el valor del atributo de asp-for en el segundo elemento de etiqueta de la vista.](new-field/_static/cr.png)

<span data-ttu-id="33076-123">La aplicación no funcionará hasta que la base de datos se actualice para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="33076-123">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="33076-124">Si la ejecuta ahora, se producirá la siguiente `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="33076-124">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="33076-125">Este error aparece porque la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="33076-125">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="33076-126">(No hay ninguna columna Rating en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="33076-126">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="33076-127">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="33076-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="33076-128">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="33076-128">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="33076-129">Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos.</span><span class="sxs-lookup"><span data-stu-id="33076-129">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="33076-130">La desventaja es que se pierden los datos existentes en la base de datos, así que no use este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="33076-130">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="33076-131">Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="33076-131">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="33076-132">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="33076-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="33076-133">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="33076-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="33076-134">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="33076-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="33076-135">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="33076-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="33076-136">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="33076-136">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="33076-137">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="33076-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="33076-138">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizarlo con cada `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="33076-138">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="33076-139">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="33076-139">Build the solution.</span></span>

<span data-ttu-id="33076-140">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="33076-140">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menú de PMC](adding-model/_static/pmc.png)

<span data-ttu-id="33076-142">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="33076-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="33076-143">El comando `Add-Migration` indica el marco de trabajo de migración para examinar el modelo `Movie` actual con el esquema de base de datos `Movie` actual y para crear el código con el que se migrará la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="33076-143">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="33076-144">El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="33076-144">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="33076-145">Resulta útil emplear un nombre descriptivo para el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="33076-145">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="33076-146">Si elimina todos los registros de la base de datos, el inicializador inicializa la base de datos e incluye el campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="33076-146">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="33076-147">Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.</span><span class="sxs-lookup"><span data-stu-id="33076-147">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="33076-148">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="33076-148">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="33076-149">También debe agregar el campo `Rating` a las plantillas de vista `Edit`, `Details` y `Delete`.</span><span class="sxs-lookup"><span data-stu-id="33076-149">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="33076-150">[Anterior](search.md)
[Siguiente](validation.md)</span><span class="sxs-lookup"><span data-stu-id="33076-150">[Previous](search.md)
[Next](validation.md)</span></span>  
