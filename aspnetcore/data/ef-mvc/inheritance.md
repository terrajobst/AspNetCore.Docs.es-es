---
title: "Núcleo de ASP.NET MVC con EF Core - herencia - 9 de 10"
author: tdykstra
description: "Este tutorial le mostrará cómo implementar la herencia en el modelo de datos con Entity Framework Core en una aplicación de ASP.NET Core."
keywords: Herencia de ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 10bde121dac3bdbbf0e55f2d146d91dea0f0210f
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="217b0-104">Herencia - Core EF con el tutorial de MVC de ASP.NET Core (9 de 10)</span><span class="sxs-lookup"><span data-stu-id="217b0-104">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="217b0-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="217b0-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="217b0-106">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="217b0-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="217b0-107">Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="217b0-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="217b0-108">En el tutorial anterior, para administrar las excepciones de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="217b0-108">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="217b0-109">Este tutorial le mostrará cómo implementar la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="217b0-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="217b0-110">En la programación orientada a objetos, puede usar la herencia para facilitar la reutilización del código.</span><span class="sxs-lookup"><span data-stu-id="217b0-110">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="217b0-111">En este tutorial, va a cambiar la `Instructor` y `Student` clases para que derivan de un `Person` clase que contiene propiedades como base `LastName` que son comunes a los instructores y alumnos.</span><span class="sxs-lookup"><span data-stu-id="217b0-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="217b0-112">No agregar ni cambiar todas las páginas web, pero podrá cambiar parte del código y esos cambios se reflejarán automáticamente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="217b0-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="217b0-113">Opciones para la asignación de herencia para las tablas de base de datos</span><span class="sxs-lookup"><span data-stu-id="217b0-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="217b0-114">El `Instructor` y `Student` las clases en el modelo de datos School tienen varias propiedades que son idénticas:</span><span class="sxs-lookup"><span data-stu-id="217b0-114">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Clases de Student y Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="217b0-116">Imagine que desea eliminar el código redundante para las propiedades que comparten el `Instructor` y `Student` entidades.</span><span class="sxs-lookup"><span data-stu-id="217b0-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="217b0-117">O bien, desea escribir un servicio que puede dar formato a los nombres sin preocuparse de si el nombre procede de un instructor o un estudiante.</span><span class="sxs-lookup"><span data-stu-id="217b0-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="217b0-118">Puede crear un `Person` clase base que contiene solo las propiedades comparten, realice la `Instructor` y `Student` clases heredan de esa clase base, como se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="217b0-118">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Student y Instructor clases derivadas de la clase de persona](inheritance/_static/inheritance.png)

<span data-ttu-id="217b0-120">Hay varias maneras de que esta estructura de herencia se podría representar en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="217b0-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="217b0-121">Podría tener una tabla de la persona que incluye información acerca de los alumnos e instructores en una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="217b0-121">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="217b0-122">Algunas de las columnas pudieron aplicar solo a instructores (HireDate), algunas solo a los alumnos (EnrollmentDate), algunos para ambos (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="217b0-122">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="217b0-123">Por lo general, tendría una columna discriminadora para indicar qué tipo de cada fila representa.</span><span class="sxs-lookup"><span data-stu-id="217b0-123">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="217b0-124">Por ejemplo, la columna discriminadora podría tener "Instructor" para instructores y "Student" para estudiantes.</span><span class="sxs-lookup"><span data-stu-id="217b0-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Ejemplo de tabla por jerarquía](inheritance/_static/tph.png)

<span data-ttu-id="217b0-126">Este patrón consiste en generar una estructura de herencia de la entidad de una tabla de base de datos único se denomina herencia de tabla por jerarquía (TPH).</span><span class="sxs-lookup"><span data-stu-id="217b0-126">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="217b0-127">Una alternativa consiste en hacer que la base de datos se parecen más a la estructura de herencia.</span><span class="sxs-lookup"><span data-stu-id="217b0-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="217b0-128">Por ejemplo, podría tener sólo los campos de nombre de la tabla Person y tener tablas Instructor y Student independientes con los campos de fecha.</span><span class="sxs-lookup"><span data-stu-id="217b0-128">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Herencia de tabla por tipo](inheritance/_static/tpt.png)

