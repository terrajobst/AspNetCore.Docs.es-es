---
title: "Páginas de Razor con Entity Framework Core - Tutorial 1 de 8"
author: rick-anderson
description: "Muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core"
keywords: Tutorial de ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: d3bcf9aaf7fa809825a0ba8631ee52d3860b090d
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="14791-104">Introducción a las páginas de Razor y Entity Framework Core con Visual Studio (1 de 8)</span><span class="sxs-lookup"><span data-stu-id="14791-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="14791-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14791-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="14791-106">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core 2.0 mediante 2017 de Visual Studio y los núcleos de Entity Framework (EF) 2.0.</span><span class="sxs-lookup"><span data-stu-id="14791-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="14791-107">La aplicación de ejemplo es un sitio web de una universidad ficticia de Contoso.</span><span class="sxs-lookup"><span data-stu-id="14791-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="14791-108">Incluye una funcionalidad, como la admisión de estudiantes, la creación de curso y asignaciones de instructor.</span><span class="sxs-lookup"><span data-stu-id="14791-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="14791-109">Esta página es el primer elemento de una serie de tutoriales que explican cómo crear la aplicación de ejemplo de la Universidad de Contoso.</span><span class="sxs-lookup"><span data-stu-id="14791-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="14791-110">Descargar o ver la aplicación completada.</span><span class="sxs-lookup"><span data-stu-id="14791-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="14791-111">[Instrucciones de descarga](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="14791-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="14791-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="14791-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="14791-113">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="14791-113">Troubleshooting</span></span>

