---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Migraciones (4 de 8)'
author: rick-anderson
description: En este tutorial, empezará a usar la característica de EF Core de migraciones para administrar los cambios de modelos de datos en una aplicación de ASP.NET Core MVC.
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: d39e1aa40ff97d5b335f2bde6170242e89f6189a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272353"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="4acd7-103">Páginas de Razor con EF Core en ASP.NET Core: Migraciones (4 de 8)</span><span class="sxs-lookup"><span data-stu-id="4acd7-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="4acd7-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4acd7-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="4acd7-105">En este tutorial, se usa la característica de migraciones de EF Core para administrar cambios en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="4acd7-106">Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="4acd7-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="4acd7-107">Cuando se desarrolla una aplicación nueva, el modelo de datos cambia con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="4acd7-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="4acd7-108">Cada vez que el modelo cambia, este deja de estar sincronizado con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="4acd7-109">Este tutorial se inició con la configuración de Entity Framework para crear la base de datos si no existía.</span><span class="sxs-lookup"><span data-stu-id="4acd7-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="4acd7-110">Cada vez que los datos del modelo cambian:</span><span class="sxs-lookup"><span data-stu-id="4acd7-110">Each time the data model changes:</span></span>

* <span data-ttu-id="4acd7-111">Se quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-111">The DB is dropped.</span></span>
* <span data-ttu-id="4acd7-112">EF crea una que coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="4acd7-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="4acd7-113">La aplicación inicializa la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="4acd7-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="4acd7-114">Este enfoque para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción.</span><span class="sxs-lookup"><span data-stu-id="4acd7-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="4acd7-115">Cuando se ejecuta la aplicación en producción, normalmente está almacenando datos que hay que mantener.</span><span class="sxs-lookup"><span data-stu-id="4acd7-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="4acd7-116">No se puede iniciar la aplicación con una prueba de base de datos cada vez que se hace un cambio (por ejemplo, agregar una nueva columna).</span><span class="sxs-lookup"><span data-stu-id="4acd7-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="4acd7-117">La característica Migraciones de EF Core soluciona este problema habilitando EF Core para actualizar el esquema de la base de datos en lugar de crear una.</span><span class="sxs-lookup"><span data-stu-id="4acd7-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="4acd7-118">En lugar de quitar y volver a crear la base de datos cuando los datos del modelo cambian, las migraciones actualizan el esquema y conservan los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="4acd7-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="4acd7-119">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="4acd7-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="4acd7-120">Para trabajar con las migraciones, use la **Consola del Administrador de paquetes** (PMC) o la interfaz de la línea de comandos (CLI).</span><span class="sxs-lookup"><span data-stu-id="4acd7-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="4acd7-121">En estos tutoriales se muestra cómo usar los comandos de la CLI.</span><span class="sxs-lookup"><span data-stu-id="4acd7-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="4acd7-122">[Al final de este tutorial](#pmc) encontrará información sobre la PMC.</span><span class="sxs-lookup"><span data-stu-id="4acd7-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="4acd7-123">Las herramientas de EF Core para la interfaz de la línea de comandos (CLI) se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="4acd7-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="4acd7-124">Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="4acd7-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="4acd7-125">**Nota:** Debe instalarse este paquete mediante la edición del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4acd7-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="4acd7-126">No se puede usar el comando `install-package` o la interfaz gráfica de usuario del administrador de paquetes para instalar este paquete.</span><span class="sxs-lookup"><span data-stu-id="4acd7-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="4acd7-127">Edite el archivo *.csproj*; para ello, haga clic con el botón derecho en el nombre del proyecto en el **Explorador de soluciones** y seleccione **Editar ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="4acd7-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="4acd7-128">El siguiente marcado muestra el archivo *.csproj* actualizado con las herramientas de la CLI de EF Core resaltadas:</span><span class="sxs-lookup"><span data-stu-id="4acd7-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="4acd7-129">Los números de versión en el ejemplo anterior eran los actuales cuando se escribió el tutorial.</span><span class="sxs-lookup"><span data-stu-id="4acd7-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="4acd7-130">Use la misma versión para las herramientas de la CLI de EF Core disponibles en los otros paquetes.</span><span class="sxs-lookup"><span data-stu-id="4acd7-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="4acd7-131">Cambiar la cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="4acd7-131">Change the connection string</span></span>

<span data-ttu-id="4acd7-132">En el archivo *appsettings.json*, cambie el nombre de la base de datos en la cadena de conexión a ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="4acd7-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="4acd7-133">Cambiar el nombre de la base de datos en la cadena de conexión hace que la primera migración cree una base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="4acd7-134">Se crea una base de datos porque no existe ninguna con ese nombre.</span><span class="sxs-lookup"><span data-stu-id="4acd7-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="4acd7-135">No es necesario cambiar la cadena de conexión para comenzar a usar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4acd7-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="4acd7-136">Una alternativa a cambiar el nombre de la base de datos es eliminarla.</span><span class="sxs-lookup"><span data-stu-id="4acd7-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="4acd7-137">Use el **Explorador de objetos de SQL Server** (SSOX) o el comando de la CLI `database drop`:</span><span class="sxs-lookup"><span data-stu-id="4acd7-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="4acd7-138">En la siguiente sección se explica cómo ejecutar comandos de la CLI.</span><span class="sxs-lookup"><span data-stu-id="4acd7-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="4acd7-139">Crear una migración inicial</span><span class="sxs-lookup"><span data-stu-id="4acd7-139">Create an initial migration</span></span>

<span data-ttu-id="4acd7-140">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4acd7-140">Build the project.</span></span>

<span data-ttu-id="4acd7-141">Abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4acd7-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="4acd7-142">La carpeta del proyecto contiene el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4acd7-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="4acd7-143">Escriba lo siguiente en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="4acd7-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="4acd7-144">La ventana de comandos muestra información similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="4acd7-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="4acd7-145">Si se produce un error en la migración y se muestra el mensaje "*No se puede tener acceso al archivo... ContosoUniversity.dll ya que otro proceso lo está usando.*"</span><span class="sxs-lookup"><span data-stu-id="4acd7-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="4acd7-146">:</span><span class="sxs-lookup"><span data-stu-id="4acd7-146">is displayed:</span></span>

* <span data-ttu-id="4acd7-147">Detenga IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4acd7-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="4acd7-148">Salga y reinicie Visual Studio o</span><span class="sxs-lookup"><span data-stu-id="4acd7-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="4acd7-149">Busque el icono de IIS Express en la bandeja del sistema de Windows.</span><span class="sxs-lookup"><span data-stu-id="4acd7-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="4acd7-150">Haga clic con el botón derecho en el icono de IIS Express y, después, haga clic en **ContosoUniversity > Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="4acd7-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="4acd7-151">Si se muestra el mensaje de error "Error de complicación.",</span><span class="sxs-lookup"><span data-stu-id="4acd7-151">If the error message "Build failed."</span></span> <span data-ttu-id="4acd7-152">vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="4acd7-152">is displayed, run the command again.</span></span> <span data-ttu-id="4acd7-153">Si obtiene este error, deje una nota en la parte inferior de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="4acd7-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="4acd7-154">Examinar los métodos Up y Down</span><span class="sxs-lookup"><span data-stu-id="4acd7-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="4acd7-155">El comando de EF Core `migrations add` generó código desde donde crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="4acd7-156">Este código de migraciones se encuentra en el archivo *Migrations\<marca_de_tiempo>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="4acd7-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="4acd7-157">El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="4acd7-158">El método `Down` las elimina, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4acd7-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="4acd7-159">Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración.</span><span class="sxs-lookup"><span data-stu-id="4acd7-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="4acd7-160">Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="4acd7-161">El código anterior es para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="4acd7-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="4acd7-162">Ese código se creó cuando se ejecutó el comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="4acd7-163">El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se usa para el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="4acd7-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="4acd7-164">El nombre de la migración puede ser cualquier nombre de archivo válido.</span><span class="sxs-lookup"><span data-stu-id="4acd7-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="4acd7-165">Es más recomendable elegir una palabra o frase que resuma lo que se hace en la migración.</span><span class="sxs-lookup"><span data-stu-id="4acd7-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="4acd7-166">Por ejemplo, una migración que ha agregado una tabla de departamento podría denominarse "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="4acd7-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="4acd7-167">Si la migración inicial está creada y la base de datos existe:</span><span class="sxs-lookup"><span data-stu-id="4acd7-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="4acd7-168">Se genera el código de creación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="4acd7-169">El código de creación de la base de datos no tiene que ejecutarse porque la base de datos ya coincide con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="4acd7-170">Si el código de creación de la base de datos se está ejecutando, no hace ningún cambio porque la base de datos ya coincide con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="4acd7-171">Cuando la aplicación se implementa en un entorno nuevo, se debe ejecutar el código de creación de la base de datos para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="4acd7-172">Anteriormente, la cadena de conexión se cambió para usar un nombre nuevo para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="4acd7-173">La base de datos especificada no existe, por lo que las migraciones crean la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="4acd7-174">La instantánea del modelo de datos</span><span class="sxs-lookup"><span data-stu-id="4acd7-174">The data model snapshot</span></span>

<span data-ttu-id="4acd7-175">Las migraciones crean una *instantánea* del esquema de la base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="4acd7-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="4acd7-176">Cuando se agrega una migración, EF determina qué ha cambiado mediante la comparación del modelo de datos con el archivo de instantánea.</span><span class="sxs-lookup"><span data-stu-id="4acd7-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="4acd7-177">Cuando elimine una migración, use el comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="4acd7-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="4acd7-178">`dotnet ef migrations remove` elimina la migración y garantiza que la instantánea se restablece correctamente.</span><span class="sxs-lookup"><span data-stu-id="4acd7-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="4acd7-179">Vea [Migraciones en entornos de equipo](/ef/core/managing-schemas/migrations/teams) para más información sobre cómo se usa el archivo de instantánea.</span><span class="sxs-lookup"><span data-stu-id="4acd7-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="4acd7-180">Quitar EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="4acd7-180">Remove EnsureCreated</span></span>

<span data-ttu-id="4acd7-181">Para el desarrollo inicial se usó el comando `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="4acd7-182">En este tutorial, se usan las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4acd7-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="4acd7-183">`EnsureCreated` tiene las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="4acd7-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="4acd7-184">Omite las migraciones y crea la base de datos y el esquema.</span><span class="sxs-lookup"><span data-stu-id="4acd7-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="4acd7-185">No crea una tabla de migraciones.</span><span class="sxs-lookup"><span data-stu-id="4acd7-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="4acd7-186">*No* puede usarse con las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4acd7-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="4acd7-187">Está diseñado para crear prototipos rápidos o de prueba donde se quita y vuelve a crear la base de datos con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="4acd7-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="4acd7-188">Quite las siguientes líneas de `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="4acd7-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="4acd7-189">Aplicar la migración a la base de datos en desarrollo</span><span class="sxs-lookup"><span data-stu-id="4acd7-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="4acd7-190">En la ventana de comandos, escriba lo siguiente para crear la base de datos y las tablas.</span><span class="sxs-lookup"><span data-stu-id="4acd7-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="4acd7-191">Nota: Si el comando `update` devuelve el error "Error de compilación.":</span><span class="sxs-lookup"><span data-stu-id="4acd7-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="4acd7-192">Vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="4acd7-192">Run the command again.</span></span>
* <span data-ttu-id="4acd7-193">Si se vuelve a producir un error, salga de Visual Studio y luego ejecute el comando `update`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="4acd7-194">Deje un mensaje en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="4acd7-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="4acd7-195">El resultado del comando es similar al resultado del comando `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="4acd7-196">En el comando anterior se muestran los registros para los comandos de SQL que configuran la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="4acd7-197">La mayoría de los registros se omite en la siguiente salida de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4acd7-197">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="4acd7-198">Para reducir el nivel de detalle en los mensajes de registro, cámbielo en el archivo *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="4acd7-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="4acd7-199">Para obtener más información, vea [Introducción al registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="4acd7-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="4acd7-200">Use el **Explorador de objetos de SQL Server** para inspeccionar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="4acd7-201">Observe la adición de una tabla `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="4acd7-202">La tabla `__EFMigrationsHistory` realiza un seguimiento de las migraciones que se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4acd7-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="4acd7-203">Examine los datos de la tabla `__EFMigrationsHistory`, muestra una fila para la primera migración.</span><span class="sxs-lookup"><span data-stu-id="4acd7-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="4acd7-204">En el último registro del ejemplo de salida de la CLI anterior se muestra la instrucción INSERT que crea esta fila.</span><span class="sxs-lookup"><span data-stu-id="4acd7-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="4acd7-205">Ejecute la aplicación y compruebe que todo funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="4acd7-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="4acd7-206">Aplicar las migraciones en producción</span><span class="sxs-lookup"><span data-stu-id="4acd7-206">Applying migrations in production</span></span>

<span data-ttu-id="4acd7-207">Se recomienda que las aplicaciones de producción **no** llamen a [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4acd7-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="4acd7-208">No debe llamarse a `Migrate` desde una aplicación en la granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="4acd7-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="4acd7-209">Por ejemplo, si la aplicación se ha implementado en la nube con escalado horizontal (se ejecutan varias instancias de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="4acd7-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="4acd7-210">La migración de bases de datos debe realizarse como parte de la implementación y de un modo controlado.</span><span class="sxs-lookup"><span data-stu-id="4acd7-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="4acd7-211">Entre los métodos de migración de base de datos de producción se incluyen:</span><span class="sxs-lookup"><span data-stu-id="4acd7-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="4acd7-212">Uso de las migraciones para crear scripts SQL y uso de scripts SQL en la implementación.</span><span class="sxs-lookup"><span data-stu-id="4acd7-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="4acd7-213">Ejecución de `dotnet ef database update` desde un entorno controlado.</span><span class="sxs-lookup"><span data-stu-id="4acd7-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="4acd7-214">EF Core usa la tabla `__MigrationsHistory` para ver si es necesario ejecutar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4acd7-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="4acd7-215">Si la base de datos está actualizada, no se ejecuta la migración.</span><span class="sxs-lookup"><span data-stu-id="4acd7-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="4acd7-216">Diferencias entre la interfaz de la línea de comandos (CLI) y la Consola del Administrador de paquetes (PMC)</span><span class="sxs-lookup"><span data-stu-id="4acd7-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="4acd7-217">El conjunto de herramientas de EF Core para administrar las migraciones está disponible desde:</span><span class="sxs-lookup"><span data-stu-id="4acd7-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="4acd7-218">Comandos de la CLI de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4acd7-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="4acd7-219">Los cmdlets de PowerShell en la ventana **Consola del Administrador de paquetes** (PMC) de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4acd7-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="4acd7-220">Este tutorial muestra cómo usar la CLI, algunos desarrolladores prefieren usar la PMC.</span><span class="sxs-lookup"><span data-stu-id="4acd7-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="4acd7-221">Los comandos de EF Core para la PMC están en el paquete [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="4acd7-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="4acd7-222">Este paquete se incluye en el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), por lo que no es necesario instalarlo.</span><span class="sxs-lookup"><span data-stu-id="4acd7-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="4acd7-223">**Importante:** Este no es el mismo paquete que el que se instala para la CLI mediante la edición del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4acd7-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="4acd7-224">El nombre de este paquete termina en `Tools`, a diferencia del nombre de paquete de la CLI que termina en `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="4acd7-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="4acd7-225">Para obtener más información sobre los comandos de la CLI, vea [CLI de .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="4acd7-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="4acd7-226">Para obtener más información sobre los comandos de la PMC, vea [Consola del Administrador de paquetes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="4acd7-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4acd7-227">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="4acd7-227">Troubleshooting</span></span>

<span data-ttu-id="4acd7-228">Descargue la [aplicación completa de esta fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="4acd7-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="4acd7-229">La aplicación genera la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="4acd7-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="4acd7-230">Solución: ejecute `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="4acd7-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="4acd7-231">Si el comando `update` devuelve el error "Error de compilación.":</span><span class="sxs-lookup"><span data-stu-id="4acd7-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="4acd7-232">Vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="4acd7-232">Run the command again.</span></span>
* <span data-ttu-id="4acd7-233">Deje un mensaje en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="4acd7-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4acd7-234">[Anterior](xref:data/ef-rp/sort-filter-page)
> [Siguiente](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="4acd7-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
