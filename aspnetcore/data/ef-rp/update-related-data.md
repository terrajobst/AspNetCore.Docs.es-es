---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Actualización de datos relacionados (7 de 8)'
author: rick-anderson
description: En este tutorial, actualizará datos relacionados mediante la actualización de campos de clave externa y propiedades de navegación.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e987971f60e5c5a9fb79e30440c7c986df64447e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38189307"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="81ae7-103">Páginas de Razor con EF Core en ASP.NET Core: Actualización de datos relacionados (7 de 8)</span><span class="sxs-lookup"><span data-stu-id="81ae7-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="81ae7-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="81ae7-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="81ae7-105">En este tutorial se muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="81ae7-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="81ae7-106">Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="81ae7-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="81ae7-107">En las ilustraciones siguientes se muestran algunas de las páginas completadas.</span><span class="sxs-lookup"><span data-stu-id="81ae7-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="81ae7-108">![Página de edición de cursos](update-related-data/_static/course-edit.png)
![Página de edición de instructores](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="81ae7-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="81ae7-109">Examine y pruebe las páginas de cursos Create y Edit.</span><span class="sxs-lookup"><span data-stu-id="81ae7-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="81ae7-110">Cree un curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-110">Create a new course.</span></span> <span data-ttu-id="81ae7-111">El departamento se selecciona por su clave principal (un entero), no su nombre.</span><span class="sxs-lookup"><span data-stu-id="81ae7-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="81ae7-112">Modifique el curso nuevo.</span><span class="sxs-lookup"><span data-stu-id="81ae7-112">Edit the new course.</span></span> <span data-ttu-id="81ae7-113">Cuando haya terminado las pruebas, elimine el curso nuevo.</span><span class="sxs-lookup"><span data-stu-id="81ae7-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="81ae7-114">Crear una clase base para compartir código común</span><span class="sxs-lookup"><span data-stu-id="81ae7-114">Create a base class to share common code</span></span>

<span data-ttu-id="81ae7-115">En las páginas Courses/Create y Courses/Edit se necesita una lista de nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="81ae7-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="81ae7-116">Cree la clase base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* para las páginas Create y Edit:</span><span class="sxs-lookup"><span data-stu-id="81ae7-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="81ae7-117">En el código anterior se crea una clase [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para que contenga la lista de nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="81ae7-117">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="81ae7-118">Si se especifica `selectedDepartment`, se selecciona ese departamento en la `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="81ae7-119">Las clases de modelo de página de Create y Edit se derivan de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="81ae7-120">Personalizar las páginas de cursos</span><span class="sxs-lookup"><span data-stu-id="81ae7-120">Customize the Courses Pages</span></span>

<span data-ttu-id="81ae7-121">Cuando se crea una entidad de curso, debe tener una relación con un departamento existente.</span><span class="sxs-lookup"><span data-stu-id="81ae7-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="81ae7-122">Para agregar un departamento durante la creación de un curso, la clase base para Create y Edit contiene una lista desplegable para seleccionar el departamento.</span><span class="sxs-lookup"><span data-stu-id="81ae7-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="81ae7-123">La lista desplegable establece la propiedad de clave externa (FK) `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="81ae7-124">EF Core usa la FK `Course.DepartmentID` para cargar la propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Crear el curso](update-related-data/_static/ddl.png)

<span data-ttu-id="81ae7-126">Actualice el modelo de página de Create con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-126">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="81ae7-127">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="81ae7-127">The preceding code:</span></span>

* <span data-ttu-id="81ae7-128">Deriva de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="81ae7-129">Usa `TryUpdateModelAsync` para evitar la [publicación excesiva](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="81ae7-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="81ae7-130">Reemplaza `ViewData["DepartmentID"]` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="81ae7-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="81ae7-131">`ViewData["DepartmentID"]` se reemplaza con `DepartmentNameSL` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="81ae7-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="81ae7-132">Los modelos fuertemente tipados son preferibles a los de establecimiento flexible de tipos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="81ae7-133">Para obtener más información, vea [Establecimiento flexible de datos (ViewData y ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="81ae7-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="81ae7-134">Actualizar la página Courses Create</span><span class="sxs-lookup"><span data-stu-id="81ae7-134">Update the Courses Create page</span></span>

<span data-ttu-id="81ae7-135">Actualice *Pages/Courses/Create.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="81ae7-136">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="81ae7-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="81ae7-137">Se cambia el título de **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="81ae7-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="81ae7-138">Se reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="81ae7-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="81ae7-139">Se agrega la opción "Select Department" (Seleccionar departamento).</span><span class="sxs-lookup"><span data-stu-id="81ae7-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="81ae7-140">Este cambio representa "Select Department" en lugar del primer departamento.</span><span class="sxs-lookup"><span data-stu-id="81ae7-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="81ae7-141">Se agrega un mensaje de validación cuando el departamento no está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="81ae7-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="81ae7-142">La página de Razor usa la [Aplicación auxiliar de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="81ae7-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="81ae7-143">Pruebe la página Create.</span><span class="sxs-lookup"><span data-stu-id="81ae7-143">Test the Create page.</span></span> <span data-ttu-id="81ae7-144">En la página Create se muestra el nombre del departamento en lugar del identificador.</span><span class="sxs-lookup"><span data-stu-id="81ae7-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="81ae7-145">Actualice la página Courses Edit.</span><span class="sxs-lookup"><span data-stu-id="81ae7-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="81ae7-146">Actualice el modelo de página de Edit con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-146">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="81ae7-147">Los cambios son similares a los realizados en el modelo de página de Create.</span><span class="sxs-lookup"><span data-stu-id="81ae7-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="81ae7-148">En el código anterior, `PopulateDepartmentsDropDownList` pasa el identificador de departamento, que selecciona el departamento especificado en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="81ae7-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="81ae7-149">Actualice *Pages/Courses/Edit.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="81ae7-150">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="81ae7-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="81ae7-151">Se muestra el identificador del curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-151">Displays the course ID.</span></span> <span data-ttu-id="81ae7-152">Por lo general no se muestra la clave principal (PK) de una entidad.</span><span class="sxs-lookup"><span data-stu-id="81ae7-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="81ae7-153">Las PK normalmente no tienen sentido para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="81ae7-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="81ae7-154">En este caso, la clave principal es el número de curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="81ae7-155">Se cambia el título de **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="81ae7-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="81ae7-156">Se reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="81ae7-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="81ae7-157">La página contiene un campo oculto (`<input type="hidden">`) para el número de curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-157">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="81ae7-158">Agregar una aplicación auxiliar de etiquetas `<label>` con `asp-for="Course.CourseID"` no elimina la necesidad del campo oculto.</span><span class="sxs-lookup"><span data-stu-id="81ae7-158">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="81ae7-159">Se requiere `<input type="hidden">` para que el número de curso se incluya en los datos enviados cuando el usuario hace clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="81ae7-159">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="81ae7-160">Pruebe el código actualizado.</span><span class="sxs-lookup"><span data-stu-id="81ae7-160">Test the updated code.</span></span> <span data-ttu-id="81ae7-161">Cree, modifique y elimine un curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-161">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="81ae7-162">Agregar AsNoTracking a los modelos de página de Details y Delete</span><span class="sxs-lookup"><span data-stu-id="81ae7-162">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="81ae7-163">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) puede mejorar el rendimiento cuando el seguimiento no es necesario.</span><span class="sxs-lookup"><span data-stu-id="81ae7-163">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="81ae7-164">Agregue `AsNoTracking` al modelo de página de Delete y Details.</span><span class="sxs-lookup"><span data-stu-id="81ae7-164">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="81ae7-165">En el código siguiente se muestra el modelo de página de Delete actualizado:</span><span class="sxs-lookup"><span data-stu-id="81ae7-165">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="81ae7-166">Actualice el método `OnGetAsync` en el archivo *Pages/Courses/Details.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="81ae7-166">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="81ae7-167">Modificar las páginas Delete y Details</span><span class="sxs-lookup"><span data-stu-id="81ae7-167">Modify the Delete and Details pages</span></span>

<span data-ttu-id="81ae7-168">Actualice la página de Razor Delete con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-168">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="81ae7-169">Realice los mismos cambios en la página Details.</span><span class="sxs-lookup"><span data-stu-id="81ae7-169">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="81ae7-170">Probar las páginas Course</span><span class="sxs-lookup"><span data-stu-id="81ae7-170">Test the Course pages</span></span>

<span data-ttu-id="81ae7-171">Pruebe las páginas Create, Edit, Details y Delete.</span><span class="sxs-lookup"><span data-stu-id="81ae7-171">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="81ae7-172">Actualizar las páginas de instructor</span><span class="sxs-lookup"><span data-stu-id="81ae7-172">Update the instructor pages</span></span>

<span data-ttu-id="81ae7-173">En las siguientes secciones se actualizan las páginas de instructor.</span><span class="sxs-lookup"><span data-stu-id="81ae7-173">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="81ae7-174">Agregar la ubicación de la oficina</span><span class="sxs-lookup"><span data-stu-id="81ae7-174">Add office location</span></span>

<span data-ttu-id="81ae7-175">Al editar un registro de instructor, es posible que quiera actualizar la asignación de la oficina del instructor.</span><span class="sxs-lookup"><span data-stu-id="81ae7-175">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="81ae7-176">La entidad `Instructor` tiene una relación de uno a cero o uno con la entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-176">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="81ae7-177">El código de instructor debe controlar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-177">The instructor code must handle:</span></span>

* <span data-ttu-id="81ae7-178">Si el usuario desactiva la asignación de la oficina, elimine la entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-178">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="81ae7-179">Si el usuario especifica una asignación de oficina y estaba vacía, cree una entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-179">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="81ae7-180">Si el usuario cambia la asignación de oficina, actualice la entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-180">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="81ae7-181">Actualice el modelo de página de Edit de los instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-181">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="81ae7-182">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="81ae7-182">The preceding code:</span></span>

- <span data-ttu-id="81ae7-183">Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-183">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="81ae7-184">Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-184">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="81ae7-185">`TryUpdateModel` evita la [publicación excesiva](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="81ae7-185">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="81ae7-186">Si la ubicación de la oficina está en blanco, establece `Instructor.OfficeAssignment` en NULL.</span><span class="sxs-lookup"><span data-stu-id="81ae7-186">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="81ae7-187">Cuando `Instructor.OfficeAssignment` es NULL, se elimina la fila relacionada en la tabla `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-187">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="81ae7-188">Actualizar la página Edit del instructor</span><span class="sxs-lookup"><span data-stu-id="81ae7-188">Update the instructor Edit page</span></span>

<span data-ttu-id="81ae7-189">Actualice *Pages/Instructors/Edit.cshtml* con la ubicación de la oficina:</span><span class="sxs-lookup"><span data-stu-id="81ae7-189">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="81ae7-190">Compruebe que puede cambiar la ubicación de la oficina de un instructor.</span><span class="sxs-lookup"><span data-stu-id="81ae7-190">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="81ae7-191">Agregar asignaciones de cursos a la página Edit de los instructores</span><span class="sxs-lookup"><span data-stu-id="81ae7-191">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="81ae7-192">Los instructores pueden impartir cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-192">Instructors may teach any number of courses.</span></span> <span data-ttu-id="81ae7-193">En esta sección, agregará la capacidad de cambiar las asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-193">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="81ae7-194">En la imagen siguiente se muestra la página Edit actualizada de los instructores:</span><span class="sxs-lookup"><span data-stu-id="81ae7-194">The following image shows the updated instructor Edit page:</span></span>

![Página de edición de instructores con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="81ae7-196">`Course` e `Instructor` tienen una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="81ae7-196">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="81ae7-197">Para agregar y eliminar relaciones, agregue y quite entidades del conjunto de entidades combinadas `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-197">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="81ae7-198">Las casillas permiten cambios en los cursos a los que está asignado un instructor.</span><span class="sxs-lookup"><span data-stu-id="81ae7-198">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="81ae7-199">Se muestra una casilla para cada curso en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-199">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="81ae7-200">Los cursos a los que el instructor está asignado se activan.</span><span class="sxs-lookup"><span data-stu-id="81ae7-200">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="81ae7-201">El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-201">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="81ae7-202">Si el número de cursos fuera mucho mayor:</span><span class="sxs-lookup"><span data-stu-id="81ae7-202">If the number of courses were much greater:</span></span>

* <span data-ttu-id="81ae7-203">Probablemente usaría una interfaz de usuario diferente para mostrar los cursos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-203">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="81ae7-204">El método de manipulación de una entidad de combinación para crear o eliminar relaciones no cambiaría.</span><span class="sxs-lookup"><span data-stu-id="81ae7-204">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="81ae7-205">Agregar clases para admitir las páginas de instructor Create y Edit</span><span class="sxs-lookup"><span data-stu-id="81ae7-205">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="81ae7-206">Cree *SchoolViewModels/AssignedCourseData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-206">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="81ae7-207">La clase `AssignedCourseData` contiene datos para crear las casillas para los cursos asignados por un instructor.</span><span class="sxs-lookup"><span data-stu-id="81ae7-207">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="81ae7-208">Cree la clase base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="81ae7-208">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="81ae7-209">`InstructorCoursesPageModel` es la clase base que se usará para los modelos de página de Edit y Create.</span><span class="sxs-lookup"><span data-stu-id="81ae7-209">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="81ae7-210">`PopulateAssignedCourseData` lee todas las entidades `Course` para rellenar `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-210">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="81ae7-211">Para cada curso, el código establece el `CourseID`, el título y si el instructor está asignado o no al curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-211">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="81ae7-212">Se usa un [HashSet](/dotnet/api/system.collections.generic.hashset-1) para crear búsquedas eficaces.</span><span class="sxs-lookup"><span data-stu-id="81ae7-212">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="81ae7-213">Modelo de página de edición de instructores</span><span class="sxs-lookup"><span data-stu-id="81ae7-213">Instructors Edit page model</span></span>

<span data-ttu-id="81ae7-214">Actualice el modelo de página de edición de instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-214">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="81ae7-215">El código anterior controla los cambios de asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="81ae7-215">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="81ae7-216">Actualice la vista de Razor del instructor:</span><span class="sxs-lookup"><span data-stu-id="81ae7-216">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="81ae7-217">Al pegar el código en Visual Studio, se cambian los saltos de línea de tal forma que el código se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="81ae7-217">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="81ae7-218">Presione Ctrl+Z una vez para deshacer el formato automático.</span><span class="sxs-lookup"><span data-stu-id="81ae7-218">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="81ae7-219">Ctrl+Z corrige los saltos de línea para que se muestren como se ven aquí.</span><span class="sxs-lookup"><span data-stu-id="81ae7-219">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="81ae7-220">No es necesario que la sangría sea perfecta, pero las líneas `@</tr><tr>`, `@:<td>`, `@:</td>` y `@:</tr>` deben estar en una única línea tal y como se muestra.</span><span class="sxs-lookup"><span data-stu-id="81ae7-220">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="81ae7-221">Con el bloque de código nuevo seleccionado, presione tres veces la tecla Tab para alinearlo con el código existente.</span><span class="sxs-lookup"><span data-stu-id="81ae7-221">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="81ae7-222">Puede votar o revisar el estado de este error [con este vínculo](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="81ae7-222">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="81ae7-223">En el código anterior se crea una tabla HTML que tiene tres columnas.</span><span class="sxs-lookup"><span data-stu-id="81ae7-223">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="81ae7-224">Cada columna tiene una casilla y una leyenda que contiene el número y el título del curso.</span><span class="sxs-lookup"><span data-stu-id="81ae7-224">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="81ae7-225">Todas las casillas tienen el mismo nombre ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="81ae7-225">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="81ae7-226">Al usar el mismo nombre se informa al enlazador de modelos que las trate como un grupo.</span><span class="sxs-lookup"><span data-stu-id="81ae7-226">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="81ae7-227">El atributo de valor de cada casilla se establece en `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-227">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="81ae7-228">Cuando se envía la página, el enlazador de modelos pasa una matriz formada solo por los valores `CourseID` de las casillas activadas.</span><span class="sxs-lookup"><span data-stu-id="81ae7-228">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="81ae7-229">Cuando se representan las casillas por primera vez, los cursos asignados al instructor tienen atributos checked.</span><span class="sxs-lookup"><span data-stu-id="81ae7-229">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="81ae7-230">Ejecute la aplicación y pruebe la página Edit de los instructores actualizada.</span><span class="sxs-lookup"><span data-stu-id="81ae7-230">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="81ae7-231">Cambie algunas asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-231">Change some course assignments.</span></span> <span data-ttu-id="81ae7-232">Los cambios se reflejan en la página Index.</span><span class="sxs-lookup"><span data-stu-id="81ae7-232">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="81ae7-233">Nota: El enfoque que se aplica aquí para modificar datos de los cursos del instructor funciona bien cuando hay un número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-233">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="81ae7-234">Para las colecciones que son mucho más grandes, una interfaz de usuario y un método de actualización diferentes serían más eficaces y útiles.</span><span class="sxs-lookup"><span data-stu-id="81ae7-234">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="81ae7-235">Actualizar la página de creación de instructores</span><span class="sxs-lookup"><span data-stu-id="81ae7-235">Update the instructors Create page</span></span>

<span data-ttu-id="81ae7-236">Actualice el modelo de página de creación de instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-236">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="81ae7-237">El código anterior es similar al de *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="81ae7-237">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="81ae7-238">Actualice la página de Razor de creación de instructores con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-238">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="81ae7-239">Pruebe la página de creación de instructores.</span><span class="sxs-lookup"><span data-stu-id="81ae7-239">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="81ae7-240">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="81ae7-240">Update the Delete page</span></span>

<span data-ttu-id="81ae7-241">Actualice el modelo de la página Delete con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="81ae7-241">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="81ae7-242">En el código anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="81ae7-242">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="81ae7-243">Se usa la carga diligente para la propiedad de navegación `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="81ae7-243">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="81ae7-244">Es necesario incluir `CourseAssignments` o no se eliminarán cuando se elimine el instructor.</span><span class="sxs-lookup"><span data-stu-id="81ae7-244">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="81ae7-245">Para evitar la necesidad de leerlos, configure la eliminación en cascada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-245">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="81ae7-246">Si el instructor que se va a eliminar está asignado como administrador de cualquiera de los departamentos, quita la asignación de instructor de esos departamentos.</span><span class="sxs-lookup"><span data-stu-id="81ae7-246">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="81ae7-247">[Anterior](xref:data/ef-rp/read-related-data)
> [Siguiente](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="81ae7-247">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
