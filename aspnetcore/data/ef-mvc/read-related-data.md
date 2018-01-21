---
title: "Núcleo de ASP.NET MVC con EF básica: leer datos relacionados - 6 de 10"
author: tdykstra
description: "En este tutorial podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 1321cb00a432669b4a97ad20063b6cf9ea75f24c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="bfb49-103">Lectura relacionadas con datos - Core EF con el tutorial de MVC de ASP.NET Core (6 de 10)</span><span class="sxs-lookup"><span data-stu-id="bfb49-103">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="bfb49-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bfb49-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bfb49-105">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfb49-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="bfb49-106">Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="bfb49-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="bfb49-107">En el tutorial anterior, ha completado el modelo de datos School.</span><span class="sxs-lookup"><span data-stu-id="bfb49-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="bfb49-108">En este tutorial, podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="bfb49-109">Las ilustraciones siguientes muestran las páginas que trabajará con.</span><span class="sxs-lookup"><span data-stu-id="bfb49-109">The following illustrations show the pages that you'll work with.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="bfb49-112">Carga diligente, explícita y diferida de los datos relacionados</span><span class="sxs-lookup"><span data-stu-id="bfb49-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="bfb49-113">Existen varias formas de que el software de asignación relacional de objetos (ORM) como Entity Framework puede cargar datos relacionados en las propiedades de navegación de una entidad:</span><span class="sxs-lookup"><span data-stu-id="bfb49-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="bfb49-114">Carga diligente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-114">Eager loading.</span></span> <span data-ttu-id="bfb49-115">Cuando se lee la entidad, se recuperan los datos relacionados con él.</span><span class="sxs-lookup"><span data-stu-id="bfb49-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="bfb49-116">Esto normalmente como resultado de una consulta de combinación única que recupera todos los datos que se necesitan.</span><span class="sxs-lookup"><span data-stu-id="bfb49-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="bfb49-117">Especificar carga diligente de Entity Framework Core mediante la `Include` y `ThenInclude` métodos.</span><span class="sxs-lookup"><span data-stu-id="bfb49-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![En el ejemplo se carga diligente](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="bfb49-119">Puede recuperar algunos de los datos en distintas consultas y EF "corrige" las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="bfb49-120">Es decir, EF agrega automáticamente las entidades recuperadas por separado que pertenecen de propiedades de navegación de entidades recuperadas previamente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="bfb49-121">En la consulta que recupera los datos relacionados, puede usar el `Load` método en lugar de un método que devuelva una lista o un objeto, como `ToList` o `Single`.</span><span class="sxs-lookup"><span data-stu-id="bfb49-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Ejemplo de separar las consultas](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="bfb49-123">Carga explícita.</span><span class="sxs-lookup"><span data-stu-id="bfb49-123">Explicit loading.</span></span> <span data-ttu-id="bfb49-124">Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="bfb49-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="bfb49-125">Escribir código que recupera los datos relacionados si es necesario.</span><span class="sxs-lookup"><span data-stu-id="bfb49-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="bfb49-126">Como en el caso de carga diligente con consultas independientes, explícitas de carga de resultados de varias consultas envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bfb49-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="bfb49-127">La diferencia es que con carga explícita, el código especifica las propiedades de navegación que va a cargarse.</span><span class="sxs-lookup"><span data-stu-id="bfb49-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="bfb49-128">En Entity Framework Core 1.1 se puede utilizar el `Load` método para realizar la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="bfb49-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="bfb49-129">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bfb49-129">For example:</span></span>

  ![En el ejemplo se carga explícita](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="bfb49-131">Carga diferida.</span><span class="sxs-lookup"><span data-stu-id="bfb49-131">Lazy loading.</span></span> <span data-ttu-id="bfb49-132">Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="bfb49-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="bfb49-133">Sin embargo, la primera vez que intente acceder a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="bfb49-134">Se envía una consulta a la base de datos cada vez que intente obtener datos de una propiedad de navegación por primera vez.</span><span class="sxs-lookup"><span data-stu-id="bfb49-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="bfb49-135">Entity Framework Core 1.0 no admite la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="bfb49-135">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="bfb49-136">Consideraciones sobre el rendimiento</span><span class="sxs-lookup"><span data-stu-id="bfb49-136">Performance considerations</span></span>

<span data-ttu-id="bfb49-137">Si sabe que necesita datos relacionados para cada entidad recuperar, a menudo carga diligente ofrece el mejor rendimiento, dado que una sola consulta que se envía a la base de datos es normalmente más eficaz que las consultas independientes para cada entidad recuperados.</span><span class="sxs-lookup"><span data-stu-id="bfb49-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="bfb49-138">Por ejemplo, suponga que cada departamento tiene diez cursos relacionados.</span><span class="sxs-lookup"><span data-stu-id="bfb49-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="bfb49-139">Carga diligente de todos los datos relacionados se crearán simplemente una consulta sencilla (combinación) y un único viaje de ida y vuelta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bfb49-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="bfb49-140">Una consulta independiente para cursos para cada departamento, se crearán en once viajes de ida y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bfb49-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="bfb49-141">La ida y vuelta adicional a la base de datos son especialmente afecta negativamente al rendimiento cuando la latencia es alta.</span><span class="sxs-lookup"><span data-stu-id="bfb49-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="bfb49-142">Por otro lado, en algunos escenarios de separar las consultas es más eficaz.</span><span class="sxs-lookup"><span data-stu-id="bfb49-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="bfb49-143">Carga diligente de todos los datos relacionados en una consulta puede dar lugar a una combinación muy compleja para generarse, que SQL Server no puede procesar eficazmente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="bfb49-144">O bien, si necesita tener acceso a propiedades de navegación de una entidad solo para un subconjunto de un conjunto de las entidades que se procesan, separar las consultas dará mejores resultados porque carga diligente de todo el contenido por adelantado recuperaría más datos de los que necesita.</span><span class="sxs-lookup"><span data-stu-id="bfb49-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="bfb49-145">Si el rendimiento es crítico, es mejor probar el rendimiento de ambas formas para elegir la mejor opción.</span><span class="sxs-lookup"><span data-stu-id="bfb49-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="bfb49-146">Crear una página de cursos que muestra el nombre de departamento</span><span class="sxs-lookup"><span data-stu-id="bfb49-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="bfb49-147">La entidad de curso incluye una propiedad de navegación que contiene la entidad de departamento del departamento que el curso se asigna a.</span><span class="sxs-lookup"><span data-stu-id="bfb49-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="bfb49-148">Para mostrar el nombre del departamento asignado en una lista de cursos, tendrá que obtener la propiedad de nombre de la entidad de departamento que se encuentra en la `Course.Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="bfb49-149">Crear un controlador denominado CoursesController para el tipo de entidad de curso, utilizando las mismas opciones para la **controlador de MVC con vistas que usan Entity Framework** scaffolder que lo hizo anteriormente para el controlador de estudiantes, como se muestra en el la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="bfb49-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Agregar controlador de cursos](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="bfb49-151">Abra *CoursesController.cs* y examine el `Index` método.</span><span class="sxs-lookup"><span data-stu-id="bfb49-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="bfb49-152">La técnica scaffolding automática se especificado carga diligente de la `Department` propiedad de navegación mediante el uso de la `Include` método.</span><span class="sxs-lookup"><span data-stu-id="bfb49-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="bfb49-153">Reemplace el `Index` método con el siguiente código que utiliza un nombre más adecuado para la `IQueryable` que devuelve las entidades de curso (`courses` en lugar de `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="bfb49-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="bfb49-154">Abra *Views/Courses/Index.cshtml* y reemplace el código de plantilla con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="bfb49-155">Se resaltan los cambios:</span><span class="sxs-lookup"><span data-stu-id="bfb49-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="bfb49-156">Ha realizado los siguientes cambios en el código con scaffolding:</span><span class="sxs-lookup"><span data-stu-id="bfb49-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="bfb49-157">Cambia el encabezado de índice a Courses.</span><span class="sxs-lookup"><span data-stu-id="bfb49-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="bfb49-158">Agrega un **número** columna que muestra la `CourseID` valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="bfb49-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="bfb49-159">De forma predeterminada, las claves principales no son scaffolding porque normalmente son importantes para los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="bfb49-159">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="bfb49-160">Sin embargo, en este caso, la clave principal es significativa y desea mostrarla.</span><span class="sxs-lookup"><span data-stu-id="bfb49-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="bfb49-161">Cambiar el **departamento** columna para mostrar el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="bfb49-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="bfb49-162">El código muestra la `Name` propiedad de la entidad de departamento que se carga en el `Department` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="bfb49-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="bfb49-163">Ejecute la aplicación y seleccione la **cursos** pestaña para ver la lista con los nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="bfb49-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="bfb49-165">Crear una página de instructores que muestra los cursos y las inscripciones</span><span class="sxs-lookup"><span data-stu-id="bfb49-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="bfb49-166">En esta sección, creará un controlador y la vista de la entidad Instructor con el fin de mostrar la página de instructores:</span><span class="sxs-lookup"><span data-stu-id="bfb49-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

<span data-ttu-id="bfb49-168">Esta página se lee y muestra los datos relacionados de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="bfb49-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="bfb49-169">La lista de instructores muestra datos relacionados de la entidad OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="bfb49-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="bfb49-170">Las entidades Instructor y OfficeAssignment se encuentran en una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="bfb49-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="bfb49-171">Carga diligente usará para las entidades OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="bfb49-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="bfb49-172">Como se explicó anteriormente, carga diligente normalmente es más eficaz cuando necesite los datos relacionados para todas las filas recuperadas de la tabla principal.</span><span class="sxs-lookup"><span data-stu-id="bfb49-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="bfb49-173">En este caso, desea mostrar las asignaciones de office para todos los instructores mostradas.</span><span class="sxs-lookup"><span data-stu-id="bfb49-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="bfb49-174">Cuando el usuario selecciona un instructor, se muestran las entidades relacionadas de curso.</span><span class="sxs-lookup"><span data-stu-id="bfb49-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="bfb49-175">Las entidades Instructor y el curso se encuentran en una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="bfb49-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="bfb49-176">Carga diligente usará para las entidades de curso y sus entidades relacionadas de departamento.</span><span class="sxs-lookup"><span data-stu-id="bfb49-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="bfb49-177">En este caso, separar las consultas pueden ser más eficaces porque necesita cursos solo para el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="bfb49-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="bfb49-178">Sin embargo, en este ejemplo se muestra cómo utilizar carga diligente para las propiedades de navegación dentro de las entidades que se encuentran a sí mismos en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="bfb49-179">Cuando el usuario selecciona un curso, se muestran datos relacionados desde el conjunto de entidades de inscripciones.</span><span class="sxs-lookup"><span data-stu-id="bfb49-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="bfb49-180">Las entidades de curso e inscripción están en una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="bfb49-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="bfb49-181">Va a utilizar consultas independientes para las entidades de inscripción y sus entidades relacionadas de estudiante.</span><span class="sxs-lookup"><span data-stu-id="bfb49-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="bfb49-182">Crear un modelo de vista para la vista de índice Instructor</span><span class="sxs-lookup"><span data-stu-id="bfb49-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="bfb49-183">La página de instructores muestra datos de tres tablas diferentes.</span><span class="sxs-lookup"><span data-stu-id="bfb49-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="bfb49-184">Por lo tanto, creará un modelo de vista que incluye tres propiedades, que contiene los datos de una de las tablas.</span><span class="sxs-lookup"><span data-stu-id="bfb49-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="bfb49-185">En el *SchoolViewModels* carpeta, crear *InstructorIndexData.cs* y reemplace el código existente por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="bfb49-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="bfb49-186">Crear el controlador de Instructor y vistas</span><span class="sxs-lookup"><span data-stu-id="bfb49-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="bfb49-187">Cree un controlador de instructores con acciones de lectura/escritura EF tal como se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="bfb49-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Agregar controlador instructores](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="bfb49-189">Abra *InstructorsController.cs* y agregue un mediante declaración para el espacio de nombres ViewModels:</span><span class="sxs-lookup"><span data-stu-id="bfb49-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="bfb49-190">Reemplace el método de índice con el código siguiente para realizar una carga diligente de los datos relacionados y colocarla en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="bfb49-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="bfb49-191">El método acepta datos de ruta opcional (`id`) y un parámetro de cadena de consulta (`courseID`) que proporcionan los valores de Id. del instructor seleccionado y el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="bfb49-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="bfb49-192">Los parámetros se proporcionan con el **seleccione** hipervínculos en la página.</span><span class="sxs-lookup"><span data-stu-id="bfb49-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="bfb49-193">El código comienza creando una instancia del modelo de vista y colocar en ella la lista de instructores.</span><span class="sxs-lookup"><span data-stu-id="bfb49-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="bfb49-194">El código especifica una carga diligente de la `Instructor.OfficeAssignment` y `Instructor.CourseAssignments` propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="bfb49-195">Dentro de la `CourseAssignments` propiedad, el `Course` propiedad esté cargado y dentro de ese, la `Enrollments` y `Department` propiedades están cargados y dentro de cada `Enrollment` entidad la `Student` propiedad está cargada.</span><span class="sxs-lookup"><span data-stu-id="bfb49-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="bfb49-196">Debido a que la vista siempre requiere la entidad OfficeAssignment, resulta más eficaz capturar en la misma consulta.</span><span class="sxs-lookup"><span data-stu-id="bfb49-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="bfb49-197">Entidades de curso son necesarias cuando se selecciona un instructor en la página web, por lo que es mejor que varias consultas una sola consulta solo si la página se muestra con más frecuencia con un curso seleccionado que sin.</span><span class="sxs-lookup"><span data-stu-id="bfb49-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="bfb49-198">El código se repite `CourseAssignments` y `Course` porque tiene dos propiedades de `Course`.</span><span class="sxs-lookup"><span data-stu-id="bfb49-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="bfb49-199">La primera cadena de `ThenInclude` llama Obtiene `CourseAssignment.Course`, `Course.Enrollments`, y `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="bfb49-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="bfb49-200">En ese momento en el código, otra `ThenInclude` sería para las propiedades de navegación de `Student`, que no es necesario.</span><span class="sxs-lookup"><span data-stu-id="bfb49-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="bfb49-201">Pero la llamada a `Include` inicia sobre con `Instructor` propiedades, por lo que tiene que pasar a través de la cadena de nuevo, esta vez especificando `Course.Department` en lugar de `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="bfb49-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="bfb49-202">El siguiente código se ejecuta cuando se ha seleccionado un instructor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="bfb49-203">El instructor seleccionado se recupera de la lista de instructores en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="bfb49-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="bfb49-204">El modelo de vista `Courses` propiedad, a continuación, se carga con las entidades de curso de esa instructor `CourseAssignments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="bfb49-205">El `Where` método devuelve una colección, pero en este caso los criterios se pasan a ese método generan solo en una única entidad Instructor devolverse.</span><span class="sxs-lookup"><span data-stu-id="bfb49-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="bfb49-206">El `Single` método convierte la colección en una única entidad Instructor, que proporciona acceso a esa entidad `CourseAssignments` propiedad.</span><span class="sxs-lookup"><span data-stu-id="bfb49-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="bfb49-207">El `CourseAssignments` propiedad contiene `CourseAssignment` entidades, que se desea solo relacionado `Course` entidades.</span><span class="sxs-lookup"><span data-stu-id="bfb49-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="bfb49-208">Usa el `Single` método en una colección cuando se sabe que la colección tiene un único elemento.</span><span class="sxs-lookup"><span data-stu-id="bfb49-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="bfb49-209">El único método produce una excepción si la colección pasada a la está vacía o si hay más de un elemento.</span><span class="sxs-lookup"><span data-stu-id="bfb49-209">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="bfb49-210">Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (null, en este caso) si la colección está vacía.</span><span class="sxs-lookup"><span data-stu-id="bfb49-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="bfb49-211">Sin embargo, en este caso que todavía produciría una excepción (de tratar de buscar un `Courses` propiedad en una referencia nula), y el mensaje de excepción menos claramente indicaría la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="bfb49-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="bfb49-212">Cuando se llama a la `Single` método, también puede pasar la donde condición en lugar de llamar el `Where` método por separado:</span><span class="sxs-lookup"><span data-stu-id="bfb49-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="bfb49-213">En lugar de:</span><span class="sxs-lookup"><span data-stu-id="bfb49-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="bfb49-214">A continuación, si se ha seleccionado un curso, el curso seleccionado se recupera de la lista de cursos en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="bfb49-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="bfb49-215">A continuación, el modelo de vista `Enrollments` propiedad se carga con las entidades de inscripción de dicho curso `Enrollments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="bfb49-216">Modificar la vista de índice Instructor</span><span class="sxs-lookup"><span data-stu-id="bfb49-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="bfb49-217">En *Views/Instructors/Index.cshtml*, reemplace el código de plantilla con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="bfb49-218">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="bfb49-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="bfb49-219">Ha realizado los siguientes cambios en el código existente:</span><span class="sxs-lookup"><span data-stu-id="bfb49-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="bfb49-220">Cambiar la clase de modelo para `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="bfb49-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="bfb49-221">Cambiar el título de la página de **índice** a **instructores**.</span><span class="sxs-lookup"><span data-stu-id="bfb49-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="bfb49-222">Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null.</span><span class="sxs-lookup"><span data-stu-id="bfb49-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="bfb49-223">(Dado que se trata de una relación de uno a cero o uno, no habrá una entidad OfficeAssignment relacionada.)</span><span class="sxs-lookup"><span data-stu-id="bfb49-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="bfb49-224">Agrega un **cursos** columna que muestra los cursos se imparte por cada instructor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="bfb49-225">Vea [transición línea explícita con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información acerca de esta sintaxis razor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="bfb49-226">Ha agregado código que agrega dinámicamente `class="success"` a la `tr` elemento del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="bfb49-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="bfb49-227">Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.</span><span class="sxs-lookup"><span data-stu-id="bfb49-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="bfb49-228">Agrega un nuevo hipervínculo con la etiqueta **seleccione** inmediatamente antes de los otros vínculos en cada fila, lo que hace que Id. del instructor seleccionado para enviarse a la `Index` método.</span><span class="sxs-lookup"><span data-stu-id="bfb49-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="bfb49-229">Ejecute la aplicación y seleccione la **instructores** ficha. La página muestra la propiedad de ubicación de las entidades relacionadas de OfficeAssignment y una celda de tabla vacía cuando no hay ninguna entidad OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="bfb49-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Página de índice de instructores que no hay nada seleccionado](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="bfb49-231">En el *Views/Instructors/Index.cshtml* archivo, después de que el cierre de la tabla elemento (situado al final del archivo), agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="bfb49-232">Este código muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="bfb49-233">Este código lee el `Courses` propiedad del modelo de vista para mostrar una lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="bfb49-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="bfb49-234">También proporciona un **seleccione** hipervínculo que envía el Id. del curso seleccionado para la `Index` método de acción.</span><span class="sxs-lookup"><span data-stu-id="bfb49-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="bfb49-235">Actualice la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="bfb49-236">Ahora verá una cuadrícula que muestra los cursos asignados al instructor seleccionado, y para cada curso verá el nombre del departamento asignado.</span><span class="sxs-lookup"><span data-stu-id="bfb49-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instructor de página de índice de instructores seleccionado](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="bfb49-238">Después del bloque de código que acaba de agregar, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bfb49-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="bfb49-239">Esto muestra una lista de los estudiantes que están inscritos en un curso cuando se selecciona dicho curso.</span><span class="sxs-lookup"><span data-stu-id="bfb49-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="bfb49-240">Este código lee la propiedad de las inscripciones del modelo de vista para mostrar una lista de estudiantes inscritos en el curso.</span><span class="sxs-lookup"><span data-stu-id="bfb49-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="bfb49-241">Actualice la página de nuevo y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="bfb49-242">A continuación, seleccione un curso para ver la lista de estudiantes inscritos y sus calificaciones.</span><span class="sxs-lookup"><span data-stu-id="bfb49-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instructor de página de índice de instructores y curso seleccionado](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="bfb49-244">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="bfb49-244">Explicit loading</span></span>

<span data-ttu-id="bfb49-245">Cuando se recupera la lista de instructores en *InstructorsController.cs*, especifica una carga diligente de la `CourseAssignments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="bfb49-246">Suponga que esperaba a los usuarios apenas desea ver las inscripciones en un instructor seleccionado y en curso.</span><span class="sxs-lookup"><span data-stu-id="bfb49-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="bfb49-247">En ese caso, puede cargar los datos de inscripción sólo si se solicita.</span><span class="sxs-lookup"><span data-stu-id="bfb49-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="bfb49-248">Para ver un ejemplo de cómo realizar la carga explícita, reemplace la `Index` método con el código siguiente, que quita carga diligente de inscripciones y los carga explícitamente esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="bfb49-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="bfb49-249">Se resaltan los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="bfb49-249">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="bfb49-250">El nuevo código quita la *ThenInclude* método se llama para los datos de inscripción desde el código que recupera entidades instructor.</span><span class="sxs-lookup"><span data-stu-id="bfb49-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="bfb49-251">Si se seleccionan un instructor y el curso, el código resaltado recupera entidades de inscripción para el curso seleccionado y entidades de estudiante para cada inscripción.</span><span class="sxs-lookup"><span data-stu-id="bfb49-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="bfb49-252">Ejecute que la aplicación, vaya a la página de índice de instructores ahora y no se verá ninguna diferencia en lo que se muestra en la página, aunque ha cambiado la forma en que se recuperan los datos.</span><span class="sxs-lookup"><span data-stu-id="bfb49-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="bfb49-253">Resumen</span><span class="sxs-lookup"><span data-stu-id="bfb49-253">Summary</span></span>

<span data-ttu-id="bfb49-254">Ahora que ha utilizado carga diligente con una consulta y con varias consultas para leer los datos relacionados en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="bfb49-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="bfb49-255">En el siguiente tutorial, aprenderá cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="bfb49-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="bfb49-256">[Anterior](complex-data-model.md)
>[Siguiente](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="bfb49-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
