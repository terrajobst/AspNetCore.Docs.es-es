---
title: 'ASP.NET Core MVC con EF Core: Actualización de datos relacionados (7 de 10)'
author: rick-anderson
description: En este tutorial, actualizará datos relacionados mediante la actualización de campos de clave externa y propiedades de navegación.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: ef8cb3916e5d1542e4d36cad694351462b94ed32
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126731"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a><span data-ttu-id="0ff8f-103">ASP.NET Core MVC con EF Core: Actualización de datos relacionados (7 de 10)</span><span class="sxs-lookup"><span data-stu-id="0ff8f-103">ASP.NET Core MVC with EF Core - Update Related Data - 7 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0ff8f-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ff8f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ff8f-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web de ASP.NET Core MVC con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="0ff8f-106">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="0ff8f-107">En el tutorial anterior, mostró los datos relacionados; en este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="0ff8f-108">En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Página Course Edit](update-related-data/_static/course-edit.png)

![Página Instructor Edit](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="0ff8f-111">Personalizar las páginas Create y Edit de Courses</span><span class="sxs-lookup"><span data-stu-id="0ff8f-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="0ff8f-112">Cuando se crea una entidad de curso, debe tener una relación con un departamento existente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="0ff8f-113">Para facilitar esto, el código con scaffolding incluye métodos de controlador y vistas de Create y Edit que incluyen una lista desplegable para seleccionar el departamento.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="0ff8f-114">La lista desplegable establece la propiedad de clave externa de `Course.DepartmentID`, y eso es todo lo que necesita de Entity Framework para cargar la propiedad de navegación de `Department` con la entidad Department adecuada.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="0ff8f-115">Podrá usar el código con scaffolding, pero cámbielo ligeramente para agregar el control de errores y ordenar la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="0ff8f-116">En *CoursesController.cs*, elimine los cuatro métodos de creación y edición, y reemplácelos con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="0ff8f-117">Después del método HttpPost de `Edit`, cree un método que cargue la información de departamento para la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="0ff8f-118">El método `PopulateDepartmentsDropDownList` obtiene una lista de todos los departamentos ordenados por nombre, crea una colección `SelectList` para obtener una lista desplegable y pasa la colección a la vista en `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="0ff8f-119">El método acepta el parámetro opcional `selectedDepartment`, que permite al código que realiza la llamada especificar el elemento que se seleccionará cuando se procese la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="0ff8f-120">La vista pasará el nombre "DepartmentID" a la aplicación auxiliar de etiquetas `<select>`, y luego la aplicación auxiliar sabe que puede buscar en el objeto `ViewBag` una `SelectList` denominada "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="0ff8f-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="0ff8f-121">El método `Create` de HttpGet llama al método `PopulateDepartmentsDropDownList` sin configurar el elemento seleccionado, ya que el departamento todavía no está establecido para un nuevo curso:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="0ff8f-122">El método `Edit` de HttpGet establece el elemento seleccionado, basándose en el identificador del departamento que ya está asignado a la línea que se está editando:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="0ff8f-123">Los métodos HttpPost para `Create` y `Edit` también incluyen código que configura el elemento seleccionado cuando vuelven a mostrar la página después de un error.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="0ff8f-124">Esto garantiza que, cuando vuelve a aparecer la página para mostrar el mensaje de error, el departamento que se haya seleccionado permanece seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="0ff8f-125">Agregar AsNoTracking a los métodos Details y Delete</span><span class="sxs-lookup"><span data-stu-id="0ff8f-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="0ff8f-126">Para optimizar el rendimiento de las páginas Course Details y Delete, agregue llamadas `AsNoTracking` en los métodos `Details` y `Delete` de HttpGet.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="0ff8f-127">Modificar las vistas de Course</span><span class="sxs-lookup"><span data-stu-id="0ff8f-127">Modify the Course views</span></span>

<span data-ttu-id="0ff8f-128">En *Views/Courses/Create.cshtml*, agregue una opción "Select Department" a la lista desplegable **Department**, cambie el título de **DepartmentID** a  **Department** y agregue un mensaje de validación.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="0ff8f-129">En *Views/Courses/Edit.cshtml*, realice el mismo cambio que acaba de hacer en *Create.cshtml* en el campo Department.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="0ff8f-130">También en *Views/Courses/Edit.cshtml*, agregue un campo de número de curso antes del campo **Title**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="0ff8f-131">Dado que el número de curso es la clave principal, esta se muestra, pero no se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="0ff8f-132">Ya hay un campo oculto (`<input type="hidden">`) para el número de curso en la vista Edit.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="0ff8f-133">Agregar una aplicación auxiliar de etiquetas `<label>` no elimina la necesidad de un campo oculto, porque no hace que el número de curso se incluya en los datos enviados cuando el usuario hace clic en **Save** en la página **Edit**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="0ff8f-134">En *Views/Courses/Delete.cshtml*, agregue un campo de número de curso en la parte superior y cambie el identificador del departamento por el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="0ff8f-135">En *Views/Courses/Details.cshtml*, realice el mismo cambio que acaba de hacer en *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="0ff8f-136">Probar las páginas Course</span><span class="sxs-lookup"><span data-stu-id="0ff8f-136">Test the Course pages</span></span>

<span data-ttu-id="0ff8f-137">Ejecute la aplicación, seleccione la pestaña **Courses**, haga clic en **Create New** y escriba los datos del curso nuevo:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Página Course Create](update-related-data/_static/course-create.png)

