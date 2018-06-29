---
title: Agregar un campo nuevo a una página de Razor en ASP.NET Core
author: rick-anderson
description: Muestra cómo agregar un nuevo campo a una página de Razor con Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d9bf8c7cea20bf38aacf432465d7b33514bcd64d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277298"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="24591-103">Agregar un campo nuevo a una página de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24591-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="24591-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="24591-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="24591-105">En esta sección se usa Migraciones de [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para agregar un nuevo campo al modelo y migrar ese cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24591-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="24591-106">Al usar EF Code First para crear una base de datos automáticamente, Code First hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24591-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="24591-107">Agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado.</span><span class="sxs-lookup"><span data-stu-id="24591-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="24591-108">Si las clases del modelo no están sincronizadas con la base de datos, EF produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="24591-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="24591-109">La comprobación automática de la sincronización del esquema/modelo facilita la detección de problemas de código o base de datos incoherentes.</span><span class="sxs-lookup"><span data-stu-id="24591-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="24591-110">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="24591-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="24591-111">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="24591-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="24591-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="24591-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="24591-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="24591-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>

::: moniker-end

<span data-ttu-id="24591-114">Compile la aplicación (Ctrl + Mayús + B).</span><span class="sxs-lookup"><span data-stu-id="24591-114">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="24591-115">Edite *Pages/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="24591-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="24591-116">Agregue el campo `Rating` a las páginas Delete y Details.</span><span class="sxs-lookup"><span data-stu-id="24591-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="24591-117">Actualice *Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="24591-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="24591-118">Puede copiar o pegar el elemento `<div>` anterior y permitir que IntelliSense le ayude a actualizar los campos.</span><span class="sxs-lookup"><span data-stu-id="24591-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="24591-119">IntelliSense funciona con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="24591-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![El desarrollador ha escrito la letra R para el valor del atributo de asp-for en el segundo elemento de etiqueta de la vista.](new-field/_static/cr.png)

<span data-ttu-id="24591-123">El código siguiente muestra *Create.cshtml* con un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="24591-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="24591-124">Agregue el campo `Rating` a la página de edición.</span><span class="sxs-lookup"><span data-stu-id="24591-124">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="24591-125">La aplicación no funciona hasta que la base de datos se actualiza para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="24591-125">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="24591-126">Si se ejecuta ahora, la aplicación produce una `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="24591-126">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="24591-127">Este error se debe a que la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24591-127">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="24591-128">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="24591-128">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="24591-129">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="24591-129">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="24591-130">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear con el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="24591-130">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="24591-131">Este enfoque resulta conveniente al principio del ciclo de desarrollo; permite desarrollar a la vez y de manera rápida el esquema del modelo y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24591-131">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="24591-132">El inconveniente es que se pierden los datos existentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24591-132">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="24591-133">No use este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="24591-133">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="24591-134">El quitar la base de datos en los cambios de esquema y el usar un inicializador para inicializar automáticamente la base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="24591-134">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="24591-135">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="24591-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="24591-136">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="24591-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="24591-137">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="24591-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="24591-138">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24591-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="24591-139">Para este tutorial, use Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="24591-139">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="24591-140">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="24591-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="24591-141">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizar este cambio para cada bloque `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="24591-141">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="24591-142">Vea el [archivo completado SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="24591-142">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="24591-143">Vea el [archivo completado SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="24591-143">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>
::: moniker-end

<span data-ttu-id="24591-144">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="24591-144">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="24591-145">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="24591-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="24591-146">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="24591-146">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="24591-147">El comando `Add-Migration` indica al marco de trabajo que:</span><span class="sxs-lookup"><span data-stu-id="24591-147">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="24591-148">Compare el modelo `Movie` con el esquema de base de datos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="24591-148">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="24591-149">Cree código para migrar el esquema de la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="24591-149">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="24591-150">El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="24591-150">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="24591-151">Resulta útil emplear un nombre descriptivo para el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="24591-151">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="24591-152">Si elimina todos los registros de la base de datos, el inicializador inicializa la base de datos e incluye el campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="24591-152">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="24591-153">Puede hacerlo con los vínculos de eliminación en el explorador o desde el [Explorador de objetos de SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="24591-153">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="24591-154">Para eliminar la base de datos desde SSOX:</span><span class="sxs-lookup"><span data-stu-id="24591-154">To delete the database from SSOX:</span></span>

* <span data-ttu-id="24591-155">Seleccione la base de datos en SSOX.</span><span class="sxs-lookup"><span data-stu-id="24591-155">Select the database in SSOX.</span></span>
* <span data-ttu-id="24591-156">Haga clic con el botón derecho en la base de datos y seleccione *Eliminar*.</span><span class="sxs-lookup"><span data-stu-id="24591-156">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="24591-157">Active **Cerrar las conexiones existentes**.</span><span class="sxs-lookup"><span data-stu-id="24591-157">Check **Close existing connections**.</span></span>
* <span data-ttu-id="24591-158">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="24591-158">Select **OK**.</span></span>
* <span data-ttu-id="24591-159">En la [PMC](xref:tutorials/razor-pages/new-field#pmc), actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="24591-159">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="24591-160">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="24591-160">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="24591-161">Si la base de datos no está inicializada, detenga IIS Express y luego ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="24591-161">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="24591-162">[Anterior: Adición de búsqueda](xref:tutorials/razor-pages/search)
> [Siguiente: Adición de una validación](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="24591-162">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