<span data-ttu-id="217b0-130">Este patrón de crear una tabla de base de datos para cada clase de entidad se denomina tabla por herencia de tipo (TPT).</span><span class="sxs-lookup"><span data-stu-id="217b0-130">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="217b0-131">Todavía otra opción es asignar todos los tipos de no abstractas para tablas individuales.</span><span class="sxs-lookup"><span data-stu-id="217b0-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="217b0-132">Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente.</span><span class="sxs-lookup"><span data-stu-id="217b0-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="217b0-133">Este patrón se denomina herencia de clases de tabla por concreto (TCP).</span><span class="sxs-lookup"><span data-stu-id="217b0-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="217b0-134">Si implementa la herencia de TCP para las clases de persona y estudiantes, Instructor tal como se muestra anteriormente, las tablas Student y Instructor tendría un aspecto diferentes no después de implementar la herencia que ocupaban antes.</span><span class="sxs-lookup"><span data-stu-id="217b0-134">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="217b0-135">TPC y TPH patrones de herencia generalmente entregar un mejor rendimiento que los patrones de herencia de TPT, porque pueden dar lugar a patrones TPT en consultas de combinaciones complejas.</span><span class="sxs-lookup"><span data-stu-id="217b0-135">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="217b0-136">Este tutorial muestra cómo implementar la herencia TPH.</span><span class="sxs-lookup"><span data-stu-id="217b0-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="217b0-137">TPH es el patrón de herencia única que admite el núcleo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="217b0-137">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="217b0-138">Lo que deberá hacer es crear un `Person` clase, cambie el `Instructor` y `Student` clases que derivan de `Person`, agregar la nueva clase a la `DbContext`y crear una migración.</span><span class="sxs-lookup"><span data-stu-id="217b0-138">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="217b0-139">Considere la posibilidad de guardar una copia del proyecto antes de realizar los siguientes cambios.</span><span class="sxs-lookup"><span data-stu-id="217b0-139">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="217b0-140">A continuación, si experimenta problemas y necesidad de volver a empezar, le resultará más fácil iniciar desde el proyecto guardado en lugar de invertir los pasos que se realiza en este tutorial o que se va hacia el principio de la serie completa.</span><span class="sxs-lookup"><span data-stu-id="217b0-140">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="217b0-141">Crear la clase de persona</span><span class="sxs-lookup"><span data-stu-id="217b0-141">Create the Person class</span></span>

<span data-ttu-id="217b0-142">En la carpeta Models, crear Person.cs y reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="217b0-142">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="217b0-143">Asegúrese de Student y Instructor clases heredan de persona</span><span class="sxs-lookup"><span data-stu-id="217b0-143">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="217b0-144">En *Instructor.cs*, derive la clase Instructor de la clase de persona y quitar los campos claves y nombres.</span><span class="sxs-lookup"><span data-stu-id="217b0-144">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="217b0-145">El código tendrá un aspecto similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="217b0-145">The code will look like the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="217b0-146">Realizar los mismos cambios en *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="217b0-146">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="217b0-147">Agregar el tipo de entidad de la persona en el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="217b0-147">Add the Person entity type to the data model</span></span>

<span data-ttu-id="217b0-148">Agregar el tipo de entidad de la persona a *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="217b0-148">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="217b0-149">Se resaltan las líneas nuevas.</span><span class="sxs-lookup"><span data-stu-id="217b0-149">The new lines are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="217b0-150">Esto es todo lo que necesita de Entity Framework para configurar la herencia de tabla por jerarquía.</span><span class="sxs-lookup"><span data-stu-id="217b0-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="217b0-151">Como verá, cuando se actualiza la base de datos, tendrá una tabla Person en lugar de las tablas Student y Instructor.</span><span class="sxs-lookup"><span data-stu-id="217b0-151">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="217b0-152">Crear y personalizar el código de la migración</span><span class="sxs-lookup"><span data-stu-id="217b0-152">Create and customize migration code</span></span>

<span data-ttu-id="217b0-153">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="217b0-153">Save your changes and build the project.</span></span> <span data-ttu-id="217b0-154">A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="217b0-154">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="217b0-155">No se ejecutan los `database update` comando todavía.</span><span class="sxs-lookup"><span data-stu-id="217b0-155">Don't run the `database update` command yet.</span></span> <span data-ttu-id="217b0-156">Este comando dará como resultado pérdida de datos debido a que elimine la tabla Instructor y cambiar el nombre de la tabla de estudiantes en persona.</span><span class="sxs-lookup"><span data-stu-id="217b0-156">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="217b0-157">Deberá proporcionar código personalizado para conservar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="217b0-157">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="217b0-158">Abra *migraciones /\<marca de tiempo > _Inheritance.cs* y reemplace el `Up` método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="217b0-158">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="217b0-159">Este código se encarga de las tareas de actualización de base de datos siguientes:</span><span class="sxs-lookup"><span data-stu-id="217b0-159">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="217b0-160">Quita las restricciones foreign key y los índices que apuntan a la tabla de estudiantes.</span><span class="sxs-lookup"><span data-stu-id="217b0-160">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="217b0-161">Cambia el nombre de la tabla Instructor como persona y realiza cambios necesarios almacenar datos de los alumnos:</span><span class="sxs-lookup"><span data-stu-id="217b0-161">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="217b0-162">Agrega EnrollmentDate que aceptan valores NULL para estudiantes.</span><span class="sxs-lookup"><span data-stu-id="217b0-162">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="217b0-163">Agrega la columna discriminadora para indicar si una fila es para un estudiante o un instructor.</span><span class="sxs-lookup"><span data-stu-id="217b0-163">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="217b0-164">Hace HireDate que aceptan valores NULL ya que las filas de estudiantes no tengan las fechas de contratación.</span><span class="sxs-lookup"><span data-stu-id="217b0-164">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="217b0-165">Agrega un campo temporal que se usará para actualizar las claves externas que apuntan a los alumnos.</span><span class="sxs-lookup"><span data-stu-id="217b0-165">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="217b0-166">Cuando copie estudiantes en la tabla Person obtendrá nuevos valores de clave principales.</span><span class="sxs-lookup"><span data-stu-id="217b0-166">When you copy students into the Person table they'll get new primary key values.</span></span>

