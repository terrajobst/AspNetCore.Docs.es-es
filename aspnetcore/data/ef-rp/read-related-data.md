---
title: "Páginas de Razor con EF básica: leer datos relacionados - 6 de 8"
author: rick-anderson
description: "En este tutorial leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 532020a8fe4c5a0312cbd89278e61f614b1825f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="c5c63-103">Lectura relacionadas con datos - Core EF con páginas de Razor (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="c5c63-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="c5c63-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c5c63-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="c5c63-105">En este tutorial, los datos relacionados lee y muestra.</span><span class="sxs-lookup"><span data-stu-id="c5c63-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="c5c63-106">Datos relacionados son datos que EF Core carga en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="c5c63-107">Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="c5c63-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="c5c63-108">Las ilustraciones siguientes muestran las páginas completadas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="c5c63-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="c5c63-111">Carga diligente, explícita y diferida de los datos relacionados</span><span class="sxs-lookup"><span data-stu-id="c5c63-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="c5c63-112">Hay varias maneras que EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad:</span><span class="sxs-lookup"><span data-stu-id="c5c63-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="c5c63-113">[Carga diligente](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="c5c63-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="c5c63-114">Carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c5c63-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="c5c63-115">Cuando se lee la entidad, se recuperan los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="c5c63-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="c5c63-116">Esto normalmente como resultado de una consulta de combinación única que recupera todos los datos que se necesitan.</span><span class="sxs-lookup"><span data-stu-id="c5c63-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="c5c63-117">Núcleo EF emitirá varias consultas para algunos tipos de carga diligente.</span><span class="sxs-lookup"><span data-stu-id="c5c63-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="c5c63-118">Emitir varias consultas puede ser más eficaz que era el caso de algunas consultas de EF6 donde hubiera una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="c5c63-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="c5c63-119">Carga diligente se especifica con el `Include` y `ThenInclude` métodos.</span><span class="sxs-lookup"><span data-stu-id="c5c63-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![En el ejemplo se carga diligente](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="c5c63-121">Carga diligente envía varias consultas cuando se incluye una colección nvavigation:</span><span class="sxs-lookup"><span data-stu-id="c5c63-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="c5c63-122">Una consulta para la consulta principal</span><span class="sxs-lookup"><span data-stu-id="c5c63-122">One query for the main query</span></span> 
 * <span data-ttu-id="c5c63-123">Una consulta para cada colección "borde" en el árbol de la carga.</span><span class="sxs-lookup"><span data-stu-id="c5c63-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="c5c63-124">Separar las consultas con `Load`: se pueden recuperar los datos en distintas consultas y EF Core "corrige" las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="c5c63-125">"correcciones de seguridad" significa que EF Core rellena automáticamente las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="c5c63-126">Separar las consultas con `Load` más parecido a Explicit carga a carga diligente.</span><span class="sxs-lookup"><span data-stu-id="c5c63-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Ejemplo de separar las consultas](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="c5c63-128">Nota: Core EF corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="c5c63-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="c5c63-129">Incluso si los datos de una propiedad de navegación son *no* incluidos explícitamente, la propiedad también puede rellenar si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="c5c63-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="c5c63-130">[Carga explícita](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="c5c63-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="c5c63-131">Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="c5c63-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="c5c63-132">Debe escribir código para recuperar los datos relacionados cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="c5c63-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="c5c63-133">Carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c5c63-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="c5c63-134">Con carga explícita, el código especifica las propiedades de navegación que va a cargarse.</span><span class="sxs-lookup"><span data-stu-id="c5c63-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="c5c63-135">Use la `Load` método para realizar la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="c5c63-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="c5c63-136">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c5c63-136">For example:</span></span>

 ![En el ejemplo se carga explícita](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="c5c63-138">[Carga diferida](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="c5c63-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="c5c63-139">[Núcleo EF no admite actualmente la carga diferida](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="c5c63-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="c5c63-140">Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="c5c63-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="c5c63-141">La primera vez que se tiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="c5c63-142">Se envía una consulta a la base de datos cada vez que se accede a una propiedad de navegación por primera vez.</span><span class="sxs-lookup"><span data-stu-id="c5c63-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="c5c63-143">El `Select` operador carga únicamente los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="c5c63-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="c5c63-144">Crear una página de cursos que muestra el nombre de departamento</span><span class="sxs-lookup"><span data-stu-id="c5c63-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="c5c63-145">La entidad de curso incluye una propiedad de navegación que contiene el `Department` entidad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="c5c63-146">El `Department` entidad contiene el departamento que el curso se asigna a.</span><span class="sxs-lookup"><span data-stu-id="c5c63-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="c5c63-147">Para mostrar el nombre del departamento asignado en una lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="c5c63-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="c5c63-148">Obtener la `Name` propiedad desde el `Department` entidad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="c5c63-149">El `Department` entidad procede del `Course.Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![curso. Departamento](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="c5c63-151">Aplicar la técnica scaffolding el modelo de curso</span><span class="sxs-lookup"><span data-stu-id="c5c63-151">Scaffold the Course model</span></span>

* <span data-ttu-id="c5c63-152">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5c63-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="c5c63-153">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c5c63-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c5c63-154">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c5c63-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="c5c63-155">El scaffolds del comando anterior la `Course` modelo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="c5c63-156">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5c63-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="c5c63-157">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c5c63-157">Build the project.</span></span> <span data-ttu-id="c5c63-158">La compilación genera errores similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="c5c63-159">Cambiar globalmente `_context.Course` a `_context.Courses` (es decir, agregue una "s" para `Course`).</span><span class="sxs-lookup"><span data-stu-id="c5c63-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="c5c63-160">7 apariciones se encuentra y se actualizan.</span><span class="sxs-lookup"><span data-stu-id="c5c63-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="c5c63-161">Abra *Pages/Courses/Index.cshtml.cs* y examine el `OnGetAsync` método.</span><span class="sxs-lookup"><span data-stu-id="c5c63-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="c5c63-162">El motor de scaffolding especificado carga diligente de la `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="c5c63-163">El `Include` método especifica carga diligente.</span><span class="sxs-lookup"><span data-stu-id="c5c63-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="c5c63-164">Ejecute la aplicación y seleccione la **cursos** vínculo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="c5c63-165">Muestra la columna department el `DepartmentID`, que no es útil.</span><span class="sxs-lookup"><span data-stu-id="c5c63-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="c5c63-166">Actualice el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="c5c63-167">Agrega el código anterior `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="c5c63-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="c5c63-168">`AsNoTracking`mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas.</span><span class="sxs-lookup"><span data-stu-id="c5c63-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="c5c63-169">No se realiza un seguimiento de las entidades porque no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="c5c63-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="c5c63-170">Actualización *Views/Courses/Index.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="c5c63-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="c5c63-171">Los siguientes cambios se realizaron en el código con scaffolding:</span><span class="sxs-lookup"><span data-stu-id="c5c63-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="c5c63-172">Cambia el encabezado de índice a Courses.</span><span class="sxs-lookup"><span data-stu-id="c5c63-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="c5c63-173">Agrega un **número** columna que muestra la `CourseID` valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="c5c63-174">De forma predeterminada, las claves principales no son scaffolding porque normalmente se encuentran sin significado para los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="c5c63-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="c5c63-175">Sin embargo, en este caso la clave principal es significativa.</span><span class="sxs-lookup"><span data-stu-id="c5c63-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="c5c63-176">Cambiar el **departamento** columna para mostrar el nombre del departamento.</span><span class="sxs-lookup"><span data-stu-id="c5c63-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="c5c63-177">El código muestra la `Name` propiedad de la `Department` entidad que se carga en el `Department` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="c5c63-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="c5c63-178">Ejecute la aplicación y seleccione la **cursos** pestaña para ver la lista con los nombres de departamento.</span><span class="sxs-lookup"><span data-stu-id="c5c63-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="c5c63-180">Carga los datos con Select relacionados</span><span class="sxs-lookup"><span data-stu-id="c5c63-180">Loading related data with Select</span></span>

<span data-ttu-id="c5c63-181">El `OnGetAsync` método carga de datos relacionados con el `Include` método:</span><span class="sxs-lookup"><span data-stu-id="c5c63-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="c5c63-182">El `Select` operador carga únicamente los datos relacionados necesarios.</span><span class="sxs-lookup"><span data-stu-id="c5c63-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="c5c63-183">Para los elementos individuales, como el `Department.Name` utiliza un INNER JOIN de SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c63-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="c5c63-184">Para las colecciones utiliza otro acceso de base de datos, pero también lo hace el.`Include`</span><span class="sxs-lookup"><span data-stu-id="c5c63-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="c5c63-185">operador en colecciones.</span><span class="sxs-lookup"><span data-stu-id="c5c63-185">operator on collections.</span></span>

<span data-ttu-id="c5c63-186">El siguiente código carga los datos relacionados con el `Select` método:</span><span class="sxs-lookup"><span data-stu-id="c5c63-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="c5c63-187">El `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="c5c63-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="c5c63-188">Vea [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) y [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="c5c63-189">Crear una página de instructores que muestra los cursos y las inscripciones</span><span class="sxs-lookup"><span data-stu-id="c5c63-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="c5c63-190">En esta sección, se crea la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="c5c63-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="c5c63-191">![Página de índice de instructores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="c5c63-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="c5c63-192">Esta página se lee y muestra los datos relacionados de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="c5c63-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="c5c63-193">La lista de instructores muestra datos relacionados de la `OfficeAssignment` entidad (Office en la imagen anterior).</span><span class="sxs-lookup"><span data-stu-id="c5c63-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="c5c63-194">El `Instructor` y `OfficeAssignment` entidades se encuentran en una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="c5c63-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="c5c63-195">Carga diligente se usa para la `OfficeAssignment` entidades.</span><span class="sxs-lookup"><span data-stu-id="c5c63-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="c5c63-196">Carga diligente es normalmente más eficaz cuando es necesitan mostrar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="c5c63-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="c5c63-197">En este caso, se muestran las asignaciones de oficina de los instructores.</span><span class="sxs-lookup"><span data-stu-id="c5c63-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="c5c63-198">Cuando el usuario selecciona un instructor (Harui en la imagen anterior), relacionados con `Course` se muestran las entidades.</span><span class="sxs-lookup"><span data-stu-id="c5c63-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="c5c63-199">El `Instructor` y `Course` entidades se encuentran en una relación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="c5c63-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="c5c63-200">Eager para cargar el `Course` entidades y sus relacionados `Department` entidades se utiliza.</span><span class="sxs-lookup"><span data-stu-id="c5c63-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="c5c63-201">En este caso, separar las consultas pueden ser más eficaces porque solo los cursos para el instructor seleccionado se necesitan.</span><span class="sxs-lookup"><span data-stu-id="c5c63-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="c5c63-202">Este ejemplo muestra cómo utilizar carga diligente de propiedades de navegación de entidades que se encuentran en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="c5c63-203">Cuando el usuario selecciona un curso (química en la imagen anterior), los datos de relacionados con el `Enrollments` aparece la entidad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="c5c63-204">En la imagen anterior, se muestran el grado y nombre del alumno.</span><span class="sxs-lookup"><span data-stu-id="c5c63-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="c5c63-205">El `Course` y `Enrollment` entidades se encuentran en una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="c5c63-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="c5c63-206">Crear un modelo de vista para la vista de índice Instructor</span><span class="sxs-lookup"><span data-stu-id="c5c63-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="c5c63-207">La página de instructores muestra datos de tres tablas diferentes.</span><span class="sxs-lookup"><span data-stu-id="c5c63-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="c5c63-208">Se crea un modelo de vista que incluya las tres entidades que representan las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="c5c63-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="c5c63-209">En el *SchoolViewModels* carpeta, crear *InstructorIndexData.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="c5c63-210">Aplicar la técnica scaffolding el modelo Instructor</span><span class="sxs-lookup"><span data-stu-id="c5c63-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="c5c63-211">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5c63-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="c5c63-212">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c5c63-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c5c63-213">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c5c63-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="c5c63-214">El scaffolds del comando anterior la `Instructor` modelo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="c5c63-215">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5c63-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="c5c63-216">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c5c63-216">Build the project.</span></span> <span data-ttu-id="c5c63-217">La compilación genera errores.</span><span class="sxs-lookup"><span data-stu-id="c5c63-217">The build generates errors.</span></span>

<span data-ttu-id="c5c63-218">Cambiar globalmente `_context.Instructor` a `_context.Instructors` (es decir, agregue una "s" para `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="c5c63-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="c5c63-219">7 apariciones se encuentra y se actualizan.</span><span class="sxs-lookup"><span data-stu-id="c5c63-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="c5c63-220">Ejecutar la aplicación y navegue hasta la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="c5c63-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="c5c63-221">Reemplace *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="c5c63-222">El `OnGetAsync` método acepta datos de ruta opcional para el Id. del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c5c63-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="c5c63-223">Examine la consulta en el *Pages/Instructors/Index.cshtml* página:</span><span class="sxs-lookup"><span data-stu-id="c5c63-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="c5c63-224">La consulta tiene dos incluye:</span><span class="sxs-lookup"><span data-stu-id="c5c63-224">The query has two includes:</span></span>

* <span data-ttu-id="c5c63-225">`OfficeAssignment`: Mostrada en el [instructores vista](#IP).</span><span class="sxs-lookup"><span data-stu-id="c5c63-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="c5c63-226">`CourseAssignments`: Abre en los cursos impartidos.</span><span class="sxs-lookup"><span data-stu-id="c5c63-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="c5c63-227">Actualizar la página de índice de instructores</span><span class="sxs-lookup"><span data-stu-id="c5c63-227">Update the instructors Index page</span></span>

<span data-ttu-id="c5c63-228">Actualización *Pages/Instructors/Index.cshtml* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="c5c63-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="c5c63-229">El marcado anterior realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="c5c63-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="c5c63-230">Las actualizaciones de la `page` la directiva de `@page` a `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="c5c63-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="c5c63-231">`"{id:int?}"`es una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="c5c63-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="c5c63-232">La plantilla de ruta cambia las cadenas de consulta de entero en la dirección URL para enrutar los datos.</span><span class="sxs-lookup"><span data-stu-id="c5c63-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="c5c63-233">Por ejemplo, al hacer clic en el **seleccione** vínculo para un instructor cuando la directiva de página genera una dirección URL similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="c5c63-234">Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL anterior es:</span><span class="sxs-lookup"><span data-stu-id="c5c63-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="c5c63-235">Título de página es **instructores**.</span><span class="sxs-lookup"><span data-stu-id="c5c63-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="c5c63-236">Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null.</span><span class="sxs-lookup"><span data-stu-id="c5c63-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="c5c63-237">Dado que se trata de una relación de uno a cero o uno, no puede haber una entidad OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="c5c63-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="c5c63-238">Agrega un **cursos** columna que muestra los cursos se imparte por cada instructor.</span><span class="sxs-lookup"><span data-stu-id="c5c63-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="c5c63-239">Vea [transición línea explícita con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información acerca de esta sintaxis razor.</span><span class="sxs-lookup"><span data-stu-id="c5c63-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="c5c63-240">Ha agregado código que agrega dinámicamente `class="success"` a la `tr` elemento del instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c5c63-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="c5c63-241">Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.</span><span class="sxs-lookup"><span data-stu-id="c5c63-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="c5c63-242">Agrega un nuevo hipervínculo con la etiqueta **seleccione**.</span><span class="sxs-lookup"><span data-stu-id="c5c63-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="c5c63-243">Este vínculo envía el Id. del instructor seleccionado para la `Index` método y establece un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="c5c63-244">Ejecute la aplicación y seleccione la **instructores** ficha. La página muestra la `Location` (office) de relacionado `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="c5c63-245">Si OfficeAssignment' es null, se muestra una celda de tabla vacía.</span><span class="sxs-lookup"><span data-stu-id="c5c63-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Página de índice de instructores que no hay nada seleccionado](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="c5c63-247">Haga clic en el **seleccione** vínculo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-247">Click on the **Select** link.</span></span> <span data-ttu-id="c5c63-248">Los cambios de estilo de la fila.</span><span class="sxs-lookup"><span data-stu-id="c5c63-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="c5c63-249">Agregar cursos impartidos instructor seleccionado</span><span class="sxs-lookup"><span data-stu-id="c5c63-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="c5c63-250">Actualización de la `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="c5c63-251">Examine la consulta actualizada:</span><span class="sxs-lookup"><span data-stu-id="c5c63-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="c5c63-252">La consulta anterior se agrega el `Department` entidades.</span><span class="sxs-lookup"><span data-stu-id="c5c63-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="c5c63-253">El siguiente código se ejecuta cuando se selecciona un instructor (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="c5c63-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="c5c63-254">El instructor seleccionado se recupera de la lista de instructores en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="c5c63-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="c5c63-255">El modelo de vista `Courses` propiedad se carga con la `Course` entidades desde ese instructor `CourseAssignments` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="c5c63-256">El `Where` método devuelve una colección.</span><span class="sxs-lookup"><span data-stu-id="c5c63-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="c5c63-257">En el anterior `Where` método, solo una `Instructor` se devuelve la entidad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="c5c63-258">El `Single` método convierte la colección en una sola `Instructor` entidad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="c5c63-259">El `Instructor` entidad proporciona acceso a la `CourseAssignments` propiedad.</span><span class="sxs-lookup"><span data-stu-id="c5c63-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="c5c63-260">`CourseAssignments`proporciona acceso a relacionado `Course` entidades.</span><span class="sxs-lookup"><span data-stu-id="c5c63-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instructor a cursos m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="c5c63-262">El `Single` método se utiliza en una colección cuando la colección tiene un solo elemento.</span><span class="sxs-lookup"><span data-stu-id="c5c63-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="c5c63-263">El `Single` método produce una excepción si la colección está vacía o si hay más de un elemento.</span><span class="sxs-lookup"><span data-stu-id="c5c63-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="c5c63-264">Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (null, en este caso) si la colección está vacía.</span><span class="sxs-lookup"><span data-stu-id="c5c63-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="c5c63-265">Usar `SingleOrDefault` en una colección vacía:</span><span class="sxs-lookup"><span data-stu-id="c5c63-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="c5c63-266">Produce una excepción (de tratar de buscar un `Courses` propiedad en una referencia nula).</span><span class="sxs-lookup"><span data-stu-id="c5c63-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="c5c63-267">El mensaje de excepción menos claramente indicaría la causa del problema.</span><span class="sxs-lookup"><span data-stu-id="c5c63-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="c5c63-268">El código siguiente rellena el modelo de vista `Enrollments` propiedad cuando se selecciona un curso:</span><span class="sxs-lookup"><span data-stu-id="c5c63-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="c5c63-269">Agregue el siguiente marcado al final de la *Pages/Courses/Index.cshtml* Razor página:</span><span class="sxs-lookup"><span data-stu-id="c5c63-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="c5c63-270">El marcado anterior muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.</span><span class="sxs-lookup"><span data-stu-id="c5c63-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="c5c63-271">Probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-271">Test the app.</span></span> <span data-ttu-id="c5c63-272">Haga clic en un **seleccione** vínculo en la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="c5c63-272">Click on a **Select** link on the instructors page.</span></span>

![Instructor de página de índice de instructores seleccionado](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="c5c63-274">Mostrar datos de estudiante</span><span class="sxs-lookup"><span data-stu-id="c5c63-274">Show student data</span></span>

<span data-ttu-id="c5c63-275">En esta sección, la aplicación se actualiza para mostrar los datos de estudiante para un curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c5c63-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="c5c63-276">Actualizar la consulta en el `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="c5c63-277">Actualización *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c5c63-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="c5c63-278">Agregue el siguiente marcado al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="c5c63-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="c5c63-279">El marcado anterior muestra una lista de los alumnos que están inscritos en el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c5c63-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="c5c63-280">Actualice la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="c5c63-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="c5c63-281">Seleccione un curso para ver la lista de estudiantes inscritos y sus calificaciones.</span><span class="sxs-lookup"><span data-stu-id="c5c63-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructor de página de índice de instructores y curso seleccionado](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="c5c63-283">Mediante un único</span><span class="sxs-lookup"><span data-stu-id="c5c63-283">Using Single</span></span>

<span data-ttu-id="c5c63-284">El `Single` método puede pasar en el `Where` condición en lugar de llamar el `Where` método por separado:</span><span class="sxs-lookup"><span data-stu-id="c5c63-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="c5c63-285">Los anteriores `Single` enfoque no ofrece ninguna ventaja con respecto a `Where`.</span><span class="sxs-lookup"><span data-stu-id="c5c63-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="c5c63-286">Algunos desarrolladores prefieren los `Single` aproximarse al estilo.</span><span class="sxs-lookup"><span data-stu-id="c5c63-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="c5c63-287">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="c5c63-287">Explicit loading</span></span>

<span data-ttu-id="c5c63-288">El código actual especifica carga diligente de `Enrollments` y `Students`:</span><span class="sxs-lookup"><span data-stu-id="c5c63-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="c5c63-289">Imagine que rara vez, los usuarios desean ver las inscripciones en un curso.</span><span class="sxs-lookup"><span data-stu-id="c5c63-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="c5c63-290">En ese caso, una optimización sería sólo se puede cargar los datos de inscripción si se solicita.</span><span class="sxs-lookup"><span data-stu-id="c5c63-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="c5c63-291">En esta sección, el `OnGetAsync` se actualiza para usar la carga explícita de `Enrollments` y `Students`.</span><span class="sxs-lookup"><span data-stu-id="c5c63-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="c5c63-292">Actualización de la `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c5c63-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="c5c63-293">El código anterior quita el *ThenInclude* método se llama para los datos de inscripción y estudiantes.</span><span class="sxs-lookup"><span data-stu-id="c5c63-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="c5c63-294">Si se selecciona un curso, se recupera el código resaltado:</span><span class="sxs-lookup"><span data-stu-id="c5c63-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="c5c63-295">El `Enrollment` entidades para el curso seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c5c63-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="c5c63-296">El `Student` entidades para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c5c63-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="c5c63-297">Tenga en cuenta el anterior código convierte en comentario `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="c5c63-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="c5c63-298">Propiedades de navegación solo se pueden cargar explícitamente para entidades sometidas a seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c5c63-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="c5c63-299">Probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c5c63-299">Test the app.</span></span> <span data-ttu-id="c5c63-300">Desde la perspectiva de los usuarios, la aplicación se comporta exactamente igual a la versión anterior.</span><span class="sxs-lookup"><span data-stu-id="c5c63-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="c5c63-301">El siguiente tutorial muestra cómo actualizar datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="c5c63-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="c5c63-302">[Anterior](xref:data/ef-rp/complex-data-model)
>[Siguiente](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="c5c63-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