<span data-ttu-id="14791-114">Si experimenta un problema no se puede resolver, por lo general puede encontrar la solución comparando el código para el [completado fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="14791-114">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="14791-115">Para obtener una lista de errores comunes y cómo resolverlos, vea [la sección de solución de problemas del último tutorial de la serie](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="14791-115">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="14791-116">Si no encuentra lo que necesita no existe, puede publicar una pregunta en StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [Core EF](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="14791-116">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="14791-117">Esta serie de tutoriales se basa en lo que se realiza en los tutoriales anteriores.</span><span class="sxs-lookup"><span data-stu-id="14791-117">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="14791-118">Considere la posibilidad de guardar una copia del proyecto tras cada tutorial finalizar correctamente.</span><span class="sxs-lookup"><span data-stu-id="14791-118">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="14791-119">Si experimenta problemas, puede empezar el tutorial anterior en lugar de volver al principio.</span><span class="sxs-lookup"><span data-stu-id="14791-119">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="14791-120">Como alternativa, puede descargar una [completado fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) y comenzar de nuevo con la fase completada.</span><span class="sxs-lookup"><span data-stu-id="14791-120">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="14791-121">La aplicación web de la Universidad de Contoso</span><span class="sxs-lookup"><span data-stu-id="14791-121">The Contoso University web app</span></span>

<span data-ttu-id="14791-122">La aplicación compilada en estos tutoriales es un sitio web de básicos universidad.</span><span class="sxs-lookup"><span data-stu-id="14791-122">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="14791-123">Los usuarios pueden ver y actualizar la información de instructor, curso y estudiantes.</span><span class="sxs-lookup"><span data-stu-id="14791-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="14791-124">Aquí le ofrecemos algunas de las pantallas que creó en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="14791-124">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de estudiantes](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

<span data-ttu-id="14791-127">El estilo de la interfaz de usuario de este sitio es casi lo que se genera por las plantillas integradas.</span><span class="sxs-lookup"><span data-stu-id="14791-127">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="14791-128">El tutorial es núcleo de EF con páginas de Razor, no en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="14791-128">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="14791-129">Crear una aplicación web de las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="14791-129">Create a Razor Pages web app</span></span>

* <span data-ttu-id="14791-130">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="14791-130">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="14791-131">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14791-131">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="14791-132">Denomine el proyecto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="14791-132">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="14791-133">Es importante al proyecto el nombre *ContosoUniversity* para que coincidan con los espacios de nombres al código es copiar y pegar.</span><span class="sxs-lookup"><span data-stu-id="14791-133">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="14791-134">![Nueva aplicación web de ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="14791-134">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="14791-135">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="14791-135">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="14791-136">![Aplicación web (páginas de Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="14791-136">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="14791-137">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="14791-137">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="14791-138">Configurar el estilo de sitio</span><span class="sxs-lookup"><span data-stu-id="14791-138">Set up the site style</span></span>

<span data-ttu-id="14791-139">Algunos cambios configurar en el menú de sitio, el diseño y la página principal.</span><span class="sxs-lookup"><span data-stu-id="14791-139">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="14791-140">Abra *Pages/_Layout.cshtml* y realice los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="14791-140">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="14791-141">Cambiar cada aparición de "ContosoUniversity" a "Universidad de Contoso".</span><span class="sxs-lookup"><span data-stu-id="14791-141">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="14791-142">Hay tres apariciones.</span><span class="sxs-lookup"><span data-stu-id="14791-142">There are three occurrences.</span></span>

* <span data-ttu-id="14791-143">Agregar entradas de menú para **estudiantes**, **cursos**, **instructores**, y **departamentos**y eliminar el **póngase en contacto con** entrada de menú.</span><span class="sxs-lookup"><span data-stu-id="14791-143">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="14791-144">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="14791-144">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-39,47&range=1-50)]

<span data-ttu-id="14791-145">En *Views/Home/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="14791-145">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="14791-146">Presione CTRL+F5 para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="14791-146">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="14791-147">La página principal se muestra con las fichas creadas en los siguientes tutoriales:</span><span class="sxs-lookup"><span data-stu-id="14791-147">The home page is displayed with tabs created in the following tutorials:</span></span>

![Página de inicio de la Universidad de Contoso](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="14791-149">Crear el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="14791-149">Create the data model</span></span>

<span data-ttu-id="14791-150">Crear clases de entidad para la aplicación de la Universidad de Contoso.</span><span class="sxs-lookup"><span data-stu-id="14791-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="14791-151">Comience con las siguientes tres entidades:</span><span class="sxs-lookup"><span data-stu-id="14791-151">Start with the following three entities:</span></span>

![Diagrama del modelo de datos de inscripción de curso de estudiante](intro/_static/data-model-diagram.png)

<span data-ttu-id="14791-153">Hay una relación de uno a varios entre `Student` y `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="14791-154">Hay una relación de uno a varios entre `Course` y `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="14791-155">Un estudiante puede inscribirse en cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="14791-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="14791-156">Un curso puede tener cualquier número de alumnos inscritos en ella.</span><span class="sxs-lookup"><span data-stu-id="14791-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="14791-157">En las secciones siguientes, se crea una clase para cada una de estas entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="14791-158">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="14791-158">The Student entity</span></span>

![Diagrama de la entidad de estudiante](intro/_static/student-entity.png)

<span data-ttu-id="14791-160">Crear un *modelos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="14791-160">Create a *Models* folder.</span></span> <span data-ttu-id="14791-161">En el *modelos* carpeta, cree un archivo de clase denominado *Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="14791-162">El `ID` propiedad se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase.</span><span class="sxs-lookup"><span data-stu-id="14791-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="14791-163">De forma predeterminada, el núcleo de EF interpreta una propiedad que se denomina `ID` o `classnameID` como clave principal.</span><span class="sxs-lookup"><span data-stu-id="14791-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="14791-164">El `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="14791-164">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="14791-165">Propiedades de navegación se vinculan a otras entidades que están relacionados con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="14791-165">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="14791-166">En este caso, el `Enrollments` propiedad de un `Student entity` conserva toda la `Enrollment` entidades que están relacionados con dicho `Student`.</span><span class="sxs-lookup"><span data-stu-id="14791-166">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="14791-167">Por ejemplo, si una fila de estudiante en la base de datos tiene dos inscripción filas relacionadas, el `Enrollments` propiedad de navegación contiene esos dos `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-167">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="14791-168">Un relacionados `Enrollment` fila es una fila que contiene el valor de clave principal de ese estudiante en la `StudentID` columna.</span><span class="sxs-lookup"><span data-stu-id="14791-168">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="14791-169">Por ejemplo, suponga que los estudiantes con Id. = 1 tiene dos filas en la `Enrollment` tabla.</span><span class="sxs-lookup"><span data-stu-id="14791-169">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="14791-170">El `Enrollment` tabla tiene dos filas con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="14791-170">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="14791-171">`StudentID`es una clave externa en la `Enrollment` tabla que especifica los estudiantes en el `Student` tabla.</span><span class="sxs-lookup"><span data-stu-id="14791-171">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="14791-172">Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="14791-172">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="14791-173">`ICollection<T>`se puede especificar, o un tipo como `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="14791-173">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="14791-174">Cuando `ICollection<T>` es usa, Core EF crea un `HashSet<T>` colección de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="14791-174">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="14791-175">Propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.</span><span class="sxs-lookup"><span data-stu-id="14791-175">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="14791-176">La entidad de inscripción</span><span class="sxs-lookup"><span data-stu-id="14791-176">The Enrollment entity</span></span>

![Diagrama de la entidad de inscripción](intro/_static/enrollment-entity.png)

<span data-ttu-id="14791-178">En el *modelos* carpeta, crear *Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-178">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="14791-179">El `EnrollmentID` propiedad es la clave principal.</span><span class="sxs-lookup"><span data-stu-id="14791-179">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="14791-180">Esta entidad se utiliza la `classnameID` de patrón en lugar de `ID` como el `Student` entidad.</span><span class="sxs-lookup"><span data-stu-id="14791-180">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="14791-181">Normalmente los programadores elegir un patrón y utilizarla en todo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-181">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="14791-182">En un tutorial posterior, con el identificador sin classname se muestra para que resulten más fáciles de implementar la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-182">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="14791-183">El `Grade` propiedad es una `enum`.</span><span class="sxs-lookup"><span data-stu-id="14791-183">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="14791-184">El signo de interrogación después de la `Grade` declaración de tipo indica que el `Grade` propiedad es que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="14791-184">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="14791-185">Un grado que sea null es diferente de una puntuación de cero, null significa una puntuación no se conoce o no se ha asignado todavía.</span><span class="sxs-lookup"><span data-stu-id="14791-185">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="14791-186">El `StudentID` propiedad es una clave externa y la propiedad de navegación correspondiente es `Student`.</span><span class="sxs-lookup"><span data-stu-id="14791-186">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="14791-187">Un `Enrollment` está asociada con una entidad `Student` entidad, por lo que la propiedad contiene una sola `Student` entidad.</span><span class="sxs-lookup"><span data-stu-id="14791-187">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="14791-188">El `Student` entidad difiere de la `Student.Enrollments` propiedad de navegación, que contiene varias `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-188">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="14791-189">El `CourseID` propiedad es una clave externa y la propiedad de navegación correspondiente es `Course`.</span><span class="sxs-lookup"><span data-stu-id="14791-189">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="14791-190">Un `Enrollment` está asociada con una entidad `Course` entidad.</span><span class="sxs-lookup"><span data-stu-id="14791-190">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="14791-191">Núcleo EF interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="14791-191">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="14791-192">Por ejemplo,`StudentID` para el `Student` propiedad de navegación, puesto que la `Student` clave principal de la entidad es `ID`.</span><span class="sxs-lookup"><span data-stu-id="14791-192">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="14791-193">Propiedades de clave externa también pueden denominarse `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="14791-193">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="14791-194">Por ejemplo, `CourseID` desde el `Course` clave principal de la entidad es `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="14791-194">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="14791-195">La entidad de curso</span><span class="sxs-lookup"><span data-stu-id="14791-195">The Course entity</span></span>

![Diagrama de la entidad de curso](intro/_static/course-entity.png)

<span data-ttu-id="14791-197">En el *modelos* carpeta, crear *Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-197">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="14791-198">El `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="14791-198">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="14791-199">A `Course` entidad se puede relacionar con cualquier número de `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-199">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="14791-200">El `DatabaseGenerated` atributo permite que la aplicación especificar la clave principal en lugar de tener la base de datos generarlo.</span><span class="sxs-lookup"><span data-stu-id="14791-200">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="14791-201">Crear el contexto de base de datos de SchoolContext</span><span class="sxs-lookup"><span data-stu-id="14791-201">Create the SchoolContext DB context</span></span>

<span data-ttu-id="14791-202">La clase principal que coordina la funcionalidad básica de EF para un modelo de datos concreto es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-202">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="14791-203">El contexto de datos se deriva de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="14791-203">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="14791-204">El contexto de datos especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-204">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="14791-205">En este proyecto, la clase se denomina `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="14791-205">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="14791-206">En la carpeta del proyecto, cree una carpeta denominada *datos*.</span><span class="sxs-lookup"><span data-stu-id="14791-206">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="14791-207">En el *datos* crear carpeta *SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-207">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="14791-208">Este código crea un `DbSet` propiedad para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="14791-208">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="14791-209">En la terminología de EF principales:</span><span class="sxs-lookup"><span data-stu-id="14791-209">In EF Core terminology:</span></span>

* <span data-ttu-id="14791-210">Una entidad establecer normalmente corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-210">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="14791-211">Una entidad corresponde a una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="14791-211">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="14791-212">`DbSet<Enrollment>`y `DbSet<Course>` puede omitirse.</span><span class="sxs-lookup"><span data-stu-id="14791-212">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="14791-213">EF Core incluye implícitamente porque la `Student` las referencias de entidad del `Enrollment` entidad y el `Enrollment` las referencias de entidad la `Course` entidad.</span><span class="sxs-lookup"><span data-stu-id="14791-213">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="14791-214">Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="14791-214">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="14791-215">Cuando se crea la base de datos, Core EF crea las tablas que tienen nombres de la misma que la `DbSet` nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="14791-215">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="14791-216">Nombres de propiedad para las colecciones son normalmente plurales (estudiantes en lugar de estudiante).</span><span class="sxs-lookup"><span data-stu-id="14791-216">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="14791-217">Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales.</span><span class="sxs-lookup"><span data-stu-id="14791-217">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="14791-218">Para estos tutoriales, se invalida el comportamiento predeterminado especificando los nombres de tabla singular en DbContext.</span><span class="sxs-lookup"><span data-stu-id="14791-218">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="14791-219">Para especificar los nombres de tabla singular, agregue el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-219">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="14791-220">Registrar el contexto con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="14791-220">Register the context with dependency injection</span></span>

<span data-ttu-id="14791-221">ASP.NET Core incluye [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="14791-221">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="14791-222">Servicios (por ejemplo, el contexto de la base de datos de EF Core) se registran con inyección de dependencia durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="14791-222">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="14791-223">Los componentes que requieren estos servicios (por ejemplo, las páginas de Razor) se proporcionan estos servicios a través de los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="14791-223">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="14791-224">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="14791-224">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="14791-225">Para registrar `SchoolContext` como un servicio, abra *Startup.cs*y agregue las líneas resaltadas en la `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="14791-225">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="14791-226">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un `DbContextOptionsBuilder` objeto.</span><span class="sxs-lookup"><span data-stu-id="14791-226">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="14791-227">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee las cadenas de conexión desde el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="14791-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="14791-228">Agregar `using` instrucciones para `ContosoUniversity.Data` y `Microsoft.EntityFrameworkCore` los espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="14791-228">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="14791-229">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="14791-229">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="14791-230">Abra la *appSettings.JSON que se* de archivos y agregar una cadena de conexión tal y como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-230">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="14791-231">La cadena de conexión anterior usa `ConnectRetryCount=0` para evitar [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) de que no responden.</span><span class="sxs-lookup"><span data-stu-id="14791-231">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="14791-232">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="14791-232">SQL Server Express LocalDB</span></span>

<span data-ttu-id="14791-233">La cadena de conexión especifica una base de datos de SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="14791-233">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="14791-234">LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está diseñada para el desarrollo de aplicaciones, no su uso en producción.</span><span class="sxs-lookup"><span data-stu-id="14791-234">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="14791-235">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="14791-235">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="14791-236">De forma predeterminada, crea LocalDB *.mdf* archivos de base de datos en el `C:/Users/<user>` directory.</span><span class="sxs-lookup"><span data-stu-id="14791-236">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="14791-237">Agregue código para inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="14791-237">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="14791-238">Núcleo EF crea una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="14791-238">EF Core creates an empty DB.</span></span> <span data-ttu-id="14791-239">En esta sección, un *inicialización* método se escribe para rellenar con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="14791-239">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="14791-240">En el *datos* carpeta, cree un nuevo archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-240">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="14791-241">El código comprueba si hay cualquier estudiantes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-241">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="14791-242">Si no hay ningún alumno en la base de datos, la base de datos es visible con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="14791-242">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="14791-243">Carga los datos de prueba en las matrices en lugar de `List<T>` colecciones para optimizar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="14791-243">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="14791-244">El `EnsureCreated` método crea automáticamente la base de datos para el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-244">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="14791-245">Si no existe la base de datos, `EnsureCreated` devuelve sin modificar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-245">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="14791-246">En *Program.cs*, modifique la `Main` método para hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-246">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="14791-247">Obtener una instancia de contexto de base de datos desde el contenedor de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="14791-247">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="14791-248">Llame al método de inicialización, pasándole el contexto.</span><span class="sxs-lookup"><span data-stu-id="14791-248">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="14791-249">Elimine el contexto cuando se completa el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="14791-249">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="14791-250">El siguiente código muestra actualizado *Program.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="14791-250">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="14791-251">La primera vez que se ejecute la aplicación, la base de datos se crea y propagado con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="14791-251">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="14791-252">Cuando se actualiza el modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="14791-252">When the data model is updated:</span></span>
* <span data-ttu-id="14791-253">Eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-253">Delete the DB.</span></span>
* <span data-ttu-id="14791-254">Actualizar el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="14791-254">Update the seed method.</span></span>
* <span data-ttu-id="14791-255">Ejecutar la aplicación y se crea una nueva base de datos a la inicialización.</span><span class="sxs-lookup"><span data-stu-id="14791-255">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="14791-256">En los tutoriales posteriores, la base de datos se actualiza cuando el modelo de datos los cambios, sin tener que eliminar y volver a crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-256">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="14791-257">Agregar herramientas de scaffolding</span><span class="sxs-lookup"><span data-stu-id="14791-257">Add scaffold tooling</span></span>

<span data-ttu-id="14791-258">En esta sección, se utiliza la consola de administrador de paquete (PMC) para agregar el paquete de generación de código de Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="14791-258">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="14791-259">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="14791-259">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="14791-260">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="14791-260">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="14791-261">En la consola de administrador de paquete (PMC), escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="14791-261">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="14791-262">El comando anterior agrega los paquetes de NuGet para el archivo *.csproj:</span><span class="sxs-lookup"><span data-stu-id="14791-262">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="14791-263">Aplicar la técnica scaffolding el modelo</span><span class="sxs-lookup"><span data-stu-id="14791-263">Scaffold the model</span></span>

* <span data-ttu-id="14791-264">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="14791-264">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="14791-265">Ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="14791-265">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="14791-266">Si se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-266">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="14791-267">Vuelva a ejecutar el comando y deje un comentario en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="14791-267">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="14791-268">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="14791-268">Build the project.</span></span> <span data-ttu-id="14791-269">La compilación genera errores similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="14791-269">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="14791-270">Cambiar globalmente `_context.Student` a `_context.Students` (es decir, agregue una "s" para `Student`).</span><span class="sxs-lookup"><span data-stu-id="14791-270">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="14791-271">7 apariciones se encuentra y se actualizan.</span><span class="sxs-lookup"><span data-stu-id="14791-271">7 occurrences are found and updated.</span></span> <span data-ttu-id="14791-272">Esperamos que corregir [este error](https://github.com/aspnet/Scaffolding/issues/633) en la próxima versión.</span><span class="sxs-lookup"><span data-stu-id="14791-272">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="14791-273">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="14791-273">Test the app</span></span>

<span data-ttu-id="14791-274">Ejecute la aplicación y seleccione la **estudiantes** vínculo.</span><span class="sxs-lookup"><span data-stu-id="14791-274">Run the app and select the **Students** link.</span></span> <span data-ttu-id="14791-275">Según el ancho del explorador, el **estudiantes** vínculo aparece en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="14791-275">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="14791-276">Si el **estudiantes** vínculo no está visible, haga clic en el icono de navegación en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="14791-276">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Página de inicio de la Universidad de Contoso estrecho](intro/_static/home-page-narrow.png)

<span data-ttu-id="14791-278">Prueba la **crear**, **editar**, y **detalles** vínculos.</span><span class="sxs-lookup"><span data-stu-id="14791-278">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="14791-279">Vista de la base de datos</span><span class="sxs-lookup"><span data-stu-id="14791-279">View the DB</span></span>

<span data-ttu-id="14791-280">Cuando se inicia la aplicación, `DbInitializer.Initialize` llamadas `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="14791-280">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="14791-281">`EnsureCreated`detecta si existe la base de datos y crea uno si es necesario.</span><span class="sxs-lookup"><span data-stu-id="14791-281">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="14791-282">Si no hay ningún alumno en la base de datos, la `Initialize` método agrega los alumnos.</span><span class="sxs-lookup"><span data-stu-id="14791-282">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="14791-283">Abra **Explorador de objetos de SQL Server** (SSOX) desde el **vista** menú en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14791-283">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="14791-284">En SSOX, haga clic en **(localdb) \MSSQLLocalDB > bases de datos > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="14791-284">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="14791-285">Expanda el **tablas** nodo.</span><span class="sxs-lookup"><span data-stu-id="14791-285">Expand the **Tables** node.</span></span>

<span data-ttu-id="14791-286">Haga clic en el **Student** de tabla y haga clic en **ver datos** para ver las columnas creadas y las filas insertadas en la tabla.</span><span class="sxs-lookup"><span data-stu-id="14791-286">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="14791-287">El *.mdf* y *.ldf* archivos de base de datos se encuentran en el *C:\Users\<suNombreDeUsuario >* carpeta.</span><span class="sxs-lookup"><span data-stu-id="14791-287">The *.mdf* and *.ldf* DB files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="14791-288">`EnsureCreated`se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguientes:</span><span class="sxs-lookup"><span data-stu-id="14791-288">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="14791-289">Eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-289">Delete the DB.</span></span>
* <span data-ttu-id="14791-290">Cambiar el esquema de base de datos (por ejemplo, agregar un `EmailAddress` campo).</span><span class="sxs-lookup"><span data-stu-id="14791-290">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="14791-291">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="14791-291">Run the app.</span></span>

<span data-ttu-id="14791-292">`EnsureCreated`crea una base de datos con la`EmailAddress` columna.</span><span class="sxs-lookup"><span data-stu-id="14791-292">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="14791-293">Convenciones</span><span class="sxs-lookup"><span data-stu-id="14791-293">Conventions</span></span>

<span data-ttu-id="14791-294">La cantidad de código escrito en orden para EF Core crear una base de datos completa es mínima debido al uso de convenciones o suposiciones que hace que EF Core.</span><span class="sxs-lookup"><span data-stu-id="14791-294">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="14791-295">Los nombres de `DbSet` propiedades se utilizan como nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="14791-295">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="14791-296">Para las entidades que no hace referencia a un `DbSet` propiedad, clase de entidad nombres se utilizan como nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="14791-296">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="14791-297">Nombres de propiedad de entidad se utilizan para los nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="14791-297">Entity property names are used for column names.</span></span>

* <span data-ttu-id="14791-298">Propiedades de la entidad que se denominan identificador o classnameID se reconocen como propiedades de clave principal.</span><span class="sxs-lookup"><span data-stu-id="14791-298">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="14791-299">Una propiedad se interpreta como una propiedad de clave externa si se denomina  *<navigation property name> <primary key property name>*  (por ejemplo, `StudentID` para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`).</span><span class="sxs-lookup"><span data-stu-id="14791-299">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="14791-300">Propiedades de clave externa pueden denominarse  *<primary key property name>*  (por ejemplo, `EnrollmentID` desde el `Enrollment` clave principal de la entidad es `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="14791-300">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="14791-301">Puede reemplazarse un comportamiento convencional.</span><span class="sxs-lookup"><span data-stu-id="14791-301">Conventional behavior can be overridden.</span></span> <span data-ttu-id="14791-302">Por ejemplo, los nombres de tabla pueden especificarse explícitamente, como se muestra anteriormente en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="14791-302">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="14791-303">Los nombres de columna se pueden establecer explícitamente.</span><span class="sxs-lookup"><span data-stu-id="14791-303">The column names can be explicitly set.</span></span> <span data-ttu-id="14791-304">Las claves principales y claves externas se pueden establecer explícitamente.</span><span class="sxs-lookup"><span data-stu-id="14791-304">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="14791-305">Código asincrónico</span><span class="sxs-lookup"><span data-stu-id="14791-305">Asynchronous code</span></span>

<span data-ttu-id="14791-306">La programación asincrónica es el modo predeterminado de EF y de núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14791-306">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="14791-307">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de alta carga todos los subprocesos disponibles pueden estar en uso.</span><span class="sxs-lookup"><span data-stu-id="14791-307">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="14791-308">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que se liberan los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="14791-308">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="14791-309">Con código sincrónico, se pueden acumular muchos subprocesos la mientras que realmente no están realizando ningún trabajo porque está a la espera de e/s completar.</span><span class="sxs-lookup"><span data-stu-id="14791-309">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="14791-310">Con código asincrónico, cuando un proceso está en espera de e/s completar, un subproceso se liberen para el servidor que se usará para procesando otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="14791-310">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="14791-311">Como resultado, código asincrónico permite que los recursos de servidor que se usará de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="14791-311">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="14791-312">Código asincrónico que introduce una pequeña cantidad de sobrecarga en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="14791-312">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="14791-313">En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras en situaciones de mucho tráfico, la mejora del rendimiento potencial es considerable.</span><span class="sxs-lookup"><span data-stu-id="14791-313">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="14791-314">En el código siguiente, la `async` palabra clave, `Task<T>` devolver valor, `await` (palabra clave), y `ToListAsync` método que el código se ejecutará de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="14791-314">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="14791-315">El `async` palabra clave indica al compilador que:</span><span class="sxs-lookup"><span data-stu-id="14791-315">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="14791-316">Generar las devoluciones de llamada para las partes del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="14791-316">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="14791-317">Crear automáticamente la [tarea](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objeto que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="14791-317">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="14791-318">Para obtener más información, consulte [tipo de valor devuelto de la tarea](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="14791-318">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="14791-319">Tipo devuelto de la parte implícita `Task` representa el trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="14791-319">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="14791-320">El `await` (palabra clave) hace que el compilador dividir el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="14791-320">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="14791-321">La primera parte se termina con la operación que se inicia de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="14791-321">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="14791-322">La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.</span><span class="sxs-lookup"><span data-stu-id="14791-322">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="14791-323">`ToListAsync`es la versión asincrónica de la `ToList` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="14791-323">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="14791-324">Algunos aspectos que tener en cuenta al escribir código asincrónico que usa EF principales:</span><span class="sxs-lookup"><span data-stu-id="14791-324">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="14791-325">Solo las instrucciones que hacen que las consultas o comandos se envíen a la base de datos se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="14791-325">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="14791-326">Que incluye, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, y `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="14791-326">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="14791-327">No incluye instrucciones que acaba de cambiar un `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="14791-327">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="14791-328">Un contexto de núcleos de EF no es de subproceso seguro: no intentar llevar a cabo varias operaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="14791-328">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="14791-329">Para sacar partido de las ventajas de rendimiento de código asincrónico, compruebe que paquetes de biblioteca (por ejemplo, para la paginación) usan async si llaman a métodos de EF Core que envían consultas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14791-329">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="14791-330">Para obtener más información sobre la programación asincrónica en. NET, vea [información general de Async](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="14791-330">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="14791-331">En el siguiente tutorial, básica CRUD (crear, leer, actualizar y eliminar) se examinan las operaciones.</span><span class="sxs-lookup"><span data-stu-id="14791-331">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="14791-332">Siguiente</span><span class="sxs-lookup"><span data-stu-id="14791-332">Next</span></span>](xref:data/ef-rp/crud)
