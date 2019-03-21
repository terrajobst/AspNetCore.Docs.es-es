---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Actualización de datos relacionados (7 de 8)'
author: rick-anderson
description: En este tutorial, actualizará datos relacionados mediante la actualización de campos de clave externa y propiedades de navegación.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e1407048157fccdbfdd854071a6fbf9785356026
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208614"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="4161e-103">Páginas de Razor con EF Core en ASP.NET Core: Actualización de datos relacionados (7 de 8)</span><span class="sxs-lookup"><span data-stu-id="4161e-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="4161e-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4161e-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="4161e-105">En este tutorial se muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="4161e-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="4161e-106">Si experimenta problemas que no puede resolver, [descargue o vea la aplicación completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="4161e-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="4161e-107">[Instrucciones de descarga](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="4161e-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="4161e-108">En las ilustraciones siguientes se muestran algunas de las páginas completadas.</span><span class="sxs-lookup"><span data-stu-id="4161e-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="4161e-109">![Página de edición de cursos](update-related-data/_static/course-edit.png)
![Página de edición de instructores](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="4161e-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="4161e-110">Examine y pruebe las páginas de cursos Create y Edit.</span><span class="sxs-lookup"><span data-stu-id="4161e-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="4161e-111">Cree un curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-111">Create a new course.</span></span> <span data-ttu-id="4161e-112">El departamento se selecciona por su clave principal (un entero), no su nombre.</span><span class="sxs-lookup"><span data-stu-id="4161e-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="4161e-113">Modifique el curso nuevo.</span><span class="sxs-lookup"><span data-stu-id="4161e-113">Edit the new course.</span></span> <span data-ttu-id="4161e-114">Cuando haya terminado las pruebas, elimine el curso nuevo.</span><span class="sxs-lookup"><span data-stu-id="4161e-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="4161e-115">Crear una clase base para compartir código común</span><span class="sxs-lookup"><span data-stu-id="4161e-115">Create a base class to share common code</span></span>

<span data-ttu-id="4161e-116">En las páginas Courses/Create y Courses/Edit se necesita una lista de nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="4161e-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="4161e-117">Cree la clase base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* para las páginas Create y Edit:</span><span class="sxs-lookup"><span data-stu-id="4161e-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="4161e-118">En el código anterior se crea una clase [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para que contenga la lista de nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="4161e-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="4161e-119">Si se especifica `selectedDepartment`, se selecciona ese departamento en la `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="4161e-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="4161e-120">Las clases de modelo de página de Create y Edit se derivan de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="4161e-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="4161e-121">Personalizar las páginas de cursos</span><span class="sxs-lookup"><span data-stu-id="4161e-121">Customize the Courses Pages</span></span>

<span data-ttu-id="4161e-122">Cuando se crea una entidad de curso, debe tener una relación con un departamento existente.</span><span class="sxs-lookup"><span data-stu-id="4161e-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="4161e-123">Para agregar un departamento durante la creación de un curso, la clase base para Create y Edit contiene una lista desplegable para seleccionar el departamento.</span><span class="sxs-lookup"><span data-stu-id="4161e-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="4161e-124">La lista desplegable establece la propiedad de clave externa (FK) `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="4161e-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="4161e-125">EF Core usa la FK `Course.DepartmentID` para cargar la propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="4161e-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Crear el curso](update-related-data/_static/ddl.png)

<span data-ttu-id="4161e-127">Actualice el modelo de página de Create con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="4161e-128">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="4161e-128">The preceding code:</span></span>

* <span data-ttu-id="4161e-129">Deriva de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="4161e-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="4161e-130">Usa `TryUpdateModelAsync` para evitar la [publicación excesiva](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="4161e-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="4161e-131">Reemplaza `ViewData["DepartmentID"]` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="4161e-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="4161e-132">`ViewData["DepartmentID"]` se reemplaza con `DepartmentNameSL` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="4161e-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="4161e-133">Los modelos fuertemente tipados son preferibles a los de establecimiento flexible de tipos.</span><span class="sxs-lookup"><span data-stu-id="4161e-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="4161e-134">Para obtener más información, vea [Establecimiento flexible de datos (ViewData y ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="4161e-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="4161e-135">Actualizar la página Courses Create</span><span class="sxs-lookup"><span data-stu-id="4161e-135">Update the Courses Create page</span></span>

<span data-ttu-id="4161e-136">Actualice *Pages/Courses/Create.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="4161e-137">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="4161e-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="4161e-138">Se cambia el título de **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="4161e-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="4161e-139">Se reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="4161e-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="4161e-140">Se agrega la opción "Select Department" (Seleccionar departamento).</span><span class="sxs-lookup"><span data-stu-id="4161e-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="4161e-141">Este cambio representa "Select Department" en lugar del primer departamento.</span><span class="sxs-lookup"><span data-stu-id="4161e-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="4161e-142">Se agrega un mensaje de validación cuando el departamento no está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="4161e-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="4161e-143">La página de Razor usa la [Asistente de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="4161e-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="4161e-144">Pruebe la página Create.</span><span class="sxs-lookup"><span data-stu-id="4161e-144">Test the Create page.</span></span> <span data-ttu-id="4161e-145">En la página Create se muestra el nombre del departamento en lugar del identificador.</span><span class="sxs-lookup"><span data-stu-id="4161e-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="4161e-146">Actualice la página Courses Edit.</span><span class="sxs-lookup"><span data-stu-id="4161e-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="4161e-147">Actualice el modelo de página de Edit con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="4161e-148">Los cambios son similares a los realizados en el modelo de página de Create.</span><span class="sxs-lookup"><span data-stu-id="4161e-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="4161e-149">En el código anterior, `PopulateDepartmentsDropDownList` pasa el identificador de departamento, que selecciona el departamento especificado en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="4161e-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="4161e-150">Actualice *Pages/Courses/Edit.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="4161e-151">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="4161e-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="4161e-152">Se muestra el identificador del curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-152">Displays the course ID.</span></span> <span data-ttu-id="4161e-153">Por lo general no se muestra la clave principal (PK) de una entidad.</span><span class="sxs-lookup"><span data-stu-id="4161e-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="4161e-154">Las PK normalmente no tienen sentido para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="4161e-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="4161e-155">En este caso, la clave principal es el número de curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="4161e-156">Se cambia el título de **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="4161e-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="4161e-157">Se reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="4161e-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="4161e-158">La página contiene un campo oculto (`<input type="hidden">`) para el número de curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="4161e-159">Agregar un asistente de etiquetas `<label>` con `asp-for="Course.CourseID"` no elimina la necesidad del campo oculto.</span><span class="sxs-lookup"><span data-stu-id="4161e-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="4161e-160">Se requiere `<input type="hidden">` para que el número de curso se incluya en los datos enviados cuando el usuario hace clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="4161e-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="4161e-161">Pruebe el código actualizado.</span><span class="sxs-lookup"><span data-stu-id="4161e-161">Test the updated code.</span></span> <span data-ttu-id="4161e-162">Cree, modifique y elimine un curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="4161e-163">Agregar AsNoTracking a los modelos de página de Details y Delete</span><span class="sxs-lookup"><span data-stu-id="4161e-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="4161e-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) puede mejorar el rendimiento cuando el seguimiento no es necesario.</span><span class="sxs-lookup"><span data-stu-id="4161e-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="4161e-165">Agregue `AsNoTracking` al modelo de página de Delete y Details.</span><span class="sxs-lookup"><span data-stu-id="4161e-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="4161e-166">En el código siguiente se muestra el modelo de página de Delete actualizado:</span><span class="sxs-lookup"><span data-stu-id="4161e-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="4161e-167">Actualice el método `OnGetAsync` en el archivo *Pages/Courses/Details.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4161e-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="4161e-168">Modificar las páginas Delete y Details</span><span class="sxs-lookup"><span data-stu-id="4161e-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="4161e-169">Actualice la página de Razor Delete con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="4161e-170">Realice los mismos cambios en la página Details.</span><span class="sxs-lookup"><span data-stu-id="4161e-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="4161e-171">Probar las páginas Course</span><span class="sxs-lookup"><span data-stu-id="4161e-171">Test the Course pages</span></span>

<span data-ttu-id="4161e-172">Pruebe las páginas Create, Edit, Details y Delete.</span><span class="sxs-lookup"><span data-stu-id="4161e-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="4161e-173">Actualizar las páginas de instructor</span><span class="sxs-lookup"><span data-stu-id="4161e-173">Update the instructor pages</span></span>

<span data-ttu-id="4161e-174">En las siguientes secciones se actualizan las páginas de instructor.</span><span class="sxs-lookup"><span data-stu-id="4161e-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="4161e-175">Agregar la ubicación de la oficina</span><span class="sxs-lookup"><span data-stu-id="4161e-175">Add office location</span></span>

<span data-ttu-id="4161e-176">Al editar un registro de instructor, es posible que quiera actualizar la asignación de la oficina del instructor.</span><span class="sxs-lookup"><span data-stu-id="4161e-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="4161e-177">La entidad `Instructor` tiene una relación de uno a cero o uno con la entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4161e-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="4161e-178">El código de instructor debe controlar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-178">The instructor code must handle:</span></span>

* <span data-ttu-id="4161e-179">Si el usuario desactiva la asignación de la oficina, elimine la entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4161e-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="4161e-180">Si el usuario especifica una asignación de oficina y estaba vacía, cree una entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4161e-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="4161e-181">Si el usuario cambia la asignación de oficina, actualice la entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4161e-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="4161e-182">Actualice el modelo de página de Edit de los instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="4161e-183">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="4161e-183">The preceding code:</span></span>

* <span data-ttu-id="4161e-184">Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4161e-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
* <span data-ttu-id="4161e-185">Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="4161e-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="4161e-186">`TryUpdateModel` evita la [publicación excesiva](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="4161e-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="4161e-187">Si la ubicación de la oficina está en blanco, establece `Instructor.OfficeAssignment` en NULL.</span><span class="sxs-lookup"><span data-stu-id="4161e-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="4161e-188">Cuando `Instructor.OfficeAssignment` es NULL, se elimina la fila relacionada en la tabla `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4161e-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="4161e-189">Actualizar la página Edit del instructor</span><span class="sxs-lookup"><span data-stu-id="4161e-189">Update the instructor Edit page</span></span>

<span data-ttu-id="4161e-190">Actualice *Pages/Instructors/Edit.cshtml* con la ubicación de la oficina:</span><span class="sxs-lookup"><span data-stu-id="4161e-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="4161e-191">Compruebe que puede cambiar la ubicación de la oficina de un instructor.</span><span class="sxs-lookup"><span data-stu-id="4161e-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="4161e-192">Agregar asignaciones de cursos a la página Edit de los instructores</span><span class="sxs-lookup"><span data-stu-id="4161e-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="4161e-193">Los instructores pueden impartir cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="4161e-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="4161e-194">En esta sección, agregará la capacidad de cambiar las asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="4161e-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="4161e-195">En la imagen siguiente se muestra la página Edit actualizada de los instructores:</span><span class="sxs-lookup"><span data-stu-id="4161e-195">The following image shows the updated instructor Edit page:</span></span>

![Página de edición de instructores con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="4161e-197">`Course` e `Instructor` tienen una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="4161e-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="4161e-198">Para agregar y eliminar relaciones, agregue y quite entidades del conjunto de entidades combinadas `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="4161e-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="4161e-199">Las casillas permiten cambios en los cursos a los que está asignado un instructor.</span><span class="sxs-lookup"><span data-stu-id="4161e-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="4161e-200">Se muestra una casilla para cada curso en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4161e-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="4161e-201">Los cursos a los que el instructor está asignado se activan.</span><span class="sxs-lookup"><span data-stu-id="4161e-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="4161e-202">El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="4161e-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="4161e-203">Si el número de cursos fuera mucho mayor:</span><span class="sxs-lookup"><span data-stu-id="4161e-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="4161e-204">Probablemente usaría una interfaz de usuario diferente para mostrar los cursos.</span><span class="sxs-lookup"><span data-stu-id="4161e-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="4161e-205">El método de manipulación de una entidad de combinación para crear o eliminar relaciones no cambiaría.</span><span class="sxs-lookup"><span data-stu-id="4161e-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="4161e-206">Agregar clases para admitir las páginas de instructor Create y Edit</span><span class="sxs-lookup"><span data-stu-id="4161e-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="4161e-207">Cree *SchoolViewModels/AssignedCourseData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="4161e-208">La clase `AssignedCourseData` contiene datos para crear las casillas para los cursos asignados por un instructor.</span><span class="sxs-lookup"><span data-stu-id="4161e-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="4161e-209">Cree la clase base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4161e-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="4161e-210">`InstructorCoursesPageModel` es la clase base que se usará para los modelos de página de Edit y Create.</span><span class="sxs-lookup"><span data-stu-id="4161e-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="4161e-211">`PopulateAssignedCourseData` lee todas las entidades `Course` para rellenar `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="4161e-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="4161e-212">Para cada curso, el código establece el `CourseID`, el título y si el instructor está asignado o no al curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="4161e-213">Se usa un [HashSet](/dotnet/api/system.collections.generic.hashset-1) para crear búsquedas eficaces.</span><span class="sxs-lookup"><span data-stu-id="4161e-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="4161e-214">Modelo de página de edición de instructores</span><span class="sxs-lookup"><span data-stu-id="4161e-214">Instructors Edit page model</span></span>

<span data-ttu-id="4161e-215">Actualice el modelo de página de edición de instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="4161e-216">El código anterior controla los cambios de asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="4161e-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="4161e-217">Actualice la vista de Razor del instructor:</span><span class="sxs-lookup"><span data-stu-id="4161e-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="4161e-218">Al pegar el código en Visual Studio, se cambian los saltos de línea de tal forma que el código se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="4161e-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="4161e-219">Presione Ctrl+Z una vez para deshacer el formato automático.</span><span class="sxs-lookup"><span data-stu-id="4161e-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="4161e-220">Ctrl+Z corrige los saltos de línea para que se muestren como se ven aquí.</span><span class="sxs-lookup"><span data-stu-id="4161e-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="4161e-221">No es necesario que la sangría sea perfecta, pero las líneas `@</tr><tr>`, `@:<td>`, `@:</td>` y `@:</tr>` deben estar en una única línea tal y como se muestra.</span><span class="sxs-lookup"><span data-stu-id="4161e-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="4161e-222">Con el bloque de código nuevo seleccionado, presione tres veces la tecla Tab para alinearlo con el código existente.</span><span class="sxs-lookup"><span data-stu-id="4161e-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="4161e-223">Puede votar o revisar el estado de este error [con este vínculo](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="4161e-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="4161e-224">En el código anterior se crea una tabla HTML que tiene tres columnas.</span><span class="sxs-lookup"><span data-stu-id="4161e-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="4161e-225">Cada columna tiene una casilla y una leyenda que contiene el número y el título del curso.</span><span class="sxs-lookup"><span data-stu-id="4161e-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="4161e-226">Todas las casillas tienen el mismo nombre ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="4161e-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="4161e-227">Al usar el mismo nombre se informa al enlazador de modelos que las trate como un grupo.</span><span class="sxs-lookup"><span data-stu-id="4161e-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="4161e-228">El atributo de valor de cada casilla se establece en `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="4161e-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="4161e-229">Cuando se envía la página, el enlazador de modelos pasa una matriz formada solo por los valores `CourseID` de las casillas activadas.</span><span class="sxs-lookup"><span data-stu-id="4161e-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="4161e-230">Cuando se representan las casillas por primera vez, los cursos asignados al instructor tienen atributos checked.</span><span class="sxs-lookup"><span data-stu-id="4161e-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="4161e-231">Ejecute la aplicación y pruebe la página Edit de los instructores actualizada.</span><span class="sxs-lookup"><span data-stu-id="4161e-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="4161e-232">Cambie algunas asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="4161e-232">Change some course assignments.</span></span> <span data-ttu-id="4161e-233">Los cambios se reflejan en la página Index.</span><span class="sxs-lookup"><span data-stu-id="4161e-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="4161e-234">Nota: El enfoque que se aplica aquí para modificar datos de los cursos del instructor funciona bien cuando hay un número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="4161e-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="4161e-235">Para las colecciones que son mucho más grandes, una interfaz de usuario y un método de actualización diferentes serían más eficaces y útiles.</span><span class="sxs-lookup"><span data-stu-id="4161e-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="4161e-236">Actualizar la página de creación de instructores</span><span class="sxs-lookup"><span data-stu-id="4161e-236">Update the instructors Create page</span></span>

<span data-ttu-id="4161e-237">Actualice el modelo de página de creación de instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="4161e-238">El código anterior es similar al de *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="4161e-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="4161e-239">Actualice la página de Razor de creación de instructores con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="4161e-240">Pruebe la página de creación de instructores.</span><span class="sxs-lookup"><span data-stu-id="4161e-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="4161e-241">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="4161e-241">Update the Delete page</span></span>

<span data-ttu-id="4161e-242">Actualice el modelo de la página Delete con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4161e-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="4161e-243">En el código anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="4161e-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="4161e-244">Se usa la carga diligente para la propiedad de navegación `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="4161e-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="4161e-245">Es necesario incluir `CourseAssignments` o no se eliminarán cuando se elimine el instructor.</span><span class="sxs-lookup"><span data-stu-id="4161e-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="4161e-246">Para evitar la necesidad de leerlos, configure la eliminación en cascada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4161e-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="4161e-247">Si el instructor que se va a eliminar está asignado como administrador de cualquiera de los departamentos, quita la asignación de instructor de esos departamentos.</span><span class="sxs-lookup"><span data-stu-id="4161e-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4161e-248">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4161e-248">Additional resources</span></span>

* [<span data-ttu-id="4161e-249">Versión en YouTube de este tutorial (parte 1)</span><span class="sxs-lookup"><span data-stu-id="4161e-249">YouTube version of this tutorial (Part 1)</span></span>](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [<span data-ttu-id="4161e-250">Versión en YouTube de este tutorial (parte 2)</span><span class="sxs-lookup"><span data-stu-id="4161e-250">YouTube version of this tutorial (Part 2)</span></span>](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> <span data-ttu-id="4161e-251">[Anterior](xref:data/ef-rp/read-related-data)
> [Siguiente](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="4161e-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
