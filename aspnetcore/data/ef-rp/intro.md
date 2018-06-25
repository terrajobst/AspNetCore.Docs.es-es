---
title: 'Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8'
author: rick-anderson
description: Se muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: cadf36f4e1ff3776ad4139e1d7c4e9b73687bc5c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279235"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="3242c-103">Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8</span><span class="sxs-lookup"><span data-stu-id="3242c-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="3242c-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3242c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3242c-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web ASP.NET Core 2.0 MVC con Entity Framework (EF) Core 2.0 y Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3242c-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="3242c-106">La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University.</span><span class="sxs-lookup"><span data-stu-id="3242c-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="3242c-107">Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.</span><span class="sxs-lookup"><span data-stu-id="3242c-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="3242c-108">Esta página es la primera de una serie de tutoriales en los que se explica cómo crear la aplicación de ejemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="3242c-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="3242c-109">Descargue o vea la aplicación completa.</span><span class="sxs-lookup"><span data-stu-id="3242c-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="3242c-110">[Instrucciones de descarga](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="3242c-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3242c-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3242c-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<span data-ttu-id="3242c-112">Familiaridad con las [Páginas de Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="3242c-112">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="3242c-113">Los programadores nuevos deben completar [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) antes de empezar esta serie.</span><span class="sxs-lookup"><span data-stu-id="3242c-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3242c-114">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3242c-114">Troubleshooting</span></span>

<span data-ttu-id="3242c-115">Si experimenta un problema que no puede resolver, por lo general podrá encontrar la solución si compara el código con la [fase completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span><span class="sxs-lookup"><span data-stu-id="3242c-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span></span> <span data-ttu-id="3242c-116">Para obtener una lista de errores comunes y cómo resolverlos, vea [la sección de solución de problemas del último tutorial de la serie](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="3242c-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="3242c-117">Si ahí no encuentra lo que necesita, puede publicar una pregunta en [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="3242c-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="3242c-118">Esta serie de tutoriales se basa en lo que se realiza en los tutoriales anteriores.</span><span class="sxs-lookup"><span data-stu-id="3242c-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="3242c-119">Considere la posibilidad de guardar una copia del proyecto después de completar correctamente cada tutorial.</span><span class="sxs-lookup"><span data-stu-id="3242c-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="3242c-120">Si experimenta problemas, puede empezar desde el tutorial anterior en lugar de volver al principio.</span><span class="sxs-lookup"><span data-stu-id="3242c-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="3242c-121">Como alternativa, puede descargar una [fase completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) y empezar de nuevo con la fase completada.</span><span class="sxs-lookup"><span data-stu-id="3242c-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="3242c-122">La aplicación web Contoso University</span><span class="sxs-lookup"><span data-stu-id="3242c-122">The Contoso University web app</span></span>

<span data-ttu-id="3242c-123">La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.</span><span class="sxs-lookup"><span data-stu-id="3242c-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="3242c-124">Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores.</span><span class="sxs-lookup"><span data-stu-id="3242c-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="3242c-125">Estas son algunas de las pantallas que se crean en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="3242c-125">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de Students](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

<span data-ttu-id="3242c-128">El estilo de la interfaz de usuario de este sitio se mantiene fiel a lo que generan las plantillas integradas.</span><span class="sxs-lookup"><span data-stu-id="3242c-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="3242c-129">El tutorial se centra en EF Core con páginas de Razor, no en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3242c-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="3242c-130">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="3242c-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="3242c-131">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3242c-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3242c-132">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3242c-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="3242c-133">Asigne el nombre **ContosoUniversity** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3242c-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="3242c-134">Es importante que el nombre del proyecto sea *ContosoUniversity* para que coincidan los espacios de nombres al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="3242c-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="3242c-135">![Nueva aplicación web de ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="3242c-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="3242c-136">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="3242c-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="3242c-137">![Aplicación web (páginas de Razor)](../../razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="3242c-137">![Web Application (Razor Pages)](../../razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="3242c-138">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="3242c-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="3242c-139">Configurar el estilo del sitio</span><span class="sxs-lookup"><span data-stu-id="3242c-139">Set up the site style</span></span>

<span data-ttu-id="3242c-140">Con algunos cambios se configura el menú del sitio, el diseño y la página principal.</span><span class="sxs-lookup"><span data-stu-id="3242c-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="3242c-141">Abra *Pages/_Layout.cshtml* y realice los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="3242c-142">Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="3242c-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="3242c-143">Hay tres repeticiones.</span><span class="sxs-lookup"><span data-stu-id="3242c-143">There are three occurrences.</span></span>

* <span data-ttu-id="3242c-144">Agregue entradas de menú para **Students**, **Courses**, **Instructors** y **Departments**, y elimine la entrada de menú **Contact**.</span><span class="sxs-lookup"><span data-stu-id="3242c-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="3242c-145">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="3242c-145">The changes are highlighted.</span></span> <span data-ttu-id="3242c-146">(*No* se muestra todo el marcado).</span><span class="sxs-lookup"><span data-stu-id="3242c-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="3242c-147">En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="3242c-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="3242c-148">Presione CTRL+F5 para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3242c-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="3242c-149">La página principal se muestra con las pestañas creadas en los tutoriales siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Página de inicio de Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="3242c-151">Crear el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="3242c-151">Create the data model</span></span>

<span data-ttu-id="3242c-152">Cree las clases de entidad para la aplicación Contoso University.</span><span class="sxs-lookup"><span data-stu-id="3242c-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="3242c-153">Comience con las tres entidades siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-153">Start with the following three entities:</span></span>

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="3242c-155">Hay una relación uno a varios entre las entidades `Student` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="3242c-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="3242c-156">Hay una relación uno a varios entre las entidades `Course` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="3242c-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="3242c-157">Un estudiante se puede inscribir en cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="3242c-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="3242c-158">Un curso puede tener cualquier número de alumnos inscritos.</span><span class="sxs-lookup"><span data-stu-id="3242c-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="3242c-159">En las secciones siguientes, se crea una clase para cada una de estas entidades.</span><span class="sxs-lookup"><span data-stu-id="3242c-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="3242c-160">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="3242c-160">The Student entity</span></span>

![Diagrama de la entidad Student](intro/_static/student-entity.png)

<span data-ttu-id="3242c-162">Cree una carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="3242c-162">Create a *Models* folder.</span></span> <span data-ttu-id="3242c-163">En la carpeta *Models*, cree un archivo de clase denominado *Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="3242c-164">La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase.</span><span class="sxs-lookup"><span data-stu-id="3242c-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="3242c-165">De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="3242c-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="3242c-166">En `classnameID`, `classname` es el nombre de la clase (como `Student` en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="3242c-166">In `classnameID`, `classname` is the name of the class, such as `Student` in the preceding example.</span></span>

<span data-ttu-id="3242c-167">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3242c-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="3242c-168">Las propiedades de navegación se vinculan a otras entidades relacionadas con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="3242c-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="3242c-169">En este caso, la propiedad `Enrollments` de una `Student entity` contiene todas las entidades `Enrollment` que están relacionadas con esa entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="3242c-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="3242c-170">Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="3242c-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="3242c-171">Una fila `Enrollment` relacionada es la que contiene el valor de clave principal de ese estudiante en la columna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="3242c-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="3242c-172">Por ejemplo, suponga que el estudiante con ID=1 tiene dos filas en la tabla `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="3242c-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="3242c-173">La tabla `Enrollment` tiene dos filas con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="3242c-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="3242c-174">`StudentID` es una clave externa en la tabla `Enrollment` que especifica el estudiante en la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="3242c-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="3242c-175">Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="3242c-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="3242c-176">Se puede especificar `ICollection<T>`, o bien un tipo como `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="3242c-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="3242c-177">Cuando se usa `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3242c-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="3242c-178">Las propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.</span><span class="sxs-lookup"><span data-stu-id="3242c-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="3242c-179">La entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="3242c-179">The Enrollment entity</span></span>

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="3242c-181">En la carpeta *Models*, cree *Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="3242c-182">La propiedad `EnrollmentID` es la clave principal.</span><span class="sxs-lookup"><span data-stu-id="3242c-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="3242c-183">En esta entidad se usa el patrón `classnameID` en lugar de `ID` como en la entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="3242c-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="3242c-184">Normalmente, los desarrolladores eligen un patrón y lo usan en todo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="3242c-185">En un tutorial posterior, se muestra el uso de ID sin un nombre de clase para facilitar la implementación de la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="3242c-186">La propiedad `Grade` es una `enum`.</span><span class="sxs-lookup"><span data-stu-id="3242c-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="3242c-187">El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="3242c-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="3242c-188">Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.</span><span class="sxs-lookup"><span data-stu-id="3242c-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="3242c-189">La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`.</span><span class="sxs-lookup"><span data-stu-id="3242c-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="3242c-190">Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="3242c-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="3242c-191">La entidad `Student` difiere de la propiedad de navegación `Student.Enrollments`, que contiene varias entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="3242c-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="3242c-192">La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`.</span><span class="sxs-lookup"><span data-stu-id="3242c-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="3242c-193">Una entidad `Enrollment` está asociada con una entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="3242c-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="3242c-194">EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="3242c-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="3242c-195">Por ejemplo, `StudentID` para la propiedad de navegación `Student`, puesto que la clave principal de la entidad `Student` es `ID`.</span><span class="sxs-lookup"><span data-stu-id="3242c-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="3242c-196">Las propiedades de clave externa también se pueden denominar `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="3242c-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="3242c-197">Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="3242c-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="3242c-198">La entidad Course</span><span class="sxs-lookup"><span data-stu-id="3242c-198">The Course entity</span></span>

![Diagrama de la entidad Course](intro/_static/course-entity.png)

<span data-ttu-id="3242c-200">En la carpeta *Models*, cree *Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="3242c-201">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3242c-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="3242c-202">Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="3242c-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="3242c-203">El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.</span><span class="sxs-lookup"><span data-stu-id="3242c-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="3242c-204">Crear el contexto de base de datos SchoolContext</span><span class="sxs-lookup"><span data-stu-id="3242c-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="3242c-205">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="3242c-206">El contexto de datos se deriva de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3242c-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="3242c-207">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="3242c-208">En este proyecto, la clase se denomina `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="3242c-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="3242c-209">En la carpeta del proyecto, cree una carpeta denominada *Data*.</span><span class="sxs-lookup"><span data-stu-id="3242c-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="3242c-210">En la carpeta *Data*, cree *SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="3242c-211">Este código crea una propiedad `DbSet` para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3242c-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="3242c-212">En la terminología de EF Core:</span><span class="sxs-lookup"><span data-stu-id="3242c-212">In EF Core terminology:</span></span>

* <span data-ttu-id="3242c-213">Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="3242c-214">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="3242c-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="3242c-215">`DbSet<Enrollment>` y `DbSet<Course>` se pueden omitir.</span><span class="sxs-lookup"><span data-stu-id="3242c-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="3242c-216">EF Core las incluye implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="3242c-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="3242c-217">Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="3242c-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="3242c-218">Cuando se crea la base de datos, EF Core crea las tablas con los mismos nombres que los nombres de propiedad `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="3242c-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="3242c-219">Los nombres de propiedad para las colecciones normalmente están en plural (Students en lugar de Student).</span><span class="sxs-lookup"><span data-stu-id="3242c-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="3242c-220">Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben estar en plural.</span><span class="sxs-lookup"><span data-stu-id="3242c-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="3242c-221">Para estos tutoriales, se invalida el comportamiento predeterminado mediante la especificación de nombres de tabla en singular en DbContext.</span><span class="sxs-lookup"><span data-stu-id="3242c-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="3242c-222">Para especificar los nombres de tabla en singular, agregue el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="3242c-223">Registro del contexto con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="3242c-223">Register the context with dependency injection</span></span>

<span data-ttu-id="3242c-224">ASP.NET Core incluye la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3242c-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3242c-225">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3242c-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3242c-226">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="3242c-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="3242c-227">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="3242c-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="3242c-228">Para registrar `SchoolContext` como servicio, abra *Startup.cs* y agregue las líneas resaltadas al método `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3242c-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="3242c-229">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3242c-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="3242c-230">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3242c-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="3242c-231">Agregue instrucciones `using` para los espacios de nombres `ContosoUniversity.Data` y `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="3242c-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="3242c-232">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3242c-232">Build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="3242c-233">Abra el archivo *appsettings.json* y agregue una cadena de conexión como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="3242c-234">En la cadena de conexión anterior se usa `ConnectRetryCount=0` para evitar que [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) se bloquee.</span><span class="sxs-lookup"><span data-stu-id="3242c-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="3242c-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="3242c-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="3242c-236">La cadena de conexión especifica una base de datos de SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="3242c-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="3242c-237">LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción.</span><span class="sxs-lookup"><span data-stu-id="3242c-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="3242c-238">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="3242c-238">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="3242c-239">De forma predeterminada, LocalDB crea archivos de base de datos *.mdf* en el directorio `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="3242c-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="3242c-240">Agregar código para inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="3242c-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="3242c-241">EF Core crea una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="3242c-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="3242c-242">En esta sección, se escribe un método *Seed* para rellenarla con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="3242c-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="3242c-243">En la carpeta *Data*, cree un archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="3242c-244">El código comprueba si hay estudiantes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="3242c-245">Si no hay ningún estudiante en la base de datos, se inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="3242c-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="3242c-246">Carga los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3242c-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="3242c-247">El método `EnsureCreated` crea automáticamente la base de datos para el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="3242c-248">Si la base de datos existe, `EnsureCreated` vuelve sin modificarla.</span><span class="sxs-lookup"><span data-stu-id="3242c-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="3242c-249">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="3242c-250">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="3242c-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="3242c-251">Llamar al método de inicialización, pasándolo al contexto.</span><span class="sxs-lookup"><span data-stu-id="3242c-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="3242c-252">Eliminar el contexto cuando el método de inicialización finalice.</span><span class="sxs-lookup"><span data-stu-id="3242c-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="3242c-253">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="3242c-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="3242c-254">La primera vez que se ejecuta la aplicación, se crea la base de datos y se inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="3242c-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="3242c-255">Cuando se actualice el modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="3242c-255">When the data model is updated:</span></span>
* <span data-ttu-id="3242c-256">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-256">Delete the DB.</span></span>
* <span data-ttu-id="3242c-257">Se actualiza el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="3242c-257">Update the seed method.</span></span>
* <span data-ttu-id="3242c-258">Se ejecuta la aplicación y se crea una base de datos inicializada.</span><span class="sxs-lookup"><span data-stu-id="3242c-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="3242c-259">En los tutoriales posteriores, la base de datos se actualiza cuando cambia el modelo de datos, sin tener que eliminarla y volver a crearla.</span><span class="sxs-lookup"><span data-stu-id="3242c-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="3242c-260">Agregar herramientas de scaffolding</span><span class="sxs-lookup"><span data-stu-id="3242c-260">Add scaffold tooling</span></span>

<span data-ttu-id="3242c-261">En esta sección, se usa la Consola del Administrador de paquetes (PMC) para agregar el paquete de generación de código web de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3242c-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="3242c-262">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="3242c-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="3242c-263">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="3242c-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="3242c-264">En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="3242c-265">El comando anterior agrega los paquetes NuGet al archivo \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="3242c-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="3242c-266">Aplicar scaffolding al modelo</span><span class="sxs-lookup"><span data-stu-id="3242c-266">Scaffold the model</span></span>

* <span data-ttu-id="3242c-267">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="3242c-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3242c-268">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

<span data-ttu-id="3242c-269">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="3242c-269">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="3242c-270">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="3242c-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="3242c-271">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3242c-271">Build the project.</span></span> <span data-ttu-id="3242c-272">La compilación genera errores similares a los siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-272">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="3242c-273">Cambie globalmente `_context.Student` por `_context.Students` (es decir, agregue una "s" a `Student`).</span><span class="sxs-lookup"><span data-stu-id="3242c-273">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="3242c-274">Se encuentran y actualizan siete repeticiones.</span><span class="sxs-lookup"><span data-stu-id="3242c-274">7 occurrences are found and updated.</span></span> <span data-ttu-id="3242c-275">Esperamos solucionar [este problema](https://github.com/aspnet/Scaffolding/issues/633) en la próxima versión.</span><span class="sxs-lookup"><span data-stu-id="3242c-275">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="3242c-276">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3242c-276">Test the app</span></span>

<span data-ttu-id="3242c-277">Ejecute la aplicación y haga clic en el vínculo **Students**.</span><span class="sxs-lookup"><span data-stu-id="3242c-277">Run the app and select the **Students** link.</span></span> <span data-ttu-id="3242c-278">Según el ancho del explorador, el vínculo **Students** aparece en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="3242c-278">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="3242c-279">Si el vínculo **Students** no se ve, haga clic en el icono de navegación en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="3242c-279">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Página de inicio estrecha de Contoso University](intro/_static/home-page-narrow.png)

<span data-ttu-id="3242c-281">Pruebe los vínculos **Create**, **Edit** y **Details**.</span><span class="sxs-lookup"><span data-stu-id="3242c-281">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="3242c-282">Ver la base de datos</span><span class="sxs-lookup"><span data-stu-id="3242c-282">View the DB</span></span>

<span data-ttu-id="3242c-283">Cuando se inicia la aplicación, `DbInitializer.Initialize` llama a `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="3242c-283">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="3242c-284">`EnsureCreated` detecta si la base de datos existe y crea una si es necesario.</span><span class="sxs-lookup"><span data-stu-id="3242c-284">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="3242c-285">Si no hay ningún estudiante en la base de datos, el método `Initialize` los agrega.</span><span class="sxs-lookup"><span data-stu-id="3242c-285">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="3242c-286">Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3242c-286">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="3242c-287">En SSOX, haga clic en **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="3242c-287">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="3242c-288">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="3242c-288">Expand the **Tables** node.</span></span>

<span data-ttu-id="3242c-289">Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.</span><span class="sxs-lookup"><span data-stu-id="3242c-289">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="3242c-290">Los archivos de base de datos <em>.mdf</em> y <em>.ldf</em> se encuentran en la carpeta <em>C:\Usuarios\\<yourusername></em>.</span><span class="sxs-lookup"><span data-stu-id="3242c-290">The <em>.mdf</em> and <em>.ldf</em> DB files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="3242c-291">`EnsureCreated` se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3242c-291">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="3242c-292">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-292">Delete the DB.</span></span>
* <span data-ttu-id="3242c-293">Se cambia el esquema de base de datos (por ejemplo, se agrega un campo `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="3242c-293">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="3242c-294">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3242c-294">Run the app.</span></span>

<span data-ttu-id="3242c-295">`EnsureCreated` crea una base de datos con la columna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="3242c-295">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="3242c-296">Convenciones</span><span class="sxs-lookup"><span data-stu-id="3242c-296">Conventions</span></span>

<span data-ttu-id="3242c-297">La cantidad de código que se escribe para que EF Core cree una base de datos completa es mínima debido al uso de convenciones o a las suposiciones que hace EF Core.</span><span class="sxs-lookup"><span data-stu-id="3242c-297">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="3242c-298">Los nombres de las propiedades `DbSet` se usan como nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="3242c-298">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="3242c-299">Para las entidades a las que no se hace referencia con una propiedad `DbSet`, los nombres de clase de entidad se usan como nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="3242c-299">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="3242c-300">Los nombres de propiedad de entidad se usan para los nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="3242c-300">Entity property names are used for column names.</span></span>

* <span data-ttu-id="3242c-301">Las propiedades de entidad que se denominan ID o classnameID se reconocen como propiedades de clave principal.</span><span class="sxs-lookup"><span data-stu-id="3242c-301">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="3242c-302">Una propiedad se interpreta como propiedad de clave externa si se denomina *<navigation property name><primary key property name>* (por ejemplo, `StudentID` para la propiedad de navegación `Student`, dado que la clave principal de la entidad `Student` es `ID`).</span><span class="sxs-lookup"><span data-stu-id="3242c-302">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="3242c-303">Las propiedades de clave externa también se pueden denominar *<primary key property name>* (por ejemplo `EnrollmentID`, dado que la clave principal de la entidad `Enrollment` es `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="3242c-303">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="3242c-304">El comportamiento de las convenciones se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="3242c-304">Conventional behavior can be overridden.</span></span> <span data-ttu-id="3242c-305">Por ejemplo, los nombres de tabla se pueden especificar explícitamente, como se muestra anteriormente en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3242c-305">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="3242c-306">Los nombres de columna se pueden establecer explícitamente.</span><span class="sxs-lookup"><span data-stu-id="3242c-306">The column names can be explicitly set.</span></span> <span data-ttu-id="3242c-307">Las claves principales y las claves externas se pueden establecer explícitamente.</span><span class="sxs-lookup"><span data-stu-id="3242c-307">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="3242c-308">Código asincrónico</span><span class="sxs-lookup"><span data-stu-id="3242c-308">Asynchronous code</span></span>

<span data-ttu-id="3242c-309">La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.</span><span class="sxs-lookup"><span data-stu-id="3242c-309">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="3242c-310">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="3242c-310">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="3242c-311">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="3242c-311">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="3242c-312">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="3242c-312">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="3242c-313">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3242c-313">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="3242c-314">Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="3242c-314">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="3242c-315">El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="3242c-315">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="3242c-316">En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.</span><span class="sxs-lookup"><span data-stu-id="3242c-316">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="3242c-317">En el código siguiente, la palabra clave `async`, el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3242c-317">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="3242c-318">La palabra clave `async` indica al compilador que:</span><span class="sxs-lookup"><span data-stu-id="3242c-318">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="3242c-319">Genere devoluciones de llamada para partes del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="3242c-319">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="3242c-320">Cree automáticamente el objeto [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="3242c-320">Automatically create the [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="3242c-321">Para más información, vea [Tipo de valor devuelto Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="3242c-321">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="3242c-322">El tipo devuelto implícito `Task` representa el trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="3242c-322">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="3242c-323">La palabra clave `await` hace que el compilador divida el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="3242c-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="3242c-324">La primera parte termina con la operación que se inició de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3242c-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="3242c-325">La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.</span><span class="sxs-lookup"><span data-stu-id="3242c-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="3242c-326">`ToListAsync` es la versión asincrónica del método de extensión `ToList`.</span><span class="sxs-lookup"><span data-stu-id="3242c-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="3242c-327">Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="3242c-327">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="3242c-328">Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-328">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="3242c-329">Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="3242c-329">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="3242c-330">No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="3242c-330">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="3242c-331">Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="3242c-331">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="3242c-332">Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3242c-332">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="3242c-333">Para obtener más información sobre la programación asincrónica en .NET, vea [Información general de Async](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="3242c-333">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="3242c-334">En el siguiente tutorial, se examinan las operaciones CRUD (crear, leer, actualizar y eliminar) básicas.</span><span class="sxs-lookup"><span data-stu-id="3242c-334">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3242c-335">Siguiente</span><span class="sxs-lookup"><span data-stu-id="3242c-335">Next</span></span>](xref:data/ef-rp/crud)