<span data-ttu-id="0ff8f-139">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-139">Click **Create**.</span></span> <span data-ttu-id="0ff8f-140">Se muestra la página de índice de cursos con el nuevo curso agregado a la lista.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="0ff8f-141">El nombre de departamento de la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="0ff8f-142">Haga clic en **Edit** en un curso en la página de índice de cursos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Página Course Edit](update-related-data/_static/course-edit.png)

<span data-ttu-id="0ff8f-144">Cambie los datos en la página y haga clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="0ff8f-145">Se muestra la página de índice de cursos con los datos del curso actualizados.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="0ff8f-146">Agregar una página Edit para Instructors</span><span class="sxs-lookup"><span data-stu-id="0ff8f-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="0ff8f-147">Al editar un registro de instructor, necesita poder actualizar la asignación de la oficina del instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="0ff8f-148">La entidad Instructor tiene una relación de uno a cero o uno con la entidad OfficeAssignment, lo que significa que el código tiene que controlar las situaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="0ff8f-149">Si el usuario borra la asignación de oficina y esta tenía originalmente un valor, elimine la entidad OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="0ff8f-150">Si el usuario escribe un valor de asignación de oficina y originalmente estaba vacío, cree una entidad OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="0ff8f-151">Si el usuario cambia el valor de una asignación de oficina, cambie el valor en una entidad OfficeAssignment existente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0ff8f-152">Actualizar el controlador de Instructors</span><span class="sxs-lookup"><span data-stu-id="0ff8f-152">Update the Instructors controller</span></span>

