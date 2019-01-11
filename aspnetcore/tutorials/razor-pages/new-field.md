---
title: Agregar un campo nuevo a una página de Razor en ASP.NET Core
author: rick-anderson
description: Muestra cómo agregar un nuevo campo a una página de Razor con Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 9b3ad5f6c4b1c9b5f016f5591127c8d1b213948d
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329138"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="85d37-103">Agregar un campo nuevo a una página de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85d37-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="85d37-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="85d37-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="85d37-105">En esta sección, Migraciones de [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First se utiliza para:</span><span class="sxs-lookup"><span data-stu-id="85d37-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="85d37-106">Agregar un campo nuevo al modelo.</span><span class="sxs-lookup"><span data-stu-id="85d37-106">Add a new field to the model.</span></span>
* <span data-ttu-id="85d37-107">Migrar el cambio de esquema del campo nuevo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="85d37-108">Al usar EF Code First para crear una base de datos automáticamente, Code First hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="85d37-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="85d37-109">Agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado.</span><span class="sxs-lookup"><span data-stu-id="85d37-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="85d37-110">Si las clases del modelo no están sincronizadas con la base de datos, EF produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="85d37-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="85d37-111">La comprobación automática de la sincronización del esquema/modelo facilita la detección de problemas de código o base de datos incoherentes.</span><span class="sxs-lookup"><span data-stu-id="85d37-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="85d37-112">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="85d37-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="85d37-113">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="85d37-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="85d37-114">Compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="85d37-114">Build the app.</span></span>

<span data-ttu-id="85d37-115">Edite *Pages/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="85d37-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="85d37-116">Actualice las páginas siguientes:</span><span class="sxs-lookup"><span data-stu-id="85d37-116">Update the following pages:</span></span>

* <span data-ttu-id="85d37-117">Agregue el campo `Rating` a las páginas Delete y Details.</span><span class="sxs-lookup"><span data-stu-id="85d37-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="85d37-118">Actualice [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="85d37-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="85d37-119">Agregue el campo `Rating` a la página de edición.</span><span class="sxs-lookup"><span data-stu-id="85d37-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="85d37-120">La aplicación no funciona hasta que la base de datos se actualiza para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="85d37-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="85d37-121">Si se ejecuta ahora, la aplicación produce una `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="85d37-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="85d37-122">Este error se debe a que la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="85d37-123">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="85d37-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="85d37-124">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="85d37-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="85d37-125">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear con el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="85d37-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="85d37-126">Este enfoque resulta conveniente al principio del ciclo de desarrollo; permite desarrollar a la vez y de manera rápida el esquema del modelo y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="85d37-127">El inconveniente es que se pierden los datos existentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="85d37-128">No use este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="85d37-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="85d37-129">El quitar la base de datos en los cambios de esquema y el usar un inicializador para inicializar automáticamente la base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="85d37-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="85d37-130">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="85d37-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="85d37-131">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="85d37-132">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="85d37-133">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="85d37-134">Para este tutorial, use Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="85d37-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="85d37-135">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="85d37-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="85d37-136">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizar este cambio para cada bloque `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="85d37-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="85d37-137">Vea el [archivo completado SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="85d37-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="85d37-138">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="85d37-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85d37-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85d37-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="85d37-140">Agregar una migración para el campo de clasificación</span><span class="sxs-lookup"><span data-stu-id="85d37-140">Add a migration for the rating field</span></span>

