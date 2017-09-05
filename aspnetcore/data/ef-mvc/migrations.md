---
title: "Núcleo de ASP.NET MVC con EF Core - Migrations - 4 de 10"
author: tdykstra
description: "En este tutorial, debe comenzar con la característica de migraciones de EF principal para administrar los cambios del modelo de datos en una aplicación MVC de ASP.NET Core."
keywords: Migraciones de ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 4d81099d1ab97a8a49d96657153a54aa96dd6bf8
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="042b6-104">Migraciones - Core EF con el tutorial de MVC de ASP.NET Core (4 de 10)</span><span class="sxs-lookup"><span data-stu-id="042b6-104">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="042b6-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="042b6-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="042b6-106">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="042b6-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="042b6-107">Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="042b6-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="042b6-108">En este tutorial, debe comenzar con la característica de migraciones de EF principal para administrar los cambios del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-108">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="042b6-109">En los tutoriales posteriores, agregará más migraciones a medida que cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-109">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="042b6-110">Introducción a las migraciones</span><span class="sxs-lookup"><span data-stu-id="042b6-110">Introduction to migrations</span></span>

<span data-ttu-id="042b6-111">Al desarrollar una aplicación nueva, el modelo de datos cambia con frecuencia y cada vez que los cambios del modelo, se le asigna sincronizada con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-111">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="042b6-112">Estos tutoriales se inició mediante la configuración de Entity Framework para crear la base de datos si no existe.</span><span class="sxs-lookup"><span data-stu-id="042b6-112">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="042b6-113">A continuación, cada vez que cambie el modelo de datos, agregar, quitar, o cambiar las clases de entidad o cambiar la clase DbContext--puede eliminar la base de datos y EF crea uno nuevo que coincide con el modelo y propaga con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="042b6-113">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="042b6-114">Este método para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que implemente la aplicación en producción.</span><span class="sxs-lookup"><span data-stu-id="042b6-114">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="042b6-115">Cuando se ejecuta la aplicación en producción normalmente almacena los datos que desea mantener, y no desea perder todo lo que cada vez que se realice un cambio como agregar una nueva columna.</span><span class="sxs-lookup"><span data-stu-id="042b6-115">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="042b6-116">La característica de EF Core migraciones soluciona este problema habilitando EF actualizar el esquema de base de datos en lugar de crear una nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-116">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="042b6-117">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="042b6-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="042b6-118">Para trabajar con las migraciones, puede usar el **Package Manager Console** (PMC) o la interfaz de línea de comandos (CLI).</span><span class="sxs-lookup"><span data-stu-id="042b6-118">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="042b6-119">Estos tutoriales muestra cómo utilizar los comandos CLI.</span><span class="sxs-lookup"><span data-stu-id="042b6-119">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="042b6-120">Obtener información sobre el PMC está en [al final de este tutorial](#pmc).</span><span class="sxs-lookup"><span data-stu-id="042b6-120">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="042b6-121">Se proporcionan las herramientas EF de la interfaz de línea de comandos (CLI) en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="042b6-121">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="042b6-122">Para instalar este paquete, agréguelo a la `DotNetCliToolReference` colección en la *.csproj* de archivos, como se muestra.</span><span class="sxs-lookup"><span data-stu-id="042b6-122">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="042b6-123">**Nota:** tendrá que instalar este paquete mediante la edición de la *.csproj* archivo; no se puede utilizar el `install-package` comando o la interfaz gráfica de usuario del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="042b6-123">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="042b6-124">Puede editar la *.csproj* archivo haciendo clic en el nombre del proyecto en **el Explorador de soluciones** y seleccionando **ContosoUniversity.csproj editar**.</span><span class="sxs-lookup"><span data-stu-id="042b6-124">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="042b6-125">(Los números de versión en este ejemplo fueron actuales cuando se escribió el tutorial.)</span><span class="sxs-lookup"><span data-stu-id="042b6-125">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="042b6-126">Cambiar la cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="042b6-126">Change the connection string</span></span>

<span data-ttu-id="042b6-127">En el *appSettings.JSON que se* de archivo, cambie el nombre de la base de datos en la cadena de conexión para ContosoUniversity2 o algún otro nombre que no has utilizado en el equipo que está utilizando.</span><span class="sxs-lookup"><span data-stu-id="042b6-127">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

<span data-ttu-id="042b6-128">[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]</span><span class="sxs-lookup"><span data-stu-id="042b6-128">[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]</span></span>

<span data-ttu-id="042b6-129">Este cambio se configura el proyecto para que la primera migración creará una nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-129">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="042b6-130">Esto no es necesario para comenzar a usar migraciones, pero se verá más adelante por eso es una buena idea.</span><span class="sxs-lookup"><span data-stu-id="042b6-130">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="042b6-131">Como alternativa a cambiar el nombre de la base de datos, puede eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-131">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="042b6-132">Use **Explorador de objetos de SQL Server** (SSOX) o `database drop` comando de CLI:</span><span class="sxs-lookup"><span data-stu-id="042b6-132">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="042b6-133">En la siguiente sección se explica cómo ejecutar comandos de CLI.</span><span class="sxs-lookup"><span data-stu-id="042b6-133">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="042b6-134">Crear una migración inicial</span><span class="sxs-lookup"><span data-stu-id="042b6-134">Create an initial migration</span></span>

