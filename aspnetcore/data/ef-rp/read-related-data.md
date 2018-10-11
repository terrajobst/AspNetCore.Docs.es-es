---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)'
author: rick-anderson
description: En este tutorial, leerá y mostrará datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: f57853fae7fb1cb7df130f38a6996c87a5c08e83
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523290"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="0b71d-103">Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="0b71d-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="0b71d-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0b71d-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="0b71d-105">En este tutorial, se leen y se muestran datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0b71d-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="0b71d-106">Los datos relacionados son los que EF Core carga en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="0b71d-107">Si experimenta problemas que no puede resolver, [descargue o vea la aplicación completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="0b71d-107">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="0b71d-108">[Instrucciones de descarga](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0b71d-108">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

<span data-ttu-id="0b71d-109">En las ilustraciones siguientes se muestran las páginas completadas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0b71d-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="0b71d-112">Carga diligente, explícita y diferida de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="0b71d-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="0b71d-113">EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="0b71d-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="0b71d-114">[Carga diligente](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="0b71d-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="0b71d-115">La carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="0b71d-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="0b71d-116">Cuando se lee la entidad, se recuperan sus datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0b71d-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="0b71d-117">Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan.</span><span class="sxs-lookup"><span data-stu-id="0b71d-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="0b71d-118">EF Core emitirá varias consultas para algunos tipos de carga diligente.</span><span class="sxs-lookup"><span data-stu-id="0b71d-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="0b71d-119">La emisión de varias consultas puede ser más eficaz de lo que eran algunas consultas de EF6 cuando había una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="0b71d-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="0b71d-120">La carga diligente se especifica con los métodos `Include` y `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Ejemplo de carga diligente](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="0b71d-122">La carga diligente envía varias consultas cuando se incluye una propiedad de navegación de colección:</span><span class="sxs-lookup"><span data-stu-id="0b71d-122">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="0b71d-123">Una consulta para la consulta principal</span><span class="sxs-lookup"><span data-stu-id="0b71d-123">One query for the main query</span></span> 
  * <span data-ttu-id="0b71d-124">Una consulta para cada colección "perimetral" en el árbol de la carga.</span><span class="sxs-lookup"><span data-stu-id="0b71d-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="0b71d-125">Separe las consultas con `Load`: los datos se pueden recuperar en distintas consultas y EF Core "corrige" las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="0b71d-126">"Corregir" significa que EF Core rellena automáticamente las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="0b71d-127">Separar las consultas con `Load` es más parecido a la carga explícita que a la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="0b71d-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Ejemplo de consultas independientes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="0b71d-129">Nota: EF Core corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="0b71d-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="0b71d-130">Incluso si los datos de una propiedad de navegación *no* se incluyen explícitamente, es posible que la propiedad se siga rellenando si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="0b71d-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="0b71d-131">[Carga explícita](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="0b71d-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="0b71d-132">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0b71d-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="0b71d-133">Se debe escribir código para recuperar los datos relacionados cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="0b71d-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="0b71d-134">La carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0b71d-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="0b71d-135">Con la carga explícita, el código especifica las propiedades de navegación que se van a cargar.</span><span class="sxs-lookup"><span data-stu-id="0b71d-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="0b71d-136">Use el método `Load` para realizar la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="0b71d-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="0b71d-137">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0b71d-137">For example:</span></span>

  ![Ejemplo de carga explícita](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="0b71d-139">[Carga diferida](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="0b71d-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="0b71d-140">[Actualmente EF Core no admite la carga diferida](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="0b71d-140">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="0b71d-141">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0b71d-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="0b71d-142">La primera vez que se obtiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="0b71d-143">Cada vez que se obtiene acceso a una propiedad de navegación, se envía una consulta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0b71d-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="0b71d-144">El operador `Select` solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="0b71d-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="0b71d-145">Crear una página de cursos en la que se muestre el nombre de departamento</span><span class="sxs-lookup"><span data-stu-id="0b71d-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="0b71d-146">La entidad Course incluye una propiedad de navegación que contiene la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="0b71d-147">La entidad `Department` contiene el departamento al que se asigna el curso.</span><span class="sxs-lookup"><span data-stu-id="0b71d-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="0b71d-148">Para mostrar el nombre del departamento asignado en una lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="0b71d-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="0b71d-149">Obtenga la propiedad `Name` desde la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="0b71d-150">La entidad `Department` procede de la propiedad de navegación `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![course.Departament](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="0b71d-152">Aplicar scaffolding al modelo de Course</span><span class="sxs-lookup"><span data-stu-id="0b71d-152">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b71d-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b71d-153">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0b71d-154">Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Course` para la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="0b71d-154">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0b71d-155">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0b71d-155">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="0b71d-156">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="0b71d-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

------

<span data-ttu-id="0b71d-157">El comando anterior aplica scaffolding al modelo `Course`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-157">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="0b71d-158">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0b71d-158">Open the project in Visual Studio.</span></span>

<span data-ttu-id="0b71d-159">Abra *Pages/Courses/Index.cshtml.cs* y examine el método `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="0b71d-160">El motor de scaffolding especificado realiza la carga diligente de la propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="0b71d-161">El método `Include` especifica la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="0b71d-161">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="0b71d-162">Ejecute la aplicación y haga clic en el vínculo **Courses**.</span><span class="sxs-lookup"><span data-stu-id="0b71d-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="0b71d-163">En la columna Department se muestra el `DepartmentID`, lo que no resulta útil.</span><span class="sxs-lookup"><span data-stu-id="0b71d-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="0b71d-164">Actualice el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-164">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="0b71d-165">El código anterior agrega `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-165">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="0b71d-166">`AsNoTracking` mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas.</span><span class="sxs-lookup"><span data-stu-id="0b71d-166">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="0b71d-167">No se realiza el seguimiento de las entidades porque no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="0b71d-167">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="0b71d-168">Actualice *Pages/Courses/Index.cshtml* con el marcado resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-168">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="0b71d-169">Se han realizado los cambios siguientes en el código con scaffolding:</span><span class="sxs-lookup"><span data-stu-id="0b71d-169">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="0b71d-170">Ha cambiado el título de Index a Courses.</span><span class="sxs-lookup"><span data-stu-id="0b71d-170">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="0b71d-171">Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-171">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="0b71d-172">De forma predeterminada, las claves principales no tienen scaffolding porque normalmente no tienen sentido para los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="0b71d-172">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="0b71d-173">Pero en este caso, la clave principal es significativa.</span><span class="sxs-lookup"><span data-stu-id="0b71d-173">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="0b71d-174">Ha cambiado la columna **Department** para mostrar el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="0b71d-174">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="0b71d-175">El código muestra la propiedad `Name` de la entidad `Department` que se carga en la propiedad de navegación `Department`:</span><span class="sxs-lookup"><span data-stu-id="0b71d-175">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="0b71d-176">Ejecute la aplicación y haga clic en la pestaña **Courses** para ver la lista con los nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="0b71d-176">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="0b71d-178">Carga de datos relacionados con Select</span><span class="sxs-lookup"><span data-stu-id="0b71d-178">Loading related data with Select</span></span>

<span data-ttu-id="0b71d-179">El método `OnGetAsync` carga los datos relacionados con el método `Include`:</span><span class="sxs-lookup"><span data-stu-id="0b71d-179">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="0b71d-180">El operador `Select` solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="0b71d-180">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="0b71d-181">Para elementos individuales, como el `Department.Name`, se usa INNER JOIN de SQL.</span><span class="sxs-lookup"><span data-stu-id="0b71d-181">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="0b71d-182">En las colecciones, se usa otro acceso de base de datos, como también hace el operador `Include` en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="0b71d-182">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="0b71d-183">En el código siguiente se cargan los datos relacionados con el método `Select`:</span><span class="sxs-lookup"><span data-stu-id="0b71d-183">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="0b71d-184">El `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="0b71d-184">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="0b71d-185">Vea [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="0b71d-185">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="0b71d-186">Crear una página de instructores en la que se muestran los cursos y las inscripciones</span><span class="sxs-lookup"><span data-stu-id="0b71d-186">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="0b71d-187">En esta sección, se crea la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="0b71d-187">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="0b71d-188">![Página de índice de instructores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="0b71d-188">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="0b71d-189">En esta página se leen y muestran los datos relacionados de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="0b71d-189">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="0b71d-190">En la lista de instructores se muestran datos relacionados de la entidad `OfficeAssignment` (Office en la imagen anterior).</span><span class="sxs-lookup"><span data-stu-id="0b71d-190">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="0b71d-191">Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="0b71d-191">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="0b71d-192">Para las entidades `OfficeAssignment` se usa la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="0b71d-192">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="0b71d-193">Normalmente la carga diligente es más eficaz cuando es necesario mostrar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0b71d-193">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="0b71d-194">En este caso, se muestran las asignaciones de oficina para los instructores.</span><span class="sxs-lookup"><span data-stu-id="0b71d-194">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="0b71d-195">Cuando el usuario selecciona un instructor (Harui en la imagen anterior), se muestran las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="0b71d-195">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="0b71d-196">Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0b71d-196">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="0b71d-197">La carga diligente se usa con las entidades `Course` y sus entidades `Department` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="0b71d-197">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="0b71d-198">En este caso, es posible que las consultas independientes sean más eficaces porque solo se necesitan cursos para el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0b71d-198">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="0b71d-199">En este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación en entidades que se encuentran en propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-199">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="0b71d-200">Cuando el usuario selecciona un curso (Chemistry [Química] en la imagen anterior), se muestran los datos relacionados de la entidad `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-200">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="0b71d-201">En la imagen anterior, se muestra el nombre del alumno y la calificación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-201">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="0b71d-202">Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="0b71d-202">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="0b71d-203">Crear un modelo de vista para la vista de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="0b71d-203">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="0b71d-204">En la página Instructors se muestran datos de tres tablas diferentes.</span><span class="sxs-lookup"><span data-stu-id="0b71d-204">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="0b71d-205">Se crea un modelo de vista que incluye las tres entidades que representan las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="0b71d-205">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="0b71d-206">En la carpeta *SchoolViewModels*, cree *InstructorIndexData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-206">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="0b71d-207">Aplicar scaffolding al modelo de Instructor</span><span class="sxs-lookup"><span data-stu-id="0b71d-207">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b71d-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b71d-208">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0b71d-209">Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Instructor` para la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="0b71d-209">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0b71d-210">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0b71d-210">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="0b71d-211">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="0b71d-211">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

------

<span data-ttu-id="0b71d-212">El comando anterior aplica scaffolding al modelo `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-212">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="0b71d-213">Ejecute la aplicación y vaya a la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="0b71d-213">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="0b71d-214">Reemplace *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-214">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="0b71d-215">El método `OnGetAsync` acepta datos de ruta opcionales para el identificador del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0b71d-215">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="0b71d-216">Examine la consulta en el archivo *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b71d-216">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="0b71d-217">La consulta tiene dos instrucciones include:</span><span class="sxs-lookup"><span data-stu-id="0b71d-217">The query has two includes:</span></span>

* <span data-ttu-id="0b71d-218">`OfficeAssignment`: se muestra en la [vista de instructores](#IP).</span><span class="sxs-lookup"><span data-stu-id="0b71d-218">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="0b71d-219">`CourseAssignments`: muestra los cursos impartidos.</span><span class="sxs-lookup"><span data-stu-id="0b71d-219">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="0b71d-220">Actualizar la página de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="0b71d-220">Update the instructors Index page</span></span>

<span data-ttu-id="0b71d-221">Actualice *Pages/Instructors/Index.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-221">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="0b71d-222">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="0b71d-222">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="0b71d-223">Se actualiza la directiva `page` de `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-223">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="0b71d-224">`"{id:int?}"` es una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="0b71d-224">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="0b71d-225">La plantilla de ruta cambia las cadenas de consulta enteras de la dirección URL por datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="0b71d-225">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="0b71d-226">Por ejemplo, al hacer clic en el vínculo **Select** de un instructor con únicamente la directiva `@page`, se genera una dirección URL similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-226">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="0b71d-227">Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL anterior es:</span><span class="sxs-lookup"><span data-stu-id="0b71d-227">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="0b71d-228">El título de página es **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="0b71d-228">Page title is **Instructors**.</span></span>
* <span data-ttu-id="0b71d-229">Se ha agregado una columna **Office** en la que se muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es NULL.</span><span class="sxs-lookup"><span data-stu-id="0b71d-229">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="0b71d-230">Dado que se trata de una relación de uno a cero o uno, es posible que no haya una entidad OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="0b71d-230">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="0b71d-231">Se ha agregado una columna **Courses** en la que se muestran los cursos que imparte cada instructor.</span><span class="sxs-lookup"><span data-stu-id="0b71d-231">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="0b71d-232">Vea [Transición de línea explícita con `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información sobre esta sintaxis de Razor.</span><span class="sxs-lookup"><span data-stu-id="0b71d-232">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="0b71d-233">Ha agregado código que agrega dinámicamente `class="success"` al elemento `tr` del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0b71d-233">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="0b71d-234">Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.</span><span class="sxs-lookup"><span data-stu-id="0b71d-234">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="0b71d-235">Se ha agregado un hipervínculo nuevo con la etiqueta **Select**.</span><span class="sxs-lookup"><span data-stu-id="0b71d-235">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="0b71d-236">Este vínculo envía el identificador del instructor seleccionado al método `Index` y establece un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="0b71d-236">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="0b71d-237">Ejecute la aplicación y haga clic en la pestaña **Instructors**. En la página se muestra la `Location` (oficina) de la entidad `OfficeAssignment` relacionada.</span><span class="sxs-lookup"><span data-stu-id="0b71d-237">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="0b71d-238">Si OfficeAssignment es NULL, se muestra una celda de tabla vacía.</span><span class="sxs-lookup"><span data-stu-id="0b71d-238">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Página de índice de instructores sin ninguna selección](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="0b71d-240">Haga clic en el vínculo **Select**.</span><span class="sxs-lookup"><span data-stu-id="0b71d-240">Click on the **Select** link.</span></span> <span data-ttu-id="0b71d-241">El estilo de la fila cambia.</span><span class="sxs-lookup"><span data-stu-id="0b71d-241">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="0b71d-242">Agregar cursos impartidos por el instructor seleccionado</span><span class="sxs-lookup"><span data-stu-id="0b71d-242">Add courses taught by selected instructor</span></span>

<span data-ttu-id="0b71d-243">Actualice el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-243">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="0b71d-244">Agregue `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="0b71d-244">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="0b71d-245">Examine la consulta actualizada:</span><span class="sxs-lookup"><span data-stu-id="0b71d-245">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="0b71d-246">En la consulta anterior se agregan las entidades `Department`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-246">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="0b71d-247">El código siguiente se ejecuta cuando se selecciona un instructor (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="0b71d-247">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="0b71d-248">El instructor seleccionado se recupera de la lista de instructores del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="0b71d-248">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="0b71d-249">Se carga la propiedad `Courses` del modelo de vista con las entidades `Course` de la propiedad de navegación `CourseAssignments` de ese instructor.</span><span class="sxs-lookup"><span data-stu-id="0b71d-249">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="0b71d-250">El método `Where` devuelve una colección.</span><span class="sxs-lookup"><span data-stu-id="0b71d-250">The `Where` method returns a collection.</span></span> <span data-ttu-id="0b71d-251">En el método `Where` anterior, solo se devuelve una entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-251">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="0b71d-252">El método `Single` convierte la colección en una sola entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-252">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="0b71d-253">La entidad `Instructor` proporciona acceso a la propiedad `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-253">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="0b71d-254">`CourseAssignments` proporciona acceso a las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="0b71d-254">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="0b71d-256">El método `Single` se usa en una colección cuando la colección tiene un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="0b71d-256">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="0b71d-257">El método `Single` inicia una excepción si la colección está vacía o hay más de un elemento.</span><span class="sxs-lookup"><span data-stu-id="0b71d-257">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="0b71d-258">Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (NULL, en este caso) si la colección está vacía.</span><span class="sxs-lookup"><span data-stu-id="0b71d-258">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="0b71d-259">El uso de `SingleOrDefault` en una colección vacía:</span><span class="sxs-lookup"><span data-stu-id="0b71d-259">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="0b71d-260">Inicia una excepción (al tratar de buscar una propiedad `Courses` en una referencia nula).</span><span class="sxs-lookup"><span data-stu-id="0b71d-260">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="0b71d-261">El mensaje de excepción indicará con menos claridad la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="0b71d-261">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="0b71d-262">El código siguiente rellena la propiedad `Enrollments` del modelo de vista cuando se selecciona un curso:</span><span class="sxs-lookup"><span data-stu-id="0b71d-262">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="0b71d-263">Agregue el siguiente marcado al final de la página de Razor *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b71d-263">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="0b71d-264">En el marcado anterior se muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.</span><span class="sxs-lookup"><span data-stu-id="0b71d-264">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="0b71d-265">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-265">Test the app.</span></span> <span data-ttu-id="0b71d-266">Haga clic en un vínculo **Select** en la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="0b71d-266">Click on a **Select** link on the instructors page.</span></span>

![Instructor seleccionado en la página de índice de instructores](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="0b71d-268">Mostrar datos de estudiante</span><span class="sxs-lookup"><span data-stu-id="0b71d-268">Show student data</span></span>

<span data-ttu-id="0b71d-269">En esta sección, la aplicación se actualiza para mostrar los datos de estudiante para un curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0b71d-269">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="0b71d-270">Actualice la consulta en el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-270">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="0b71d-271">Actualice *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0b71d-271">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="0b71d-272">Agregue el marcado siguiente al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="0b71d-272">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="0b71d-273">En el marcado anterior se muestra una lista de los estudiantes que están inscritos en el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0b71d-273">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="0b71d-274">Actualice la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="0b71d-274">Refresh the page and select an instructor.</span></span> <span data-ttu-id="0b71d-275">Seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.</span><span class="sxs-lookup"><span data-stu-id="0b71d-275">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructor y curso seleccionados en la página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="0b71d-277">Uso de Single</span><span class="sxs-lookup"><span data-stu-id="0b71d-277">Using Single</span></span>

<span data-ttu-id="0b71d-278">Se puede pasar el método `Single` en la condición `Where` en lugar de llamar al método `Where` por separado:</span><span class="sxs-lookup"><span data-stu-id="0b71d-278">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="0b71d-279">El enfoque de `Single` anterior no ofrece ninguna ventaja con respecto a `Where`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-279">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="0b71d-280">Algunos desarrolladores prefieren el estilo del enfoque de `Single`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-280">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="0b71d-281">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="0b71d-281">Explicit loading</span></span>

<span data-ttu-id="0b71d-282">En el código actual se especifica la carga diligente para `Enrollments` y `Students`:</span><span class="sxs-lookup"><span data-stu-id="0b71d-282">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="0b71d-283">Imagine que los usuarios rara vez querrán ver las inscripciones en un curso.</span><span class="sxs-lookup"><span data-stu-id="0b71d-283">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="0b71d-284">En ese caso, una optimización sería cargar solamente los datos de inscripción si se solicitan.</span><span class="sxs-lookup"><span data-stu-id="0b71d-284">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="0b71d-285">En esta sección, se actualiza `OnGetAsync` para usar la carga explícita de `Enrollments` y `Students`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-285">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="0b71d-286">Actualice `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-286">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="0b71d-287">En el código anterior se quitan las llamadas al método *ThenInclude* para los datos de inscripción y estudiantes.</span><span class="sxs-lookup"><span data-stu-id="0b71d-287">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="0b71d-288">Si se selecciona un curso, el código resaltado recupera lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b71d-288">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="0b71d-289">Las entidades `Enrollment` para el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0b71d-289">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="0b71d-290">Las entidades `Student` para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="0b71d-290">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="0b71d-291">Tenga en cuenta que en el código anterior `.AsNoTracking()` se convierte en comentario.</span><span class="sxs-lookup"><span data-stu-id="0b71d-291">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="0b71d-292">Las propiedades de navegación solo se pueden cargar explícitamente para las entidades sometidas a seguimiento.</span><span class="sxs-lookup"><span data-stu-id="0b71d-292">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="0b71d-293">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b71d-293">Test the app.</span></span> <span data-ttu-id="0b71d-294">Desde la perspectiva de los usuarios, la aplicación se comporta exactamente igual a la versión anterior.</span><span class="sxs-lookup"><span data-stu-id="0b71d-294">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="0b71d-295">En el siguiente tutorial se muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0b71d-295">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="0b71d-296">[Anterior](xref:data/ef-rp/complex-data-model)
>[Siguiente](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="0b71d-296">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
