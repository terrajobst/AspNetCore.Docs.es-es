---
title: 'Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8'
author: rick-anderson
description: Se muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core
ms.author: riande
ms.custom: seodec18
ms.date: 11/22/2018
uid: data/ef-rp/intro
ms.openlocfilehash: 7b048c8543f51a158530b8c7952b5aec7b753b42
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264698"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="f3437-103">Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8</span><span class="sxs-lookup"><span data-stu-id="f3437-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f3437-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f3437-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f3437-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear una aplicación web de Razor Pages de ASP.NET Core con Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="f3437-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="f3437-106">La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University.</span><span class="sxs-lookup"><span data-stu-id="f3437-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="f3437-107">Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.</span><span class="sxs-lookup"><span data-stu-id="f3437-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="f3437-108">Esta página es la primera de una serie de tutoriales en los que se explica cómo crear la aplicación de ejemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="f3437-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="f3437-109">Descargue o vea la aplicación completa.</span><span class="sxs-lookup"><span data-stu-id="f3437-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="f3437-110">[Instrucciones de descarga](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f3437-110">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3437-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f3437-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3437-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3437-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f3437-113">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3437-113">.NET Core CLI</span></span>](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

<span data-ttu-id="f3437-114">Familiaridad con las [Páginas de Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f3437-114">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="f3437-115">Los programadores nuevos deben completar [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) antes de empezar esta serie.</span><span class="sxs-lookup"><span data-stu-id="f3437-115">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f3437-116">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="f3437-116">Troubleshooting</span></span>

<span data-ttu-id="f3437-117">Si experimenta un problema que no puede resolver, por lo general podrá encontrar la solución si compara el código con el [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="f3437-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="f3437-118">Una buena forma de obtener ayuda consiste en publicar una pregunta en [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="f3437-118">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="f3437-119">La aplicación web Contoso University</span><span class="sxs-lookup"><span data-stu-id="f3437-119">The Contoso University web app</span></span>

<span data-ttu-id="f3437-120">La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.</span><span class="sxs-lookup"><span data-stu-id="f3437-120">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="f3437-121">Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores.</span><span class="sxs-lookup"><span data-stu-id="f3437-121">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="f3437-122">Estas son algunas de las pantallas que se crean en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="f3437-122">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de Students](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

<span data-ttu-id="f3437-125">El estilo de la interfaz de usuario de este sitio se mantiene fiel a lo que generan las plantillas integradas.</span><span class="sxs-lookup"><span data-stu-id="f3437-125">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="f3437-126">El tutorial se centra en EF Core con páginas de Razor, no en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="f3437-126">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="f3437-127">Creación de la aplicación web de Razor Pages ContosoUniversity</span><span class="sxs-lookup"><span data-stu-id="f3437-127">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3437-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3437-128">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f3437-129">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f3437-129">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f3437-130">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3437-130">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f3437-131">Asigne el nombre **ContosoUniversity** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3437-131">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="f3437-132">Es importante que el nombre del proyecto sea *ContosoUniversity* para que coincidan los espacios de nombres al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="f3437-132">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="f3437-133">Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="f3437-133">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="f3437-134">Para ver las imágenes de los pasos anteriores, consulte [Creación de una aplicación web de Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="f3437-134">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="f3437-135">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f3437-135">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f3437-136">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3437-136">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="f3437-137">Configurar el estilo del sitio</span><span class="sxs-lookup"><span data-stu-id="f3437-137">Set up the site style</span></span>

<span data-ttu-id="f3437-138">Con algunos cambios se configura el menú del sitio, el diseño y la página principal.</span><span class="sxs-lookup"><span data-stu-id="f3437-138">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="f3437-139">Actualice *Pages/Shared/_Layout.cshtml* con los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="f3437-139">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="f3437-140">Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="f3437-140">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="f3437-141">Hay tres repeticiones.</span><span class="sxs-lookup"><span data-stu-id="f3437-141">There are three occurrences.</span></span>

* <span data-ttu-id="f3437-142">Agregue entradas de menú para **Students**, **Courses**, **Instructors** y **Departments**, y elimine la entrada de menú **Contact**.</span><span class="sxs-lookup"><span data-stu-id="f3437-142">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="f3437-143">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="f3437-143">The changes are highlighted.</span></span> <span data-ttu-id="f3437-144">(*No* se muestra todo el marcado).</span><span class="sxs-lookup"><span data-stu-id="f3437-144">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="f3437-145">En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="f3437-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="f3437-146">Crear el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="f3437-146">Create the data model</span></span>

<span data-ttu-id="f3437-147">Cree las clases de entidad para la aplicación Contoso University.</span><span class="sxs-lookup"><span data-stu-id="f3437-147">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="f3437-148">Comience con las tres entidades siguientes:</span><span class="sxs-lookup"><span data-stu-id="f3437-148">Start with the following three entities:</span></span>

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="f3437-150">Hay una relación uno a varios entre las entidades `Student` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f3437-150">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="f3437-151">Hay una relación uno a varios entre las entidades `Course` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f3437-151">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="f3437-152">Un estudiante se puede inscribir en cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="f3437-152">A student can enroll in any number of courses.</span></span> <span data-ttu-id="f3437-153">Un curso puede tener cualquier número de alumnos inscritos.</span><span class="sxs-lookup"><span data-stu-id="f3437-153">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="f3437-154">En las secciones siguientes, se crea una clase para cada una de estas entidades.</span><span class="sxs-lookup"><span data-stu-id="f3437-154">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="f3437-155">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="f3437-155">The Student entity</span></span>

![Diagrama de la entidad Student](intro/_static/student-entity.png)

<span data-ttu-id="f3437-157">Cree una carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="f3437-157">Create a *Models* folder.</span></span> <span data-ttu-id="f3437-158">En la carpeta *Models*, cree un archivo de clase denominado *Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-158">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="f3437-159">La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase.</span><span class="sxs-lookup"><span data-stu-id="f3437-159">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="f3437-160">De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="f3437-160">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="f3437-161">En `classnameID`, `classname` es el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="f3437-161">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="f3437-162">En el ejemplo anterior, la clave principal alternativa que se reconoce de forma automática es `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="f3437-162">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="f3437-163">La propiedad `Enrollments` es una [propiedad de navegación](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="f3437-163">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="f3437-164">Las propiedades de navegación se vinculan a otras entidades relacionadas con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="f3437-164">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="f3437-165">En este caso, la propiedad `Enrollments` de una `Student entity` contiene todas las entidades `Enrollment` que están relacionadas con esa entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="f3437-165">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="f3437-166">Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f3437-166">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="f3437-167">Una fila `Enrollment` relacionada es la que contiene el valor de clave principal de ese estudiante en la columna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="f3437-167">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="f3437-168">Por ejemplo, suponga que el estudiante con ID=1 tiene dos filas en la tabla `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f3437-168">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="f3437-169">La tabla `Enrollment` tiene dos filas con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="f3437-169">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="f3437-170">`StudentID` es una clave externa en la tabla `Enrollment` que especifica el estudiante en la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="f3437-170">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="f3437-171">Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="f3437-171">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="f3437-172">Se puede especificar `ICollection<T>`, o bien un tipo como `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="f3437-172">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="f3437-173">Cuando se usa `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f3437-173">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="f3437-174">Las propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.</span><span class="sxs-lookup"><span data-stu-id="f3437-174">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="f3437-175">La entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="f3437-175">The Enrollment entity</span></span>

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="f3437-177">En la carpeta *Models*, cree *Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-177">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="f3437-178">La propiedad `EnrollmentID` es la clave principal.</span><span class="sxs-lookup"><span data-stu-id="f3437-178">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="f3437-179">En esta entidad se usa el patrón `classnameID` en lugar de `ID` como en la entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="f3437-179">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="f3437-180">Normalmente, los desarrolladores eligen un patrón y lo usan en todo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-180">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="f3437-181">En un tutorial posterior, se muestra el uso de ID sin un nombre de clase para facilitar la implementación de la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-181">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="f3437-182">La propiedad `Grade` es una `enum`.</span><span class="sxs-lookup"><span data-stu-id="f3437-182">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="f3437-183">El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="f3437-183">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="f3437-184">Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.</span><span class="sxs-lookup"><span data-stu-id="f3437-184">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="f3437-185">La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`.</span><span class="sxs-lookup"><span data-stu-id="f3437-185">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="f3437-186">Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="f3437-186">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="f3437-187">La entidad `Student` difiere de la propiedad de navegación `Student.Enrollments`, que contiene varias entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f3437-187">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="f3437-188">La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`.</span><span class="sxs-lookup"><span data-stu-id="f3437-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="f3437-189">Una entidad `Enrollment` está asociada con una entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="f3437-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="f3437-190">EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f3437-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="f3437-191">Por ejemplo, `StudentID` para la propiedad de navegación `Student`, puesto que la clave principal de la entidad `Student` es `ID`.</span><span class="sxs-lookup"><span data-stu-id="f3437-191">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="f3437-192">Las propiedades de clave externa también se pueden denominar `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f3437-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="f3437-193">Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="f3437-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="f3437-194">La entidad Course</span><span class="sxs-lookup"><span data-stu-id="f3437-194">The Course entity</span></span>

![Diagrama de la entidad Course](intro/_static/course-entity.png)

<span data-ttu-id="f3437-196">En la carpeta *Models*, cree *Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-196">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="f3437-197">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="f3437-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f3437-198">Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f3437-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="f3437-199">El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.</span><span class="sxs-lookup"><span data-stu-id="f3437-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="f3437-200">Aplicación de scaffolding al modelo de alumnos</span><span class="sxs-lookup"><span data-stu-id="f3437-200">Scaffold the student model</span></span>

<span data-ttu-id="f3437-201">En esta sección, se aplica scaffolding al modelo de alumnos.</span><span class="sxs-lookup"><span data-stu-id="f3437-201">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="f3437-202">Es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de alumnos.</span><span class="sxs-lookup"><span data-stu-id="f3437-202">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="f3437-203">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3437-203">Build the project.</span></span>
* <span data-ttu-id="f3437-204">Cree la carpeta *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="f3437-204">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3437-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3437-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f3437-206">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Students* > **Agregar** > **Nuevo elemento con scanffold**.</span><span class="sxs-lookup"><span data-stu-id="f3437-206">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f3437-207">En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.</span><span class="sxs-lookup"><span data-stu-id="f3437-207">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="f3437-208">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="f3437-208">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="f3437-209">En la lista desplegable **Clase de modelo**, seleccione **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="f3437-209">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="f3437-210">En la fila **Clase de contexto de datos**, haga clic en el signo **+** (más) y cambie el nombre generado por **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="f3437-210">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="f3437-211">En la lista desplegable **Clase de contexto de datos**, seleccione **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="f3437-211">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="f3437-212">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f3437-212">Select **Add**.</span></span>

![Cuadro de diálogo CRUD](intro/_static/s1.png)

<span data-ttu-id="f3437-214">Si tiene algún problema con el paso anterior, consulte [Aplicar scaffolding al modelo de película](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="f3437-214">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f3437-215">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3437-215">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f3437-216">Ejecute los comandos siguientes para aplicar scaffolding al modelo de alumnos.</span><span class="sxs-lookup"><span data-stu-id="f3437-216">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

------

<span data-ttu-id="f3437-217">El proceso de scaffolding ha creado y cambiado los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f3437-217">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="f3437-218">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="f3437-218">Files created</span></span>

* <span data-ttu-id="f3437-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="f3437-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="f3437-220">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="f3437-220">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="f3437-221">Actualizaciones de archivos</span><span class="sxs-lookup"><span data-stu-id="f3437-221">File updates</span></span>

* <span data-ttu-id="f3437-222">*Startup.cs*: en la sección siguiente se detallan los cambios realizados en este archivo.</span><span class="sxs-lookup"><span data-stu-id="f3437-222">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="f3437-223">*appsettings.json*: se agrega la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="f3437-223">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="f3437-224">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="f3437-224">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="f3437-225">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f3437-225">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f3437-226">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f3437-226">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f3437-227">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="f3437-227">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f3437-228">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="f3437-228">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="f3437-229">La herramienta de scaffolding creó de forma automática un contexto de base de datos y lo registró con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="f3437-229">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="f3437-230">Examine el método `ConfigureServices` de *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f3437-230">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="f3437-231">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="f3437-231">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="f3437-232">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="f3437-232">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="f3437-233">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f3437-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="f3437-234">Actualización de main</span><span class="sxs-lookup"><span data-stu-id="f3437-234">Update main</span></span>

<span data-ttu-id="f3437-235">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-235">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="f3437-236">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="f3437-236">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="f3437-237">Llame a [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="f3437-237">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="f3437-238">Elimine el contexto cuando finalice el método `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="f3437-238">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="f3437-239">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="f3437-239">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="f3437-240">`EnsureCreated` garantiza la existencia de la base de datos para el contexto.</span><span class="sxs-lookup"><span data-stu-id="f3437-240">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="f3437-241">Si existe, no se realiza ninguna acción.</span><span class="sxs-lookup"><span data-stu-id="f3437-241">If it exists, no action is taken.</span></span> <span data-ttu-id="f3437-242">Si no existe, se crean la base de datos y todo su esquema.</span><span class="sxs-lookup"><span data-stu-id="f3437-242">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="f3437-243">En `EnsureCreated` no se usan migraciones para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-243">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="f3437-244">Una base de datos que se cree con `EnsureCreated` no se podrá actualizar más adelante mediante las migraciones.</span><span class="sxs-lookup"><span data-stu-id="f3437-244">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="f3437-245">`EnsureCreated` se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-245">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="f3437-246">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-246">Delete the DB.</span></span>
* <span data-ttu-id="f3437-247">Se cambia el esquema de base de datos (por ejemplo, se agrega un campo `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="f3437-247">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="f3437-248">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f3437-248">Run the app.</span></span>
* <span data-ttu-id="f3437-249">`EnsureCreated` crea una base de datos con la columna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="f3437-249">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="f3437-250">`EnsureCreated` es útil al principio del desarrollo, cuando el esquema evoluciona rápidamente.</span><span class="sxs-lookup"><span data-stu-id="f3437-250">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="f3437-251">Más adelante, en el tutorial se elimina la base de datos y se usan las migraciones.</span><span class="sxs-lookup"><span data-stu-id="f3437-251">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="f3437-252">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f3437-252">Test the app</span></span>

<span data-ttu-id="f3437-253">Ejecute la aplicación y acepte la directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="f3437-253">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="f3437-254">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="f3437-254">This app doesn't keep personal information.</span></span> <span data-ttu-id="f3437-255">Puede obtener más información sobre la directiva de cookies en [Compatibilidad con el Reglamento general de protección de datos (RGPD) de la UE](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f3437-255">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="f3437-256">Haga clic en el vínculo **Students** y, después, en **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="f3437-256">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="f3437-257">Pruebe los vínculos Edit, Details y Delete.</span><span class="sxs-lookup"><span data-stu-id="f3437-257">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="f3437-258">Examinar el contexto de base de datos SchoolContext</span><span class="sxs-lookup"><span data-stu-id="f3437-258">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="f3437-259">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-259">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="f3437-260">El contexto de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="f3437-260">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="f3437-261">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-261">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="f3437-262">En este proyecto, la clase se denomina `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f3437-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="f3437-263">Actualice *SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="f3437-264">El código resaltado crea una propiedad [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="f3437-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="f3437-265">En la terminología de EF Core:</span><span class="sxs-lookup"><span data-stu-id="f3437-265">In EF Core terminology:</span></span>

* <span data-ttu-id="f3437-266">Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-266">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="f3437-267">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="f3437-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f3437-268">`DbSet<Enrollment>` y `DbSet<Course>` se pueden omitir.</span><span class="sxs-lookup"><span data-stu-id="f3437-268">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="f3437-269">EF Core las incluye implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="f3437-269">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="f3437-270">Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f3437-270">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="f3437-271">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f3437-271">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f3437-272">La cadena de conexión especifica [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="f3437-272">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="f3437-273">LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción.</span><span class="sxs-lookup"><span data-stu-id="f3437-273">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="f3437-274">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="f3437-274">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="f3437-275">De forma predeterminada, LocalDB crea archivos de base de datos *.mdf* en el directorio `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="f3437-275">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="f3437-276">Agregar código para inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="f3437-276">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="f3437-277">EF Core crea una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="f3437-277">EF Core creates an empty DB.</span></span> <span data-ttu-id="f3437-278">En esta sección, se escribe un método `Initialize` para rellenarlo con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="f3437-278">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="f3437-279">En la carpeta *Data*, cree un archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3437-279">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="f3437-280">Nota: El código anterior usa `Models` para el espacio de nombres (`namespace ContosoUniversity.Models`) en lugar de `Data`.</span><span class="sxs-lookup"><span data-stu-id="f3437-280">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="f3437-281">`Models` es coherente con el código generado por el proveedor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f3437-281">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="f3437-282">Para obtener más información, consulte [este problema de scaffolding de GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="f3437-282">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="f3437-283">El código comprueba si hay estudiantes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-283">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="f3437-284">Si no hay alumnos en la base de datos, se inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="f3437-284">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="f3437-285">Carga los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="f3437-285">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="f3437-286">El método `EnsureCreated` crea automáticamente la base de datos para el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-286">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="f3437-287">Si la base de datos existe, `EnsureCreated` vuelve sin modificarla.</span><span class="sxs-lookup"><span data-stu-id="f3437-287">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="f3437-288">En *Program.cs*, modifique el método `Main` para que llame a `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="f3437-288">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="f3437-289">Elimine los registros de los alumnos y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f3437-289">Delete any student records and restart the app.</span></span> <span data-ttu-id="f3437-290">Si la base de datos no se ha inicializado, establezca un punto de interrupción en `Initialize` para diagnosticar el problema.</span><span class="sxs-lookup"><span data-stu-id="f3437-290">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="f3437-291">Ver la base de datos</span><span class="sxs-lookup"><span data-stu-id="f3437-291">View the DB</span></span>

<span data-ttu-id="f3437-292">Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3437-292">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="f3437-293">En SSOX, haga clic en **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="f3437-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="f3437-294">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="f3437-294">Expand the **Tables** node.</span></span>

<span data-ttu-id="f3437-295">Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.</span><span class="sxs-lookup"><span data-stu-id="f3437-295">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="f3437-296">Código asincrónico</span><span class="sxs-lookup"><span data-stu-id="f3437-296">Asynchronous code</span></span>

<span data-ttu-id="f3437-297">La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.</span><span class="sxs-lookup"><span data-stu-id="f3437-297">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="f3437-298">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="f3437-298">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="f3437-299">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="f3437-299">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="f3437-300">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="f3437-300">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="f3437-301">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f3437-301">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="f3437-302">Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="f3437-302">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="f3437-303">El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f3437-303">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="f3437-304">En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.</span><span class="sxs-lookup"><span data-stu-id="f3437-304">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="f3437-305">En el código siguiente, la palabra clave [async](/dotnet/csharp/language-reference/keywords/async), el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="f3437-305">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="f3437-306">La palabra clave `async` indica al compilador que:</span><span class="sxs-lookup"><span data-stu-id="f3437-306">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="f3437-307">Genere devoluciones de llamada para partes del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="f3437-307">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="f3437-308">Cree automáticamente el objeto [Task](/dotnet/api/system.threading.tasks.task) que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="f3437-308">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="f3437-309">Para más información, vea [Tipo de valor devuelto Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="f3437-309">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="f3437-310">El tipo devuelto implícito `Task` representa el trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="f3437-310">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="f3437-311">La palabra clave `await` hace que el compilador divida el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="f3437-311">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="f3437-312">La primera parte termina con la operación que se inició de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="f3437-312">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="f3437-313">La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.</span><span class="sxs-lookup"><span data-stu-id="f3437-313">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="f3437-314">`ToListAsync` es la versión asincrónica del método de extensión `ToList`.</span><span class="sxs-lookup"><span data-stu-id="f3437-314">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="f3437-315">Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="f3437-315">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="f3437-316">Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-316">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="f3437-317">Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="f3437-317">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="f3437-318">No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="f3437-318">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="f3437-319">Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="f3437-319">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="f3437-320">Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3437-320">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="f3437-321">Para obtener más información sobre la programación asincrónica en .NET, vea [Programación asincrónica](/dotnet/standard/async) y [Programación asincrónica con async y await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="f3437-321">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="f3437-322">En el siguiente tutorial, se examinan las operaciones CRUD (crear, leer, actualizar y eliminar) básicas.</span><span class="sxs-lookup"><span data-stu-id="f3437-322">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f3437-323">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f3437-323">Additional resources</span></span>

* [<span data-ttu-id="f3437-324">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="f3437-324">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="f3437-325">Siguiente</span><span class="sxs-lookup"><span data-stu-id="f3437-325">Next</span></span>](xref:data/ef-rp/crud)
