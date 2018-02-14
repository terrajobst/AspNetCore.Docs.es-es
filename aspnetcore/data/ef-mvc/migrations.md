---
title: 'ASP.NET Core MVC con EF Core: Migraciones (4 de 10)'
author: tdykstra
description: "En este tutorial, empezará usando la característica de migraciones de EF Core para administrar cambios en el modelo de datos en una aplicación ASP.NET Core MVC."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="491a9-103">Migraciones: tutorial de EF Core con ASP.NET Core MVC (4 de 10)</span><span class="sxs-lookup"><span data-stu-id="491a9-103">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="491a9-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="491a9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="491a9-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web de ASP.NET Core MVC con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="491a9-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="491a9-106">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="491a9-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="491a9-107">En este tutorial, empezará usando la característica de migraciones de EF Core para administrar cambios en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="491a9-108">En los tutoriales posteriores, agregará más migraciones a medida que cambie el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="491a9-109">Introducción a las migraciones</span><span class="sxs-lookup"><span data-stu-id="491a9-109">Introduction to migrations</span></span>

<span data-ttu-id="491a9-110">Al desarrollar una aplicación nueva, el modelo de datos cambia con frecuencia y, cada vez que lo hace, se deja de sincronizar con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="491a9-111">Estos tutoriales se iniciaron con la configuración de Entity Framework para crear la base de datos si no existía.</span><span class="sxs-lookup"><span data-stu-id="491a9-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="491a9-112">Después, cada vez que cambie el modelo de datos (agregar, quitar o cambiar las clases de entidad, o bien cambiar la clase DbContext), puede eliminar la base de datos y EF crea una que coincida con el modelo y la inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="491a9-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="491a9-113">Este método para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción.</span><span class="sxs-lookup"><span data-stu-id="491a9-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="491a9-114">Cuando la aplicación se ejecuta en producción, normalmente almacena los datos que le interesa mantener y no querrá perderlo todo cada vez que realice un cambio, como al agregar una columna nueva.</span><span class="sxs-lookup"><span data-stu-id="491a9-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="491a9-115">La característica Migraciones de EF Core soluciona este problema habilitando EF para actualizar el esquema de la base de datos en lugar de crear una.</span><span class="sxs-lookup"><span data-stu-id="491a9-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="491a9-116">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="491a9-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="491a9-117">Para trabajar con las migraciones, puede usar la **Consola del Administrador de paquetes** (PMC) o la interfaz de la línea de comandos (CLI).</span><span class="sxs-lookup"><span data-stu-id="491a9-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="491a9-118">En estos tutoriales se muestra cómo usar los comandos de la CLI.</span><span class="sxs-lookup"><span data-stu-id="491a9-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="491a9-119">[Al final de este tutorial](#pmc) encontrará información sobre la PMC.</span><span class="sxs-lookup"><span data-stu-id="491a9-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="491a9-120">Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="491a9-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="491a9-121">Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="491a9-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="491a9-122">**Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="491a9-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="491a9-123">Puede editar el archivo *.csproj* si hace clic con el botón derecho en el nombre del proyecto en el **Explorador de soluciones** y selecciona **Editar ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="491a9-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="491a9-124">(Los números de versión en este ejemplo eran los actuales cuando se escribió el tutorial).</span><span class="sxs-lookup"><span data-stu-id="491a9-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="491a9-125">Cambiar la cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="491a9-125">Change the connection string</span></span>

<span data-ttu-id="491a9-126">En el archivo *appsettings.json*, cambie el nombre de la base de datos en la cadena de conexión por ContosoUniversity2 u otro nombre que no haya usado en el equipo que esté usando.</span><span class="sxs-lookup"><span data-stu-id="491a9-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="491a9-127">Este cambio configura el proyecto para que la primera migración cree una base de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="491a9-128">Esto no es necesario para comenzar a usar las migraciones, pero más adelante se verá por qué es una buena idea.</span><span class="sxs-lookup"><span data-stu-id="491a9-128">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="491a9-129">Como alternativa a cambiar el nombre de la base de datos, puede eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="491a9-130">Use el **Explorador de objetos de SQL Server** (SSOX) o el comando de la CLI `database drop`:</span><span class="sxs-lookup"><span data-stu-id="491a9-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="491a9-131">En la siguiente sección se explica cómo ejecutar comandos de la CLI.</span><span class="sxs-lookup"><span data-stu-id="491a9-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="491a9-132">Crear una migración inicial</span><span class="sxs-lookup"><span data-stu-id="491a9-132">Create an initial migration</span></span>

<span data-ttu-id="491a9-133">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="491a9-133">Save your changes and build the project.</span></span> <span data-ttu-id="491a9-134">Después, abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="491a9-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="491a9-135">Esta es una forma rápida de hacerlo:</span><span class="sxs-lookup"><span data-stu-id="491a9-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="491a9-136">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y elija **Abrir en el Explorador de archivos** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="491a9-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Elemento de menú Abrir en el Explorador de archivos](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="491a9-138">Escriba "cmd" en la barra de direcciones y presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="491a9-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Abrir ventana de comandos](migrations/_static/open-command-window.png)

