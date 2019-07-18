---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Migraciones (4 de 8)'
author: rick-anderson
description: En este tutorial, empezará a usar la característica de EF Core de migraciones para administrar los cambios de modelos de datos en una aplicación de ASP.NET Core MVC.
ms.author: riande
ms.date: 06/30/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 54225a8126e04eb4ff3a6a0cde9d305249299887
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223866"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="1ccaf-103">Páginas de Razor con EF Core en ASP.NET Core: Migraciones (4 de 8)</span><span class="sxs-lookup"><span data-stu-id="1ccaf-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="1ccaf-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ccaf-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="1ccaf-105">En este tutorial, se usa la característica de migraciones de EF Core para administrar cambios en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="1ccaf-106">Si experimenta problemas que no puede resolver, descargue la [aplicación completada](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="1ccaf-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="1ccaf-107">Cuando se desarrolla una aplicación nueva, el modelo de datos cambia con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="1ccaf-108">Cada vez que el modelo cambia, este deja de estar sincronizado con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="1ccaf-109">Este tutorial se inició con la configuración de Entity Framework para crear la base de datos si no existía.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="1ccaf-110">Cada vez que los datos del modelo cambian:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-110">Each time the data model changes:</span></span>

* <span data-ttu-id="1ccaf-111">Se quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-111">The DB is dropped.</span></span>
* <span data-ttu-id="1ccaf-112">EF crea una que coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="1ccaf-113">La aplicación inicializa la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="1ccaf-114">Este enfoque para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="1ccaf-115">Cuando se ejecuta la aplicación en producción, normalmente está almacenando datos que hay que mantener.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="1ccaf-116">No se puede iniciar la aplicación con una prueba de base de datos cada vez que se hace un cambio (por ejemplo, agregar una nueva columna).</span><span class="sxs-lookup"><span data-stu-id="1ccaf-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="1ccaf-117">La característica Migraciones de EF Core soluciona este problema habilitando EF Core para actualizar el esquema de la base de datos en lugar de crear una.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="1ccaf-118">En lugar de quitar y volver a crear la base de datos cuando los datos del modelo cambian, las migraciones actualizan el esquema y conservan los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="1ccaf-119">Eliminación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="1ccaf-119">Drop the database</span></span>

<span data-ttu-id="1ccaf-120">Use el **Explorador de objetos de SQL Server** (SSOX) o el comando `database drop`:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ccaf-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ccaf-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ccaf-122">En la **Consola del Administrador de paquetes** (PMC), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="1ccaf-123">Ejecute `Get-Help about_EntityFrameworkCore` desde PMC para obtener información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ccaf-124">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1ccaf-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1ccaf-125">Abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="1ccaf-126">La carpeta del proyecto contiene el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="1ccaf-127">Escriba lo siguiente en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="1ccaf-128">Creación de una migración inicial y actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="1ccaf-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="1ccaf-129">Compile el proyecto y cree la primera migración.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ccaf-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ccaf-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ccaf-131">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1ccaf-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="1ccaf-132">Examinar los métodos Up y Down</span><span class="sxs-lookup"><span data-stu-id="1ccaf-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="1ccaf-133">El comando `migrations add` de EF Core ha generado código para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="1ccaf-134">Este código de migraciones se encuentra en el archivo *Migrations\<marca_de_tiempo>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1ccaf-135">El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="1ccaf-136">El método `Down` las elimina, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="1ccaf-137">Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="1ccaf-138">Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="1ccaf-139">El código anterior es para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="1ccaf-140">Ese código se creó cuando se ejecutó el comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="1ccaf-141">El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se usa para el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="1ccaf-142">El nombre de la migración puede ser cualquier nombre de archivo válido.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="1ccaf-143">Es más recomendable elegir una palabra o frase que resuma lo que se hace en la migración.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="1ccaf-144">Por ejemplo, una migración que ha agregado una tabla de departamento podría denominarse "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="1ccaf-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="1ccaf-145">Si la migración inicial está creada y la base de datos existe:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="1ccaf-146">Se genera el código de creación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="1ccaf-147">El código de creación de la base de datos no tiene que ejecutarse porque la base de datos ya coincide con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="1ccaf-148">Si el código de creación de la base de datos se está ejecutando, no hace ningún cambio porque la base de datos ya coincide con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="1ccaf-149">Cuando la aplicación se implementa en un entorno nuevo, se debe ejecutar el código de creación de la base de datos para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="1ccaf-150">Anteriormente, la base de datos se eliminó, de modo que ya no existe y ahora se crea mediante las migraciones.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="1ccaf-151">La instantánea del modelo de datos</span><span class="sxs-lookup"><span data-stu-id="1ccaf-151">The data model snapshot</span></span>

