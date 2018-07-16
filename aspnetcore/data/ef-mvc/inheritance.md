---
title: 'ASP.NET Core MVC con EF Core: herencia (9 de 10)'
author: rick-anderson
description: En este tutorial se explica cómo implementar la herencia en el modelo de datos con Entity Framework Core en una aplicación de ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a71954297f44f936893a7f1e9d3b0685f81378b9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126709"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="f9922-103">ASP.NET Core MVC con EF Core: herencia (9 de 10)</span><span class="sxs-lookup"><span data-stu-id="f9922-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f9922-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f9922-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f9922-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web de ASP.NET Core MVC con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9922-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="f9922-106">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="f9922-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="f9922-107">En el tutorial anterior, se trataron las excepciones de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="f9922-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="f9922-108">En este tutorial se muestra cómo implementar la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f9922-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="f9922-109">En la programación orientada a objetos, puede usar la herencia para facilitar la reutilización del código.</span><span class="sxs-lookup"><span data-stu-id="f9922-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="f9922-110">En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos.</span><span class="sxs-lookup"><span data-stu-id="f9922-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="f9922-111">No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f9922-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="f9922-112">Opciones para asignar herencia a las tablas de bases de datos</span><span class="sxs-lookup"><span data-stu-id="f9922-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="f9922-113">Las clases `Instructor` y `Student` del modelo de datos School tienen varias propiedades idénticas:</span><span class="sxs-lookup"><span data-stu-id="f9922-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Clases Student e Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="f9922-115">Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`.</span><span class="sxs-lookup"><span data-stu-id="f9922-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="f9922-116">O que quiere escribir un servicio que pueda dar formato a los nombres sin preocuparse de si el nombre es de un instructor o de un alumno.</span><span class="sxs-lookup"><span data-stu-id="f9922-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="f9922-117">Puede crear una clase base `Person` que solo contenga las propiedades compartidas y después hacer que las clases `Instructor` y `Student` hereden de esa clase base, como se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="f9922-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Clases Student e Instructor derivadas de la clase Person](inheritance/_static/inheritance.png)

<span data-ttu-id="f9922-119">Esta estructura de herencia se puede representar de varias formas en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f9922-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="f9922-120">Puede tener una sola tabla Person que incluya información sobre los alumnos y los instructores.</span><span class="sxs-lookup"><span data-stu-id="f9922-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="f9922-121">Algunas de las columnas podrían aplicarse solo a los instructores (HireDate), otras solo a los alumnos (EnrollmentDate) y otras a ambos (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="f9922-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="f9922-122">Lo más común sería que tuviera una columna discriminadora para indicar qué tipo representa cada fila.</span><span class="sxs-lookup"><span data-stu-id="f9922-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="f9922-123">Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.</span><span class="sxs-lookup"><span data-stu-id="f9922-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Ejemplo de tabla por jerarquía](inheritance/_static/tph.png)

<span data-ttu-id="f9922-125">Este patrón, que consiste en generar una estructura de herencia de la entidad a partir de una tabla de base de datos única, se denomina herencia de tabla por jerarquía (TPH).</span><span class="sxs-lookup"><span data-stu-id="f9922-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="f9922-126">Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia.</span><span class="sxs-lookup"><span data-stu-id="f9922-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="f9922-127">Por ejemplo, podría tener solo los campos de nombre en la tabla Person y tener tablas Instructor y Student independientes con los campos de fecha.</span><span class="sxs-lookup"><span data-stu-id="f9922-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Herencia de tabla por tipo](inheritance/_static/tpt.png)

<span data-ttu-id="f9922-129">Este patrón, que consiste en crear una tabla de base de datos para cada clase de entidad, se denomina herencia de tabla por tipo (TPT).</span><span class="sxs-lookup"><span data-stu-id="f9922-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="f9922-130">Y todavía otra opción pasa por asignar todos los tipos no abstractos a tablas individuales.</span><span class="sxs-lookup"><span data-stu-id="f9922-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="f9922-131">Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente.</span><span class="sxs-lookup"><span data-stu-id="f9922-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="f9922-132">Este patrón se denomina herencia de tabla por clase concreta (TPC).</span><span class="sxs-lookup"><span data-stu-id="f9922-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="f9922-133">Si implementara la herencia de TPC en las clases Person, Student e Instructor tal como se ha explicado anteriormente, las tablas Student e Instructor se verían igual antes que después de implementar la herencia.</span><span class="sxs-lookup"><span data-stu-id="f9922-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="f9922-134">Los patrones de herencia de TPC y TPH acostumbran a tener un mejor rendimiento que los patrones de herencia de TPT, porque estos pueden provocar consultas de combinaciones complejas.</span><span class="sxs-lookup"><span data-stu-id="f9922-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="f9922-135">Este tutorial muestra cómo implementar la herencia de TPH.</span><span class="sxs-lookup"><span data-stu-id="f9922-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="f9922-136">TPH es el único patrón de herencia que Entity Framework Core admite.</span><span class="sxs-lookup"><span data-stu-id="f9922-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="f9922-137">Lo que tiene que hacer es crear una clase `Person`, cambiar las clases `Instructor` y `Student` que deriven de `Person`, agregar la nueva clase a `DbContext` y crear una migración.</span><span class="sxs-lookup"><span data-stu-id="f9922-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="f9922-138">Considere la posibilidad de guardar una copia del proyecto antes de realizar los siguientes cambios.</span><span class="sxs-lookup"><span data-stu-id="f9922-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="f9922-139">De este modo, si tiene problemas y necesita volver a empezar, le resultará más fácil comenzar desde el proyecto guardado que tener que revertir los pasos realizados en este tutorial o volver al principio de toda la serie.</span><span class="sxs-lookup"><span data-stu-id="f9922-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="f9922-140">Creación de la clase Person</span><span class="sxs-lookup"><span data-stu-id="f9922-140">Create the Person class</span></span>

<span data-ttu-id="f9922-141">En la carpeta Models, cree Person.cs y reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9922-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="f9922-142">Asegúrese de que las clases Student e Instructor heredan de Person</span><span class="sxs-lookup"><span data-stu-id="f9922-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="f9922-143">En *Instructor.cs*, derive la clase Instructor de la clase Person y quite los campos de clave y nombre.</span><span class="sxs-lookup"><span data-stu-id="f9922-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="f9922-144">El código tendrá un aspecto similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9922-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="f9922-145">Realice los mismos cambios en *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="f9922-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="f9922-146">Agregar el tipo de entidad Person al modelo de datos</span><span class="sxs-lookup"><span data-stu-id="f9922-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="f9922-147">Agregue el tipo de entidad Person a *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="f9922-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="f9922-148">Se resaltan las líneas nuevas.</span><span class="sxs-lookup"><span data-stu-id="f9922-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="f9922-149">Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía.</span><span class="sxs-lookup"><span data-stu-id="f9922-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="f9922-150">Como verá, cuando la base de datos esté actualizada, tendrá una tabla Person en lugar de las tablas Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="f9922-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="f9922-151">Creación y personalización del código de migración</span><span class="sxs-lookup"><span data-stu-id="f9922-151">Create and customize migration code</span></span>

<span data-ttu-id="f9922-152">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f9922-152">Save your changes and build the project.</span></span> <span data-ttu-id="f9922-153">A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f9922-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="f9922-154">No ejecute el comando `database update` todavía.</span><span class="sxs-lookup"><span data-stu-id="f9922-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="f9922-155">Este comando provocará la pérdida de datos porque colocará la tabla Instructor y cambiará el nombre de la tabla Student por Person.</span><span class="sxs-lookup"><span data-stu-id="f9922-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="f9922-156">Deberá proporcionar código personalizado para conservar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="f9922-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="f9922-157">Abra *Migrations/\<marca_de_tiempo>_Inheritance.cs* y reemplace el método `Up` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9922-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="f9922-158">Este código se encarga de las siguientes tareas de actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="f9922-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="f9922-159">Quita las restricciones de la clave externa y los índices que apuntan a la tabla Student.</span><span class="sxs-lookup"><span data-stu-id="f9922-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="f9922-160">Cambia el nombre de la tabla Instructor por Person y realiza los cambios necesarios para que pueda almacenar datos de los alumnos:</span><span class="sxs-lookup"><span data-stu-id="f9922-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="f9922-161">Agrega EnrollmentDate que acepta valores NULL para alumnos.</span><span class="sxs-lookup"><span data-stu-id="f9922-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="f9922-162">Agrega la columna discriminadora para indicar si una fila es para un alumno o para un instructor.</span><span class="sxs-lookup"><span data-stu-id="f9922-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="f9922-163">Hace que HireDate admita un valor NULL, puesto que las filas de alumnos no dispondrán de fechas de contratación.</span><span class="sxs-lookup"><span data-stu-id="f9922-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="f9922-164">Agrega un campo temporal que se usará para actualizar las claves externas que apuntan a los alumnos.</span><span class="sxs-lookup"><span data-stu-id="f9922-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="f9922-165">Cuando copie alumnos en la tabla Person, obtendrán nuevos valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="f9922-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="f9922-166">Copia datos de la tabla Student a la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="f9922-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="f9922-167">Esto hace que los alumnos se asignen a nuevos valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="f9922-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="f9922-168">Corrige los valores de clave externa correspondientes a los alumnos.</span><span class="sxs-lookup"><span data-stu-id="f9922-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="f9922-169">Vuelve a crear las restricciones y los índices de la clave externa, pero ahora los dirige a la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="f9922-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="f9922-170">(Si hubiera usado el GUID en lugar de un número entero como tipo de clave principal, los valores de la clave principal de alumno no tendrían que cambiar y algunos de estos pasos se podrían haber omitido).</span><span class="sxs-lookup"><span data-stu-id="f9922-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="f9922-171">Ejecute el comando `database update`:</span><span class="sxs-lookup"><span data-stu-id="f9922-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="f9922-172">(En un sistema de producción haría los cambios correspondientes en el método `Down` por si alguna vez tuviera que usarlo para volver a la versión anterior de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f9922-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="f9922-173">Para este tutorial, no se usará el método `Down`).</span><span class="sxs-lookup"><span data-stu-id="f9922-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="f9922-174">Al hacer cambios en el esquema, se pueden generar otros errores en una base de datos que contenga los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="f9922-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="f9922-175">Si se producen errores de migración que no se pueden resolver, puede cambiar el nombre de la base de datos en la cadena de conexión o eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f9922-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="f9922-176">Con una nueva base de datos, no habrá ningún dato para migrar y es más probable que el comando de actualización de base de datos se complete sin errores.</span><span class="sxs-lookup"><span data-stu-id="f9922-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="f9922-177">Para eliminar la base de datos, use SSOX o ejecute el comando `database drop` de la CLI.</span><span class="sxs-lookup"><span data-stu-id="f9922-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="f9922-178">Prueba con herencia implementada</span><span class="sxs-lookup"><span data-stu-id="f9922-178">Test with inheritance implemented</span></span>

<span data-ttu-id="f9922-179">Ejecute la aplicación y haga la prueba en distintas páginas.</span><span class="sxs-lookup"><span data-stu-id="f9922-179">Run the app and try various pages.</span></span> <span data-ttu-id="f9922-180">Todo funciona igual que antes.</span><span class="sxs-lookup"><span data-stu-id="f9922-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="f9922-181">En **Explorador de objetos de SQL Server**, expanda **Conexiones de datos/SchoolContext** y después **Tables**, y verá que las tablas Student e Instructor se han reemplazado por una tabla Person.</span><span class="sxs-lookup"><span data-stu-id="f9922-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="f9922-182">Abra el diseñador de la tabla Person y verá que contiene todas las columnas que solía haber en las tablas Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="f9922-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabla Person en SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="f9922-184">Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.</span><span class="sxs-lookup"><span data-stu-id="f9922-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabla Person en SSOX: datos de la tabla](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="f9922-186">Resumen</span><span class="sxs-lookup"><span data-stu-id="f9922-186">Summary</span></span>

<span data-ttu-id="f9922-187">Ha implementado la herencia de tabla por jerarquía en las clases `Person`, `Student` y `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="f9922-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="f9922-188">Para obtener más información sobre la herencia en Entity Framework Core, consulte [Herencia](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="f9922-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="f9922-189">En el siguiente tutorial, aprenderá a controlar una serie de escenarios de Entity Framework relativamente avanzados.</span><span class="sxs-lookup"><span data-stu-id="f9922-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="f9922-190">[Anterior](concurrency.md)
> [Siguiente](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="f9922-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>
