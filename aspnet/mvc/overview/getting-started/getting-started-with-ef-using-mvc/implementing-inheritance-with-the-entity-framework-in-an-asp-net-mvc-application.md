---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Plantilla: Implementar la herencia con EF en una de las aplicaciones MVC de ASP.NET 5'
description: En este tutorial se muestra cómo implementar la herencia en el modelo de datos.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: df8715e4416ce3ccdf1d9e380addcded553d85f8
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444290"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="d0d51-103">Plantilla: Implementar la herencia con EF en una aplicación ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="d0d51-103">Template: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="d0d51-104">En el tutorial anterior controla las excepciones de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="d0d51-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="d0d51-105">En este tutorial se muestra cómo implementar la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="d0d51-106">En la programación orientada a objetos, puede usar [herencia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) para facilitar la [reutilización del código](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="d0d51-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="d0d51-107">En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="d0d51-108">No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="d0d51-109">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="d0d51-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d0d51-110">Obtenga información sobre cómo asignar herencia a la base de datos</span><span class="sxs-lookup"><span data-stu-id="d0d51-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="d0d51-111">Creación de la clase Person</span><span class="sxs-lookup"><span data-stu-id="d0d51-111">Create the Person class</span></span>
> * <span data-ttu-id="d0d51-112">Student e Instructor de actualización</span><span class="sxs-lookup"><span data-stu-id="d0d51-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="d0d51-113">Agregar a persona al modelo</span><span class="sxs-lookup"><span data-stu-id="d0d51-113">Add Person to the Model</span></span>
> * <span data-ttu-id="d0d51-114">Crear y actualizar las migraciones</span><span class="sxs-lookup"><span data-stu-id="d0d51-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="d0d51-115">Probar la implementación</span><span class="sxs-lookup"><span data-stu-id="d0d51-115">Test the implementation</span></span>
> * <span data-ttu-id="d0d51-116">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="d0d51-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0d51-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d0d51-117">Prerequisites</span></span>

* [<span data-ttu-id="d0d51-118">Implementar la herencia</span><span class="sxs-lookup"><span data-stu-id="d0d51-118">Implementing Inheritance</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="d0d51-119">Asignar herencia a la base de datos</span><span class="sxs-lookup"><span data-stu-id="d0d51-119">Map inheritance to database</span></span>

<span data-ttu-id="d0d51-120">El `Instructor` y `Student` clases en el `School` modelo de datos tienen varias propiedades que son idénticas:</span><span class="sxs-lookup"><span data-stu-id="d0d51-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="d0d51-122">Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`.</span><span class="sxs-lookup"><span data-stu-id="d0d51-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="d0d51-123">O que quiere escribir un servicio que pueda dar formato a los nombres sin preocuparse de si el nombre es de un instructor o de un alumno.</span><span class="sxs-lookup"><span data-stu-id="d0d51-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="d0d51-124">Puede crear un `Person` clase base que contiene solo las propiedades compartidas y, después, realizar la `Instructor` y `Student` entidades hereden de esa clase base, como se muestra en la ilustración siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="d0d51-126">Esta estructura de herencia se puede representar de varias formas en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="d0d51-127">Podría tener un `Person` tabla que incluye información acerca de los estudiantes y profesores en una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="d0d51-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="d0d51-128">Algunas de las columnas podrían aplicarse solo a los instructores (`HireDate`), otras solo a los alumnos (`EnrollmentDate`), algunos en ambas (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="d0d51-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="d0d51-129">Por lo general, tendría un *discriminador* columna para indicar qué tipo de cada fila representa.</span><span class="sxs-lookup"><span data-stu-id="d0d51-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="d0d51-130">Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="d0d51-132">Este patrón de generar una estructura de herencia de entidad de una tabla de base de datos única se denomina *tabla por jerarquía* herencia (TPH).</span><span class="sxs-lookup"><span data-stu-id="d0d51-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="d0d51-133">Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia.</span><span class="sxs-lookup"><span data-stu-id="d0d51-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="d0d51-134">Por ejemplo, podría tener solo los campos de nombre el `Person` de tabla y tenga distintos `Instructor` y `Student` tablas con los campos de fecha.</span><span class="sxs-lookup"><span data-stu-id="d0d51-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="d0d51-136">Este patrón de hacer que una tabla de base de datos para cada clase de entidad se denomina *tabla por tipo* herencia (TPT).</span><span class="sxs-lookup"><span data-stu-id="d0d51-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="d0d51-137">Y todavía otra opción pasa por asignar todos los tipos no abstractos a tablas individuales.</span><span class="sxs-lookup"><span data-stu-id="d0d51-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="d0d51-138">Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d0d51-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="d0d51-139">Este patrón se denomina herencia de tabla por clase concreta (TPC).</span><span class="sxs-lookup"><span data-stu-id="d0d51-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="d0d51-140">Si se implementó la herencia de TPC para el `Person`, `Student`, y `Instructor` clases tal como se muestra anteriormente, el `Student` y `Instructor` tablas tendría el aspecto no diferentes después de que lo hacían antes de implementar la herencia.</span><span class="sxs-lookup"><span data-stu-id="d0d51-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="d0d51-141">TPC y patrones de herencia de TPH acostumbran un mejor rendimiento en Entity Framework que patrones de herencia de TPT, porque estos pueden provocar consultas de combinaciones complejas.</span><span class="sxs-lookup"><span data-stu-id="d0d51-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="d0d51-142">Este tutorial muestra cómo implementar la herencia de TPH.</span><span class="sxs-lookup"><span data-stu-id="d0d51-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="d0d51-143">TPH es el patrón de herencia predeterminado en Entity Framework, por lo que todo lo que debe hacer es crear un `Person` clase, cambie el `Instructor` y `Student` clases se deriven de `Person`, agregue la nueva clase a la `DbContext`y crear un migración.</span><span class="sxs-lookup"><span data-stu-id="d0d51-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="d0d51-144">(Para obtener información sobre cómo implementar otros patrones de herencia, vea [asignar la herencia de tabla por tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) y [asignar la herencia de la clase concreta por tabla (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) en MSDN Documentación de Entity Framework.)</span><span class="sxs-lookup"><span data-stu-id="d0d51-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="d0d51-145">Creación de la clase Person</span><span class="sxs-lookup"><span data-stu-id="d0d51-145">Create the Person class</span></span>

<span data-ttu-id="d0d51-146">En el *modelos* carpeta, cree *Person.cs* y reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="d0d51-147">Student e Instructor de actualización</span><span class="sxs-lookup"><span data-stu-id="d0d51-147">Update Instructor and Student</span></span>

<span data-ttu-id="d0d51-148">Ahora, actualice el *Instructor.cs* y *Sudent.cs* para heredar valores de la *Person.sc*.</span><span class="sxs-lookup"><span data-stu-id="d0d51-148">Now update the *Instructor.cs* and *Sudent.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="d0d51-149">En *Instructor.cs*, derivar el `Instructor` clase desde el `Person` clase y quite los campos claves y nombres.</span><span class="sxs-lookup"><span data-stu-id="d0d51-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="d0d51-150">El código tendrá un aspecto similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="d0d51-151">Realizar modificaciones similares a *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="d0d51-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="d0d51-152">La `Student` clase tendrá un aspecto similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="d0d51-153">Agregar a persona al modelo</span><span class="sxs-lookup"><span data-stu-id="d0d51-153">Add Person to the Model</span></span>

<span data-ttu-id="d0d51-154">En *SchoolContext.cs*, agregue un `DbSet` propiedad para el `Person` tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="d0d51-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="d0d51-155">Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía.</span><span class="sxs-lookup"><span data-stu-id="d0d51-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="d0d51-156">Como verá, cuando se actualiza la base de datos, tendrá una `Person` de tabla en lugar de la `Student` y `Instructor` tablas.</span><span class="sxs-lookup"><span data-stu-id="d0d51-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="d0d51-157">Crear y actualizar las migraciones</span><span class="sxs-lookup"><span data-stu-id="d0d51-157">Create and Update Migrations</span></span>

<span data-ttu-id="d0d51-158">En la consola de administrador de paquetes (PMC), escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d0d51-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="d0d51-159">Ejecute el `Update-Database` comando en la PMC.</span><span class="sxs-lookup"><span data-stu-id="d0d51-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="d0d51-160">El comando fallará en este momento porque tenemos las migraciones no sabe cómo tratar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="d0d51-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="d0d51-161">Obtenga un mensaje de error como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="d0d51-162">*No se pudo quitar el objeto ' dbo. Instructor' porque se hace referencia a una restricción FOREIGN KEY.*</span><span class="sxs-lookup"><span data-stu-id="d0d51-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="d0d51-163">Abra *migraciones\&lt; marca de tiempo&gt;\_Inheritance.cs* y reemplace el `Up` método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="d0d51-164">Este código se encarga de las siguientes tareas de actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d0d51-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="d0d51-165">Quita las restricciones de la clave externa y los índices que apuntan a la tabla Student.</span><span class="sxs-lookup"><span data-stu-id="d0d51-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="d0d51-166">Cambia el nombre de la tabla Instructor por Person y realiza los cambios necesarios para que pueda almacenar datos de los alumnos:</span><span class="sxs-lookup"><span data-stu-id="d0d51-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="d0d51-167">Agrega EnrollmentDate que acepta valores NULL para alumnos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="d0d51-168">Agrega la columna discriminadora para indicar si una fila es para un alumno o para un instructor.</span><span class="sxs-lookup"><span data-stu-id="d0d51-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="d0d51-169">Hace que HireDate admita un valor NULL, puesto que las filas de alumnos no dispondrán de fechas de contratación.</span><span class="sxs-lookup"><span data-stu-id="d0d51-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="d0d51-170">Agrega un campo temporal que se usará para actualizar las claves externas que apuntan a los alumnos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="d0d51-171">Cuando copie alumnos en la tabla Person obtendrán nuevos valores de clave principales.</span><span class="sxs-lookup"><span data-stu-id="d0d51-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="d0d51-172">Copia datos de la tabla Student a la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="d0d51-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="d0d51-173">Esto hace que los alumnos se asignen a nuevos valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="d0d51-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="d0d51-174">Corrige los valores de clave externa correspondientes a los alumnos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="d0d51-175">Vuelve a crear las restricciones y los índices de la clave externa, pero ahora los dirige a la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="d0d51-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="d0d51-176">(Si hubiera usado el GUID en lugar de un número entero como tipo de clave principal, los valores de la clave principal de alumno no tendrían que cambiar y algunos de estos pasos se podrían haber omitido).</span><span class="sxs-lookup"><span data-stu-id="d0d51-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="d0d51-177">Ejecute el `update-database` nuevo comando.</span><span class="sxs-lookup"><span data-stu-id="d0d51-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="d0d51-178">(En un sistema de producción provocará que los cambios correspondientes en el método Down en caso de que alguna vez ha tenido usar para volver a la versión anterior de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="d0d51-179">En este tutorial no va a usar el método Down.)</span><span class="sxs-lookup"><span data-stu-id="d0d51-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="d0d51-180">Es posible generar otros errores al migrar datos y realizar cambios de esquema.</span><span class="sxs-lookup"><span data-stu-id="d0d51-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="d0d51-181">Si se producen errores de migración no puede resolver, puede continuar con el tutorial, cambie la cadena de conexión en el *Web.config* de archivo o la base de datos mediante la eliminación.</span><span class="sxs-lookup"><span data-stu-id="d0d51-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="d0d51-182">El enfoque más sencillo consiste en cambiar el nombre de la base de datos en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="d0d51-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="d0d51-183">Por ejemplo, cambiar el nombre de la base de datos a ContosoUniversity2 tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d51-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="d0d51-184">Con una base de datos, no hay ningún dato para migrar y el `update-database` comando es mucho más probable que se completará sin errores.</span><span class="sxs-lookup"><span data-stu-id="d0d51-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="d0d51-185">Para obtener instrucciones sobre cómo eliminar la base de datos, consulte [cómo quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="d0d51-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="d0d51-186">Si adopta este enfoque para poder continuar con el tutorial, omita el paso de implementación al final de este tutorial o implementar un nuevo sitio y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d0d51-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="d0d51-187">Si implementa una actualización en el mismo sitio que ha sido implementando ya, EF obtendrá el mismo error cuando se ejecutan automáticamente las migraciones.</span><span class="sxs-lookup"><span data-stu-id="d0d51-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="d0d51-188">Si desea solucionar un error de las migraciones, el mejor recurso es uno de los foros de Entity Framework o StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="d0d51-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="d0d51-189">Probar la implementación</span><span class="sxs-lookup"><span data-stu-id="d0d51-189">Test the implementation</span></span>

<span data-ttu-id="d0d51-190">Ejecute el sitio Web y prueba en distintas páginas.</span><span class="sxs-lookup"><span data-stu-id="d0d51-190">Run the site and try various pages.</span></span> <span data-ttu-id="d0d51-191">Todo funciona igual que antes.</span><span class="sxs-lookup"><span data-stu-id="d0d51-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="d0d51-192">En **Explorador de servidores,** expanda **datos Connections\SchoolContext** y, a continuación, **tablas**, y verá que el **estudiante** y **Instructor** tablas han sido reemplazadas por un **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="d0d51-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="d0d51-193">Expanda el **persona** tabla y verá que tiene todas las columnas que solían haber en el **estudiante** y **Instructor** tablas.</span><span class="sxs-lookup"><span data-stu-id="d0d51-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="d0d51-194">Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.</span><span class="sxs-lookup"><span data-stu-id="d0d51-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="d0d51-195">El siguiente diagrama muestra la estructura de la nueva base de datos School:</span><span class="sxs-lookup"><span data-stu-id="d0d51-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="d0d51-197">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="d0d51-197">Deploy to Azure</span></span>

<span data-ttu-id="d0d51-198">En esta sección, deberá haber completado opcional **implementar la aplicación en Azure** sección [parte 3, ordenación, filtrado y paginación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="d0d51-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="d0d51-199">Si tenía errores de las migraciones que puede resolver mediante la eliminación de la base de datos en su proyecto local, omita este paso; o crear un nuevo sitio y la base de datos e implementar en el nuevo entorno.</span><span class="sxs-lookup"><span data-stu-id="d0d51-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="d0d51-200">En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="d0d51-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="d0d51-201">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="d0d51-201">Click **Publish**.</span></span>

    <span data-ttu-id="d0d51-202">La aplicación Web se abre en el explorador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d0d51-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="d0d51-203">Probar la aplicación para comprobar funciona.</span><span class="sxs-lookup"><span data-stu-id="d0d51-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="d0d51-204">La primera vez que ejecute una página que tiene acceso a la base de datos, Entity Framework se ejecuta todas las migraciones `Up` métodos necesarios para la base de datos al día con el modelo de datos actual.</span><span class="sxs-lookup"><span data-stu-id="d0d51-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="d0d51-205">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="d0d51-205">Get the code</span></span>

[<span data-ttu-id="d0d51-206">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d0d51-206">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="d0d51-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d0d51-207">Additional resources</span></span>

<span data-ttu-id="d0d51-208">Pueden encontrar vínculos a otros recursos de Entity Framework en el [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d0d51-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="d0d51-209">Para obtener más información acerca de ésta y otras estructuras de herencia, vea [patrón de herencia de TPT](https://msdn.microsoft.com/data/jj618293) y [patrón de herencia de TPH](https://msdn.microsoft.com/data/jj618292) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="d0d51-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="d0d51-210">En el siguiente tutorial, aprenderá a controlar una serie de escenarios de Entity Framework relativamente avanzados.</span><span class="sxs-lookup"><span data-stu-id="d0d51-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0d51-211">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d0d51-211">Next steps</span></span>

<span data-ttu-id="d0d51-212">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="d0d51-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d0d51-213">Ha aprendido para asignar herencia a la base de datos</span><span class="sxs-lookup"><span data-stu-id="d0d51-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="d0d51-214">Crea la clase Person</span><span class="sxs-lookup"><span data-stu-id="d0d51-214">Created the Person class</span></span>
> * <span data-ttu-id="d0d51-215">Student e Instructor actualizado</span><span class="sxs-lookup"><span data-stu-id="d0d51-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="d0d51-216">Persona agregada al modelo</span><span class="sxs-lookup"><span data-stu-id="d0d51-216">Added Person to the Model</span></span>
> * <span data-ttu-id="d0d51-217">Crear y actualizar las migraciones</span><span class="sxs-lookup"><span data-stu-id="d0d51-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="d0d51-218">Probar la implementación</span><span class="sxs-lookup"><span data-stu-id="d0d51-218">Tested the implementation</span></span>
> * <span data-ttu-id="d0d51-219">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="d0d51-219">Deployed to Azure</span></span>

<span data-ttu-id="d0d51-220">Avance al siguiente artículo para obtener información sobre temas que es importante tener en cuenta cuando va más allá de los aspectos básicos del desarrollo de aplicaciones web ASP.NET que usan Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d0d51-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d0d51-221">Escenarios avanzados de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d0d51-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)