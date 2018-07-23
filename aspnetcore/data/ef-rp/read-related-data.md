---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)'
author: rick-anderson
description: En este tutorial, leerá y mostrará datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: bcea6aa6018a937979b8e0aaa2edcdd96da41559
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202684"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="d387e-103">Páginas de Razor con EF Core en ASP.NET Core: Lectura de datos relacionados (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="d387e-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="d387e-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d387e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d387e-105">En este tutorial, se leen y se muestran datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d387e-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="d387e-106">Los datos relacionados son los que EF Core carga en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="d387e-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="d387e-107">Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="d387e-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="d387e-108">En las ilustraciones siguientes se muestran las páginas completadas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="d387e-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="d387e-111">Carga diligente, explícita y diferida de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="d387e-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="d387e-112">EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="d387e-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="d387e-113">[Carga diligente](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="d387e-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="d387e-114">La carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d387e-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="d387e-115">Cuando se lee la entidad, se recuperan sus datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d387e-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="d387e-116">Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan.</span><span class="sxs-lookup"><span data-stu-id="d387e-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="d387e-117">EF Core emitirá varias consultas para algunos tipos de carga diligente.</span><span class="sxs-lookup"><span data-stu-id="d387e-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="d387e-118">La emisión de varias consultas puede ser más eficaz de lo que eran algunas consultas de EF6 cuando había una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="d387e-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="d387e-119">La carga diligente se especifica con los métodos `Include` y `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="d387e-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Ejemplo de carga diligente](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="d387e-121">La carga diligente envía varias consultas cuando se incluye una propiedad de navegación de colección:</span><span class="sxs-lookup"><span data-stu-id="d387e-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="d387e-122">Una consulta para la consulta principal</span><span class="sxs-lookup"><span data-stu-id="d387e-122">One query for the main query</span></span> 
  * <span data-ttu-id="d387e-123">Una consulta para cada colección "perimetral" en el árbol de la carga.</span><span class="sxs-lookup"><span data-stu-id="d387e-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="d387e-124">Separe las consultas con `Load`: los datos se pueden recuperar en distintas consultas y EF Core "corrige" las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="d387e-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="d387e-125">"Corregir" significa que EF Core rellena automáticamente las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="d387e-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="d387e-126">Separar las consultas con `Load` es más parecido a la carga explícita que a la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="d387e-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Ejemplo de consultas independientes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="d387e-128">Nota: EF Core corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="d387e-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="d387e-129">Incluso si los datos de una propiedad de navegación *no* se incluyen explícitamente, es posible que la propiedad se siga rellenando si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="d387e-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="d387e-130">[Carga explícita](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="d387e-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="d387e-131">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d387e-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="d387e-132">Se debe escribir código para recuperar los datos relacionados cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="d387e-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="d387e-133">La carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d387e-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="d387e-134">Con la carga explícita, el código especifica las propiedades de navegación que se van a cargar.</span><span class="sxs-lookup"><span data-stu-id="d387e-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="d387e-135">Use el método `Load` para realizar la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="d387e-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="d387e-136">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d387e-136">For example:</span></span>

  ![Ejemplo de carga explícita](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="d387e-138">[Carga diferida](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="d387e-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="d387e-139">[Actualmente EF Core no admite la carga diferida](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="d387e-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="d387e-140">Cuando la entidad se lee por primera vez, no se recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d387e-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="d387e-141">La primera vez que se obtiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="d387e-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="d387e-142">Cada vez que se obtiene acceso a una propiedad de navegación, se envía una consulta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d387e-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="d387e-143">El operador `Select` solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="d387e-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="d387e-144">Crear una página de cursos en la que se muestre el nombre de departamento</span><span class="sxs-lookup"><span data-stu-id="d387e-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="d387e-145">La entidad Course incluye una propiedad de navegación que contiene la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="d387e-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="d387e-146">La entidad `Department` contiene el departamento al que se asigna el curso.</span><span class="sxs-lookup"><span data-stu-id="d387e-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="d387e-147">Para mostrar el nombre del departamento asignado en una lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="d387e-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="d387e-148">Obtenga la propiedad `Name` desde la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="d387e-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="d387e-149">La entidad `Department` procede de la propiedad de navegación `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="d387e-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![course.Departament](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="d387e-151">Aplicar scaffolding al modelo de Course</span><span class="sxs-lookup"><span data-stu-id="d387e-151">Scaffold the Course model</span></span>

* <span data-ttu-id="d387e-152">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d387e-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="d387e-153">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="d387e-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d387e-154">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d387e-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="d387e-155">El comando anterior aplica scaffolding al modelo `Course`.</span><span class="sxs-lookup"><span data-stu-id="d387e-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="d387e-156">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d387e-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="d387e-157">Abra *Pages/Courses/Index.cshtml.cs* y examine el método `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="d387e-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="d387e-158">El motor de scaffolding especificado realiza la carga diligente de la propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="d387e-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="d387e-159">El método `Include` especifica la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="d387e-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="d387e-160">Ejecute la aplicación y haga clic en el vínculo **Courses**.</span><span class="sxs-lookup"><span data-stu-id="d387e-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="d387e-161">En la columna Department se muestra el `DepartmentID`, lo que no resulta útil.</span><span class="sxs-lookup"><span data-stu-id="d387e-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="d387e-162">Actualice el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="d387e-163">El código anterior agrega `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="d387e-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="d387e-164">`AsNoTracking` mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas.</span><span class="sxs-lookup"><span data-stu-id="d387e-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="d387e-165">No se realiza el seguimiento de las entidades porque no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="d387e-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="d387e-166">Actualice *Pages/Courses/Index.cshtml* con el marcado resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="d387e-167">Se han realizado los cambios siguientes en el código con scaffolding:</span><span class="sxs-lookup"><span data-stu-id="d387e-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="d387e-168">Ha cambiado el título de Index a Courses.</span><span class="sxs-lookup"><span data-stu-id="d387e-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="d387e-169">Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d387e-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="d387e-170">De forma predeterminada, las claves principales no tienen scaffolding porque normalmente no tienen sentido para los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="d387e-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="d387e-171">Pero en este caso, la clave principal es significativa.</span><span class="sxs-lookup"><span data-stu-id="d387e-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="d387e-172">Ha cambiado la columna **Department** para mostrar el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="d387e-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="d387e-173">El código muestra la propiedad `Name` de la entidad `Department` que se carga en la propiedad de navegación `Department`:</span><span class="sxs-lookup"><span data-stu-id="d387e-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="d387e-174">Ejecute la aplicación y haga clic en la pestaña **Courses** para ver la lista con los nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="d387e-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="d387e-176">Carga de datos relacionados con Select</span><span class="sxs-lookup"><span data-stu-id="d387e-176">Loading related data with Select</span></span>

<span data-ttu-id="d387e-177">El método `OnGetAsync` carga los datos relacionados con el método `Include`:</span><span class="sxs-lookup"><span data-stu-id="d387e-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="d387e-178">El operador `Select` solo carga los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="d387e-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="d387e-179">Para elementos individuales, como el `Department.Name`, se usa INNER JOIN de SQL.</span><span class="sxs-lookup"><span data-stu-id="d387e-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="d387e-180">En las colecciones, se usa otro acceso de base de datos, como también hace el operador `Include` en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="d387e-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="d387e-181">En el código siguiente se cargan los datos relacionados con el método `Select`:</span><span class="sxs-lookup"><span data-stu-id="d387e-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="d387e-182">El `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="d387e-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="d387e-183">Vea [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="d387e-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="d387e-184">Crear una página de instructores en la que se muestran los cursos y las inscripciones</span><span class="sxs-lookup"><span data-stu-id="d387e-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="d387e-185">En esta sección, se crea la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="d387e-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="d387e-186">![Página de índice de instructores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="d387e-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="d387e-187">En esta página se leen y muestran los datos relacionados de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="d387e-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="d387e-188">En la lista de instructores se muestran datos relacionados de la entidad `OfficeAssignment` (Office en la imagen anterior).</span><span class="sxs-lookup"><span data-stu-id="d387e-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="d387e-189">Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="d387e-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="d387e-190">Para las entidades `OfficeAssignment` se usa la carga diligente.</span><span class="sxs-lookup"><span data-stu-id="d387e-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="d387e-191">Normalmente la carga diligente es más eficaz cuando es necesario mostrar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d387e-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="d387e-192">En este caso, se muestran las asignaciones de oficina para los instructores.</span><span class="sxs-lookup"><span data-stu-id="d387e-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="d387e-193">Cuando el usuario selecciona un instructor (Harui en la imagen anterior), se muestran las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d387e-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="d387e-194">Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="d387e-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="d387e-195">La carga diligente se usa con las entidades `Course` y sus entidades `Department` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d387e-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="d387e-196">En este caso, es posible que las consultas independientes sean más eficaces porque solo se necesitan cursos para el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d387e-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="d387e-197">En este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación en entidades que se encuentran en propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="d387e-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="d387e-198">Cuando el usuario selecciona un curso (Chemistry [Química] en la imagen anterior), se muestran los datos relacionados de la entidad `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="d387e-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="d387e-199">En la imagen anterior, se muestra el nombre del alumno y la calificación.</span><span class="sxs-lookup"><span data-stu-id="d387e-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="d387e-200">Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="d387e-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="d387e-201">Crear un modelo de vista para la vista de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="d387e-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="d387e-202">En la página Instructors se muestran datos de tres tablas diferentes.</span><span class="sxs-lookup"><span data-stu-id="d387e-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="d387e-203">Se crea un modelo de vista que incluye las tres entidades que representan las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="d387e-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="d387e-204">En la carpeta *SchoolViewModels*, cree *InstructorIndexData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="d387e-205">Aplicar scaffolding al modelo de Instructor</span><span class="sxs-lookup"><span data-stu-id="d387e-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="d387e-206">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d387e-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="d387e-207">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="d387e-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d387e-208">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d387e-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="d387e-209">El comando anterior aplica scaffolding al modelo `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d387e-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="d387e-210">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d387e-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="d387e-211">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d387e-211">Build the project.</span></span> <span data-ttu-id="d387e-212">La compilación genera errores.</span><span class="sxs-lookup"><span data-stu-id="d387e-212">The build generates errors.</span></span>

<span data-ttu-id="d387e-213">Cambie globalmente `_context.Instructor` por `_context.Instructors` (es decir, agregue una "s" a `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="d387e-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="d387e-214">Se encuentran y actualizan siete repeticiones.</span><span class="sxs-lookup"><span data-stu-id="d387e-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="d387e-215">Ejecute la aplicación y vaya a la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="d387e-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="d387e-216">Reemplace *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="d387e-217">El método `OnGetAsync` acepta datos de ruta opcionales para el identificador del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d387e-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="d387e-218">Examine la consulta en el archivo *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d387e-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="d387e-219">La consulta tiene dos instrucciones include:</span><span class="sxs-lookup"><span data-stu-id="d387e-219">The query has two includes:</span></span>

* <span data-ttu-id="d387e-220">`OfficeAssignment`: se muestra en la [vista de instructores](#IP).</span><span class="sxs-lookup"><span data-stu-id="d387e-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="d387e-221">`CourseAssignments`: muestra los cursos impartidos.</span><span class="sxs-lookup"><span data-stu-id="d387e-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="d387e-222">Actualizar la página de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="d387e-222">Update the instructors Index page</span></span>

<span data-ttu-id="d387e-223">Actualice *Pages/Instructors/Index.cshtml* con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="d387e-224">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="d387e-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d387e-225">Se actualiza la directiva `page` de `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="d387e-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="d387e-226">`"{id:int?}"` es una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="d387e-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="d387e-227">La plantilla de ruta cambia las cadenas de consulta enteras de la dirección URL por datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="d387e-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="d387e-228">Por ejemplo, al hacer clic en el vínculo **Select** de un instructor con únicamente la directiva `@page`, se genera una dirección URL similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="d387e-229">Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL anterior es:</span><span class="sxs-lookup"><span data-stu-id="d387e-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="d387e-230">El título de página es **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="d387e-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="d387e-231">Se ha agregado una columna **Office** en la que se muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es NULL.</span><span class="sxs-lookup"><span data-stu-id="d387e-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="d387e-232">Dado que se trata de una relación de uno a cero o uno, es posible que no haya una entidad OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="d387e-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="d387e-233">Se ha agregado una columna **Courses** en la que se muestran los cursos que imparte cada instructor.</span><span class="sxs-lookup"><span data-stu-id="d387e-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="d387e-234">Vea [Transición de línea explícita con `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información sobre esta sintaxis de Razor.</span><span class="sxs-lookup"><span data-stu-id="d387e-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="d387e-235">Ha agregado código que agrega dinámicamente `class="success"` al elemento `tr` del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d387e-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="d387e-236">Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.</span><span class="sxs-lookup"><span data-stu-id="d387e-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="d387e-237">Se ha agregado un hipervínculo nuevo con la etiqueta **Select**.</span><span class="sxs-lookup"><span data-stu-id="d387e-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="d387e-238">Este vínculo envía el identificador del instructor seleccionado al método `Index` y establece un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="d387e-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="d387e-239">Ejecute la aplicación y haga clic en la pestaña **Instructors**. En la página se muestra la `Location` (oficina) de la entidad `OfficeAssignment` relacionada.</span><span class="sxs-lookup"><span data-stu-id="d387e-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="d387e-240">Si OfficeAssignment es NULL, se muestra una celda de tabla vacía.</span><span class="sxs-lookup"><span data-stu-id="d387e-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Página de índice de instructores sin ninguna selección](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="d387e-242">Haga clic en el vínculo **Select**.</span><span class="sxs-lookup"><span data-stu-id="d387e-242">Click on the **Select** link.</span></span> <span data-ttu-id="d387e-243">El estilo de la fila cambia.</span><span class="sxs-lookup"><span data-stu-id="d387e-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="d387e-244">Agregar cursos impartidos por el instructor seleccionado</span><span class="sxs-lookup"><span data-stu-id="d387e-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="d387e-245">Actualice el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="d387e-246">Agregue `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="d387e-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="d387e-247">Examine la consulta actualizada:</span><span class="sxs-lookup"><span data-stu-id="d387e-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="d387e-248">En la consulta anterior se agregan las entidades `Department`.</span><span class="sxs-lookup"><span data-stu-id="d387e-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="d387e-249">El código siguiente se ejecuta cuando se selecciona un instructor (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="d387e-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="d387e-250">El instructor seleccionado se recupera de la lista de instructores del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="d387e-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="d387e-251">Se carga la propiedad `Courses` del modelo de vista con las entidades `Course` de la propiedad de navegación `CourseAssignments` de ese instructor.</span><span class="sxs-lookup"><span data-stu-id="d387e-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="d387e-252">El método `Where` devuelve una colección.</span><span class="sxs-lookup"><span data-stu-id="d387e-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="d387e-253">En el método `Where` anterior, solo se devuelve una entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d387e-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="d387e-254">El método `Single` convierte la colección en una sola entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d387e-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="d387e-255">La entidad `Instructor` proporciona acceso a la propiedad `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="d387e-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="d387e-256">`CourseAssignments` proporciona acceso a las entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d387e-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="d387e-258">El método `Single` se usa en una colección cuando la colección tiene un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="d387e-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="d387e-259">El método `Single` inicia una excepción si la colección está vacía o hay más de un elemento.</span><span class="sxs-lookup"><span data-stu-id="d387e-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="d387e-260">Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (NULL, en este caso) si la colección está vacía.</span><span class="sxs-lookup"><span data-stu-id="d387e-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="d387e-261">El uso de `SingleOrDefault` en una colección vacía:</span><span class="sxs-lookup"><span data-stu-id="d387e-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="d387e-262">Inicia una excepción (al tratar de buscar una propiedad `Courses` en una referencia nula).</span><span class="sxs-lookup"><span data-stu-id="d387e-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="d387e-263">El mensaje de excepción indicará con menos claridad la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="d387e-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="d387e-264">El código siguiente rellena la propiedad `Enrollments` del modelo de vista cuando se selecciona un curso:</span><span class="sxs-lookup"><span data-stu-id="d387e-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="d387e-265">Agregue el siguiente marcado al final de la página de Razor *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d387e-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="d387e-266">En el marcado anterior se muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.</span><span class="sxs-lookup"><span data-stu-id="d387e-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="d387e-267">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d387e-267">Test the app.</span></span> <span data-ttu-id="d387e-268">Haga clic en un vínculo **Select** en la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="d387e-268">Click on a **Select** link on the instructors page.</span></span>

![Instructor seleccionado en la página de índice de instructores](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="d387e-270">Mostrar datos de estudiante</span><span class="sxs-lookup"><span data-stu-id="d387e-270">Show student data</span></span>

<span data-ttu-id="d387e-271">En esta sección, la aplicación se actualiza para mostrar los datos de estudiante para un curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d387e-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="d387e-272">Actualice la consulta en el método `OnGetAsync` de *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="d387e-273">Actualice *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d387e-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="d387e-274">Agregue el marcado siguiente al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="d387e-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="d387e-275">En el marcado anterior se muestra una lista de los estudiantes que están inscritos en el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d387e-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="d387e-276">Actualice la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="d387e-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="d387e-277">Seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.</span><span class="sxs-lookup"><span data-stu-id="d387e-277">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructor y curso seleccionados en la página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="d387e-279">Uso de Single</span><span class="sxs-lookup"><span data-stu-id="d387e-279">Using Single</span></span>

<span data-ttu-id="d387e-280">Se puede pasar el método `Single` en la condición `Where` en lugar de llamar al método `Where` por separado:</span><span class="sxs-lookup"><span data-stu-id="d387e-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="d387e-281">El enfoque de `Single` anterior no ofrece ninguna ventaja con respecto a `Where`.</span><span class="sxs-lookup"><span data-stu-id="d387e-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="d387e-282">Algunos desarrolladores prefieren el estilo del enfoque de `Single`.</span><span class="sxs-lookup"><span data-stu-id="d387e-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="d387e-283">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="d387e-283">Explicit loading</span></span>

<span data-ttu-id="d387e-284">En el código actual se especifica la carga diligente para `Enrollments` y `Students`:</span><span class="sxs-lookup"><span data-stu-id="d387e-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="d387e-285">Imagine que los usuarios rara vez querrán ver las inscripciones en un curso.</span><span class="sxs-lookup"><span data-stu-id="d387e-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="d387e-286">En ese caso, una optimización sería cargar solamente los datos de inscripción si se solicitan.</span><span class="sxs-lookup"><span data-stu-id="d387e-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="d387e-287">En esta sección, se actualiza `OnGetAsync` para usar la carga explícita de `Enrollments` y `Students`.</span><span class="sxs-lookup"><span data-stu-id="d387e-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="d387e-288">Actualice `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="d387e-289">En el código anterior se quitan las llamadas al método *ThenInclude* para los datos de inscripción y estudiantes.</span><span class="sxs-lookup"><span data-stu-id="d387e-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="d387e-290">Si se selecciona un curso, el código resaltado recupera lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d387e-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="d387e-291">Las entidades `Enrollment` para el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d387e-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="d387e-292">Las entidades `Student` para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d387e-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="d387e-293">Tenga en cuenta que en el código anterior `.AsNoTracking()` se convierte en comentario.</span><span class="sxs-lookup"><span data-stu-id="d387e-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="d387e-294">Las propiedades de navegación solo se pueden cargar explícitamente para las entidades sometidas a seguimiento.</span><span class="sxs-lookup"><span data-stu-id="d387e-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="d387e-295">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d387e-295">Test the app.</span></span> <span data-ttu-id="d387e-296">Desde la perspectiva de los usuarios, la aplicación se comporta exactamente igual a la versión anterior.</span><span class="sxs-lookup"><span data-stu-id="d387e-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="d387e-297">En el siguiente tutorial se muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d387e-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="d387e-298">[Anterior](xref:data/ef-rp/complex-data-model)
>[Siguiente](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="d387e-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