<span data-ttu-id="1ccaf-152">Las migraciones crean una *instantánea* del esquema de la base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="1ccaf-153">Cuando se agrega una migración, EF determina qué ha cambiado mediante la comparación del modelo de datos con el archivo de instantánea.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="1ccaf-154">Para eliminar una migración, use el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1ccaf-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ccaf-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1ccaf-156">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="1ccaf-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1ccaf-157">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1ccaf-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="1ccaf-158">Para obtener más información, vea [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="1ccaf-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

---

<span data-ttu-id="1ccaf-159">El comando remove migrations elimina la migración y garantiza que la instantánea se restablece correctamente.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="1ccaf-160">Eliminación de EnsureCreated y prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="1ccaf-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="1ccaf-161">Para el desarrollo inicial se ha utilizado `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="1ccaf-162">En este tutorial, se usan las migraciones.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="1ccaf-163">`EnsureCreated` tiene las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="1ccaf-164">Omite las migraciones y crea la base de datos y el esquema.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="1ccaf-165">No crea una tabla de migraciones.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="1ccaf-166">*No* puede usarse con las migraciones.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="1ccaf-167">Está diseñado para crear prototipos rápidos o de prueba donde se quita y vuelve a crear la base de datos con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="1ccaf-168">Quite `EnsureCreated`:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-168">Remove `EnsureCreated`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="1ccaf-169">Ejecute la aplicación y compruebe que la base de datos se haya inicializado.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="1ccaf-170">Inspección de la base de datos</span><span class="sxs-lookup"><span data-stu-id="1ccaf-170">Inspect the database</span></span>

<span data-ttu-id="1ccaf-171">Use el **Explorador de objetos de SQL Server** para inspeccionar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="1ccaf-172">Observe la adición de una tabla `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="1ccaf-173">La tabla `__EFMigrationsHistory` realiza un seguimiento de las migraciones que se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="1ccaf-174">Examine los datos de la tabla `__EFMigrationsHistory`, muestra una fila para la primera migración.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="1ccaf-175">En el último registro del ejemplo de salida de la CLI anterior se muestra la instrucción INSERT que crea esta fila.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="1ccaf-176">Ejecute la aplicación y compruebe que todo funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="1ccaf-177">Aplicar las migraciones en producción</span><span class="sxs-lookup"><span data-stu-id="1ccaf-177">Applying migrations in production</span></span>

<span data-ttu-id="1ccaf-178">Se recomienda que las aplicaciones de producción **no** llamen a [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="1ccaf-179">No debe llamarse a `Migrate` desde una aplicación en la granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="1ccaf-180">Por ejemplo, si la aplicación se ha implementado en la nube con escalado horizontal (se ejecutan varias instancias de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="1ccaf-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="1ccaf-181">La migración de bases de datos debe realizarse como parte de la implementación y de un modo controlado.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="1ccaf-182">Entre los métodos de migración de base de datos de producción se incluyen:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="1ccaf-183">Uso de las migraciones para crear scripts SQL y uso de scripts SQL en la implementación.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="1ccaf-184">Ejecución de `dotnet ef database update` desde un entorno controlado.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="1ccaf-185">EF Core usa la tabla `__MigrationsHistory` para ver si es necesario ejecutar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="1ccaf-186">Si la base de datos está actualizada, no se ejecuta ninguna migración.</span><span class="sxs-lookup"><span data-stu-id="1ccaf-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1ccaf-187">solución de problemas</span><span class="sxs-lookup"><span data-stu-id="1ccaf-187">Troubleshooting</span></span>

<span data-ttu-id="1ccaf-188">Descargue la [aplicación completada](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1ccaf-188">Download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1ccaf-189">La aplicación genera la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="1ccaf-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="1ccaf-190">Solución: Ejecute `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="1ccaf-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1ccaf-191">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1ccaf-191">Additional resources</span></span>

* [<span data-ttu-id="1ccaf-192">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="1ccaf-192">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="1ccaf-193">[CLI de .NET Core](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1ccaf-193">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="1ccaf-194">Consola del administrador de paquetes (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="1ccaf-194">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> <span data-ttu-id="1ccaf-195">[Anterior](xref:data/ef-rp/sort-filter-page)
> [Siguiente](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="1ccaf-195">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