<span data-ttu-id="491a9-140">Escriba el siguiente comando en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="491a9-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="491a9-141">En la ventana de comandos verá un resultado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="491a9-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="491a9-142">Si ve un mensaje de error *No se encuentra ningún archivo ejecutable que coincida con el comando "dotnet-ef"*, vea [esta entrada de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para obtener ayuda para solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="491a9-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="491a9-143">Si ve un mensaje de error "*No se puede obtener acceso al archivo... ContosoUniversity.dll porque lo está usando otro proceso.*", busque el icono de IIS Express en la bandeja del sistema de Windows, haga clic con el botón derecho en él y, después, haga clic en **ContosoUniversity > Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="491a9-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="491a9-144">Examinar los métodos Up y Down</span><span class="sxs-lookup"><span data-stu-id="491a9-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="491a9-145">Cuando ejecutó el comando `migrations add`, EF generó el código que va a crear la base de datos desde cero.</span><span class="sxs-lookup"><span data-stu-id="491a9-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="491a9-146">Este código está en la carpeta *Migrations*, en el archivo denominado *\<marca_de_tiempo>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="491a9-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="491a9-147">El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos y el método `Down` las elimina, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="491a9-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="491a9-148">Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración.</span><span class="sxs-lookup"><span data-stu-id="491a9-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="491a9-149">Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.</span><span class="sxs-lookup"><span data-stu-id="491a9-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="491a9-150">Este código es para la migración inicial que se creó cuando se escribió el comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="491a9-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="491a9-151">El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se usa para el nombre de archivo y puede ser lo que quiera.</span><span class="sxs-lookup"><span data-stu-id="491a9-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="491a9-152">Es más recomendable elegir una palabra o frase que resuma lo que se hace en la migración.</span><span class="sxs-lookup"><span data-stu-id="491a9-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="491a9-153">Por ejemplo, podría denominar "AddDepartmentTable" a una migración posterior.</span><span class="sxs-lookup"><span data-stu-id="491a9-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="491a9-154">Si creó la migración inicial cuando la base de datos ya existía, se genera el código de creación de la base de datos pero no es necesario ejecutarlo porque la base de datos ya coincide con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="491a9-155">Al implementar la aplicación en otro entorno donde la base de datos todavía no existe, se ejecutará este código para crear la base de datos, por lo que es recomendable probarlo primero.</span><span class="sxs-lookup"><span data-stu-id="491a9-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="491a9-156">Por ese motivo se cambió antes el nombre de la base de datos en la cadena de conexión, para que las migraciones puedan crear uno desde cero.</span><span class="sxs-lookup"><span data-stu-id="491a9-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="491a9-157">Examinar la instantánea del modelo de datos</span><span class="sxs-lookup"><span data-stu-id="491a9-157">Examine the data model snapshot</span></span>

<span data-ttu-id="491a9-158">Migrations también crea una *instantánea* del esquema de la base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="491a9-158">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="491a9-159">Este es el aspecto de ese código:</span><span class="sxs-lookup"><span data-stu-id="491a9-159">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="491a9-160">Como el esquema de la base de datos actual se representa en el código, EF Core no tiene que interactuar con la base de datos para crear las migraciones.</span><span class="sxs-lookup"><span data-stu-id="491a9-160">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="491a9-161">Cuando se agrega una migración, EF determina qué ha cambiado mediante la comparación del modelo de datos con el archivo de instantánea.</span><span class="sxs-lookup"><span data-stu-id="491a9-161">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="491a9-162">EF interactúa con la base de datos solo cuando tiene que actualizarla.</span><span class="sxs-lookup"><span data-stu-id="491a9-162">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="491a9-163">El archivo de instantánea tiene que estar sincronizado con las migraciones que lo crean, por lo que no se puede quitar una migración eliminando simplemente el archivo denominado *\<marca_de_tiempo>_\<nombre_de_la_migración>.cs*.</span><span class="sxs-lookup"><span data-stu-id="491a9-163">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="491a9-164">Si elimina ese archivo, las migraciones restantes no estarán sincronizadas con el archivo de instantánea de base de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-164">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="491a9-165">Para eliminar la última migración que se ha agregado, use el comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="491a9-165">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="491a9-166">Aplicar la migración a la base de datos</span><span class="sxs-lookup"><span data-stu-id="491a9-166">Apply the migration to the database</span></span>

<span data-ttu-id="491a9-167">En la ventana de comandos, escriba el comando siguiente para crear la base de datos y tablas en su interior.</span><span class="sxs-lookup"><span data-stu-id="491a9-167">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="491a9-168">El resultado del comando es similar al comando `migrations add`, con la excepción de que verá registros para los comandos SQL que configuran la base de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-168">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="491a9-169">La mayoría de los registros se omite en la siguiente salida de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="491a9-169">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="491a9-170">Si prefiere no ver este nivel de detalle en los mensajes de registro, puede cambiarlo en el archivo *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="491a9-170">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="491a9-171">Para obtener más información, vea [Introducción al registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="491a9-171">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="491a9-172">Use el **Explorador de objetos de SQL Server** para inspeccionar la base de datos como hizo en el primer tutorial.</span><span class="sxs-lookup"><span data-stu-id="491a9-172">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="491a9-173">Observará la adición de una tabla __EFMigrationsHistory que realiza el seguimiento de las migraciones que se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-173">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="491a9-174">Si examina los datos de esa tabla, verá una fila para la primera migración.</span><span class="sxs-lookup"><span data-stu-id="491a9-174">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="491a9-175">(En el último registro del ejemplo de salida de la CLI anterior se muestra la instrucción INSERT que crea esta fila).</span><span class="sxs-lookup"><span data-stu-id="491a9-175">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="491a9-176">Ejecute la aplicación para comprobar que todo funciona igual que antes.</span><span class="sxs-lookup"><span data-stu-id="491a9-176">Run the application to verify that everything still works the same as before.</span></span>

![Página de índice de Students](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="491a9-178">Diferencias entre la interfaz de la línea de comandos (CLI) y la Consola del Administrador de paquetes (PMC)</span><span class="sxs-lookup"><span data-stu-id="491a9-178">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="491a9-179">Las herramientas de EF para la administración de migraciones están disponibles desde los comandos de la CLI de .NET Core o los cmdlets de PowerShell en la ventana **Consola del Administrador de paquetes** (PMC) de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="491a9-179">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="491a9-180">En este tutorial se muestra cómo usar la CLI, pero puede usar la PMC si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="491a9-180">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="491a9-181">Los comandos de EF para los comandos de la PMC están en el paquete [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="491a9-181">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="491a9-182">Este paquete ya está incluido en el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), por lo que no es necesario instalarlo.</span><span class="sxs-lookup"><span data-stu-id="491a9-182">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="491a9-183">**Importante:** Este no es el mismo paquete que el que se instala para la CLI mediante la edición del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="491a9-183">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="491a9-184">El nombre de este paquete termina en `Tools`, a diferencia del nombre de paquete de la CLI que termina en `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="491a9-184">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="491a9-185">Para obtener más información sobre los comandos de la CLI, vea [CLI de .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="491a9-185">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="491a9-186">Para obtener más información sobre los comandos de la PMC, vea [Consola del Administrador de paquetes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="491a9-186">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="491a9-187">Resumen</span><span class="sxs-lookup"><span data-stu-id="491a9-187">Summary</span></span>

<span data-ttu-id="491a9-188">En este tutorial, ha visto cómo crear y aplicar la primera migración.</span><span class="sxs-lookup"><span data-stu-id="491a9-188">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="491a9-189">En el siguiente, comenzará examinando temas más avanzados expandiendo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="491a9-189">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="491a9-190">Por el camino, podrá crear y aplicar migraciones adicionales.</span><span class="sxs-lookup"><span data-stu-id="491a9-190">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="491a9-191">[Anterior](sort-filter-page.md)
[Siguiente](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="491a9-191">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
