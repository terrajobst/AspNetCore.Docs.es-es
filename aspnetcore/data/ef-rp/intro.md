---
title: "Páginas de Razor con Entity Framework Core: Tutorial 1 de 8"
author: rick-anderson
description: "Se muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="871b6-103">Introducción a las páginas de Razor y Entity Framework Core con Visual Studio (1 de 8)</span><span class="sxs-lookup"><span data-stu-id="871b6-103">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="871b6-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="871b6-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="871b6-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web ASP.NET Core 2.0 MVC con Entity Framework (EF) Core 2.0 y Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="871b6-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="871b6-106">La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University.</span><span class="sxs-lookup"><span data-stu-id="871b6-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="871b6-107">Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.</span><span class="sxs-lookup"><span data-stu-id="871b6-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="871b6-108">Esta página es la primera de una serie de tutoriales en los que se explica cómo crear la aplicación de ejemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="871b6-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="871b6-109">Descargue o vea la aplicación completa.</span><span class="sxs-lookup"><span data-stu-id="871b6-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="871b6-110">[Instrucciones de descarga](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="871b6-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="871b6-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="871b6-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="871b6-112">Familiaridad con las [Páginas de Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="871b6-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="871b6-113">Los programadores nuevos deben completar [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) antes de empezar esta serie.</span><span class="sxs-lookup"><span data-stu-id="871b6-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="871b6-114">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="871b6-114">Troubleshooting</span></span>

<span data-ttu-id="871b6-115">Si experimenta un problema que no puede resolver, por lo general podrá encontrar la solución si compara el código con la [fase completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o el [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="871b6-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="871b6-116">Para obtener una lista de errores comunes y cómo resolverlos, vea [la sección de solución de problemas del último tutorial de la serie](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="871b6-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="871b6-117">Si ahí no encuentra lo que necesita, puede publicar una pregunta en [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="871b6-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="871b6-118">Esta serie de tutoriales se basa en lo que se realiza en los tutoriales anteriores.</span><span class="sxs-lookup"><span data-stu-id="871b6-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="871b6-119">Considere la posibilidad de guardar una copia del proyecto después de completar correctamente cada tutorial.</span><span class="sxs-lookup"><span data-stu-id="871b6-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="871b6-120">Si experimenta problemas, puede empezar desde el tutorial anterior en lugar de volver al principio.</span><span class="sxs-lookup"><span data-stu-id="871b6-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="871b6-121">Como alternativa, puede descargar una [fase completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) y empezar de nuevo con la fase completada.</span><span class="sxs-lookup"><span data-stu-id="871b6-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="871b6-122">La aplicación web Contoso University</span><span class="sxs-lookup"><span data-stu-id="871b6-122">The Contoso University web app</span></span>

<span data-ttu-id="871b6-123">La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.</span><span class="sxs-lookup"><span data-stu-id="871b6-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="871b6-124">Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores.</span><span class="sxs-lookup"><span data-stu-id="871b6-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="871b6-125">Estas son algunas de las pantallas que se crean en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="871b6-125">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de Students](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

<span data-ttu-id="871b6-128">El estilo de la interfaz de usuario de este sitio se mantiene fiel a lo que generan las plantillas integradas.</span><span class="sxs-lookup"><span data-stu-id="871b6-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="871b6-129">El tutorial se centra en EF Core con páginas de Razor, no en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="871b6-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="871b6-130">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="871b6-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="871b6-131">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="871b6-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="871b6-132">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="871b6-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="871b6-133">Asigne el nombre **ContosoUniversity** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="871b6-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="871b6-134">Es importante que el nombre del proyecto sea *ContosoUniversity* para que coincidan los espacios de nombres al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="871b6-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="871b6-135">![Nueva aplicación web de ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="871b6-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="871b6-136">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="871b6-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="871b6-137">![Aplicación web (páginas de Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="871b6-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="871b6-138">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="871b6-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="871b6-139">Configurar el estilo del sitio</span><span class="sxs-lookup"><span data-stu-id="871b6-139">Set up the site style</span></span>

<span data-ttu-id="871b6-140">Con algunos cambios se configura el menú del sitio, el diseño y la página principal.</span><span class="sxs-lookup"><span data-stu-id="871b6-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="871b6-141">Abra *Pages/_Layout.cshtml* y realice los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="871b6-142">Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="871b6-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="871b6-143">Hay tres repeticiones.</span><span class="sxs-lookup"><span data-stu-id="871b6-143">There are three occurrences.</span></span>

* <span data-ttu-id="871b6-144">Agregue entradas de menú para **Students**, **Courses**, **Instructors** y **Departments**, y elimine la entrada de menú **Contact**.</span><span class="sxs-lookup"><span data-stu-id="871b6-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="871b6-145">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="871b6-145">The changes are highlighted.</span></span> <span data-ttu-id="871b6-146">(*No* se muestra todo el marcado).</span><span class="sxs-lookup"><span data-stu-id="871b6-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="871b6-147">En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="871b6-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="871b6-148">Presione CTRL+F5 para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="871b6-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="871b6-149">La página principal se muestra con las pestañas creadas en los tutoriales siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Página de inicio de Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="871b6-151">Crear el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="871b6-151">Create the data model</span></span>

<span data-ttu-id="871b6-152">Cree las clases de entidad para la aplicación Contoso University.</span><span class="sxs-lookup"><span data-stu-id="871b6-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="871b6-153">Comience con las tres entidades siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-153">Start with the following three entities:</span></span>

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="871b6-155">Hay una relación uno a varios entre las entidades `Student` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="871b6-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="871b6-156">Hay una relación uno a varios entre las entidades `Course` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="871b6-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="871b6-157">Un estudiante se puede inscribir en cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="871b6-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="871b6-158">Un curso puede tener cualquier número de alumnos inscritos.</span><span class="sxs-lookup"><span data-stu-id="871b6-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="871b6-159">En las secciones siguientes, se crea una clase para cada una de estas entidades.</span><span class="sxs-lookup"><span data-stu-id="871b6-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="871b6-160">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="871b6-160">The Student entity</span></span>

![Diagrama de la entidad Student](intro/_static/student-entity.png)

<span data-ttu-id="871b6-162">Cree una carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="871b6-162">Create a *Models* folder.</span></span> <span data-ttu-id="871b6-163">En la carpeta *Models*, cree un archivo de clase denominado *Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="871b6-164">La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase.</span><span class="sxs-lookup"><span data-stu-id="871b6-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="871b6-165">De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="871b6-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="871b6-166">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="871b6-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="871b6-167">Las propiedades de navegación se vinculan a otras entidades relacionadas con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="871b6-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="871b6-168">En este caso, la propiedad `Enrollments` de una `Student entity` contiene todas las entidades `Enrollment` que están relacionadas con esa entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="871b6-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="871b6-169">Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="871b6-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="871b6-170">Una fila `Enrollment` relacionada es la que contiene el valor de clave principal de ese estudiante en la columna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="871b6-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="871b6-171">Por ejemplo, suponga que el estudiante con ID=1 tiene dos filas en la tabla `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="871b6-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="871b6-172">La tabla `Enrollment` tiene dos filas con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="871b6-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="871b6-173">`StudentID` es una clave externa en la tabla `Enrollment` que especifica el estudiante en la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="871b6-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="871b6-174">Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="871b6-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="871b6-175">Se puede especificar `ICollection<T>`, o bien un tipo como `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="871b6-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="871b6-176">Cuando se usa `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="871b6-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="871b6-177">Las propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.</span><span class="sxs-lookup"><span data-stu-id="871b6-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="871b6-178">La entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="871b6-178">The Enrollment entity</span></span>

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="871b6-180">En la carpeta *Models*, cree *Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="871b6-181">La propiedad `EnrollmentID` es la clave principal.</span><span class="sxs-lookup"><span data-stu-id="871b6-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="871b6-182">En esta entidad se usa el patrón `classnameID` en lugar de `ID` como en la entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="871b6-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="871b6-183">Normalmente, los desarrolladores eligen un patrón y lo usan en todo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="871b6-184">En un tutorial posterior, se muestra el uso de ID sin un nombre de clase para facilitar la implementación de la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="871b6-185">La propiedad `Grade` es una `enum`.</span><span class="sxs-lookup"><span data-stu-id="871b6-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="871b6-186">El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="871b6-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="871b6-187">Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.</span><span class="sxs-lookup"><span data-stu-id="871b6-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="871b6-188">La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`.</span><span class="sxs-lookup"><span data-stu-id="871b6-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="871b6-189">Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="871b6-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="871b6-190">La entidad `Student` difiere de la propiedad de navegación `Student.Enrollments`, que contiene varias entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="871b6-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="871b6-191">La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`.</span><span class="sxs-lookup"><span data-stu-id="871b6-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="871b6-192">Una entidad `Enrollment` está asociada con una entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="871b6-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="871b6-193">EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="871b6-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="871b6-194">Por ejemplo, `StudentID` para la propiedad de navegación `Student`, puesto que la clave principal de la entidad `Student` es `ID`.</span><span class="sxs-lookup"><span data-stu-id="871b6-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="871b6-195">Las propiedades de clave externa también se pueden denominar `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="871b6-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="871b6-196">Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="871b6-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="871b6-197">La entidad Course</span><span class="sxs-lookup"><span data-stu-id="871b6-197">The Course entity</span></span>

![Diagrama de la entidad Course](intro/_static/course-entity.png)

<span data-ttu-id="871b6-199">En la carpeta *Models*, cree *Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="871b6-200">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="871b6-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="871b6-201">Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="871b6-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="871b6-202">El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.</span><span class="sxs-lookup"><span data-stu-id="871b6-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="871b6-203">Crear el contexto de base de datos SchoolContext</span><span class="sxs-lookup"><span data-stu-id="871b6-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="871b6-204">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="871b6-205">El contexto de datos se deriva de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="871b6-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="871b6-206">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="871b6-207">En este proyecto, la clase se denomina `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="871b6-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="871b6-208">En la carpeta del proyecto, cree una carpeta denominada *Data*.</span><span class="sxs-lookup"><span data-stu-id="871b6-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="871b6-209">En la carpeta *Data*, cree *SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="871b6-210">Este código crea una propiedad `DbSet` para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="871b6-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="871b6-211">En la terminología de EF Core:</span><span class="sxs-lookup"><span data-stu-id="871b6-211">In EF Core terminology:</span></span>

* <span data-ttu-id="871b6-212">Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="871b6-213">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="871b6-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="871b6-214">`DbSet<Enrollment>` y `DbSet<Course>` se pueden omitir.</span><span class="sxs-lookup"><span data-stu-id="871b6-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="871b6-215">EF Core las incluye implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="871b6-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="871b6-216">Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="871b6-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="871b6-217">Cuando se crea la base de datos, EF Core crea las tablas con los mismos nombres que los nombres de propiedad `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="871b6-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="871b6-218">Los nombres de propiedad para las colecciones normalmente están en plural (Students en lugar de Student).</span><span class="sxs-lookup"><span data-stu-id="871b6-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="871b6-219">Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben estar en plural.</span><span class="sxs-lookup"><span data-stu-id="871b6-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="871b6-220">Para estos tutoriales, se invalida el comportamiento predeterminado mediante la especificación de nombres de tabla en singular en DbContext.</span><span class="sxs-lookup"><span data-stu-id="871b6-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="871b6-221">Para especificar los nombres de tabla en singular, agregue el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="871b6-222">Registro del contexto con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="871b6-222">Register the context with dependency injection</span></span>

<span data-ttu-id="871b6-223">ASP.NET Core incluye la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="871b6-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="871b6-224">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="871b6-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="871b6-225">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="871b6-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="871b6-226">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="871b6-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="871b6-227">Para registrar `SchoolContext` como servicio, abra *Startup.cs* y agregue las líneas resaltadas al método `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="871b6-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="871b6-228">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="871b6-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="871b6-229">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="871b6-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="871b6-230">Agregue instrucciones `using` para los espacios de nombres `ContosoUniversity.Data` y `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="871b6-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="871b6-231">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="871b6-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="871b6-232">Abra el archivo *appsettings.json* y agregue una cadena de conexión como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="871b6-233">En la cadena de conexión anterior se usa `ConnectRetryCount=0` para evitar que [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) se bloquee.</span><span class="sxs-lookup"><span data-stu-id="871b6-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="871b6-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="871b6-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="871b6-235">La cadena de conexión especifica una base de datos de SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="871b6-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="871b6-236">LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción.</span><span class="sxs-lookup"><span data-stu-id="871b6-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="871b6-237">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="871b6-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="871b6-238">De forma predeterminada, LocalDB crea archivos de base de datos *.mdf* en el directorio `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="871b6-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="871b6-239">Agregar código para inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="871b6-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="871b6-240">EF Core crea una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="871b6-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="871b6-241">En esta sección, se escribe un método *Seed* para rellenarla con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="871b6-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="871b6-242">En la carpeta *Data*, cree un archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="871b6-243">El código comprueba si hay estudiantes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="871b6-244">Si no hay ningún estudiante en la base de datos, se inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="871b6-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="871b6-245">Carga los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="871b6-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="871b6-246">El método `EnsureCreated` crea automáticamente la base de datos para el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="871b6-247">Si la base de datos existe, `EnsureCreated` vuelve sin modificarla.</span><span class="sxs-lookup"><span data-stu-id="871b6-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="871b6-248">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="871b6-249">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="871b6-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="871b6-250">Llamar al método de inicialización, pasándolo al contexto.</span><span class="sxs-lookup"><span data-stu-id="871b6-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="871b6-251">Eliminar el contexto cuando el método de inicialización finalice.</span><span class="sxs-lookup"><span data-stu-id="871b6-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="871b6-252">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="871b6-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="871b6-253">La primera vez que se ejecuta la aplicación, se crea la base de datos y se inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="871b6-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="871b6-254">Cuando se actualice el modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="871b6-254">When the data model is updated:</span></span>
* <span data-ttu-id="871b6-255">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-255">Delete the DB.</span></span>
* <span data-ttu-id="871b6-256">Se actualiza el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="871b6-256">Update the seed method.</span></span>
* <span data-ttu-id="871b6-257">Se ejecuta la aplicación y se crea una base de datos inicializada.</span><span class="sxs-lookup"><span data-stu-id="871b6-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="871b6-258">En los tutoriales posteriores, la base de datos se actualiza cuando cambia el modelo de datos, sin tener que eliminarla y volver a crearla.</span><span class="sxs-lookup"><span data-stu-id="871b6-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="871b6-259">Agregar herramientas de scaffolding</span><span class="sxs-lookup"><span data-stu-id="871b6-259">Add scaffold tooling</span></span>

<span data-ttu-id="871b6-260">En esta sección, se usa la Consola del Administrador de paquetes (PMC) para agregar el paquete de generación de código web de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="871b6-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="871b6-261">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="871b6-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="871b6-262">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="871b6-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="871b6-263">En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="871b6-264">El comando anterior agrega los paquetes NuGet al archivo \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="871b6-264">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="871b6-265">Aplicar scaffolding al modelo</span><span class="sxs-lookup"><span data-stu-id="871b6-265">Scaffold the model</span></span>

* <span data-ttu-id="871b6-266">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="871b6-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="871b6-267">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="871b6-268">Si se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="871b6-269">Vuelva a ejecutar el comando y deje un comentario en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="871b6-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="871b6-270">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="871b6-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="871b6-271">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="871b6-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="871b6-272">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="871b6-272">Build the project.</span></span> <span data-ttu-id="871b6-273">La compilación genera errores similares a los siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="871b6-274">Cambie globalmente `_context.Student` por `_context.Students` (es decir, agregue una "s" a `Student`).</span><span class="sxs-lookup"><span data-stu-id="871b6-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="871b6-275">Se encuentran y actualizan siete repeticiones.</span><span class="sxs-lookup"><span data-stu-id="871b6-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="871b6-276">Esperamos solucionar [este problema](https://github.com/aspnet/Scaffolding/issues/633) en la próxima versión.</span><span class="sxs-lookup"><span data-stu-id="871b6-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="871b6-277">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="871b6-277">Test the app</span></span>

<span data-ttu-id="871b6-278">Ejecute la aplicación y haga clic en el vínculo **Students**.</span><span class="sxs-lookup"><span data-stu-id="871b6-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="871b6-279">Según el ancho del explorador, el vínculo **Students** aparece en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="871b6-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="871b6-280">Si el vínculo **Students** no se ve, haga clic en el icono de navegación en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="871b6-280">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Página de inicio estrecha de Contoso University](intro/_static/home-page-narrow.png)

<span data-ttu-id="871b6-282">Pruebe los vínculos **Create**, **Edit** y **Details**.</span><span class="sxs-lookup"><span data-stu-id="871b6-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="871b6-283">Ver la base de datos</span><span class="sxs-lookup"><span data-stu-id="871b6-283">View the DB</span></span>

<span data-ttu-id="871b6-284">Cuando se inicia la aplicación, `DbInitializer.Initialize` llama a `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="871b6-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="871b6-285">`EnsureCreated` detecta si la base de datos existe y crea una si es necesario.</span><span class="sxs-lookup"><span data-stu-id="871b6-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="871b6-286">Si no hay ningún estudiante en la base de datos, el método `Initialize` los agrega.</span><span class="sxs-lookup"><span data-stu-id="871b6-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="871b6-287">Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="871b6-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="871b6-288">En SSOX, haga clic en **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="871b6-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="871b6-289">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="871b6-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="871b6-290">Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.</span><span class="sxs-lookup"><span data-stu-id="871b6-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="871b6-291">Los archivos de base de datos *.mdf* y *.ldf* se encuentran en la carpeta *C:\Usuarios\\<yourusername>*.</span><span class="sxs-lookup"><span data-stu-id="871b6-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="871b6-292">`EnsureCreated` se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguiente:</span><span class="sxs-lookup"><span data-stu-id="871b6-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="871b6-293">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-293">Delete the DB.</span></span>
* <span data-ttu-id="871b6-294">Se cambia el esquema de base de datos (por ejemplo, se agrega un campo `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="871b6-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="871b6-295">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="871b6-295">Run the app.</span></span>

<span data-ttu-id="871b6-296">`EnsureCreated` crea una base de datos con la columna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="871b6-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="871b6-297">Convenciones</span><span class="sxs-lookup"><span data-stu-id="871b6-297">Conventions</span></span>

<span data-ttu-id="871b6-298">La cantidad de código que se escribe para que EF Core cree una base de datos completa es mínima debido al uso de convenciones o a las suposiciones que hace EF Core.</span><span class="sxs-lookup"><span data-stu-id="871b6-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="871b6-299">Los nombres de las propiedades `DbSet` se usan como nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="871b6-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="871b6-300">Para las entidades a las que no se hace referencia con una propiedad `DbSet`, los nombres de clase de entidad se usan como nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="871b6-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="871b6-301">Los nombres de propiedad de entidad se usan para los nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="871b6-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="871b6-302">Las propiedades de entidad que se denominan ID o classnameID se reconocen como propiedades de clave principal.</span><span class="sxs-lookup"><span data-stu-id="871b6-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="871b6-303">Una propiedad se interpreta como propiedad de clave externa si se denomina *<navigation property name><primary key property name>* (por ejemplo, `StudentID` para la propiedad de navegación `Student`, dado que la clave principal de la entidad `Student` es `ID`).</span><span class="sxs-lookup"><span data-stu-id="871b6-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="871b6-304">Las propiedades de clave externa también se pueden denominar *<primary key property name>* (por ejemplo `EnrollmentID`, dado que la clave principal de la entidad `Enrollment` es `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="871b6-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="871b6-305">El comportamiento de las convenciones se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="871b6-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="871b6-306">Por ejemplo, los nombres de tabla se pueden especificar explícitamente, como se muestra anteriormente en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="871b6-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="871b6-307">Los nombres de columna se pueden establecer explícitamente.</span><span class="sxs-lookup"><span data-stu-id="871b6-307">The column names can be explicitly set.</span></span> <span data-ttu-id="871b6-308">Las claves principales y las claves externas se pueden establecer explícitamente.</span><span class="sxs-lookup"><span data-stu-id="871b6-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="871b6-309">Código asincrónico</span><span class="sxs-lookup"><span data-stu-id="871b6-309">Asynchronous code</span></span>

<span data-ttu-id="871b6-310">La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.</span><span class="sxs-lookup"><span data-stu-id="871b6-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="871b6-311">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="871b6-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="871b6-312">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="871b6-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="871b6-313">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="871b6-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="871b6-314">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="871b6-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="871b6-315">Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="871b6-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="871b6-316">El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="871b6-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="871b6-317">En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.</span><span class="sxs-lookup"><span data-stu-id="871b6-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="871b6-318">En el código siguiente, la palabra clave `async`, el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="871b6-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="871b6-319">La palabra clave `async` indica al compilador que:</span><span class="sxs-lookup"><span data-stu-id="871b6-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="871b6-320">Genere devoluciones de llamada para partes del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="871b6-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="871b6-321">Cree automáticamente el objeto [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="871b6-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="871b6-322">Para más información, vea [Tipo de valor devuelto Task](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="871b6-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="871b6-323">El tipo devuelto implícito `Task` representa el trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="871b6-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="871b6-324">La palabra clave `await` hace que el compilador divida el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="871b6-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="871b6-325">La primera parte termina con la operación que se inició de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="871b6-325">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="871b6-326">La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.</span><span class="sxs-lookup"><span data-stu-id="871b6-326">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="871b6-327">`ToListAsync` es la versión asincrónica del método de extensión `ToList`.</span><span class="sxs-lookup"><span data-stu-id="871b6-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="871b6-328">Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="871b6-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="871b6-329">Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="871b6-330">Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="871b6-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="871b6-331">No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="871b6-331">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="871b6-332">Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="871b6-332">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="871b6-333">Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="871b6-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="871b6-334">Para obtener más información sobre la programación asincrónica en .NET, vea [Información general de Async](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="871b6-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="871b6-335">En el siguiente tutorial, se examinan las operaciones CRUD (crear, leer, actualizar y eliminar) básicas.</span><span class="sxs-lookup"><span data-stu-id="871b6-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="871b6-336">Siguiente</span><span class="sxs-lookup"><span data-stu-id="871b6-336">Next</span></span>](xref:data/ef-rp/crud)