* <span data-ttu-id="217b0-167">Copia datos de la tabla de estudiante en la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="217b0-167">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="217b0-168">Esto hace que los alumnos se asignen nuevos valores de clave principales.</span><span class="sxs-lookup"><span data-stu-id="217b0-168">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="217b0-169">Corrige los valores de clave externa que señalan a los alumnos.</span><span class="sxs-lookup"><span data-stu-id="217b0-169">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="217b0-170">Vuelve a crear las restricciones foreign key y los índices, ahora que apunta a la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="217b0-170">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="217b0-171">(Si hubiera usado el GUID en lugar de entero como el tipo de clave principal, los valores de clave principal de estudiante no tienen que cambiar y algunos de estos pasos se ha omitido).</span><span class="sxs-lookup"><span data-stu-id="217b0-171">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="217b0-172">Ejecute el `database update` comando:</span><span class="sxs-lookup"><span data-stu-id="217b0-172">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="217b0-173">(En un sistema de producción haría que los cambios correspondientes en el `Down` método en caso de que alguna vez ha tenido que usar para volver a la versión anterior de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="217b0-173">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="217b0-174">Para este tutorial no va a usar el `Down` método.)</span><span class="sxs-lookup"><span data-stu-id="217b0-174">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="217b0-175">Es posible obtener otros errores al realizar cambios de esquema en una base de datos con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="217b0-175">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="217b0-176">Si se producen errores de migración que no se puede resolver, puede cambiar el nombre de la base de datos en la cadena de conexión o eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="217b0-176">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="217b0-177">Con una base de datos, no hay ningún dato para migrar y el comando de actualización de base de datos es más probable que se completan sin errores.</span><span class="sxs-lookup"><span data-stu-id="217b0-177">With a new database, there is no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="217b0-178">Para eliminar la base de datos, utilice SSOX o ejecute la `database drop` comando de CLI.</span><span class="sxs-lookup"><span data-stu-id="217b0-178">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="217b0-179">Probar con herencia implementada</span><span class="sxs-lookup"><span data-stu-id="217b0-179">Test with inheritance implemented</span></span>

<span data-ttu-id="217b0-180">Ejecute la aplicación e intente distintas páginas.</span><span class="sxs-lookup"><span data-stu-id="217b0-180">Run the app and try various pages.</span></span> <span data-ttu-id="217b0-181">El mismo todo funciona igual que antes.</span><span class="sxs-lookup"><span data-stu-id="217b0-181">Everything works the same as it did before.</span></span>

<span data-ttu-id="217b0-182">En **Explorador de objetos de SQL Server**, expanda **las conexiones de datos/SchoolContext** y, a continuación, **tablas**, y verá que las tablas Student y Instructor se han reemplazado por un Tabla Person.</span><span class="sxs-lookup"><span data-stu-id="217b0-182">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="217b0-183">Abra el Diseñador de tablas de la persona y puede ver que tiene todas las columnas que solían haber en las tablas Student y Instructor.</span><span class="sxs-lookup"><span data-stu-id="217b0-183">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabla Person en SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="217b0-185">Haga clic en la tabla Person y, a continuación, haga clic en **mostrar datos de tabla** para ver la columna discriminadora.</span><span class="sxs-lookup"><span data-stu-id="217b0-185">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabla Person en SSOX - datos de la tabla](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="217b0-187">Resumen</span><span class="sxs-lookup"><span data-stu-id="217b0-187">Summary</span></span>

<span data-ttu-id="217b0-188">Ha implementado la herencia de tabla por jerarquía para el `Person`, `Student`, y `Instructor` clases.</span><span class="sxs-lookup"><span data-stu-id="217b0-188">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="217b0-189">Para obtener más información acerca de la herencia en Entity Framework Core, vea [herencia](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="217b0-189">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="217b0-190">En el siguiente tutorial verá cómo controlar una variedad de escenarios de Entity Framework relativamente avanzados.</span><span class="sxs-lookup"><span data-stu-id="217b0-190">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="217b0-191">[Anterior](concurrency.md)
[Siguiente](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="217b0-191">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