<span data-ttu-id="85d37-141">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="85d37-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="85d37-142">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="85d37-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="85d37-143">El comando `Add-Migration` indica al marco de trabajo que:</span><span class="sxs-lookup"><span data-stu-id="85d37-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="85d37-144">Compare el modelo `Movie` con el esquema de base de datos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="85d37-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="85d37-145">Cree código para migrar el esquema de la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="85d37-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="85d37-146">El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="85d37-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="85d37-147">Resulta útil emplear un nombre descriptivo para el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="85d37-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="85d37-148">El comando `Update-Database` le indica al marco que aplique los cambios de esquema a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="85d37-149">Si elimina todos los registros de la base de datos, el inicializador inicializará la base de datos e incluirá el campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="85d37-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="85d37-150">Puede hacerlo con los vínculos de eliminación en el explorador o desde el [Explorador de objetos de SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="85d37-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="85d37-151">Otra opción es eliminar la base de datos y usar las migraciones para volver a crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="85d37-152">Para eliminar la base de datos de SSOX:</span><span class="sxs-lookup"><span data-stu-id="85d37-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="85d37-153">Seleccione la base de datos en SSOX.</span><span class="sxs-lookup"><span data-stu-id="85d37-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="85d37-154">Haga clic con el botón derecho en la base de datos y seleccione *Eliminar*.</span><span class="sxs-lookup"><span data-stu-id="85d37-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="85d37-155">Active **Cerrar las conexiones existentes**.</span><span class="sxs-lookup"><span data-stu-id="85d37-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="85d37-156">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="85d37-156">Select **OK**.</span></span>
* <span data-ttu-id="85d37-157">En la [PMC](xref:tutorials/razor-pages/new-field#pmc), actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="85d37-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="85d37-158">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="85d37-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

<span data-ttu-id="85d37-159">Ejecute los siguientes comandos CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="85d37-159">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<span data-ttu-id="85d37-160">El comando `ef migrations add` indica al marco de trabajo que:</span><span class="sxs-lookup"><span data-stu-id="85d37-160">The `ef migrations add` command tells the framework to:</span></span>

* <span data-ttu-id="85d37-161">Compare el modelo `Movie` con el esquema de base de datos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="85d37-161">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="85d37-162">Cree código para migrar el esquema de la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="85d37-162">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="85d37-163">El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="85d37-163">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="85d37-164">Resulta útil emplear un nombre descriptivo para el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="85d37-164">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="85d37-165">El comando `ef database update` le indica al marco que aplique los cambios de esquema a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-165">The `ef database update` command tells the framework to apply the schema changes to the database.</span></span>

<span data-ttu-id="85d37-166">Si elimina todos los registros de la base de datos, el inicializador inicializará la base de datos e incluirá el campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="85d37-166">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="85d37-167">Puede hacerlo con los vínculos de eliminación del explorador o mediante la herramienta SQLite.</span><span class="sxs-lookup"><span data-stu-id="85d37-167">You can do this with the delete links in the browser or by using a SQLite tool.</span></span>

<span data-ttu-id="85d37-168">Otra opción es eliminar la base de datos y usar las migraciones para volver a crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="85d37-168">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="85d37-169">Para eliminar la base de datos, elimine el archivo de base de datos (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="85d37-169">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="85d37-170">Luego, ejecute el comando `ef database update`:</span><span class="sxs-lookup"><span data-stu-id="85d37-170">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

> [!NOTE]
> <span data-ttu-id="85d37-171">Muchas operaciones de cambio de esquema no se admiten con el proveedor de SQLite de EF Core.</span><span class="sxs-lookup"><span data-stu-id="85d37-171">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="85d37-172">Por ejemplo, se permite agregar una columna, pero no eliminarla.</span><span class="sxs-lookup"><span data-stu-id="85d37-172">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="85d37-173">Si agrega una migración para quitar una columna, el comando `ef migrations add` se ejecuta correctamente, pero el comando `ef database update` produce un error.</span><span class="sxs-lookup"><span data-stu-id="85d37-173">If you add a migration to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="85d37-174">Puede solucionar algunas de las limitaciones escribiendo manualmente el código de las migraciones para llevar a cabo una recompilación de la tabla.</span><span class="sxs-lookup"><span data-stu-id="85d37-174">You can work around some of the limitations by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="85d37-175">Una recompilación de la tabla implica cambiar el nombre de la tabla existente, crear otra tabla, copiar los datos en la tabla nueva y eliminar la anterior.</span><span class="sxs-lookup"><span data-stu-id="85d37-175">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="85d37-176">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="85d37-176">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="85d37-177">Limitaciones del proveedor de base de datos SQLite para EF Core</span><span class="sxs-lookup"><span data-stu-id="85d37-177">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="85d37-178">Personalización del código de migración</span><span class="sxs-lookup"><span data-stu-id="85d37-178">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="85d37-179">Propagación de los datos</span><span class="sxs-lookup"><span data-stu-id="85d37-179">Data seeding</span></span>](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

<span data-ttu-id="85d37-180">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="85d37-180">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="85d37-181">Si la base de datos no se ha propagado, establezca un punto de interrupción en el método `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="85d37-181">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85d37-182">[Anterior: Adición de búsqueda](xref:tutorials/razor-pages/search)
> [Siguiente: Adición de validación](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="85d37-182">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