<span data-ttu-id="0ff8f-153">En *InstructorsController.cs*, cambie el código en el método `Edit` de HttpGet para que cargue la propiedad de navegación `OfficeAssignment` de la entidad Instructor y llame a `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="0ff8f-154">Reemplace el método `Edit` de HttpPost con el siguiente código para controlar las actualizaciones de asignaciones de oficina:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="0ff8f-155">El código realiza lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-155">The code does the following:</span></span>

-  <span data-ttu-id="0ff8f-156">Cambia el nombre del método a `EditPost` porque la firma ahora es la misma que el método `Edit` de HttpGet (el atributo `ActionName` especifica que la dirección URL de `/Edit/` aún está en uso).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="0ff8f-157">Obtiene la entidad Instructor actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="0ff8f-158">Esto es lo mismo que hizo en el método `Edit` de HttpGet.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="0ff8f-159">Actualiza la entidad Instructor recuperada con valores del enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="0ff8f-160">La sobrecarga de `TryUpdateModel` le permite crear una lista de permitidos con las propiedades que quiera incluir.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="0ff8f-161">Esto evita el registro excesivo, como se explica en el [segundo tutorial](crud.md).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   <span data-ttu-id="0ff8f-162">Si la ubicación de la oficina está en blanco, establece la propiedad Instructor.OfficeAssignment en NULL para que se elimine la fila relacionada en la tabla OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="0ff8f-163">Guarda los cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="0ff8f-164">Actualizar la vista de Edit de Instructor</span><span class="sxs-lookup"><span data-stu-id="0ff8f-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="0ff8f-165">En *Views/Instructors/Edit.cshtml*, agregue un nuevo campo para editar la ubicación de la oficina, al final antes del botón **Save**:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="0ff8f-166">Ejecute la aplicación, seleccione la pestaña **Instructors** y, después, haga clic en **Edit** en un instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="0ff8f-167">Cambie el valor de **Office Location** y haga clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-167">Change the **Office Location** and click **Save**.</span></span>

![Página Instructor Edit](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="0ff8f-169">Agregar asignaciones de cursos a la página Edit de los instructores</span><span class="sxs-lookup"><span data-stu-id="0ff8f-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="0ff8f-170">Los instructores pueden impartir cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="0ff8f-171">Ahora mejorará la página Edit Instructor al agregar la capacidad de cambiar las asignaciones de cursos mediante un grupo de casillas, tal y como se muestra en la siguiente captura de pantalla:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Página de edición de instructores con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0ff8f-173">La relación entre las entidades Course e Instructor es varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="0ff8f-174">Para agregar y eliminar relaciones, agregue y quite entidades del conjunto de entidades combinadas CourseAssignments.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="0ff8f-175">La interfaz de usuario que le permite cambiar los cursos a los que está asignado un instructor es un grupo de casillas.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="0ff8f-176">Se muestra una casilla para cada curso en la base de datos y se seleccionan aquellos a los que está asignado actualmente el instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="0ff8f-177">El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="0ff8f-178">Si el número de cursos fuera mucho mayor, probablemente tendría que usar un método diferente de presentar los datos en la vista, pero usaría el mismo método de manipulación de una entidad de combinación para crear o eliminar relaciones.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0ff8f-179">Actualizar el controlador de Instructors</span><span class="sxs-lookup"><span data-stu-id="0ff8f-179">Update the Instructors controller</span></span>

<span data-ttu-id="0ff8f-180">Para proporcionar datos a la vista de la lista de casillas, deberá usar una clase de modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="0ff8f-181">Cree *AssignedCourseData.cs* en la carpeta *SchoolViewModels* y reemplace el código existente con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="0ff8f-182">En *InstructorsController.cs*, reemplace el método `Edit` de HttpGet por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="0ff8f-183">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-183">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="0ff8f-184">El código agrega carga diligente para la propiedad de navegación `Courses` y llama al método `PopulateAssignedCourseData` nuevo para proporcionar información de la matriz de casilla mediante la clase de modelo de vista `AssignedCourseData`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="0ff8f-185">El código en el método `PopulateAssignedCourseData` lee a través de todas las entidades Course para cargar una lista de cursos mediante la clase de modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="0ff8f-186">Para cada curso, el código comprueba si existe el curso en la propiedad de navegación `Courses` del instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="0ff8f-187">Para crear una búsqueda eficaz al comprobar si un curso está asignado al instructor, los cursos asignados a él se colocan en una colección `HashSet`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="0ff8f-188">La propiedad `Assigned` está establecida en true para los cursos a los que está asignado el instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="0ff8f-189">La vista usará esta propiedad para determinar qué casilla debe mostrarse como seleccionada.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="0ff8f-190">Por último, la lista se pasa a la vista en `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="0ff8f-191">A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="0ff8f-192">Reemplace el método `EditPost` con el siguiente código y agregue un nuevo método que actualiza la propiedad de navegación `Courses` de la entidad Instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="0ff8f-193">La firma del método ahora es diferente del método `Edit` de HttpGet, por lo que el nombre del método cambia de `EditPost` a `Edit`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="0ff8f-194">Puesto que la vista no tiene una colección de entidades Course, el enlazador de modelos no puede actualizar automáticamente la propiedad de navegación `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="0ff8f-195">En lugar de usar el enlazador de modelos para actualizar la propiedad de navegación `CourseAssignments`, lo hace en el nuevo método `UpdateInstructorCourses`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="0ff8f-196">Por lo tanto, tendrá que excluir la propiedad `CourseAssignments` del enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="0ff8f-197">Esto no requiere ningún cambio en el código que llama a `TryUpdateModel` porque está usando la sobrecarga de la creación de listas de permitidos y `CourseAssignments` no se encuentra en la lista de inclusión.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="0ff8f-198">Si no se ha seleccionado ninguna casilla, el código en `UpdateInstructorCourses` inicializa la propiedad de navegación `CourseAssignments` con una colección vacía y devuelve:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="0ff8f-199">A continuación, el código recorre en bucle todos los cursos de la base de datos y coteja los que están asignados actualmente al instructor frente a los que se han seleccionado en la vista.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="0ff8f-200">Para facilitar las búsquedas eficaces, estas dos últimas colecciones se almacenan en objetos `HashSet`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="0ff8f-201">Si se ha activado la casilla para un curso pero este no se encuentra en la propiedad de navegación `Instructor.CourseAssignments`, el curso se agrega a la colección en la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="0ff8f-202">Si no se ha activado la casilla para un curso pero este se encuentra en la propiedad de navegación `Instructor.CourseAssignments`, el curso se quita de la colección en la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="0ff8f-203">Actualizar las vistas de Instructor</span><span class="sxs-lookup"><span data-stu-id="0ff8f-203">Update the Instructor views</span></span>

<span data-ttu-id="0ff8f-204">En *Views/Instructors/Edit.cshtml*, agregue un campo **Courses** con una matriz de casillas al agregar el siguiente código inmediatamente después de los elementos `div` del campo **Office** y antes del elemento `div` del botón **Save**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="0ff8f-205">Al pegar el código en Visual Studio, se cambiarán los saltos de línea de tal forma que el código se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="0ff8f-206">Presione Ctrl+Z una vez para deshacer el formato automático.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="0ff8f-207">Esto corregirá los saltos de línea para que se muestren como se ven aquí.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="0ff8f-208">No es necesario que la sangría sea perfecta, pero las líneas `@</tr><tr>`, `@:<td>`, `@:</td>` y `@:</tr>` deben estar en una única línea tal y como se muestra, de lo contrario, obtendrá un error en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="0ff8f-209">Con el bloque de código nuevo seleccionado, presione tres veces la tecla TAB para alinearlo con el código existente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="0ff8f-210">Puede comprobar el estado de este problema [aquí](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="0ff8f-211">Este código crea una tabla HTML que tiene tres columnas.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="0ff8f-212">En cada columna hay una casilla seguida de un título que está formado por el número y el título del curso.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="0ff8f-213">Todas las casillas tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que se deben tratar como un grupo.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="0ff8f-214">El atributo de valor de cada casilla se establece en el valor de `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="0ff8f-215">Cuando se envía la página, el enlazador de modelos pasa una matriz al controlador formada solo por los valores `CourseID` de las casillas activadas.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="0ff8f-216">Cuando las casillas se representan inicialmente, aquellas que son para cursos asignados al instructor tienen atributos seleccionados, lo que las selecciona (las muestra activadas).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="0ff8f-217">Ejecute la aplicación, seleccione la pestaña **Instructors** y haga clic en **Edit** en un instructor para ver la página **Edit**.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Página de edición de instructores con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0ff8f-219">Cambie algunas asignaciones de cursos y haga clic en Save.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="0ff8f-220">Los cambios que haga se reflejan en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE]
> <span data-ttu-id="0ff8f-221">El enfoque que se aplica aquí para modificar datos de los cursos del instructor funciona bien cuando hay un número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-221">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="0ff8f-222">Para las colecciones que son mucho más grandes, se necesitaría una interfaz de usuario y un método de actualización diferentes.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0ff8f-223">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="0ff8f-223">Update the Delete page</span></span>

