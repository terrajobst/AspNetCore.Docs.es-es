---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementar la herencia con Entity Framework en una aplicación de MVC de ASP.NET (8 de 10) | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="1ea2b-103">Implementar la herencia con Entity Framework en una aplicación de MVC de ASP.NET (8 de 10)</span><span class="sxs-lookup"><span data-stu-id="1ea2b-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="1ea2b-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1ea2b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1ea2b-105">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="1ea2b-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="1ea2b-106">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="1ea2b-107">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="1ea2b-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="1ea2b-108">Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="1ea2b-109">Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="1ea2b-110">Por lo general puede encontrar la solución al problema comparando el código para el código completo.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="1ea2b-111">Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="1ea2b-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="1ea2b-112">En el tutorial anterior para administrar las excepciones de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="1ea2b-113">En este tutorial se muestra cómo implementar la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="1ea2b-114">En la programación orientada a objetos, puede utilizar la herencia para eliminar código redundante.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="1ea2b-115">En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="1ea2b-116">No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="1ea2b-117">Tabla por jerarquía frente a la herencia de tabla por tipo</span><span class="sxs-lookup"><span data-stu-id="1ea2b-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="1ea2b-118">En la programación orientada a objetos, puede utilizar la herencia para que sea más fácil trabajar con clases relacionadas.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="1ea2b-119">Por ejemplo, el `Instructor` y `Student` clases en el `School` modelo de datos comparten varias propiedades, que da como resultado código redundante:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="1ea2b-121">Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="1ea2b-122">Puede crear un `Person` clase que contiene solo las propiedades comparten base, a continuación, realizar la `Instructor` y `Student` entidades heredan de esa clase base, como se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="1ea2b-124">Esta estructura de herencia se puede representar de varias formas en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="1ea2b-125">Podría tener un `Person` tabla que incluye información acerca de los alumnos e instructores en una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="1ea2b-126">Algunas de las columnas pudieron aplicar solo a instructores (`HireDate`), algunas solo a los alumnos (`EnrollmentDate`), algunos a los métodos (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="1ea2b-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="1ea2b-127">Por lo general, tendría un *discriminador* representa la columna para indicar qué tipo de cada fila.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="1ea2b-128">Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="1ea2b-130">Este patrón consiste en generar una estructura de herencia de la entidad de una tabla de base de datos único se denomina *tabla por jerarquía* herencia (TPH).</span><span class="sxs-lookup"><span data-stu-id="1ea2b-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="1ea2b-131">Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="1ea2b-132">Por ejemplo, podría tener sólo los campos de nombre el `Person` de tabla y tenga distintos `Instructor` y `Student` tablas con los campos de fecha.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="1ea2b-134">Este modelo de proceso de realizar una tabla de base de datos para cada clase de entidad se denomina *tabla por tipo* herencia (TPT).</span><span class="sxs-lookup"><span data-stu-id="1ea2b-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="1ea2b-135">Patrones de herencia TPH suele proporcionar un mejor rendimiento en Entity Framework de patrones de herencia de TPT, porque pueden dar lugar a patrones TPT en consultas de combinaciones complejas.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="1ea2b-136">Este tutorial muestra cómo implementar la herencia de TPH.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="1ea2b-137">Llevará a cabo mediante la realización de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="1ea2b-138">Crear un `Person` clase y cambiar la `Instructor` y `Student` derivar de las clases `Person`.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="1ea2b-139">Agregar código de asignación de modelo para bases de datos a la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="1ea2b-140">Cambio `InstructorID` y `StudentID` las referencias en el proyecto para `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="1ea2b-141">Crear la clase de persona</span><span class="sxs-lookup"><span data-stu-id="1ea2b-141">Creating the Person Class</span></span>

 <span data-ttu-id="1ea2b-142">Nota: No podrá compilar el proyecto después de crear las clases siguientes hasta que actualice los controladores que usa estas clases.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="1ea2b-143">En el *modelos* carpeta, crear *Person.cs* y reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="1ea2b-144">En *Instructor.cs*, derivar el `Instructor` clase desde el `Person` clase y quite los campos de clave y el nombres.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="1ea2b-145">El código tendrá un aspecto similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="1ea2b-146">Realizar modificaciones similares a *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="1ea2b-147">La `Student` clase tendrá un aspecto similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="1ea2b-148">Agregar el tipo de entidad de persona al modelo</span><span class="sxs-lookup"><span data-stu-id="1ea2b-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="1ea2b-149">En *SchoolContext.cs*, agregue un `DbSet` propiedad para el `Person` tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="1ea2b-150">Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="1ea2b-151">Como verá, cuando se vuelva a crear la base de datos tendrá un `Person` tabla en lugar de la `Student` y `Instructor` tablas.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="1ea2b-152">Cambiar InstructorID y StudentID a PersonID</span><span class="sxs-lookup"><span data-stu-id="1ea2b-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="1ea2b-153">En *SchoolContext.cs*, en la instrucción de asignación Instructor curso, cambiar `MapRightKey("InstructorID")` a `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="1ea2b-154">Este cambio no es necesario; solo cambia el nombre de la columna InstructorID en la tabla de combinación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="1ea2b-155">Si se deja el nombre como InstructorID, la aplicación todavía podría funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="1ea2b-156">Este es el completado *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="1ea2b-157">A continuación debe cambiar `InstructorID` a `PersonID` y `StudentID` a `PersonID` a lo largo del proyecto ***excepto*** en los archivos de las migraciones con marca de tiempo en el *migraciones* carpeta.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="1ea2b-158">Para ello podrá buscar y abrir sólo los archivos que deben cambiarse y después realizar un cambio global en los archivos abiertos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="1ea2b-159">El único archivo en el *migraciones* carpeta debe cambiar es *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="1ea2b-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="1ea2b-160">Comience por cerrar todos los archivos abiertos en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="1ea2b-161">Haga clic en **buscar y reemplazar: encontrar todos los archivos** en el **editar** menú y, a continuación, busque todos los archivos en el proyecto que contienen `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="1ea2b-162">Abra cada archivo en el **resultados de la búsqueda** ventana ***excepto*** el &lt;marca de tiempo&gt;*\_.cs* archivos de migración en el *Migraciones* carpeta, haciendo doble clic en una línea para cada archivo.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="1ea2b-163">Abra la **reemplazar en archivos** cuadro de diálogo y cambio **buscar en** a **todos los documentos abiertos**.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="1ea2b-164">Use la **reemplazar en archivos** cuadro de diálogo para todas las cambiar `InstructorID` a `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="1ea2b-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="1ea2b-165">Buscar todos los archivos en el proyecto que contienen `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="1ea2b-166">Abra cada archivo en el **resultados de la búsqueda** ventana ***excepto*** el &lt;marca de tiempo&gt;*\_\*.cs* archivos de migración en el *migraciones* carpeta, haciendo doble clic en una línea para cada archivo.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="1ea2b-167">Abra la **reemplazar en archivos** cuadro de diálogo y cambio **buscar en** a **todos los documentos abiertos**.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="1ea2b-168">Use la **reemplazar en archivos** cuadro de diálogo para todas las cambiar `StudentID` a `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="1ea2b-169">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-169">Build the project.</span></span>

