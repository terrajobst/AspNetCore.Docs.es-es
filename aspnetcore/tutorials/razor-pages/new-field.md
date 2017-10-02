---
title: "Adición de un nuevo campo a una página de Razor"
author: rick-anderson
description: "Muestra cómo agregar un nuevo campo a una página de Razor con Entity Framework Core"
keywords: ASP.NET Core,Entity Framework Core,migraciones
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: cab986d0a7b7ac68cdda36a558e9b05c429108d0
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="47c98-104">Adición de un nuevo campo a una página de Razor</span><span class="sxs-lookup"><span data-stu-id="47c98-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="47c98-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47c98-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47c98-106">En esta sección se usa Migraciones de [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para agregar un nuevo campo al modelo y migrar ese cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-106">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="47c98-107">Cuando se usa EF Code First para crear una base de datos de forma automática, Code First agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado.</span><span class="sxs-lookup"><span data-stu-id="47c98-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="47c98-108">Si no está sincronizado, EF produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="47c98-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="47c98-109">Esto facilita la detección de problemas de código o base de datos incoherentes.</span><span class="sxs-lookup"><span data-stu-id="47c98-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="47c98-110">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="47c98-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="47c98-111">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="47c98-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="47c98-112">Compile la aplicación (Ctrl + Mayús + B).</span><span class="sxs-lookup"><span data-stu-id="47c98-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="47c98-113">Edite *Pages/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="47c98-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="47c98-114">Agregue el campo `Rating` a las páginas Delete y Details.</span><span class="sxs-lookup"><span data-stu-id="47c98-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="47c98-115">Actualice *Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="47c98-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="47c98-116">Puede copiar o pegar el elemento `<div>` anterior y permitir que IntelliSense le ayude a actualizar los campos.</span><span class="sxs-lookup"><span data-stu-id="47c98-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="47c98-117">IntelliSense funciona con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="47c98-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![El desarrollador ha escrito la letra R para el valor del atributo de asp-for en el segundo elemento de etiqueta de la vista.](new-field/_static/cr.png)

<span data-ttu-id="47c98-121">El código siguiente muestra *Create.cshtml* con un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="47c98-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="47c98-122">Agregue el campo `Rating` a la página de edición.</span><span class="sxs-lookup"><span data-stu-id="47c98-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="47c98-123">La aplicación no funciona hasta que la base de datos se actualiza para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="47c98-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="47c98-124">Si se ejecuta ahora, la aplicación produce una `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="47c98-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="47c98-125">Este error se debe a que la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="47c98-126">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="47c98-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="47c98-127">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="47c98-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="47c98-128">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear con el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="47c98-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="47c98-129">Este enfoque resulta conveniente al principio del ciclo de desarrollo; permite desarrollar a la vez y de manera rápida el esquema del modelo y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="47c98-130">El inconveniente es que se pierden los datos existentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="47c98-131">No use este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="47c98-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="47c98-132">El quitar la base de datos en los cambios de esquema y el usar un inicializador para inicializar automáticamente la base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="47c98-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="47c98-133">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="47c98-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="47c98-134">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="47c98-135">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="47c98-136">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47c98-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="47c98-137">Para este tutorial, use Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="47c98-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="47c98-138">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="47c98-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="47c98-139">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizar este cambio para cada bloque `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="47c98-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="47c98-140">Vea el [archivo completado SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="47c98-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="47c98-141">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="47c98-141">Build the solution.</span></span>

<span data-ttu-id="47c98-142"><a name="pmc"></a> En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="47c98-142"><a name="pmc"></a> From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="47c98-143">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="47c98-143">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Rating
Update-Database
```

<span data-ttu-id="47c98-144">El comando `Add-Migration` indica al marco de trabajo que:</span><span class="sxs-lookup"><span data-stu-id="47c98-144">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="47c98-145">Compare el modelo `Movie` con el esquema de base de datos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="47c98-145">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="47c98-146">Cree código para migrar el esquema de la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="47c98-146">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="47c98-147">El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="47c98-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="47c98-148">Resulta útil emplear un nombre descriptivo para el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="47c98-148">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="47c98-149"><a name="ssox"></a> Si elimina todos los registros de la base de datos, el inicializador inicializa la base de datos e incluye el campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="47c98-149"><a name="ssox"></a> If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="47c98-150">Puede hacerlo con los vínculos de eliminación en el explorador o desde el [Explorador de objetos de SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="47c98-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="47c98-151">Para eliminar la base de datos desde SSOX:</span><span class="sxs-lookup"><span data-stu-id="47c98-151">To delete the database from SSOX:</span></span>

* <span data-ttu-id="47c98-152">Seleccione la base de datos en SSOX.</span><span class="sxs-lookup"><span data-stu-id="47c98-152">Select the database in SSOX.</span></span>
* <span data-ttu-id="47c98-153">Haga clic con el botón derecho en la base de datos y seleccione *Eliminar*.</span><span class="sxs-lookup"><span data-stu-id="47c98-153">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="47c98-154">Active **Cerrar las conexiones existentes**.</span><span class="sxs-lookup"><span data-stu-id="47c98-154">Check **Close existing connections**.</span></span>
* <span data-ttu-id="47c98-155">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="47c98-155">Select **OK**.</span></span>
* <span data-ttu-id="47c98-156">En la [PMC](xref:tutorials/razor-pages/new-field#pmc), actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="47c98-156">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```PMC
  Update-Database
  ```

<span data-ttu-id="47c98-157">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="47c98-157">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="47c98-158">Si la base de datos no está inicializada, detenga IIS Express y luego ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="47c98-158">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="47c98-159">[Anterior: Adición de búsqueda](xref:tutorials/razor-pages/search)
[Siguiente: Adición de una validación](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="47c98-159">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