<span data-ttu-id="0ff8f-224">En *InstructorsController.cs*, elimine el método `DeleteConfirmed` e inserte el siguiente código en su lugar.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="0ff8f-225">Este código realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-225">This code makes the following changes:</span></span>

* <span data-ttu-id="0ff8f-226">Hace la carga diligente para la propiedad de navegación `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="0ff8f-227">Tiene que incluir esto o EF no conocerá las entidades `CourseAssignment` relacionadas y no las eliminará.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="0ff8f-228">Para evitar la necesidad de leerlos aquí, puede configurar la eliminación en cascada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="0ff8f-229">Si el instructor que se va a eliminar está asignado como administrador de cualquiera de los departamentos, quita la asignación de instructor de esos departamentos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="0ff8f-230">Agregar la ubicación de la oficina y cursos a la página Create</span><span class="sxs-lookup"><span data-stu-id="0ff8f-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="0ff8f-231">En *InstructorsController.cs*, elimine los métodos `Create` de HttpGet y HttpPost y, después, agregue el código siguiente en su lugar:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="0ff8f-232">Este código es similar a lo que ha visto para los métodos `Edit`, excepto que no hay cursos seleccionados inicialmente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="0ff8f-233">El método `Create` de HttpGet no llama al método `PopulateAssignedCourseData` porque pueda haber cursos seleccionados sino para proporcionar una colección vacía para el bucle `foreach` en la vista (en caso contrario, el código de vista podría producir una excepción de referencia nula).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="0ff8f-234">El método `Create` de HttpPost agrega cada curso seleccionado a la propiedad de navegación `CourseAssignments` antes de comprobar si hay errores de validación y agrega el instructor nuevo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="0ff8f-235">Los cursos se agregan incluso si hay errores de modelo, por lo que cuando hay errores del modelo (por ejemplo, el usuario escribió una fecha no válida) y se vuelve a abrir la página con un mensaje de error, las selecciones de cursos que se habían realizado se restauran todas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="0ff8f-236">Tenga en cuenta que, para poder agregar cursos a la propiedad de navegación `CourseAssignments`, debe inicializar la propiedad como una colección vacía:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="0ff8f-237">Como alternativa a hacerlo en el código de control, podría hacerlo en el modelo de Instructor cambiando el captador de propiedad para que cree automáticamente la colección si no existe, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0ff8f-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="0ff8f-238">Si modifica la propiedad `CourseAssignments` de esta manera, puede quitar el código de inicialización de propiedad explícito del controlador.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="0ff8f-239">En *Views/Instructor/Create.cshtml*, agregue un cuadro de texto de la ubicación de la oficina y casillas para cursos antes del botón Submit.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="0ff8f-240">Al igual que en el caso de la página Edit, [corrija el formato si Visual Studio vuelve a aplicar formato al código al pegarlo](#notepad).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="0ff8f-241">Pruebe a ejecutar la aplicación y crear un instructor.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-241">Test by running the app and creating an instructor.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="0ff8f-242">Control de transacciones</span><span class="sxs-lookup"><span data-stu-id="0ff8f-242">Handling Transactions</span></span>

<span data-ttu-id="0ff8f-243">Como se explicó en el [tutorial de CRUD](crud.md), Entity Framework implementa las transacciones de manera implícita.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="0ff8f-244">Para escenarios donde se necesita más control, por ejemplo, si se quieren incluir operaciones realizadas fuera de Entity Framework en una transacción, vea [Transacciones](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="0ff8f-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="0ff8f-245">Resumen</span><span class="sxs-lookup"><span data-stu-id="0ff8f-245">Summary</span></span>

<span data-ttu-id="0ff8f-246">Ahora ha completado la introducción para trabajar con datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="0ff8f-247">En el siguiente tutorial verá cómo tratar los conflictos de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="0ff8f-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="0ff8f-248">[Anterior](read-related-data.md)
> [Siguiente](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="0ff8f-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>
