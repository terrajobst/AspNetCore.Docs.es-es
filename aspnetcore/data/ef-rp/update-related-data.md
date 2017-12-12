---
title: "Páginas de Razor con EF básica: actualizar datos relacionados - 7 de 8"
author: rick-anderson
description: "En este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación."
keywords: "Combinaciones de núcleo de ASP.NET, Entity Framework Core, datos relacionados,"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: f07a33c19ba1be623fae14228f8fbc909d766816
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2017
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="78338-104">Actualizar datos relacionados - páginas de Razor EF Core (7 de 8)</span><span class="sxs-lookup"><span data-stu-id="78338-104">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="78338-105">Por [Tom Dykstra](https://github.com/tdykstra), y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="78338-105">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="78338-106">Este tutorial muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="78338-106">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="78338-107">Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="78338-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="78338-108">Las siguientes ilustraciones se muestran algunas de las páginas completadas.</span><span class="sxs-lookup"><span data-stu-id="78338-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="78338-109">![Página de edición de curso](update-related-data/_static/course-edit.png)
![Instructor Editar página](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="78338-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="78338-110">Examine y probar las páginas de curso crear y editar.</span><span class="sxs-lookup"><span data-stu-id="78338-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="78338-111">Crear un nuevo curso.</span><span class="sxs-lookup"><span data-stu-id="78338-111">Create a new course.</span></span> <span data-ttu-id="78338-112">El departamento se selecciona por su clave principal (un entero), no su nombre.</span><span class="sxs-lookup"><span data-stu-id="78338-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="78338-113">Modifique el curso nuevo.</span><span class="sxs-lookup"><span data-stu-id="78338-113">Edit the new course.</span></span> <span data-ttu-id="78338-114">Cuando haya terminado las pruebas, elimine el curso nuevo.</span><span class="sxs-lookup"><span data-stu-id="78338-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="78338-115">Crear una clase base para compartir código común</span><span class="sxs-lookup"><span data-stu-id="78338-115">Create a base class to share common code</span></span>

<span data-ttu-id="78338-116">Las páginas cursos/creación y edición de cursos/necesitan una lista de nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="78338-117">Crear el *Pages/Courses/DepartmentNamePageModel.cshtml.cs* la clase base para la creación y edición de páginas:</span><span class="sxs-lookup"><span data-stu-id="78338-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="78338-118">El código anterior crea un [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para que contenga la lista de nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-118">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="78338-119">Si `selectedDepartment` se especifica, se selecciona ese departamento en el `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="78338-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="78338-120">Se derivan las clases de modelo de página de creación y edición de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="78338-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="78338-121">Personalizar las páginas de cursos</span><span class="sxs-lookup"><span data-stu-id="78338-121">Customize the Courses Pages</span></span>

<span data-ttu-id="78338-122">Cuando se crea una nueva entidad de curso, debe tener una relación con un departamento existente.</span><span class="sxs-lookup"><span data-stu-id="78338-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="78338-123">Para agregar un departamento durante la creación de un curso, la clase base para crear y editar contiene una lista desplegable para seleccionar el departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="78338-124">Los conjuntos de la lista desplegable el `Course.DepartmentID` propiedad de clave externa (FK).</span><span class="sxs-lookup"><span data-stu-id="78338-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="78338-125">EF Core utiliza el `Course.DepartmentID` FK para cargar la `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="78338-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Crear curso](update-related-data/_static/ddl.png)

<span data-ttu-id="78338-127">Actualice el modelo de páginas de crear con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-127">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="78338-128">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="78338-128">The preceding code:</span></span>

* <span data-ttu-id="78338-129">Deriva de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="78338-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="78338-130">Usa `TryUpdateModelAsync` para evitar [de publicación excesiva](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="78338-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="78338-131">Reemplaza `ViewData["DepartmentID"]` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="78338-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="78338-132">`ViewData["DepartmentID"]`se reemplaza con fuertemente tipado `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="78338-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="78338-133">Modelos fuertemente tipados están la preferida en establecimiento flexible.</span><span class="sxs-lookup"><span data-stu-id="78338-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="78338-134">Para obtener más información, consulte [establecimiento flexible de datos (ViewData y ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="78338-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="78338-135">Actualizar la página Crear cursos</span><span class="sxs-lookup"><span data-stu-id="78338-135">Update the Courses Create page</span></span>

<span data-ttu-id="78338-136">Actualización *Pages/Courses/Create.cshtml* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="78338-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="78338-137">El marcado anterior realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="78338-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="78338-138">Cambia el título de **DepartmentID** a **departamento**.</span><span class="sxs-lookup"><span data-stu-id="78338-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="78338-139">Reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="78338-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="78338-140">Agrega la opción "Seleccionar departamento".</span><span class="sxs-lookup"><span data-stu-id="78338-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="78338-141">Este cambio representa "Seleccione Departamento" en lugar de la primera departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="78338-142">Agrega un mensaje de validación si no se selecciona el departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-142">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="78338-143">La página de Razor utiliza el [seleccione etiqueta auxiliar](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="78338-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="78338-144">Probar la página Crear.</span><span class="sxs-lookup"><span data-stu-id="78338-144">Test the Create page.</span></span> <span data-ttu-id="78338-145">La página Crear muestra el nombre del departamento en lugar de lo Id. de departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="78338-146">Actualice la página Editar cursos.</span><span class="sxs-lookup"><span data-stu-id="78338-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="78338-147">Actualizar el modelo de página de edición con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-147">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="78338-148">Los cambios son similares a las realizadas en el modelo de páginas de crear.</span><span class="sxs-lookup"><span data-stu-id="78338-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="78338-149">En el código anterior, `PopulateDepartmentsDropDownList` pasa el identificador de departamento, que selecciona el departamento especificado en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="78338-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="78338-150">Actualización *Pages/Courses/Edit.cshtml* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="78338-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="78338-151">El marcado anterior realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="78338-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="78338-152">Muestra el identificador de curso.</span><span class="sxs-lookup"><span data-stu-id="78338-152">Displays the course ID.</span></span> <span data-ttu-id="78338-153">Por lo general no se muestra la clave principal (PK) de una entidad.</span><span class="sxs-lookup"><span data-stu-id="78338-153">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="78338-154">PK es normalmente tiene sentido para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="78338-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="78338-155">En este caso, la clave principal es el número de curso.</span><span class="sxs-lookup"><span data-stu-id="78338-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="78338-156">Cambia el título de **DepartmentID** a **departamento**.</span><span class="sxs-lookup"><span data-stu-id="78338-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="78338-157">Reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).</span><span class="sxs-lookup"><span data-stu-id="78338-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="78338-158">Agrega la opción "Seleccionar departamento".</span><span class="sxs-lookup"><span data-stu-id="78338-158">Adds the "Select Department" option.</span></span> <span data-ttu-id="78338-159">Este cambio representa "Seleccione Departamento" en lugar de la primera departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-159">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="78338-160">Agrega un mensaje de validación si no se selecciona el departamento.</span><span class="sxs-lookup"><span data-stu-id="78338-160">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="78338-161">La página contiene un campo oculto (`<input type="hidden">`) para el número de curso.</span><span class="sxs-lookup"><span data-stu-id="78338-161">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="78338-162">Agregar un `<label>` etiqueta auxiliar con `asp-for="Course.CourseID"` no elimina la necesidad de que el campo oculto.</span><span class="sxs-lookup"><span data-stu-id="78338-162">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="78338-163">`<input type="hidden">`se requiere para el número de curso que se incluirá en los datos enviados cuando el usuario hace clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="78338-163">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="78338-164">Probar el código actualizado.</span><span class="sxs-lookup"><span data-stu-id="78338-164">Test the updated code.</span></span> <span data-ttu-id="78338-165">Crear, editar y eliminar un curso.</span><span class="sxs-lookup"><span data-stu-id="78338-165">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="78338-166">Agregar AsNoTracking a los detalles y eliminar modelos de página</span><span class="sxs-lookup"><span data-stu-id="78338-166">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="78338-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) puede mejorar el rendimiento cuando el seguimiento no es necesario.</span><span class="sxs-lookup"><span data-stu-id="78338-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="78338-168">Agregar `AsNoTracking` para el modelo de páginas de eliminación y los detalles.</span><span class="sxs-lookup"><span data-stu-id="78338-168">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="78338-169">El código siguiente muestra el modelo de página de eliminación actualizado:</span><span class="sxs-lookup"><span data-stu-id="78338-169">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="78338-170">Actualización de la `OnGetAsync` método en el *Pages/Courses/Details.cshtml.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="78338-170">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="78338-171">Modificar las páginas de eliminación y detalles</span><span class="sxs-lookup"><span data-stu-id="78338-171">Modify the Delete and Details pages</span></span>

<span data-ttu-id="78338-172">Actualice la página Eliminar Razor con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="78338-172">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="78338-173">Realizar los mismos cambios en la página de detalles.</span><span class="sxs-lookup"><span data-stu-id="78338-173">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="78338-174">Probar las páginas de curso</span><span class="sxs-lookup"><span data-stu-id="78338-174">Test the Course pages</span></span>

<span data-ttu-id="78338-175">Prueba de crear, editar, detalles y eliminar.</span><span class="sxs-lookup"><span data-stu-id="78338-175">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="78338-176">Actualizar las páginas de instructor</span><span class="sxs-lookup"><span data-stu-id="78338-176">Update the instructor pages</span></span>

<span data-ttu-id="78338-177">En las siguientes secciones actualización las páginas de instructor.</span><span class="sxs-lookup"><span data-stu-id="78338-177">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="78338-178">Agregar ubicación de la oficina</span><span class="sxs-lookup"><span data-stu-id="78338-178">Add office location</span></span>

<span data-ttu-id="78338-179">Al editar un registro de instructor, desea actualizar la asignación de office del instructor.</span><span class="sxs-lookup"><span data-stu-id="78338-179">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="78338-180">El `Instructor` entidad tiene una relación de uno a cero o uno con el `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="78338-180">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="78338-181">El código de instructor debe administrar:</span><span class="sxs-lookup"><span data-stu-id="78338-181">The instructor code must handle:</span></span>

* <span data-ttu-id="78338-182">Si el usuario desactiva la asignación de office, elimine la `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="78338-182">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="78338-183">Si el usuario especifica una asignación de oficina y estaba vacío, cree un nuevo `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="78338-183">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="78338-184">Si el usuario cambia la asignación de office, actualizar el `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="78338-184">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="78338-185">Actualizar el modelo de página de edición de instructores con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-185">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="78338-186">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="78338-186">The preceding code:</span></span>

- <span data-ttu-id="78338-187">Obtiene el actual `Instructor` entidad desde la base de datos con carga diligente de la `OfficeAssignment` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="78338-187">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="78338-188">Actualiza el objeto recuperado `Instructor` entidad con valores desde el enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="78338-188">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="78338-189">`TryUpdateModel`evita que [de publicación excesiva](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="78338-189">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="78338-190">Si la ubicación de la oficina está en blanco, establece `Instructor.OfficeAssignment` en null.</span><span class="sxs-lookup"><span data-stu-id="78338-190">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="78338-191">Cuando `Instructor.OfficeAssignment` es null, la fila relacionada en el `OfficeAssignment` se elimina la tabla.</span><span class="sxs-lookup"><span data-stu-id="78338-191">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="78338-192">Actualizar la página de edición de instructor</span><span class="sxs-lookup"><span data-stu-id="78338-192">Update the instructor Edit page</span></span>

<span data-ttu-id="78338-193">Actualización *Pages/Instructors/Edit.cshtml* con la ubicación de office:</span><span class="sxs-lookup"><span data-stu-id="78338-193">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="78338-194">Compruebe que puede cambiar una ubicación de oficina de instructores.</span><span class="sxs-lookup"><span data-stu-id="78338-194">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="78338-195">Agregar trabajos del curso a la página de edición de instructor</span><span class="sxs-lookup"><span data-stu-id="78338-195">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="78338-196">Instructores pueden enseñar a cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="78338-196">Instructors may teach any number of courses.</span></span> <span data-ttu-id="78338-197">En esta sección, agregará la capacidad de cambiar las asignaciones de curso.</span><span class="sxs-lookup"><span data-stu-id="78338-197">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="78338-198">La siguiente imagen muestra al instructor actualizado Editar página:</span><span class="sxs-lookup"><span data-stu-id="78338-198">The following image shows the updated instructor Edit page:</span></span>

![Página de edición de instructor con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="78338-200">`Course`y `Instructor` tiene una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="78338-200">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="78338-201">Para agregar y eliminar las relaciones, agregar y quitar entidades de la `CourseAssignments` unir el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="78338-201">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="78338-202">Casillas de verificación Permitir cambios en un instructor se asigna a los cursos.</span><span class="sxs-lookup"><span data-stu-id="78338-202">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="78338-203">Se muestra una casilla de verificación para cada curso en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="78338-203">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="78338-204">Se comprueban los cursos en los que se asigna el instructor a.</span><span class="sxs-lookup"><span data-stu-id="78338-204">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="78338-205">El usuario puede activar o desactivar casillas de verificación para cambiar las asignaciones de curso.</span><span class="sxs-lookup"><span data-stu-id="78338-205">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="78338-206">Si el número de cursos era mucho más grandes:</span><span class="sxs-lookup"><span data-stu-id="78338-206">If the number of courses were much greater:</span></span>

* <span data-ttu-id="78338-207">Probablemente utilizará una interfaz de usuario diferente para mostrar los cursos.</span><span class="sxs-lookup"><span data-stu-id="78338-207">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="78338-208">El método de manipulación de una entidad de combinación para crear o eliminar relaciones no cambiaría.</span><span class="sxs-lookup"><span data-stu-id="78338-208">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="78338-209">Agregar clases para admitir la creación y edición de páginas de instructor</span><span class="sxs-lookup"><span data-stu-id="78338-209">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="78338-210">Crear *SchoolViewModels/AssignedCourseData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-210">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="78338-211">La `AssignedCourseData` clase contiene datos para crear las casillas de verificación para cursos asignados por un instructor.</span><span class="sxs-lookup"><span data-stu-id="78338-211">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="78338-212">Crear el *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* clase base:</span><span class="sxs-lookup"><span data-stu-id="78338-212">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="78338-213">El `InstructorCoursesPageModel` es la clase base que se utilizará para la edición y crear modelos de página.</span><span class="sxs-lookup"><span data-stu-id="78338-213">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="78338-214">`PopulateAssignedCourseData`todos los lee `Course` entidades para rellenar `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="78338-214">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="78338-215">Para cada curso, el código establece la `CourseID`, título, y si no se asigna el instructor al curso.</span><span class="sxs-lookup"><span data-stu-id="78338-215">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="78338-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) se utiliza para crear búsquedas eficaces.</span><span class="sxs-lookup"><span data-stu-id="78338-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="78338-217">Modelo de página de edición de instructores</span><span class="sxs-lookup"><span data-stu-id="78338-217">Instructors Edit page model</span></span>

<span data-ttu-id="78338-218">Actualizar el modelo de página de edición de instructor con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-218">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="78338-219">El código anterior controla los cambios de asignación de office.</span><span class="sxs-lookup"><span data-stu-id="78338-219">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="78338-220">Actualice la vista Razor instructor:</span><span class="sxs-lookup"><span data-stu-id="78338-220">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="78338-221">Cuando pegue el código en Visual Studio, saltos de línea se cambian de forma que el código se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="78338-221">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="78338-222">Presione CTRL+z una vez para deshacer el formato automático.</span><span class="sxs-lookup"><span data-stu-id="78338-222">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="78338-223">CTRL+z corrige los saltos de línea para que se muestren como lo que ve aquí.</span><span class="sxs-lookup"><span data-stu-id="78338-223">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="78338-224">La sangría no tiene que ser perfecto, pero la `@</tr><tr>`, `@:<td>`, `@:</td>`, y `@:</tr>` líneas deben estar en una sola línea tal como se muestra.</span><span class="sxs-lookup"><span data-stu-id="78338-224">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="78338-225">Con el bloque de código nuevo seleccionado, presione la tecla Tab tres veces para alinear el nuevo código con el código existente.</span><span class="sxs-lookup"><span data-stu-id="78338-225">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="78338-226">Votar sobre o revisar el estado de este error [con este vínculo](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="78338-226">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="78338-227">El código anterior crea una tabla HTML que tiene tres columnas.</span><span class="sxs-lookup"><span data-stu-id="78338-227">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="78338-228">Cada columna tiene una casilla de verificación y un título que contiene el número y el título.</span><span class="sxs-lookup"><span data-stu-id="78338-228">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="78338-229">Las casillas de verificación todos tienen el mismo nombre ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="78338-229">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="78338-230">Con el mismo nombre, informa el enlazador de modelos para tratarlos como un grupo.</span><span class="sxs-lookup"><span data-stu-id="78338-230">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="78338-231">El atributo de valor de cada casilla de verificación se establece en `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="78338-231">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="78338-232">Cuando se envía la página, el enlazador de modelos pasa una matriz formada por el `CourseID` valores de las casillas de verificación que están seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="78338-232">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="78338-233">Cuando inicialmente se representan las casillas de verificación, cursos asignados al instructor comprobaron atributos.</span><span class="sxs-lookup"><span data-stu-id="78338-233">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="78338-234">Ejecutar la aplicación y probar la página de edición de instructores actualizada.</span><span class="sxs-lookup"><span data-stu-id="78338-234">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="78338-235">Cambiar algunos trabajos del curso.</span><span class="sxs-lookup"><span data-stu-id="78338-235">Change some course assignments.</span></span> <span data-ttu-id="78338-236">Los cambios se reflejan en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="78338-236">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="78338-237">Nota: Al enfoque tomado aquí para editar datos de curso instructor funciona bien cuando hay un número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="78338-237">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="78338-238">Para las colecciones que son mucho más grandes, una interfaz de usuario diferente y un método de actualización diferente sería más eficaz y utilizable.</span><span class="sxs-lookup"><span data-stu-id="78338-238">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="78338-239">Actualizar la página de creación de instructores</span><span class="sxs-lookup"><span data-stu-id="78338-239">Update the instructors Create page</span></span>

<span data-ttu-id="78338-240">Actualice el modelo de páginas de instructor crear con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-240">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="78338-241">El código anterior es similar a la *Pages/Instructors/Edit.cshtml.cs* código.</span><span class="sxs-lookup"><span data-stu-id="78338-241">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="78338-242">Actualizar la página de crear Razor instructor con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-242">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="78338-243">Probar la página de creación de instructor.</span><span class="sxs-lookup"><span data-stu-id="78338-243">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="78338-244">Actualizar la página de borrado</span><span class="sxs-lookup"><span data-stu-id="78338-244">Update the Delete page</span></span>

<span data-ttu-id="78338-245">Actualice el modelo de páginas de eliminación con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="78338-245">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="78338-246">El código anterior realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="78338-246">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="78338-247">Carga diligente de se utiliza el `CourseAssignments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="78338-247">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="78338-248">`CourseAssignments`se deben incluir o no se eliminan cuando se elimina el instructor.</span><span class="sxs-lookup"><span data-stu-id="78338-248">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="78338-249">Para evitar la necesidad de leerlos, configure la eliminación en cascada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="78338-249">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="78338-250">Si el instructor va a eliminar está asignado como administrador de los departamentos, quita la asignación de instructor de los departamentos.</span><span class="sxs-lookup"><span data-stu-id="78338-250">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="78338-251">[Anterior](xref:data/ef-rp/read-related-data)
[Siguiente](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="78338-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>