<span data-ttu-id="1ea2b-170">(Tenga en cuenta que esto se muestra un *desventaja* de la `classnameID` patrón para asignar nombres a las claves principales.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="1ea2b-171">Si se hubiera designado Id. de las claves principales sin un prefijo del nombre de clase, *sin* cambiar el nombre sería necesario ahora.)</span><span class="sxs-lookup"><span data-stu-id="1ea2b-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="1ea2b-172">Crear y actualizar un archivo de migraciones</span><span class="sxs-lookup"><span data-stu-id="1ea2b-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="1ea2b-173">En la consola de administrador de paquete (PMC), escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="1ea2b-174">Ejecute el `Update-Database` comando el PMC.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="1ea2b-175">El comando dará error en este momento porque es necesario que los datos existentes que migraciones no sepa cómo controlar.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="1ea2b-176">Se produce el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-176">You get the following error:</span></span>

<span data-ttu-id="1ea2b-177">*La instrucción ALTER TABLE entran en conflicto con la restricción FOREIGN KEY "FK\_dbo. Departamento\_dbo. Persona\_PersonID ". Se produjo el conflicto en la tabla base de datos "ContosoUniversity", "dbo. Persona", columna 'PersonID'.*</span><span class="sxs-lookup"><span data-stu-id="1ea2b-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="1ea2b-178">Abra *migraciones\&lt; marca de tiempo&gt;\_Inheritance.cs* y reemplace el `Up` método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="1ea2b-179">Ejecute el `update-database` comando nuevo.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="1ea2b-180">Es posible obtener otros errores durante la migración de datos y realizar cambios de esquema.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="1ea2b-181">Si se producen errores de migración no puede resolver, puede continuar con el tutorial, cambie la cadena de conexión en el *Web.config* archivo o eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="1ea2b-182">El enfoque más sencillo consiste en cambiar el nombre de la base de datos en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="1ea2b-183">Por ejemplo, cambiar el nombre de la base de datos a CU\_probar tal y como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="1ea2b-184">Con una base de datos, no hay ningún dato para migrar y el `update-database` comando es mucho más probable que se completan sin errores.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="1ea2b-185">Para obtener instrucciones sobre cómo eliminar la base de datos, vea [cómo quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="1ea2b-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="1ea2b-186">Si adopta este enfoque para poder continuar con el tutorial, omita el paso de implementación al final de este tutorial, puesto que el sitio implementado obtendría el mismo error cuando se ejecuta automáticamente las migraciones.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="1ea2b-187">Si desea solucionar un error de las migraciones, el mejor recurso es uno de los foros de Entity Framework o StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="1ea2b-188">Pruebas</span><span class="sxs-lookup"><span data-stu-id="1ea2b-188">Testing</span></span>

<span data-ttu-id="1ea2b-189">Ejecute el sitio Web y vuelva a intentarlo varias páginas.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-189">Run the site and try various pages.</span></span> <span data-ttu-id="1ea2b-190">Todo funciona igual que antes.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="1ea2b-191">En **Explorador de servidores,** expanda **SchoolContext** y, a continuación, **tablas**, y verá que la **Student** y **Instructor**  tablas se han reemplazado por un **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="1ea2b-192">Expanda el **persona** tabla y verá que tiene todas las columnas que solían haber en el **Student** y **Instructor** tablas.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="1ea2b-194">Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="1ea2b-195">El siguiente diagrama muestra la estructura de la nueva base de datos School:</span><span class="sxs-lookup"><span data-stu-id="1ea2b-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="1ea2b-197">Resumen</span><span class="sxs-lookup"><span data-stu-id="1ea2b-197">Summary</span></span>

<span data-ttu-id="1ea2b-198">Herencia de tabla por jerarquía ahora se ha implementado para la `Person`, `Student`, y `Instructor` clases.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="1ea2b-199">Para obtener más información acerca de ésta y otras estructuras de herencia, vea [estrategias de asignación de herencia](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) en el blog de Morteza Manavi.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="1ea2b-200">En el siguiente tutorial, verá algunas maneras de implementar el repositorio y una unidad de patrones de trabajo.</span><span class="sxs-lookup"><span data-stu-id="1ea2b-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="1ea2b-201">Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="1ea2b-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ea2b-202">[Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="1ea2b-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
