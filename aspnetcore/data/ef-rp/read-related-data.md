---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)'
author: tdykstra
description: En este tutorial, leerá y mostrará datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: f53f8edef7fe8690d0e414bc094d81dc99ad198a
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082089"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="16dbd-103">Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="16dbd-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="16dbd-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16dbd-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="16dbd-105">En este tutorial se muestra cómo leer y mostrar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="16dbd-106">Los datos relacionados son los que EF Core carga en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="16dbd-107">En las ilustraciones siguientes se muestran las páginas completadas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="16dbd-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index30.png)

![Página de índice de instructores](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="16dbd-110">Carga diligente, explícita y diferida</span><span class="sxs-lookup"><span data-stu-id="16dbd-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="16dbd-111">EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="16dbd-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="16dbd-112">[Carga diligente](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="16dbd-113">La carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="16dbd-114">Cuando se lee una entidad, se recuperan sus datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="16dbd-115">Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan.</span><span class="sxs-lookup"><span data-stu-id="16dbd-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="16dbd-116">EF Core emitirá varias consultas para algunos tipos de carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="16dbd-117">La emisión de varias consultas puede ser más eficaz que una sola consulta gigante.</span><span class="sxs-lookup"><span data-stu-id="16dbd-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="16dbd-118">La carga diligente se especifica con los métodos `Include` y `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Ejemplo de carga diligente](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="16dbd-120">La carga diligente envía varias consultas cuando se incluye una propiedad de navegación de colección:</span><span class="sxs-lookup"><span data-stu-id="16dbd-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="16dbd-121">Una consulta para la consulta principal</span><span class="sxs-lookup"><span data-stu-id="16dbd-121">One query for the main query</span></span> 
  * <span data-ttu-id="16dbd-122">Una consulta para cada colección "perimetral" en el árbol de la carga.</span><span class="sxs-lookup"><span data-stu-id="16dbd-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="16dbd-123">Separe las consultas con `Load`: los datos se pueden recuperar en distintas consultas y EF Core "corrige" las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="16dbd-124">"Corregir" significa que EF Core rellena de forma automática las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="16dbd-125">La separación de las consultas con `Load` es más parecido a la carga explícita que a la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Ejemplo de consultas independientes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="16dbd-127">Nota: EF Core corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="16dbd-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="16dbd-128">Incluso si los datos de una propiedad de navegación *no* se incluyen explícitamente, es posible que la propiedad se siga rellenando si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="16dbd-129">[Carga explícita](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="16dbd-130">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="16dbd-131">Se debe escribir código para recuperar los datos relacionados cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="16dbd-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="16dbd-132">La carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16dbd-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="16dbd-133">Con la carga explícita, el código especifica las propiedades de navegación que se van a cargar.</span><span class="sxs-lookup"><span data-stu-id="16dbd-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="16dbd-134">Use el método `Load` para realizar la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="16dbd-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="16dbd-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="16dbd-135">For example:</span></span>

  ![Ejemplo de carga explícita](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="16dbd-137">[Carga diferida](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="16dbd-138">[Se ha agregado la carga diferida a EF Core en la versión 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="16dbd-139">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="16dbd-140">La primera vez que se obtiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="16dbd-141">Cada vez que se accede por primera vez a una propiedad de navegación, se envía una consulta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16dbd-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="16dbd-142">Creación de las páginas Course</span><span class="sxs-lookup"><span data-stu-id="16dbd-142">Create Course pages</span></span>

<span data-ttu-id="16dbd-143">La entidad `Course` incluye una propiedad de navegación que contiene la entidad `Department` relacionada.</span><span class="sxs-lookup"><span data-stu-id="16dbd-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<span data-ttu-id="16dbd-145">Para mostrar el nombre del departamento asignado a un curso:</span><span class="sxs-lookup"><span data-stu-id="16dbd-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="16dbd-146">Cargue la entidad `Department` relacionada en la propiedad de navegación `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="16dbd-147">Obtenga el nombre de la propiedad `Name` de la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="16dbd-148">Scaffolding de las páginas Course</span><span class="sxs-lookup"><span data-stu-id="16dbd-148">Scaffold Course pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="16dbd-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16dbd-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="16dbd-150">Siga las instrucciones de [Scaffolding de las páginas Student](xref:data/ef-rp/intro#scaffold-student-pages) con las siguientes excepciones:</span><span class="sxs-lookup"><span data-stu-id="16dbd-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="16dbd-151">Cree una carpeta *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="16dbd-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="16dbd-152">Use `Course` para la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="16dbd-153">Use la clase de contexto existente en lugar de crear una.</span><span class="sxs-lookup"><span data-stu-id="16dbd-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="16dbd-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16dbd-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="16dbd-155">Cree una carpeta *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="16dbd-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="16dbd-156">Ejecute el comando siguiente para aplicar scaffolding a las páginas Course.</span><span class="sxs-lookup"><span data-stu-id="16dbd-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="16dbd-157">**En Windows:**</span><span class="sxs-lookup"><span data-stu-id="16dbd-157">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="16dbd-158">**En Linux o macOS:**</span><span class="sxs-lookup"><span data-stu-id="16dbd-158">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="16dbd-159">Abra *Pages/Courses/Index.cshtml.cs* y examine el método `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="16dbd-160">El motor de scaffolding especificado realiza la carga diligente de la propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="16dbd-161">El método `Include` especifica la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="16dbd-162">Ejecute la aplicación y haga clic en el vínculo **Courses**.</span><span class="sxs-lookup"><span data-stu-id="16dbd-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="16dbd-163">En la columna Department se muestra el `DepartmentID`, lo que no resulta útil.</span><span class="sxs-lookup"><span data-stu-id="16dbd-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="16dbd-164">Representación del nombre del departamento</span><span class="sxs-lookup"><span data-stu-id="16dbd-164">Display the department name</span></span>

<span data-ttu-id="16dbd-165">Actualice Pages/Courses/Index.cshtml.cs con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="16dbd-166">En el código anterior se cambia la propiedad `Course` a `Courses` y se agrega `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="16dbd-167">`AsNoTracking` mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="16dbd-168">No es necesario realizar el seguimiento de las entidades porque no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="16dbd-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="16dbd-169">Actualice *Pages/Courses/Index.cshtml* con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="16dbd-170">Se han realizado los cambios siguientes en el código con scaffolding:</span><span class="sxs-lookup"><span data-stu-id="16dbd-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="16dbd-171">Se ha cambiado el nombre de la propiedad `Course` a `Courses`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="16dbd-172">Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="16dbd-173">De forma predeterminada, las claves principales no tienen scaffolding porque normalmente no tienen sentido para los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="16dbd-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="16dbd-174">Pero en este caso, la clave principal es significativa.</span><span class="sxs-lookup"><span data-stu-id="16dbd-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="16dbd-175">Ha cambiado la columna **Department** para mostrar el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="16dbd-176">El código muestra la propiedad `Name` de la entidad `Department` que se carga en la propiedad de navegación `Department`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="16dbd-177">Ejecute la aplicación y haga clic en la pestaña **Courses** para ver la lista con los nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="16dbd-179">Carga de datos relacionados con Select</span><span class="sxs-lookup"><span data-stu-id="16dbd-179">Loading related data with Select</span></span>

<span data-ttu-id="16dbd-180">El método `OnGetAsync` carga los datos relacionados con el método `Include`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="16dbd-181">El método `Select` es una alternativa que solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="16dbd-182">Para elementos individuales, como el `Department.Name`, se usa INNER JOIN de SQL.</span><span class="sxs-lookup"><span data-stu-id="16dbd-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="16dbd-183">En las colecciones, se usa otro acceso de base de datos, como también hace el operador `Include` en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="16dbd-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="16dbd-184">En el código siguiente se cargan los datos relacionados con el método `Select`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="16dbd-185">El `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="16dbd-186">Vea [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-186">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="16dbd-187">Creación de las páginas Instructor</span><span class="sxs-lookup"><span data-stu-id="16dbd-187">Create Instructor pages</span></span>

<span data-ttu-id="16dbd-188">Esta sección se aplica scaffolding a las páginas Instructor y se agregan entidades Course y Enrollment a la página Instructors Index (índice de instructores).</span><span class="sxs-lookup"><span data-stu-id="16dbd-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="16dbd-189">![Página de índice de instructores](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="16dbd-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="16dbd-190">En esta página se leen y muestran los datos relacionados de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dbd-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="16dbd-191">En la lista de instructores se muestran datos relacionados de la entidad `OfficeAssignment` (Office en la imagen anterior).</span><span class="sxs-lookup"><span data-stu-id="16dbd-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="16dbd-192">Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="16dbd-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="16dbd-193">Para las entidades `OfficeAssignment` se usa la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="16dbd-194">Normalmente la carga diligente es más eficaz cuando es necesario mostrar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="16dbd-195">En este caso, se muestran las asignaciones de oficina para los instructores.</span><span class="sxs-lookup"><span data-stu-id="16dbd-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="16dbd-196">Cuando el usuario selecciona un instructor, se muestran las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="16dbd-197">Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="16dbd-198">La carga diligente se usa con las entidades `Course` y sus entidades `Department` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="16dbd-199">En este caso, es posible que las consultas independientes sean más eficaces porque solo se necesitan cursos para el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="16dbd-200">En este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación en entidades que se encuentran en propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="16dbd-201">Cuando el usuario selecciona un curso, se muestran los datos relacionados de la entidad `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="16dbd-202">En la imagen anterior, se muestra el nombre del alumno y la calificación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="16dbd-203">Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="16dbd-204">Creación de un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="16dbd-204">Create a view model</span></span>

<span data-ttu-id="16dbd-205">En la página Instructors se muestran datos de tres tablas diferentes.</span><span class="sxs-lookup"><span data-stu-id="16dbd-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="16dbd-206">Se necesita un modelo de vista que incluya tres entidades que representen las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="16dbd-207">Cree *SchoolViewModels/InstructorIndexData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="16dbd-208">Scaffolding de las páginas Instructor</span><span class="sxs-lookup"><span data-stu-id="16dbd-208">Scaffold Instructor pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="16dbd-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16dbd-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="16dbd-210">Siga las instrucciones de [Scaffolding de las páginas Student](xref:data/ef-rp/intro#scaffold-student-pages) con las siguientes excepciones:</span><span class="sxs-lookup"><span data-stu-id="16dbd-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="16dbd-211">Cree una carpeta *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="16dbd-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="16dbd-212">Use `Instructor` para la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="16dbd-213">Use la clase de contexto existente en lugar de crear una.</span><span class="sxs-lookup"><span data-stu-id="16dbd-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="16dbd-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16dbd-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="16dbd-215">Cree una carpeta *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="16dbd-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="16dbd-216">Ejecute el comando siguiente para aplicar scaffolding a las páginas Instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="16dbd-217">**En Windows:**</span><span class="sxs-lookup"><span data-stu-id="16dbd-217">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="16dbd-218">**En Linux o macOS:**</span><span class="sxs-lookup"><span data-stu-id="16dbd-218">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="16dbd-219">Para ver el aspecto de la página con scaffolding antes de actualizarla, ejecute la aplicación y vaya a la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="16dbd-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="16dbd-220">Actualice *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="16dbd-221">El método `OnGetAsync` acepta datos de ruta opcionales para el identificador del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="16dbd-222">Examine la consulta en el archivo *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="16dbd-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="16dbd-223">El código especifica la carga diligente para las propiedades de navegación siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dbd-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="16dbd-224">Observe la repetición de los métodos `Include` y `ThenInclude` para `CourseAssignments` y `Course`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="16dbd-225">Esta repetición es necesaria para especificar la carga diligente para dos propiedades de navegación de la entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="16dbd-226">El código siguiente se ejecuta cuando se selecciona un instructor (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="16dbd-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="16dbd-227">El instructor seleccionado se recupera de la lista de instructores del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="16dbd-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="16dbd-228">Se carga la propiedad `Courses` del modelo de vista con las entidades `Course` de la propiedad de navegación `CourseAssignments` de ese instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="16dbd-229">El método `Where` devuelve una colección.</span><span class="sxs-lookup"><span data-stu-id="16dbd-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="16dbd-230">Pero en este caso, el filtro seleccionará una sola entidad.</span><span class="sxs-lookup"><span data-stu-id="16dbd-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="16dbd-231">Por tanto, se llama al método `Single` para convertir la colección en una sola entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="16dbd-232">La entidad `Instructor` proporciona acceso a la propiedad `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="16dbd-233">`CourseAssignments` proporciona acceso a las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="16dbd-235">El método `Single` se usa en una colección cuando la colección tiene un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="16dbd-236">El método `Single` inicia una excepción si la colección está vacía o hay más de un elemento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="16dbd-237">Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (NULL, en este caso) si la colección está vacía.</span><span class="sxs-lookup"><span data-stu-id="16dbd-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="16dbd-238">El código siguiente rellena la propiedad `Enrollments` del modelo de vista cuando se selecciona un curso:</span><span class="sxs-lookup"><span data-stu-id="16dbd-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="16dbd-239">Actualizar la página de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="16dbd-239">Update the instructors Index page</span></span>

<span data-ttu-id="16dbd-240">Actualice *Pages/Instructors/Index.cshtml* con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="16dbd-241">En el código anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dbd-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="16dbd-242">Se actualiza la directiva `page` de `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="16dbd-243">`"{id:int?}"` es una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16dbd-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="16dbd-244">La plantilla de ruta cambia las cadenas de consulta enteras de la dirección URL por datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="16dbd-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="16dbd-245">Por ejemplo, al hacer clic en el vínculo **Select** de un instructor con únicamente la directiva `@page`, se genera una dirección URL similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="16dbd-246">Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL es:</span><span class="sxs-lookup"><span data-stu-id="16dbd-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="16dbd-247">Se agrega una columna **Office** en la que se muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es NULL.</span><span class="sxs-lookup"><span data-stu-id="16dbd-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="16dbd-248">Dado que se trata de una relación de uno a cero o uno, es posible que no haya una entidad OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="16dbd-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="16dbd-249">Se agrega una columna **Courses** en la que se muestran los cursos que imparte cada instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="16dbd-250">Vea [Transición de línea explícita con `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información sobre esta sintaxis de Razor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-250">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="16dbd-251">Se agrega código que agrega de forma dinámica `class="success"` al elemento `tr` del instructor y curso seleccionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="16dbd-252">Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.</span><span class="sxs-lookup"><span data-stu-id="16dbd-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="16dbd-253">Se agrega un hipervínculo nuevo con la etiqueta **Select**.</span><span class="sxs-lookup"><span data-stu-id="16dbd-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="16dbd-254">Este vínculo envía el identificador del instructor seleccionado al método `Index` y establece un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="16dbd-255">Se agrega una tabla de cursos para el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="16dbd-256">Se agrega una tabla de inscripciones de alumnos para el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="16dbd-257">Ejecute la aplicación y haga clic en la pestaña **Instructors**. En la página se muestra la `Location` (oficina) de la entidad `OfficeAssignment` relacionada.</span><span class="sxs-lookup"><span data-stu-id="16dbd-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="16dbd-258">Si `OfficeAssignment` es NULL, se muestra una celda de tabla vacía.</span><span class="sxs-lookup"><span data-stu-id="16dbd-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="16dbd-259">Haga clic en el vínculo **Select** de un instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="16dbd-260">Se muestran los cambios de estilo de fila y los cursos asignados a ese instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="16dbd-261">Seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.</span><span class="sxs-lookup"><span data-stu-id="16dbd-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructor y curso seleccionados en la página de índice de instructores](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="16dbd-263">Uso de Single</span><span class="sxs-lookup"><span data-stu-id="16dbd-263">Using Single</span></span>

<span data-ttu-id="16dbd-264">Se puede pasar el método `Single` en la condición `Where` en lugar de llamar al método `Where` por separado:</span><span class="sxs-lookup"><span data-stu-id="16dbd-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="16dbd-265">El uso de `Single` con una condición Where es una cuestión de preferencia personal.</span><span class="sxs-lookup"><span data-stu-id="16dbd-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="16dbd-266">No proporciona ninguna ventaja sobre el uso del método `Where`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="16dbd-267">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="16dbd-267">Explicit loading</span></span>

<span data-ttu-id="16dbd-268">En el código actual se especifica la carga diligente para `Enrollments` y `Students`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="16dbd-269">Imagine que los usuarios rara vez querrán ver las inscripciones en un curso.</span><span class="sxs-lookup"><span data-stu-id="16dbd-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="16dbd-270">En ese caso, una optimización sería cargar solamente los datos de inscripción si se solicitan.</span><span class="sxs-lookup"><span data-stu-id="16dbd-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="16dbd-271">En esta sección, se actualiza `OnGetAsync` para usar la carga explícita de `Enrollments` y `Students`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="16dbd-272">Actualice *Pages/Instructors/Index.cshtml.cs* con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="16dbd-273">En el código anterior se quitan las llamadas al método *ThenInclude* para los datos de inscripción y estudiantes.</span><span class="sxs-lookup"><span data-stu-id="16dbd-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="16dbd-274">Si se selecciona un curso, el código de carga explícita recupera lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="16dbd-275">Las entidades `Enrollment` para el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="16dbd-276">Las entidades `Student` para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="16dbd-277">Observe que en el código anterior `.AsNoTracking()` se convierte en comentario.</span><span class="sxs-lookup"><span data-stu-id="16dbd-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="16dbd-278">Las propiedades de navegación solo se pueden cargar explícitamente para las entidades sometidas a seguimiento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="16dbd-279">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-279">Test the app.</span></span> <span data-ttu-id="16dbd-280">Desde la perspectiva del usuario, la aplicación se comporta exactamente igual a la versión anterior.</span><span class="sxs-lookup"><span data-stu-id="16dbd-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16dbd-281">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="16dbd-281">Next steps</span></span>

<span data-ttu-id="16dbd-282">En el siguiente tutorial se muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="16dbd-283">[Tutorial anterior](xref:data/ef-rp/complex-data-model)
>[Tutorial siguiente](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="16dbd-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="16dbd-284">En este tutorial, se leen y se muestran datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="16dbd-285">Los datos relacionados son los que EF Core carga en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="16dbd-286">Si experimenta problemas que no puede resolver, [descargue o vea la aplicación completada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="16dbd-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="16dbd-287">[Instrucciones de descarga](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="16dbd-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="16dbd-288">En las ilustraciones siguientes se muestran las páginas completadas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="16dbd-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="16dbd-291">Carga diligente, explícita y diferida de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="16dbd-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="16dbd-292">EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="16dbd-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="16dbd-293">[Carga diligente](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="16dbd-294">La carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="16dbd-295">Cuando se lee la entidad, se recuperan sus datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="16dbd-296">Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan.</span><span class="sxs-lookup"><span data-stu-id="16dbd-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="16dbd-297">EF Core emitirá varias consultas para algunos tipos de carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="16dbd-298">La emisión de varias consultas puede ser más eficaz de lo que eran algunas consultas de EF6 cuando había una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="16dbd-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="16dbd-299">La carga diligente se especifica con los métodos `Include` y `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Ejemplo de carga diligente](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="16dbd-301">La carga diligente envía varias consultas cuando se incluye una propiedad de navegación de colección:</span><span class="sxs-lookup"><span data-stu-id="16dbd-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="16dbd-302">Una consulta para la consulta principal</span><span class="sxs-lookup"><span data-stu-id="16dbd-302">One query for the main query</span></span> 
  * <span data-ttu-id="16dbd-303">Una consulta para cada colección "perimetral" en el árbol de la carga.</span><span class="sxs-lookup"><span data-stu-id="16dbd-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="16dbd-304">Separe las consultas con `Load`: los datos se pueden recuperar en distintas consultas y EF Core "corrige" las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="16dbd-305">"Corregir" significa que EF Core rellena automáticamente las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="16dbd-306">La separación de las consultas con `Load` es más parecido a la carga explícita que a la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Ejemplo de consultas independientes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="16dbd-308">Nota: EF Core corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="16dbd-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="16dbd-309">Incluso si los datos de una propiedad de navegación *no* se incluyen explícitamente, es posible que la propiedad se siga rellenando si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="16dbd-310">[Carga explícita](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="16dbd-311">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="16dbd-312">Se debe escribir código para recuperar los datos relacionados cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="16dbd-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="16dbd-313">La carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16dbd-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="16dbd-314">Con la carga explícita, el código especifica las propiedades de navegación que se van a cargar.</span><span class="sxs-lookup"><span data-stu-id="16dbd-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="16dbd-315">Use el método `Load` para realizar la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="16dbd-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="16dbd-316">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="16dbd-316">For example:</span></span>

  ![Ejemplo de carga explícita](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="16dbd-318">[Carga diferida](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="16dbd-319">[Se ha agregado la carga diferida a EF Core en la versión 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="16dbd-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="16dbd-320">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="16dbd-321">La primera vez que se obtiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="16dbd-322">Cada vez que se obtiene acceso a una propiedad de navegación, se envía una consulta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16dbd-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="16dbd-323">El operador `Select` solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="16dbd-324">Crear una página de cursos en la que se muestre el nombre de departamento</span><span class="sxs-lookup"><span data-stu-id="16dbd-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="16dbd-325">La entidad Course incluye una propiedad de navegación que contiene la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="16dbd-326">La entidad `Department` contiene el departamento al que se asigna el curso.</span><span class="sxs-lookup"><span data-stu-id="16dbd-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="16dbd-327">Para mostrar el nombre del departamento asignado en una lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="16dbd-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="16dbd-328">Obtenga la propiedad `Name` desde la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="16dbd-329">La entidad `Department` procede de la propiedad de navegación `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="16dbd-331">Aplicar scaffolding al modelo de Course</span><span class="sxs-lookup"><span data-stu-id="16dbd-331">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="16dbd-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16dbd-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="16dbd-333">Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Course` para la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="16dbd-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16dbd-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="16dbd-335">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="16dbd-335">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="16dbd-336">El comando anterior aplica scaffolding al modelo `Course`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="16dbd-337">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16dbd-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="16dbd-338">Abra *Pages/Courses/Index.cshtml.cs* y examine el método `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="16dbd-339">El motor de scaffolding especificado realiza la carga diligente de la propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="16dbd-340">El método `Include` especifica la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="16dbd-341">Ejecute la aplicación y haga clic en el vínculo **Courses**.</span><span class="sxs-lookup"><span data-stu-id="16dbd-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="16dbd-342">En la columna Department se muestra el `DepartmentID`, lo que no resulta útil.</span><span class="sxs-lookup"><span data-stu-id="16dbd-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="16dbd-343">Actualice el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="16dbd-344">El código anterior agrega `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="16dbd-345">`AsNoTracking` mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="16dbd-346">No se realiza el seguimiento de las entidades porque no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="16dbd-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="16dbd-347">Actualice *Pages/Courses/Index.cshtml* con el marcado resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="16dbd-348">Se han realizado los cambios siguientes en el código con scaffolding:</span><span class="sxs-lookup"><span data-stu-id="16dbd-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="16dbd-349">Ha cambiado el título de Index a Courses.</span><span class="sxs-lookup"><span data-stu-id="16dbd-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="16dbd-350">Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="16dbd-351">De forma predeterminada, las claves principales no tienen scaffolding porque normalmente no tienen sentido para los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="16dbd-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="16dbd-352">Pero en este caso, la clave principal es significativa.</span><span class="sxs-lookup"><span data-stu-id="16dbd-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="16dbd-353">Ha cambiado la columna **Department** para mostrar el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="16dbd-354">El código muestra la propiedad `Name` de la entidad `Department` que se carga en la propiedad de navegación `Department`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="16dbd-355">Ejecute la aplicación y haga clic en la pestaña **Courses** para ver la lista con los nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="16dbd-357">Carga de datos relacionados con Select</span><span class="sxs-lookup"><span data-stu-id="16dbd-357">Loading related data with Select</span></span>

<span data-ttu-id="16dbd-358">El método `OnGetAsync` carga los datos relacionados con el método `Include`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="16dbd-359">El operador `Select` solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="16dbd-360">Para elementos individuales, como el `Department.Name`, se usa INNER JOIN de SQL.</span><span class="sxs-lookup"><span data-stu-id="16dbd-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="16dbd-361">En las colecciones, se usa otro acceso de base de datos, como también hace el operador `Include` en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="16dbd-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="16dbd-362">En el código siguiente se cargan los datos relacionados con el método `Select`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="16dbd-363">El `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="16dbd-364">Vea [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-364">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="16dbd-365">Crear una página de instructores en la que se muestran los cursos y las inscripciones</span><span class="sxs-lookup"><span data-stu-id="16dbd-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="16dbd-366">En esta sección, se crea la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="16dbd-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="16dbd-367">![Página de índice de instructores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="16dbd-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="16dbd-368">En esta página se leen y muestran los datos relacionados de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dbd-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="16dbd-369">En la lista de instructores se muestran datos relacionados de la entidad `OfficeAssignment` (Office en la imagen anterior).</span><span class="sxs-lookup"><span data-stu-id="16dbd-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="16dbd-370">Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="16dbd-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="16dbd-371">Para las entidades `OfficeAssignment` se usa la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="16dbd-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="16dbd-372">Normalmente la carga diligente es más eficaz cuando es necesario mostrar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="16dbd-373">En este caso, se muestran las asignaciones de oficina para los instructores.</span><span class="sxs-lookup"><span data-stu-id="16dbd-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="16dbd-374">Cuando el usuario selecciona un instructor (Harui en la imagen anterior), se muestran las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="16dbd-375">Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="16dbd-376">La carga diligente se usa con las entidades `Course` y sus entidades `Department` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="16dbd-377">En este caso, es posible que las consultas independientes sean más eficaces porque solo se necesitan cursos para el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="16dbd-378">En este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación en entidades que se encuentran en propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="16dbd-379">Cuando el usuario selecciona un curso (Chemistry [Química] en la imagen anterior), se muestran los datos relacionados de la entidad `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="16dbd-380">En la imagen anterior, se muestra el nombre del alumno y la calificación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="16dbd-381">Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="16dbd-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="16dbd-382">Crear un modelo de vista para la vista de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="16dbd-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="16dbd-383">En la página Instructors se muestran datos de tres tablas diferentes.</span><span class="sxs-lookup"><span data-stu-id="16dbd-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="16dbd-384">Se crea un modelo de vista que incluye las tres entidades que representan las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="16dbd-385">En la carpeta *SchoolViewModels*, cree *InstructorIndexData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="16dbd-386">Aplicar scaffolding al modelo de Instructor</span><span class="sxs-lookup"><span data-stu-id="16dbd-386">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="16dbd-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16dbd-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="16dbd-388">Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Instructor` para la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="16dbd-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16dbd-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="16dbd-390">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="16dbd-390">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="16dbd-391">El comando anterior aplica scaffolding al modelo `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="16dbd-392">Ejecute la aplicación y vaya a la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="16dbd-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="16dbd-393">Reemplace *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="16dbd-394">El método `OnGetAsync` acepta datos de ruta opcionales para el identificador del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="16dbd-395">Examine la consulta en el archivo *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="16dbd-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="16dbd-396">La consulta tiene dos instrucciones include:</span><span class="sxs-lookup"><span data-stu-id="16dbd-396">The query has two includes:</span></span>

* <span data-ttu-id="16dbd-397">`OfficeAssignment`: se muestra en la [vista de instructores](#IP).</span><span class="sxs-lookup"><span data-stu-id="16dbd-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="16dbd-398">`CourseAssignments`: muestra los cursos impartidos.</span><span class="sxs-lookup"><span data-stu-id="16dbd-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="16dbd-399">Actualizar la página de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="16dbd-399">Update the instructors Index page</span></span>

<span data-ttu-id="16dbd-400">Actualice *Pages/Instructors/Index.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="16dbd-401">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dbd-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="16dbd-402">Se actualiza la directiva `page` de `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="16dbd-403">`"{id:int?}"` es una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16dbd-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="16dbd-404">La plantilla de ruta cambia las cadenas de consulta enteras de la dirección URL por datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="16dbd-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="16dbd-405">Por ejemplo, al hacer clic en el vínculo **Select** de un instructor con únicamente la directiva `@page`, se genera una dirección URL similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="16dbd-406">Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL anterior es:</span><span class="sxs-lookup"><span data-stu-id="16dbd-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="16dbd-407">El título de página es **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="16dbd-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="16dbd-408">Se ha agregado una columna **Office** en la que se muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es NULL.</span><span class="sxs-lookup"><span data-stu-id="16dbd-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="16dbd-409">Dado que se trata de una relación de uno a cero o uno, es posible que no haya una entidad OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="16dbd-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="16dbd-410">Se ha agregado una columna **Courses** en la que se muestran los cursos que imparte cada instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="16dbd-411">Para obtener más información sobre esta sintaxis de Razor, vea [Transición de línea explícita con `@:`](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="16dbd-411">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="16dbd-412">Ha agregado código que agrega dinámicamente `class="success"` al elemento `tr` del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="16dbd-413">Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.</span><span class="sxs-lookup"><span data-stu-id="16dbd-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="16dbd-414">Se ha agregado un hipervínculo nuevo con la etiqueta **Select**.</span><span class="sxs-lookup"><span data-stu-id="16dbd-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="16dbd-415">Este vínculo envía el identificador del instructor seleccionado al método `Index` y establece un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="16dbd-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="16dbd-416">Ejecute la aplicación y haga clic en la pestaña **Instructors**. En la página se muestra la `Location` (oficina) de la entidad `OfficeAssignment` relacionada.</span><span class="sxs-lookup"><span data-stu-id="16dbd-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="16dbd-417">Si OfficeAssignment es NULL, se muestra una celda de tabla vacía.</span><span class="sxs-lookup"><span data-stu-id="16dbd-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="16dbd-418">Haga clic en el vínculo **Select**.</span><span class="sxs-lookup"><span data-stu-id="16dbd-418">Click on the **Select** link.</span></span> <span data-ttu-id="16dbd-419">El estilo de la fila cambia.</span><span class="sxs-lookup"><span data-stu-id="16dbd-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="16dbd-420">Agregar cursos impartidos por el instructor seleccionado</span><span class="sxs-lookup"><span data-stu-id="16dbd-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="16dbd-421">Actualice el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="16dbd-422">Agregue `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="16dbd-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="16dbd-423">Examine la consulta actualizada:</span><span class="sxs-lookup"><span data-stu-id="16dbd-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="16dbd-424">En la consulta anterior se agregan las entidades `Department`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="16dbd-425">El código siguiente se ejecuta cuando se selecciona un instructor (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="16dbd-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="16dbd-426">El instructor seleccionado se recupera de la lista de instructores del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="16dbd-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="16dbd-427">Se carga la propiedad `Courses` del modelo de vista con las entidades `Course` de la propiedad de navegación `CourseAssignments` de ese instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="16dbd-428">El método `Where` devuelve una colección.</span><span class="sxs-lookup"><span data-stu-id="16dbd-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="16dbd-429">En el método `Where` anterior, solo se devuelve una entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="16dbd-430">El método `Single` convierte la colección en una sola entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="16dbd-431">La entidad `Instructor` proporciona acceso a la propiedad `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="16dbd-432">`CourseAssignments` proporciona acceso a las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="16dbd-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="16dbd-434">El método `Single` se usa en una colección cuando la colección tiene un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="16dbd-435">El método `Single` inicia una excepción si la colección está vacía o hay más de un elemento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="16dbd-436">Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (NULL, en este caso) si la colección está vacía.</span><span class="sxs-lookup"><span data-stu-id="16dbd-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="16dbd-437">El uso de `SingleOrDefault` en una colección vacía:</span><span class="sxs-lookup"><span data-stu-id="16dbd-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="16dbd-438">Inicia una excepción (al tratar de buscar una propiedad `Courses` en una referencia nula).</span><span class="sxs-lookup"><span data-stu-id="16dbd-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="16dbd-439">El mensaje de excepción indicará con menos claridad la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="16dbd-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="16dbd-440">El código siguiente rellena la propiedad `Enrollments` del modelo de vista cuando se selecciona un curso:</span><span class="sxs-lookup"><span data-stu-id="16dbd-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="16dbd-441">Agregue el siguiente marcado al final de la página de Razor *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="16dbd-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="16dbd-442">En el marcado anterior se muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="16dbd-443">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-443">Test the app.</span></span> <span data-ttu-id="16dbd-444">Haga clic en un vínculo **Select** en la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="16dbd-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="16dbd-445">Mostrar datos de estudiante</span><span class="sxs-lookup"><span data-stu-id="16dbd-445">Show student data</span></span>

<span data-ttu-id="16dbd-446">En esta sección, la aplicación se actualiza para mostrar los datos de estudiante para un curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="16dbd-447">Actualice la consulta en el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="16dbd-448">Actualice *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="16dbd-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="16dbd-449">Agregue el marcado siguiente al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="16dbd-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="16dbd-450">En el marcado anterior se muestra una lista de los estudiantes que están inscritos en el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="16dbd-451">Actualice la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="16dbd-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="16dbd-452">Seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.</span><span class="sxs-lookup"><span data-stu-id="16dbd-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructor y curso seleccionados en la página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="16dbd-454">Uso de Single</span><span class="sxs-lookup"><span data-stu-id="16dbd-454">Using Single</span></span>

<span data-ttu-id="16dbd-455">Se puede pasar el método `Single` en la condición `Where` en lugar de llamar al método `Where` por separado:</span><span class="sxs-lookup"><span data-stu-id="16dbd-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="16dbd-456">El enfoque de `Single` anterior no ofrece ninguna ventaja con respecto a `Where`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="16dbd-457">Algunos desarrolladores prefieren el estilo del enfoque de `Single`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="16dbd-458">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="16dbd-458">Explicit loading</span></span>

<span data-ttu-id="16dbd-459">En el código actual se especifica la carga diligente para `Enrollments` y `Students`:</span><span class="sxs-lookup"><span data-stu-id="16dbd-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="16dbd-460">Imagine que los usuarios rara vez querrán ver las inscripciones en un curso.</span><span class="sxs-lookup"><span data-stu-id="16dbd-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="16dbd-461">En ese caso, una optimización sería cargar solamente los datos de inscripción si se solicitan.</span><span class="sxs-lookup"><span data-stu-id="16dbd-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="16dbd-462">En esta sección, se actualiza `OnGetAsync` para usar la carga explícita de `Enrollments` y `Students`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="16dbd-463">Actualice `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="16dbd-464">En el código anterior se quitan las llamadas al método *ThenInclude* para los datos de inscripción y estudiantes.</span><span class="sxs-lookup"><span data-stu-id="16dbd-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="16dbd-465">Si se selecciona un curso, el código resaltado recupera lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="16dbd-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="16dbd-466">Las entidades `Enrollment` para el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="16dbd-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="16dbd-467">Las entidades `Student` para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="16dbd-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="16dbd-468">Tenga en cuenta que en el código anterior `.AsNoTracking()` se convierte en comentario.</span><span class="sxs-lookup"><span data-stu-id="16dbd-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="16dbd-469">Las propiedades de navegación solo se pueden cargar explícitamente para las entidades sometidas a seguimiento.</span><span class="sxs-lookup"><span data-stu-id="16dbd-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="16dbd-470">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="16dbd-470">Test the app.</span></span> <span data-ttu-id="16dbd-471">Desde la perspectiva de los usuarios, la aplicación se comporta exactamente igual a la versión anterior.</span><span class="sxs-lookup"><span data-stu-id="16dbd-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="16dbd-472">En el siguiente tutorial se muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="16dbd-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16dbd-473">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="16dbd-473">Additional resources</span></span>

* [<span data-ttu-id="16dbd-474">Versión en YouTube de este tutorial (parte 1)</span><span class="sxs-lookup"><span data-stu-id="16dbd-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="16dbd-475">Versión en YouTube de este tutorial (parte 2)</span><span class="sxs-lookup"><span data-stu-id="16dbd-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="16dbd-476">[Anterior](xref:data/ef-rp/complex-data-model)
>[Siguiente](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="16dbd-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