<span data-ttu-id="042b6-135">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="042b6-135">Save your changes and build the project.</span></span> <span data-ttu-id="042b6-136">A continuación, abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="042b6-136">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="042b6-137">Aquí es una forma rápida de hacerlo:</span><span class="sxs-lookup"><span data-stu-id="042b6-137">Here's a quick way to do that:</span></span>

* <span data-ttu-id="042b6-138">En **el Explorador de soluciones**, haga clic en el proyecto y elija **abierto en el Explorador de archivos** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="042b6-138">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Abrir en el elemento de menú del explorador de archivos](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="042b6-140">Escriba "cmd" en la barra de direcciones y presione ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="042b6-140">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Abrir ventana de comandos](migrations/_static/open-command-window.png)

<span data-ttu-id="042b6-142">Escriba el siguiente comando en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="042b6-142">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="042b6-143">Ve un resultado similar al siguiente en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="042b6-143">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="042b6-144">Si ve un mensaje de error *ningún archivo ejecutable encuentra coincidencia comando "dotnet-ef"*, consulte [esta entrada de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para solucionar problemas de la Ayuda.</span><span class="sxs-lookup"><span data-stu-id="042b6-144">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="042b6-145">Si ve un mensaje de error "*no se puede obtener acceso al archivo... ContosoUniversity.dll porque está siendo utilizado por otro proceso.* ", el icono IIS Express se encuentra en la bandeja del sistema Windows y haga clic en él, haga clic en **ContosoUniversity > Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="042b6-145">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="042b6-146">Examine el arriba y abajo métodos</span><span class="sxs-lookup"><span data-stu-id="042b6-146">Examine the Up and Down methods</span></span>

<span data-ttu-id="042b6-147">Cuando ejecuta el `migrations add` comando EF generó el código que va a crear la base de datos desde el principio.</span><span class="sxs-lookup"><span data-stu-id="042b6-147">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="042b6-148">Este código está en el *migraciones* carpeta, en el archivo llamado  *\<timestamp > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="042b6-148">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="042b6-149">El `Up` método de la `InitialCreate` clase crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos, y el `Down` método elimina, tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="042b6-149">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

<span data-ttu-id="042b6-150">[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]</span><span class="sxs-lookup"><span data-stu-id="042b6-150">[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]</span></span>

<span data-ttu-id="042b6-151">Llamadas de migraciones el `Up` método para implementar los cambios de modelo de datos para una migración.</span><span class="sxs-lookup"><span data-stu-id="042b6-151">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="042b6-152">Cuando se especifica un comando para revertir la actualización, llamadas de migraciones el `Down` método.</span><span class="sxs-lookup"><span data-stu-id="042b6-152">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="042b6-153">Este código es para la migración inicial que se creó cuando escribe el `migrations add InitialCreate` comando.</span><span class="sxs-lookup"><span data-stu-id="042b6-153">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="042b6-154">El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se utiliza para el nombre de archivo y puede ser todo lo que desees.</span><span class="sxs-lookup"><span data-stu-id="042b6-154">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="042b6-155">Es mejor elegir una palabra o frase que resume lo que se hace en la migración.</span><span class="sxs-lookup"><span data-stu-id="042b6-155">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="042b6-156">Por ejemplo, podría denominar una migración más adelante "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="042b6-156">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="042b6-157">Si ha creado la migración inicial cuando la base de datos ya existe, se genera el código de creación de la base de datos pero no es necesario ejecutar porque la base de datos ya coincide con el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-157">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="042b6-158">Al implementar la aplicación en otro entorno donde la base de datos no existe aún, se ejecutará este código para crear la base de datos, por lo que es una buena idea para probarla en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="042b6-158">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="042b6-159">Por eso ha cambiado el nombre de la base de datos en versiones anteriores, la cadena de conexión para que las migraciones pueden crear uno nuevo desde cero.</span><span class="sxs-lookup"><span data-stu-id="042b6-159">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="042b6-160">Examine la instantánea del modelo de datos</span><span class="sxs-lookup"><span data-stu-id="042b6-160">Examine the data model snapshot</span></span>

<span data-ttu-id="042b6-161">Las migraciones también crea un *instantánea* del esquema de base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="042b6-161">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="042b6-162">Este es el aspecto de ese código:</span><span class="sxs-lookup"><span data-stu-id="042b6-162">Here's what that code looks like:</span></span>

<span data-ttu-id="042b6-163">[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]</span><span class="sxs-lookup"><span data-stu-id="042b6-163">[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]</span></span>

<span data-ttu-id="042b6-164">Como el esquema de base de datos actual se representa en el código, Core EF no tiene que interactuar con la base de datos para crear las migraciones.</span><span class="sxs-lookup"><span data-stu-id="042b6-164">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="042b6-165">Cuando se agrega una migración, EF determina qué cambiado al comparar los modelos de datos para el archivo de instantánea.</span><span class="sxs-lookup"><span data-stu-id="042b6-165">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="042b6-166">EF interactúa con la base de datos sólo cuando tiene que actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-166">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="042b6-167">El archivo de instantánea tiene que estar sincronizada con las migraciones que crea, por lo que no se puede eliminar una migración eliminando el archivo denominado  *\<timestamp > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="042b6-167">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="042b6-168">Si elimina ese archivo, las restantes migraciones será sincronizadas con el archivo de instantánea de base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-168">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="042b6-169">Para eliminar la última migración que ha agregado, use la [migraciones de ef dotnet quitar](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.</span><span class="sxs-lookup"><span data-stu-id="042b6-169">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="042b6-170">La migración se aplican a la base de datos</span><span class="sxs-lookup"><span data-stu-id="042b6-170">Apply the migration to the database</span></span>

<span data-ttu-id="042b6-171">En la ventana de comandos, escriba el siguiente comando para crear la base de datos y tablas en él.</span><span class="sxs-lookup"><span data-stu-id="042b6-171">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="042b6-172">El resultado del comando es similar a la `migrations add` de comandos, salvo que verá registros para los comandos SQL que configura la base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-172">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="042b6-173">La mayoría de los registros se omite en la siguiente salida de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="042b6-173">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="042b6-174">Si no desea ver este nivel de detalle en los mensajes de registro, puede cambiar los niveles de registro en el *appsettings. Development.JSON* archivo.</span><span class="sxs-lookup"><span data-stu-id="042b6-174">If you prefer not to see this level of detail in log messages, you can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="042b6-175">Para obtener más información, consulte [Introducción al registro](xref:fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="042b6-175">For more information, see [Introduction to logging](xref:fundamentals/logging).</span></span>

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

<span data-ttu-id="042b6-176">Use **Explorador de objetos de SQL Server** para inspeccionar la base de datos como lo hizo en el primer tutorial.</span><span class="sxs-lookup"><span data-stu-id="042b6-176">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="042b6-177">Observará que la adición de una tabla de __EFMigrationsHistory que realiza un seguimiento de las migraciones se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-177">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="042b6-178">Ver los datos de esa tabla y verá una fila para la primera migración.</span><span class="sxs-lookup"><span data-stu-id="042b6-178">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="042b6-179">(El último registro en el ejemplo de salida CLI anterior muestra la instrucción INSERT que crea esta fila).</span><span class="sxs-lookup"><span data-stu-id="042b6-179">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="042b6-180">Ejecute la aplicación para comprobar que todo funciona sigue el mismo que antes.</span><span class="sxs-lookup"><span data-stu-id="042b6-180">Run the application to verify that everything still works the same as before.</span></span>

![Página de índice de estudiantes](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="042b6-182">Diferencias entre la interfaz de línea de comandos (CLI) y Consola de administrador de paquetes (PMC)</span><span class="sxs-lookup"><span data-stu-id="042b6-182">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="042b6-183">Las herramientas de EF para administrar las migraciones está disponible desde los comandos de CLI de núcleo de .NET o de los cmdlets de PowerShell en Visual Studio **Package Manager Console** ventana (PMC).</span><span class="sxs-lookup"><span data-stu-id="042b6-183">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="042b6-184">Este tutorial muestra cómo utilizar la CLI, pero puede usar el PMC si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="042b6-184">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="042b6-185">Los comandos EF de los comandos PMC están en el [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paquete.</span><span class="sxs-lookup"><span data-stu-id="042b6-185">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="042b6-186">Este paquete ya está incluido en el [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, por lo que no tiene que instalarlo.</span><span class="sxs-lookup"><span data-stu-id="042b6-186">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="042b6-187">**Importante:** no es el mismo paquete que la instalación de la CLI mediante la edición de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="042b6-187">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="042b6-188">El nombre de éste termina en `Tools`, a diferencia del nombre de paquete CLI que finaliza en `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="042b6-188">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="042b6-189">Para obtener más información acerca de los comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="042b6-189">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="042b6-190">Para obtener más información acerca de los comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="042b6-190">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="042b6-191">Resumen</span><span class="sxs-lookup"><span data-stu-id="042b6-191">Summary</span></span>

<span data-ttu-id="042b6-192">En este tutorial, ha visto cómo crear y aplicar la primera migración.</span><span class="sxs-lookup"><span data-stu-id="042b6-192">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="042b6-193">En el siguiente tutorial, comenzará examinando temas más avanzados expandiendo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="042b6-193">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="042b6-194">A lo largo de la forma podrá crear y aplicar migraciones adicionales.</span><span class="sxs-lookup"><span data-stu-id="042b6-194">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="042b6-195">[Anterior](sort-filter-page.md)
[Siguiente](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="042b6-195">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
