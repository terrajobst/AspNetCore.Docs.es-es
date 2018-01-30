---
title: "Núcleo de ASP.NET MVC con EF Core - actualización relacionadas con datos - 7 de 10"
author: tdykstra
description: "En este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 4085ca9340291f6ab594285360f3b65738699098
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="5174c-103">Actualizar datos relacionados - Core EF con el tutorial de MVC de ASP.NET Core (7 de 10)</span><span class="sxs-lookup"><span data-stu-id="5174c-103">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="5174c-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5174c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5174c-105">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5174c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="5174c-106">Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="5174c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="5174c-107">En el tutorial anterior se muestran datos relacionados; en este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="5174c-108">Las ilustraciones siguientes muestran algunas de las páginas que va a trabajar con.</span><span class="sxs-lookup"><span data-stu-id="5174c-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Página de edición de curso](update-related-data/_static/course-edit.png)

![Página de edición de instructor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="5174c-111">Personalizar la creación y edición de páginas para cursos</span><span class="sxs-lookup"><span data-stu-id="5174c-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="5174c-112">Cuando se crea una nueva entidad de curso, debe tener una relación con un departamento existente.</span><span class="sxs-lookup"><span data-stu-id="5174c-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="5174c-113">Para facilitar esto, el código con scaffolding incluye métodos de controlador y crear y editar vistas que incluyen una lista desplegable para seleccionar el departamento.</span><span class="sxs-lookup"><span data-stu-id="5174c-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="5174c-114">Los conjuntos de la lista desplegable el `Course.DepartmentID` propiedad de clave externa, y eso es todo lo que necesita de Entity Framework para cargar la `Department` propiedad de navegación con la entidad Department adecuada.</span><span class="sxs-lookup"><span data-stu-id="5174c-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="5174c-115">Podrá usar el código con scaffolding, pero cambiar ligeramente para agregar el control de errores y ordenar la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="5174c-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="5174c-116">En *CoursesController.cs*, elimine los cuatro métodos de creación y edición y reemplácelas con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5174c-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="5174c-117">Después de la `Edit` método HttpPost, cree un nuevo método que carga la información de departamento de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="5174c-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="5174c-118">El `PopulateDepartmentsDropDownList` /método siguiente obtiene una lista de todos los departamentos que se ordenan por nombre, se crea un `SelectList` colección para obtener una lista desplegable y pasa la colección a la vista en `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="5174c-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="5174c-119">El método acepta opcional `selectedDepartment` parámetro que permita al código que realiza la llamada especificar el elemento que se seleccionará cuando se procesa la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="5174c-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="5174c-120">La vista pasará el nombre "DepartmentID" a la `<select>` auxiliar de etiquetas y la aplicación auxiliar, a continuación, sepa que puede para buscar en el `ViewBag` de objeto para un `SelectList` denominado "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="5174c-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="5174c-121">El HttpGet `Create` llamadas al método el `PopulateDepartmentsDropDownList` método sin tener que configurar el elemento seleccionado, ya que para un nuevo curso el departamento no establecido todavía:</span><span class="sxs-lookup"><span data-stu-id="5174c-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="5174c-122">El HttpGet `Edit` método establece el elemento seleccionado, basándose en el identificador del departamento que ya está asignado a la línea que se está edita:</span><span class="sxs-lookup"><span data-stu-id="5174c-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="5174c-123">Los métodos HttpPost para ambos `Create` y `Edit` también incluir código que establece el elemento seleccionado cuando vuelva a mostrar la página después de un error.</span><span class="sxs-lookup"><span data-stu-id="5174c-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="5174c-124">Esto garantiza que cuando se vuelve a mostrar la página para mostrar el mensaje de error, se seleccionó cualquier departamento permanece seleccionada.</span><span class="sxs-lookup"><span data-stu-id="5174c-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="5174c-125">Agregar. AsNoTracking para obtener más información y eliminar métodos</span><span class="sxs-lookup"><span data-stu-id="5174c-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="5174c-126">Para optimizar el rendimiento de los detalles de curso y eliminar páginas, agregar `AsNoTracking` llama el `Details` y HttpGet `Delete` métodos.</span><span class="sxs-lookup"><span data-stu-id="5174c-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="5174c-127">Modificar las vistas de curso</span><span class="sxs-lookup"><span data-stu-id="5174c-127">Modify the Course views</span></span>

<span data-ttu-id="5174c-128">En *Views/Courses/Create.cshtml*, agregar una opción de "Departamento de seleccionar" para el **departamento** lista desplegable lista, cambie el título de **DepartmentID** a  **Departamento**y agregar un mensaje de validación.</span><span class="sxs-lookup"><span data-stu-id="5174c-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="5174c-129">En *Views/Courses/Edit.cshtml*, realizar el mismo cambio en el campo de departamento que acaba de *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5174c-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="5174c-130">También en *Views/Courses/Edit.cshtml*, agregar un campo de número de curso antes de la **título** campo.</span><span class="sxs-lookup"><span data-stu-id="5174c-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="5174c-131">Dado que el número de curso es la clave principal, se muestra, pero no se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="5174c-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="5174c-132">Ya hay un campo oculto (`<input type="hidden">`) para el número de curso en la vista de edición.</span><span class="sxs-lookup"><span data-stu-id="5174c-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="5174c-133">Agregar un `<label>` auxiliar de etiqueta no elimina la necesidad de para el campo oculto porque no hacer que el número de curso que se incluirá en los datos enviados cuando el usuario hace clic en **guardar** en el **editar** página.</span><span class="sxs-lookup"><span data-stu-id="5174c-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="5174c-134">En *Views/Courses/Delete.cshtml*, agregar un campo de número de curso en la parte superior y cambie el Id. de departamento al nombre de departamento.</span><span class="sxs-lookup"><span data-stu-id="5174c-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="5174c-135">En *Views/Courses/Details.cshtml*, realizar el mismo cambio en la que se hizo simplemente *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5174c-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="5174c-136">Probar las páginas de curso</span><span class="sxs-lookup"><span data-stu-id="5174c-136">Test the Course pages</span></span>

<span data-ttu-id="5174c-137">Ejecutar la aplicación, seleccione la **cursos** , haga clic en **crear nuevo**y escriba los datos de un curso nuevo:</span><span class="sxs-lookup"><span data-stu-id="5174c-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Página de creación de curso](update-related-data/_static/course-create.png)

<span data-ttu-id="5174c-139">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="5174c-139">Click **Create**.</span></span> <span data-ttu-id="5174c-140">Se muestra la página de índice de cursos con el nuevo curso agregado a la lista.</span><span class="sxs-lookup"><span data-stu-id="5174c-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="5174c-141">El nombre de departamento en la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.</span><span class="sxs-lookup"><span data-stu-id="5174c-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="5174c-142">Haga clic en **editar** en un curso en la página de índice de cursos.</span><span class="sxs-lookup"><span data-stu-id="5174c-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Página de edición de curso](update-related-data/_static/course-edit.png)

<span data-ttu-id="5174c-144">Cambiar los datos en la página y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="5174c-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="5174c-145">Se muestra la página de índice de cursos con los datos de curso actualizado.</span><span class="sxs-lookup"><span data-stu-id="5174c-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="5174c-146">Agregar una página de edición para instructores</span><span class="sxs-lookup"><span data-stu-id="5174c-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="5174c-147">Cuando se edita un registro instructor, desea poder actualizar la asignación de office del instructor.</span><span class="sxs-lookup"><span data-stu-id="5174c-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="5174c-148">La entidad Instructor tiene una relación de uno a cero o uno a la entidad OfficeAssignment, lo que significa que el código tiene que controlar las situaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="5174c-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="5174c-149">Si el usuario borra la asignación de office y que tenía originalmente un valor, elimine la entidad OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="5174c-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="5174c-150">Si el usuario escribe un valor de asignación de office y originalmente estaba vacío, cree una nueva entidad OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="5174c-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="5174c-151">Si el usuario cambia el valor de una asignación de office, cambie el valor en una entidad OfficeAssignment existente.</span><span class="sxs-lookup"><span data-stu-id="5174c-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="5174c-152">Actualice el controlador de instructores</span><span class="sxs-lookup"><span data-stu-id="5174c-152">Update the Instructors controller</span></span>

<span data-ttu-id="5174c-153">En *InstructorsController.cs*, cambie el código en el HttpGet `Edit` método por lo que TI carga la entidad Instructor `OfficeAssignment` propiedad de navegación y llamadas `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="5174c-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="5174c-154">Reemplace el HttpPost `Edit` método con el siguiente código para controlar las actualizaciones de asignación de office:</span><span class="sxs-lookup"><span data-stu-id="5174c-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="5174c-155">El código hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5174c-155">The code does the following:</span></span>

-  <span data-ttu-id="5174c-156">Cambia el nombre del método para `EditPost` porque la firma ahora es el mismo que el HttpGet `Edit` método (el `ActionName` atributo especifica que el `/Edit/` aún se utiliza la dirección URL).</span><span class="sxs-lookup"><span data-stu-id="5174c-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="5174c-157">Obtiene la entidad Instructor actual de la base de datos mediante diligente para cargar la `OfficeAssignment` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="5174c-158">Esto equivale a lo que hizo en el HttpGet `Edit` método.</span><span class="sxs-lookup"><span data-stu-id="5174c-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="5174c-159">Actualiza la entidad Instructor recuperada con valores desde el enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="5174c-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="5174c-160">El `TryUpdateModel` sobrecarga permite a la lista blanca las propiedades que van a incluir.</span><span class="sxs-lookup"><span data-stu-id="5174c-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="5174c-161">Esto evita que el registro excesivo, como se explica en la [segundo tutorial](crud.md).</span><span class="sxs-lookup"><span data-stu-id="5174c-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="5174c-162">Si la ubicación de la oficina está en blanco, Establece la propiedad Instructor.OfficeAssignment en null para que se eliminará la fila en la tabla OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="5174c-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="5174c-163">Guarda los cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5174c-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="5174c-164">Actualizar la vista de edición Instructor</span><span class="sxs-lookup"><span data-stu-id="5174c-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="5174c-165">En *Views/Instructors/Edit.cshtml*, agregar un nuevo campo para editar la ubicación de la oficina, al final antes de la **guardar** botón:</span><span class="sxs-lookup"><span data-stu-id="5174c-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="5174c-166">Ejecutar la aplicación, seleccione la **instructores** ficha y, a continuación, haga clic en **editar** en un instructor.</span><span class="sxs-lookup"><span data-stu-id="5174c-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="5174c-167">Cambiar el **oficina** y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="5174c-167">Change the **Office Location** and click **Save**.</span></span>

![Página de edición de instructor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="5174c-169">Agregar trabajos del curso en la página Editar Instructor</span><span class="sxs-lookup"><span data-stu-id="5174c-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="5174c-170">Instructores pueden enseñar a cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="5174c-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="5174c-171">Ahora mejorará la página Editar Instructor agregando la capacidad de cambiar las asignaciones de curso mediante un grupo de casillas de verificación, tal y como se muestra en la siguiente captura de pantalla:</span><span class="sxs-lookup"><span data-stu-id="5174c-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Página de edición de instructor con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="5174c-173">La relación entre las entidades de curso e Instructor es varios a varios.</span><span class="sxs-lookup"><span data-stu-id="5174c-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="5174c-174">Para agregar y eliminar las relaciones, agregar y quitar entidades a y desde el conjunto de entidades de la combinación de CourseAssignments.</span><span class="sxs-lookup"><span data-stu-id="5174c-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="5174c-175">La interfaz de usuario que permite cambiar los cursos un instructor está asignado a es un grupo de casillas de verificación.</span><span class="sxs-lookup"><span data-stu-id="5174c-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="5174c-176">Se muestra una casilla de verificación para cada curso en la base de datos y se seleccionan los que está asignado actualmente el instructor a.</span><span class="sxs-lookup"><span data-stu-id="5174c-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="5174c-177">El usuario puede activar o desactivar casillas de verificación para cambiar las asignaciones de curso.</span><span class="sxs-lookup"><span data-stu-id="5174c-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="5174c-178">Si el número de cursos era mucho mayor, probablemente desee utilizar un método diferente de presentar los datos en la vista, pero usaría el mismo método de manipulación de una entidad de combinación para crear o eliminar relaciones.</span><span class="sxs-lookup"><span data-stu-id="5174c-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="5174c-179">Actualice el controlador de instructores</span><span class="sxs-lookup"><span data-stu-id="5174c-179">Update the Instructors controller</span></span>

<span data-ttu-id="5174c-180">Para proporcionar datos a la vista de la lista de casillas de verificación, deberá usar una clase de modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="5174c-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="5174c-181">Crear *AssignedCourseData.cs* en el *SchoolViewModels* carpeta y reemplace el código existente por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5174c-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="5174c-182">En *InstructorsController.cs*, reemplace el HttpGet `Edit` método por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5174c-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="5174c-183">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="5174c-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="5174c-184">El código agrega carga diligente de la `Courses` propiedad de navegación y llama a la nueva `PopulateAssignedCourseData` método para proporcionar información de la matriz de casilla utilizando la `AssignedCourseData` Ver clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="5174c-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="5174c-185">El código en el `PopulateAssignedCourseData` método lee a través de todas las entidades de curso para cargar una lista de cursos mediante la clase de modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="5174c-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="5174c-186">Para cada curso, el código comprueba si existe el curso en el instructor `Courses` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="5174c-187">Para crear una búsqueda eficaz al comprobar si se asigna un curso para el instructor, los cursos asignados al instructor se colocan en un `HashSet` colección.</span><span class="sxs-lookup"><span data-stu-id="5174c-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="5174c-188">El `Assigned` propiedad está establecida en true para cursos el instructor se asigna a.</span><span class="sxs-lookup"><span data-stu-id="5174c-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="5174c-189">La vista utilizará esta propiedad para determinar qué Compruebe cuadros deben mostrarse como seleccionado.</span><span class="sxs-lookup"><span data-stu-id="5174c-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="5174c-190">Por último, la lista se pasa a la vista en `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5174c-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="5174c-191">A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="5174c-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="5174c-192">Reemplace el `EditPost` método con el siguiente código y agregar un nuevo método que actualiza el `Courses` propiedad de navegación de la entidad Instructor.</span><span class="sxs-lookup"><span data-stu-id="5174c-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="5174c-193">La firma de método ahora es diferente de la HttpGet `Edit` método, por lo que cambia el nombre del método de `EditPost` a `Edit`.</span><span class="sxs-lookup"><span data-stu-id="5174c-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="5174c-194">Puesto que la vista no tiene una colección de entidades del curso, el enlazador de modelos no se puede actualizar automáticamente el `CourseAssignments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="5174c-195">En lugar de usar el enlazador de modelos para actualizar la `CourseAssignments` propiedad de navegación, hacerlo en el nuevo `UpdateInstructorCourses` método.</span><span class="sxs-lookup"><span data-stu-id="5174c-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="5174c-196">Por lo tanto, tendrá que excluir el `CourseAssignments` propiedad de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="5174c-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="5174c-197">Esto no requiere ningún cambio en el código que llama `TryUpdateModel` porque lo está utilizando la sobrecarga de la creación de listas blancas y `CourseAssignments` no se encuentra en la lista de inclusión.</span><span class="sxs-lookup"><span data-stu-id="5174c-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="5174c-198">Si ninguna comprobación cuadros estaban seleccionados, el código en `UpdateInstructorCourses` inicializa el `CourseAssignments` propiedad de navegación con una colección vacía y devuelve:</span><span class="sxs-lookup"><span data-stu-id="5174c-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="5174c-199">A continuación, el código recorre en bucle todos los cursos en la base de datos y comprueba cada curso en los que está asignado actualmente al instructor frente a los que se han seleccionado en la vista.</span><span class="sxs-lookup"><span data-stu-id="5174c-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="5174c-200">Para facilitar las búsquedas eficaces, las dos colecciones este últimas se almacenan en `HashSet` objetos.</span><span class="sxs-lookup"><span data-stu-id="5174c-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="5174c-201">Si se activó la casilla de verificación para un curso pero no se encuentra en el transcurso del `Instructor.CourseAssignments` propiedad de navegación, el curso se agrega a la colección en la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="5174c-202">Si no se ha seleccionado la casilla de verificación para un curso, pero el curso está en el `Instructor.CourseAssignments` propiedad de navegación, el curso se quita de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="5174c-203">Actualizar las vistas de Instructor</span><span class="sxs-lookup"><span data-stu-id="5174c-203">Update the Instructor views</span></span>

<span data-ttu-id="5174c-204">En *Views/Instructors/Edit.cshtml*, agregar un **cursos** campo con una matriz de casillas de verificación agregando el siguiente código inmediatamente después de la `div` elementos para el **Office**  campo y antes de la `div` (elemento) para la **guardar** botón.</span><span class="sxs-lookup"><span data-stu-id="5174c-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="5174c-205">Cuando pegue el código en Visual Studio, saltos de línea se cambiará de forma que el código se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="5174c-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="5174c-206">Presione CTRL+z una vez para deshacer el formato automático.</span><span class="sxs-lookup"><span data-stu-id="5174c-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="5174c-207">Esto solucionará los saltos de línea para que se muestren como lo que ve aquí.</span><span class="sxs-lookup"><span data-stu-id="5174c-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="5174c-208">La sangría no tiene que ser perfecto, pero la `@</tr><tr>`, `@:<td>`, `@:</td>`, y `@:</tr>` líneas deben estar en una sola línea tal como se muestra o se obtendrá un error en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5174c-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="5174c-209">Con el bloque de código nuevo seleccionado, presione la tecla Tab tres veces para alinear el nuevo código con el código existente.</span><span class="sxs-lookup"><span data-stu-id="5174c-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="5174c-210">Puede comprobar el estado de este problema [aquí](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="5174c-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="5174c-211">Este código crea una tabla HTML que tiene tres columnas.</span><span class="sxs-lookup"><span data-stu-id="5174c-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="5174c-212">En cada columna es una casilla de verificación seguida de un título que está formada por el número y el título.</span><span class="sxs-lookup"><span data-stu-id="5174c-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="5174c-213">Las casillas de verificación que todos tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que sean se trate como un grupo.</span><span class="sxs-lookup"><span data-stu-id="5174c-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="5174c-214">El atributo de valor de cada casilla de verificación se establece en el valor de `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="5174c-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="5174c-215">Cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que está formada por el `CourseID` valores de las casillas de verificación que están seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="5174c-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="5174c-216">Cuando inicialmente se representan las casillas de verificación, aquellos que por cursos asignados al instructor comprobaron atributos, que selecciona (muestra ellos activadas).</span><span class="sxs-lookup"><span data-stu-id="5174c-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="5174c-217">Ejecutar la aplicación, seleccione la **instructores** ficha y haga clic en **editar** en un instructor para ver el **editar** página.</span><span class="sxs-lookup"><span data-stu-id="5174c-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Página de edición de instructor con cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="5174c-219">Cambiar algunos trabajos del curso y haga clic en Guardar.</span><span class="sxs-lookup"><span data-stu-id="5174c-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="5174c-220">Los cambios que realice se reflejan en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="5174c-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="5174c-221">El enfoque adoptado aquí para editar datos de curso instructor funciona bien cuando hay un número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="5174c-221">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="5174c-222">Para las colecciones que son mucho más grandes, una interfaz de usuario diferente y un método de actualización diferentes sería necesarios.</span><span class="sxs-lookup"><span data-stu-id="5174c-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="5174c-223">Actualizar la página de borrado</span><span class="sxs-lookup"><span data-stu-id="5174c-223">Update the Delete page</span></span>

<span data-ttu-id="5174c-224">En *InstructorsController.cs*, eliminar el `DeleteConfirmed` método e insertar el siguiente código en su lugar.</span><span class="sxs-lookup"><span data-stu-id="5174c-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="5174c-225">Este código realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="5174c-225">This code makes the following changes:</span></span>

* <span data-ttu-id="5174c-226">Eager hace para cargar la `CourseAssignments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="5174c-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="5174c-227">Tiene que incluir esto o EF no sabe todo sobre relacionados `CourseAssignment` entidades y no se eliminan.</span><span class="sxs-lookup"><span data-stu-id="5174c-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="5174c-228">Para evitar la necesidad de leerlos aquí puede configurar la eliminación en cascada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5174c-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="5174c-229">Si el instructor va a eliminar está asignado como administrador de los departamentos, quita la asignación de instructor de los departamentos.</span><span class="sxs-lookup"><span data-stu-id="5174c-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="5174c-230">Agregar ubicación de oficina y cursos en la página Crear</span><span class="sxs-lookup"><span data-stu-id="5174c-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="5174c-231">En *InstructorsController.cs*, elimine el HttpGet y HttpPost `Create` métodos y, a continuación, agregue el código siguiente en su lugar:</span><span class="sxs-lookup"><span data-stu-id="5174c-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="5174c-232">Este código es similar a los que ha visto el `Edit` métodos, excepto que inicialmente no cursos están seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="5174c-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="5174c-233">El HttpGet `Create` llamadas al método el `PopulateAssignedCourseData` método no porque puede haber cursos seleccionados pero en orden para proporcionar una colección vacía para el `foreach` bucle en la vista (en caso contrario, el código de vista podría producir una excepción de referencia nula).</span><span class="sxs-lookup"><span data-stu-id="5174c-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="5174c-234">El HttpPost `Create` método agrega cada curso seleccionado para la `CourseAssignments` propiedad de navegación antes de que se comprueba si hay errores de validación y agrega el instructor nuevo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5174c-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="5174c-235">Se agregan cursos incluso si hay errores del modelo para que cuando hay errores del modelo (por ejemplo, el usuario ordenado una fecha no válida) y se volverá a abrir la página con un mensaje de error, automáticamente se restauran todas las selecciones de curso que se realizaron.</span><span class="sxs-lookup"><span data-stu-id="5174c-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="5174c-236">Tenga en cuenta que para poder agregar cursos a los `CourseAssignments` propiedad de navegación se deben inicializar la propiedad como una colección vacía:</span><span class="sxs-lookup"><span data-stu-id="5174c-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="5174c-237">Como alternativa a hacerlo en el código del controlador, podría hacer en el modelo de Instructor cambiando el captador de propiedad para crear automáticamente la colección si no existe, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5174c-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="5174c-238">Si modifica el `CourseAssignments` propiedad de esta manera, puede quitar el código de inicialización de propiedad explícitos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="5174c-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="5174c-239">En *Views/Instructor/Create.cshtml*, agregue un cuadro de texto de la ubicación de office y casillas de verificación para cursos antes el botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="5174c-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="5174c-240">Como en el caso de la página Editar, [corrija el formato si Visual Studio vuelve a dar formato el código al pegarlo](#notepad).</span><span class="sxs-lookup"><span data-stu-id="5174c-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="5174c-241">Pruebe ejecutando la aplicación y creando un instructor.</span><span class="sxs-lookup"><span data-stu-id="5174c-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="5174c-242">Controlar transacciones</span><span class="sxs-lookup"><span data-stu-id="5174c-242">Handling Transactions</span></span>

<span data-ttu-id="5174c-243">Como se explica en la [tutorial CRUD](crud.md), Entity Framework implementa de manera implícita las transacciones.</span><span class="sxs-lookup"><span data-stu-id="5174c-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="5174c-244">Para escenarios donde necesita más control, por ejemplo, si van a incluir operaciones realizadas fuera de Entity Framework en una transacción, consulte [transacciones](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="5174c-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="5174c-245">Resumen</span><span class="sxs-lookup"><span data-stu-id="5174c-245">Summary</span></span>

<span data-ttu-id="5174c-246">Ahora ha completado la introducción a trabajar con los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="5174c-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="5174c-247">En el siguiente tutorial verá cómo tratar los conflictos de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="5174c-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5174c-248">[Anterior](read-related-data.md)
[Siguiente](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="5174c-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
