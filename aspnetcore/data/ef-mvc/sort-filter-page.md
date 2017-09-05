---
title: "Núcleo de ASP.NET MVC con EF Core - ordenar, filtrar y paginación: 3 de 10"
author: tdykstra
description: "En este tutorial agregará ordenación, filtrado y paginación funcionalidad en la página utilizando ASP.NET Core y Entity Framework Core."
keywords: "ASP.NET Core, Entity Framework Core, ordenar, filtrar, paginación, agrupación"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 1140f4916ca39cb454eaa41fdf6adfe7ad26cc31
ms.sourcegitcommit: dfd6af48cf66813eaf04b011cb9341339a751254
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="06082-104">Ordenación, filtrado, paginación y agrupar - Core EF con el tutorial de MVC de ASP.NET Core (3 de 10)</span><span class="sxs-lookup"><span data-stu-id="06082-104">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="06082-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="06082-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="06082-106">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06082-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="06082-107">Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="06082-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="06082-108">En el tutorial anterior, se implementa un conjunto de páginas web para las operaciones CRUD básicas para las entidades de estudiante.</span><span class="sxs-lookup"><span data-stu-id="06082-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="06082-109">En este tutorial agregará la ordenación, el filtrado y la funcionalidad de paginación a la página de índice de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="06082-109">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="06082-110">También podrá crear una página que realiza la agrupación sencilla.</span><span class="sxs-lookup"><span data-stu-id="06082-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="06082-111">En la siguiente ilustración muestra el aspecto de la página cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="06082-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="06082-112">Los encabezados de columna son vínculos que el usuario puede hacer clic para ordenar por esa columna.</span><span class="sxs-lookup"><span data-stu-id="06082-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="06082-113">Al hacer clic en una columna por repetidamente alterna entre orden ascendente y orden descendente.</span><span class="sxs-lookup"><span data-stu-id="06082-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Página de índice de estudiantes](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="06082-115">Agregar vínculos de ordenación de la columna a la página de índice de estudiantes</span><span class="sxs-lookup"><span data-stu-id="06082-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="06082-116">Para agregar ordenación, en la página de índice de estudiantes, cambiará la `Index` método del controlador de los alumnos y agregue código a la vista de índice de estudiante.</span><span class="sxs-lookup"><span data-stu-id="06082-116">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="06082-117">Agregar funcionalidad de ordenación para el método de índice</span><span class="sxs-lookup"><span data-stu-id="06082-117">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="06082-118">En *StudentsController.cs*, reemplace el `Index` método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="06082-118">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

<span data-ttu-id="06082-119">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]</span><span class="sxs-lookup"><span data-stu-id="06082-119">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]</span></span>

<span data-ttu-id="06082-120">Este código recibe un `sortOrder` parámetro de la cadena de consulta en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="06082-120">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="06082-121">El valor de cadena de consulta se proporciona por núcleo de ASP.NET MVC como un parámetro al método de acción.</span><span class="sxs-lookup"><span data-stu-id="06082-121">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="06082-122">El parámetro será una cadena que es "Name" o "Fecha", seguido opcionalmente por un guión bajo y la cadena "desc" para especificar el orden descendente.</span><span class="sxs-lookup"><span data-stu-id="06082-122">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="06082-123">El criterio de ordenación predeterminado es el ascendente.</span><span class="sxs-lookup"><span data-stu-id="06082-123">The default sort order is ascending.</span></span>

<span data-ttu-id="06082-124">La primera vez que se solicita la página de índice, no hay ninguna cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="06082-124">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="06082-125">Los alumnos se muestran en orden ascendente por apellido, que es el valor predeterminado establecido por el caso paso explícito en el `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="06082-125">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="06082-126">Cuando el usuario hace clic en un hipervínculo de encabezado de columna, la correspondiente `sortOrder` valor se proporciona en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="06082-126">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="06082-127">Los dos `ViewData` elementos (NameSortParm y DateSortParm) se usan por la vista para configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuada.</span><span class="sxs-lookup"><span data-stu-id="06082-127">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="06082-128">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="06082-128">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]</span></span>

<span data-ttu-id="06082-129">Estas son las instrucciones ternarias.</span><span class="sxs-lookup"><span data-stu-id="06082-129">These are ternary statements.</span></span> <span data-ttu-id="06082-130">La primera de ellas especifica que si el `sortOrder` del parámetro es null o está vacío, NameSortParm debe establecerse en "name_desc"; en caso contrario, se debe establecer en una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="06082-130">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="06082-131">Estas dos instrucciones habilitar la vista establecer la columna hipervínculos de encabezado como sigue:</span><span class="sxs-lookup"><span data-stu-id="06082-131">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="06082-132">Criterio de ordenación actual</span><span class="sxs-lookup"><span data-stu-id="06082-132">Current sort order</span></span>  | <span data-ttu-id="06082-133">Hipervínculo apellido</span><span class="sxs-lookup"><span data-stu-id="06082-133">Last Name Hyperlink</span></span> | <span data-ttu-id="06082-134">Hipervínculo de fecha</span><span class="sxs-lookup"><span data-stu-id="06082-134">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="06082-135">Último nombre ascendente</span><span class="sxs-lookup"><span data-stu-id="06082-135">Last Name ascending</span></span>  | <span data-ttu-id="06082-136">descending</span><span class="sxs-lookup"><span data-stu-id="06082-136">descending</span></span>          | <span data-ttu-id="06082-137">ascending</span><span class="sxs-lookup"><span data-stu-id="06082-137">ascending</span></span>      |
| <span data-ttu-id="06082-138">Último nombre descendente</span><span class="sxs-lookup"><span data-stu-id="06082-138">Last Name descending</span></span> | <span data-ttu-id="06082-139">ascending</span><span class="sxs-lookup"><span data-stu-id="06082-139">ascending</span></span>           | <span data-ttu-id="06082-140">ascending</span><span class="sxs-lookup"><span data-stu-id="06082-140">ascending</span></span>      |
| <span data-ttu-id="06082-141">Fecha ascendente</span><span class="sxs-lookup"><span data-stu-id="06082-141">Date ascending</span></span>       | <span data-ttu-id="06082-142">ascending</span><span class="sxs-lookup"><span data-stu-id="06082-142">ascending</span></span>           | <span data-ttu-id="06082-143">descending</span><span class="sxs-lookup"><span data-stu-id="06082-143">descending</span></span>     |
| <span data-ttu-id="06082-144">Fecha descendente</span><span class="sxs-lookup"><span data-stu-id="06082-144">Date descending</span></span>      | <span data-ttu-id="06082-145">ascending</span><span class="sxs-lookup"><span data-stu-id="06082-145">ascending</span></span>           | <span data-ttu-id="06082-146">ascending</span><span class="sxs-lookup"><span data-stu-id="06082-146">ascending</span></span>      |

<span data-ttu-id="06082-147">El método usa LINQ to Entities para especificar la columna para ordenar por.</span><span class="sxs-lookup"><span data-stu-id="06082-147">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="06082-148">El código crea un `IQueryable` variable antes de la instrucción switch, se modifica en la instrucción switch y llama el `ToListAsync` método después de la `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="06082-148">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="06082-149">Al crear y modificar `IQueryable` variables, no hay ninguna consulta se envía a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="06082-149">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="06082-150">La consulta no se ejecuta hasta que convierta el `IQueryable` objeto en una colección mediante una llamada a un método como `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="06082-150">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="06082-151">Por lo tanto, este código produce una única consulta que no se ejecuta hasta que el `return View` instrucción.</span><span class="sxs-lookup"><span data-stu-id="06082-151">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="06082-152">Este código podría obtener detallado con un gran número de columnas.</span><span class="sxs-lookup"><span data-stu-id="06082-152">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="06082-153">[El último tutorial de esta serie](advanced.md#dynamic-linq) muestra cómo escribir código que le permite pasar el nombre de la `OrderBy` columna en una variable de cadena.</span><span class="sxs-lookup"><span data-stu-id="06082-153">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="06082-154">Agregar los hipervínculos del encabezado de columna a la vista de índice de estudiante</span><span class="sxs-lookup"><span data-stu-id="06082-154">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="06082-155">Reemplace el código de *Views/Students/Index.cshtml*, con el código siguiente para agregar los hipervínculos del encabezado de columna.</span><span class="sxs-lookup"><span data-stu-id="06082-155">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="06082-156">Se resaltan las líneas modificadas.</span><span class="sxs-lookup"><span data-stu-id="06082-156">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="06082-157">Este código usa la información de `ViewData` valores de cadena de propiedades para configurar hipervínculos con la consulta adecuada.</span><span class="sxs-lookup"><span data-stu-id="06082-157">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="06082-158">Ejecute la página y haga clic en el **Last Name** y **fecha de inscripción** funciona encabezados de columna para comprobar que la ordenación.</span><span class="sxs-lookup"><span data-stu-id="06082-158">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Página de índice de estudiantes en orden de nombres](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="06082-160">Agregue un cuadro de búsqueda a la página de índice de estudiantes</span><span class="sxs-lookup"><span data-stu-id="06082-160">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="06082-161">Para agregar el filtrado a la página de índice de estudiantes, podrá agregar un cuadro de texto y un botón de envío a la vista y aplicar los cambios correspondientes en el `Index` método.</span><span class="sxs-lookup"><span data-stu-id="06082-161">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="06082-162">El cuadro de texto le permitirá escribir una cadena que se busca en el nombre y los últimos campos de nombre.</span><span class="sxs-lookup"><span data-stu-id="06082-162">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="06082-163">Agregar funcionalidad de filtrado para el método de índice</span><span class="sxs-lookup"><span data-stu-id="06082-163">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="06082-164">En *StudentsController.cs*, reemplace el `Index` método por el código siguiente (se resaltan los cambios).</span><span class="sxs-lookup"><span data-stu-id="06082-164">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

<span data-ttu-id="06082-165">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]</span><span class="sxs-lookup"><span data-stu-id="06082-165">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]</span></span>

<span data-ttu-id="06082-166">Ha agregado un `searchString` parámetro para el `Index` método.</span><span class="sxs-lookup"><span data-stu-id="06082-166">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="06082-167">El valor de cadena de búsqueda se recibe de un cuadro de texto que agregará a la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="06082-167">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="06082-168">También ha agregado a la instrucción LINQ where cláusula que selecciona sólo los estudiantes cuyo apellido o apellidos contienen la cadena de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="06082-168">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="06082-169">La instrucción que agrega where cláusula se ejecuta sólo si hay un valor que se buscará.</span><span class="sxs-lookup"><span data-stu-id="06082-169">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="06082-170">Aquí se está llamando a la `Where` método en un `IQueryable` objeto y el filtro se procesarán en el servidor.</span><span class="sxs-lookup"><span data-stu-id="06082-170">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="06082-171">En algunos escenarios que podría llamando el `Where` método como un método de extensión en una colección en memoria.</span><span class="sxs-lookup"><span data-stu-id="06082-171">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="06082-172">(Por ejemplo, imagine que cambiar la referencia a `_context.Students` para que en lugar de un EF `DbSet` hace referencia a un método de repositorio que devuelve un `IEnumerable` colección.) El resultado podría ser normalmente el mismo, pero en algunos casos puede ser diferente.</span><span class="sxs-lookup"><span data-stu-id="06082-172">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="06082-173">Por ejemplo, la implementación de .NET Framework de la `Contains` método realiza una comparación entre mayúsculas y minúsculas de forma predeterminada, pero en SQL Server esto viene determinado por la configuración de intercalación de la instancia de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="06082-173">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="06082-174">Ese valor predeterminado es en mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="06082-174">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="06082-175">Podría llamar a la `ToUpper` método para realizar la prueba explícitamente entre mayúsculas y minúsculas: *donde (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="06082-175">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="06082-176">Que garantiza que resultados permanecen invariables, si cambia el código más adelante para usar un repositorio que devuelve un `IEnumerable` colección en lugar de un `IQueryable` objeto.</span><span class="sxs-lookup"><span data-stu-id="06082-176">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="06082-177">(Cuando se llama a la `Contains` método en un `IEnumerable` colección, obtendrá la implementación en .NET Framework; cuando se llama en un `IQueryable` de objeto, obtendrá la implementación del proveedor de base de datos.) Sin embargo, hay una reducción del rendimiento para esta solución.</span><span class="sxs-lookup"><span data-stu-id="06082-177">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="06082-178">La `ToUpper` código tendría que poner una función en la cláusula WHERE de la instrucción SELECT de TSQL.</span><span class="sxs-lookup"><span data-stu-id="06082-178">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="06082-179">Que evitaría que el optimizador utiliza un índice.</span><span class="sxs-lookup"><span data-stu-id="06082-179">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="06082-180">Dado que SQL principalmente se instala como entre mayúsculas y minúsculas, es mejor evitar la `ToUpper` código hasta que se migra a un almacén de datos distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="06082-180">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="06082-181">Agregue un cuadro de búsqueda a la vista de índice de estudiante</span><span class="sxs-lookup"><span data-stu-id="06082-181">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="06082-182">En *Views/Student/Index.cshtml*, agregue el código resaltado inmediatamente antes de que la apertura etiqueta de tabla para crear un título, un cuadro de texto y un **búsqueda** botón.</span><span class="sxs-lookup"><span data-stu-id="06082-182">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="06082-183">Este código usa el `<form>` [etiqueta auxiliar](https://docs.asp.net/en/latest/mvc/views/tag-helpers/intro.html) para agregar el cuadro de texto de búsqueda y el botón.</span><span class="sxs-lookup"><span data-stu-id="06082-183">This code uses the `<form>` [tag helper](https://docs.asp.net/en/latest/mvc/views/tag-helpers/intro.html) to add the search text box and button.</span></span> <span data-ttu-id="06082-184">De forma predeterminada, la `<form>` auxiliar de etiquetas envía datos de formulario con una entrada, lo que significa que se pasan en el cuerpo del mensaje HTTP y no en la dirección URL como las cadenas de consulta de parámetros.</span><span class="sxs-lookup"><span data-stu-id="06082-184">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="06082-185">Cuando se especifica HTTP GET, los datos del formulario se pasan en la dirección URL como cadenas de consulta, lo que permite a los usuarios marcar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="06082-185">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="06082-186">El W3C. se recomienda instrucciones que debe usar obtener cuando la acción no da como resultado una actualización.</span><span class="sxs-lookup"><span data-stu-id="06082-186">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="06082-187">Ejecute la página, escriba una cadena de búsqueda y haga clic en Buscar para comprobar que el filtrado está funcionando.</span><span class="sxs-lookup"><span data-stu-id="06082-187">Run the page, enter a search string, and click Search to verify that filtering is working.</span></span>

![Página de índice de los alumnos con filtrado](sort-filter-page/_static/filtering.png)

<span data-ttu-id="06082-189">Observe que la dirección URL contiene la cadena de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="06082-189">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="06082-190">Si Marque esta página, obtendrá la lista filtrada cuando se utiliza el marcador.</span><span class="sxs-lookup"><span data-stu-id="06082-190">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="06082-191">Agregar `method="get"` a la `form` etiqueta es la causa de la cadena de consulta que se genere.</span><span class="sxs-lookup"><span data-stu-id="06082-191">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="06082-192">En esta fase, si hace clic en un vínculo de ordenación del encabezado de columna se pierde el valor de filtro que especificó en el **búsqueda** cuadro.</span><span class="sxs-lookup"><span data-stu-id="06082-192">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="06082-193">Se corregirá en la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="06082-193">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="06082-194">Agregar funcionalidad de paginación a la página de índice de estudiantes</span><span class="sxs-lookup"><span data-stu-id="06082-194">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="06082-195">Para agregar paginación a la página de índice de estudiantes, podrá crear un `PaginatedList` clase que utiliza `Skip` y `Take` instrucciones para filtrar los datos en el servidor en lugar de recuperar siempre todas las filas de la tabla.</span><span class="sxs-lookup"><span data-stu-id="06082-195">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="06082-196">A continuación, podrá realizar cambios adicionales en el `Index` (método) y agregar botones de paginación para el `Index` vista.</span><span class="sxs-lookup"><span data-stu-id="06082-196">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="06082-197">La ilustración siguiente muestra los botones de paginación.</span><span class="sxs-lookup"><span data-stu-id="06082-197">The following illustration shows the paging buttons.</span></span>

![Página con vínculos de paginación de índice de estudiantes](sort-filter-page/_static/paging.png)

<span data-ttu-id="06082-199">En la carpeta del proyecto crea `PaginatedList.cs`y, a continuación, reemplace el código de plantilla con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="06082-199">In the project folder create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

<span data-ttu-id="06082-200">[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]</span><span class="sxs-lookup"><span data-stu-id="06082-200">[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]</span></span>

<span data-ttu-id="06082-201">El `CreateAsync` método en este código toma el tamaño de página y número de página y se aplica a la correspondiente `Skip` y `Take` instrucciones para el `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="06082-201">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="06082-202">Cuando `ToListAsync` se llama en el `IQueryable`, devolverá una lista que contiene solo la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="06082-202">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="06082-203">Las propiedades `HasPreviousPage` y `HasNextPage` puede usarse para habilitar o deshabilitar **anterior** y **siguiente** botones de paginación.</span><span class="sxs-lookup"><span data-stu-id="06082-203">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="06082-204">A `CreateAsync` método se utiliza en lugar de un constructor para crear el `PaginatedList<T>` objeto porque los constructores no pueden ejecutar código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="06082-204">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="06082-205">Agregar funcionalidad de paginación para el método de índice</span><span class="sxs-lookup"><span data-stu-id="06082-205">Add paging functionality to the Index method</span></span>

<span data-ttu-id="06082-206">En *StudentsController.cs*, reemplace el `Index` método por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="06082-206">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

<span data-ttu-id="06082-207">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]</span><span class="sxs-lookup"><span data-stu-id="06082-207">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]</span></span>

<span data-ttu-id="06082-208">Este código agrega un parámetro de número de página, un parámetro de criterio de ordenación actual y un parámetro de filtro actual para la firma del método.</span><span class="sxs-lookup"><span data-stu-id="06082-208">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="06082-209">La primera vez que se muestra la página, o si el usuario no ha hecho clic en una paginación u ordenación vínculo, todos los parámetros será nulos.</span><span class="sxs-lookup"><span data-stu-id="06082-209">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="06082-210">Si se hace clic en un vínculo de paginación, la variable de página contendrá el número de página para mostrar.</span><span class="sxs-lookup"><span data-stu-id="06082-210">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="06082-211">El `ViewData` elemento denominado CurrentSort proporciona la vista con el criterio de ordenación actual, porque esto debe estar incluida en los vínculos de paginación para mantener el criterio de ordenación de los mismos durante la paginación.</span><span class="sxs-lookup"><span data-stu-id="06082-211">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="06082-212">El `ViewData` elemento denominado FiltroActual ofrece la vista con la cadena de filtro actual.</span><span class="sxs-lookup"><span data-stu-id="06082-212">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="06082-213">Este valor debe estar incluido en los vínculos de paginación para mantener la configuración del filtro durante la paginación y deben restaurarse en el cuadro de texto cuando se vuelve a mostrar la página.</span><span class="sxs-lookup"><span data-stu-id="06082-213">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="06082-214">Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede dar lugar a datos diferente para mostrar.</span><span class="sxs-lookup"><span data-stu-id="06082-214">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="06082-215">Cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar, se cambia la cadena de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="06082-215">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="06082-216">En ese caso, el `searchString` parámetro no es null.</span><span class="sxs-lookup"><span data-stu-id="06082-216">In that case, the `searchString` parameter is not null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="06082-217">Al final de la `Index` método, el `PaginatedList.CreateAsync` método convierte la consulta de estudiante en una sola página de alumnos de un tipo de colección que admite la paginación.</span><span class="sxs-lookup"><span data-stu-id="06082-217">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="06082-218">Esa única página de estudiantes, a continuación, se pasa a la vista.</span><span class="sxs-lookup"><span data-stu-id="06082-218">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="06082-219">El `PaginatedList.CreateAsync` método toma un número de página.</span><span class="sxs-lookup"><span data-stu-id="06082-219">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="06082-220">Los dos signos de interrogación representar el operador de uso combinado de null.</span><span class="sxs-lookup"><span data-stu-id="06082-220">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="06082-221">El operador de uso combinado de null define un valor predeterminado para un tipo que acepta valores null; la expresión `(page ?? 1)` significa que devuelva el valor de `page` si tiene un valor, o devuelve 1 si `page` es null.</span><span class="sxs-lookup"><span data-stu-id="06082-221">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="06082-222">Agregar vínculos de paginación a la vista de índice de estudiante</span><span class="sxs-lookup"><span data-stu-id="06082-222">Add paging links to the Student Index view</span></span>

<span data-ttu-id="06082-223">En *Views/Students/Index.cshtml*, reemplace el código existente por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="06082-223">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="06082-224">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="06082-224">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="06082-225">El `@model` instrucción en la parte superior de la página especifica que la vista obtiene ahora un `PaginatedList<T>` en lugar del objeto un `List<T>` objeto.</span><span class="sxs-lookup"><span data-stu-id="06082-225">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="06082-226">Los vínculos de encabezado de columna use la cadena de consulta para pasar la cadena de búsqueda actual al controlador de modo que el usuario puede ordenar en los resultados del filtro:</span><span class="sxs-lookup"><span data-stu-id="06082-226">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="06082-227">Se muestran los botones de paginación mediante aplicaciones auxiliares de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="06082-227">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="06082-228">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="06082-228">Run the page.</span></span>

![Página con vínculos de paginación de índice de estudiantes](sort-filter-page/_static/paging.png)

<span data-ttu-id="06082-230">Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que funciona de paginación.</span><span class="sxs-lookup"><span data-stu-id="06082-230">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="06082-231">A continuación, escriba una cadena de búsqueda e inténtelo de paginación nuevo para comprobar que la paginación también la funciona correctamente con el filtrado y ordenación.</span><span class="sxs-lookup"><span data-stu-id="06082-231">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="06082-232">Crear una página acerca de que muestra estadísticas de estudiante</span><span class="sxs-lookup"><span data-stu-id="06082-232">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="06082-233">Para el sitio Web de Contoso Universidad **sobre** página, podrá mostrar alumnos cuántos han inscrito para cada fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="06082-233">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="06082-234">Esto requiere cálculos de agrupación y simples en los grupos.</span><span class="sxs-lookup"><span data-stu-id="06082-234">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="06082-235">Para ello, deberá llevar a cabo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="06082-235">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="06082-236">Cree una clase de modelo de vista para los datos que necesita para pasar a la vista.</span><span class="sxs-lookup"><span data-stu-id="06082-236">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="06082-237">Modifique el método acerca del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="06082-237">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="06082-238">Modificar la vista acerca de.</span><span class="sxs-lookup"><span data-stu-id="06082-238">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="06082-239">Crear el modelo de vista</span><span class="sxs-lookup"><span data-stu-id="06082-239">Create the view model</span></span>

<span data-ttu-id="06082-240">Crear un *SchoolViewModels* carpeta en el *modelos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="06082-240">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="06082-241">En la nueva carpeta, agregue un archivo de clase EnrollmentDateGroup.cs y reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="06082-241">In the new folder, add a class file EnrollmentDateGroup.cs and replace the template code with the following code:</span></span>

<span data-ttu-id="06082-242">[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]</span><span class="sxs-lookup"><span data-stu-id="06082-242">[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]</span></span>

### <a name="modify-the-home-controller"></a><span data-ttu-id="06082-243">Modifique el controlador Home</span><span class="sxs-lookup"><span data-stu-id="06082-243">Modify the Home Controller</span></span>

<span data-ttu-id="06082-244">En *HomeController.cs*, agregue las siguientes instrucciones using en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="06082-244">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

<span data-ttu-id="06082-245">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]</span><span class="sxs-lookup"><span data-stu-id="06082-245">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]</span></span>

<span data-ttu-id="06082-246">Agregue una variable de clase para el contexto de base de datos inmediatamente después de la llave de apertura para la clase y obtener una instancia del contexto de ASP.NET Core DI:</span><span class="sxs-lookup"><span data-stu-id="06082-246">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

<span data-ttu-id="06082-247">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]</span><span class="sxs-lookup"><span data-stu-id="06082-247">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]</span></span>

<span data-ttu-id="06082-248">Reemplace el método `About` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="06082-248">Replace the `About` method with the following code:</span></span>

<span data-ttu-id="06082-249">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]</span><span class="sxs-lookup"><span data-stu-id="06082-249">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]</span></span>

<span data-ttu-id="06082-250">La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula el número de entidades en cada grupo y almacena los resultados en una colección de `EnrollmentDateGroup` ver objetos del modelo.</span><span class="sxs-lookup"><span data-stu-id="06082-250">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="06082-251">En la 1.0 versión de Entity Framework Core, el conjunto de resultados completo se devuelve al cliente y agrupación se realiza en el cliente.</span><span class="sxs-lookup"><span data-stu-id="06082-251">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="06082-252">En algunos casos esto puede crear problemas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="06082-252">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="06082-253">Asegúrese de probar el rendimiento con los volúmenes de datos de producción y, si es necesario usar SQL sin formato para realizar la agrupación en el servidor.</span><span class="sxs-lookup"><span data-stu-id="06082-253">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="06082-254">Para obtener información sobre cómo usar SQL sin formato, vea [del último tutorial de esta serie](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="06082-254">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="06082-255">Modificar la vista</span><span class="sxs-lookup"><span data-stu-id="06082-255">Modify the About View</span></span>

<span data-ttu-id="06082-256">Reemplace el código de la *Views/Home/About.cshtml* archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="06082-256">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="06082-257">Ejecutar la aplicación y haga clic en el **sobre** vínculo.</span><span class="sxs-lookup"><span data-stu-id="06082-257">Run the app and click the **About** link.</span></span> <span data-ttu-id="06082-258">El número de alumnos para cada fecha de inscripción se muestra en una tabla.</span><span class="sxs-lookup"><span data-stu-id="06082-258">The count of students for each enrollment date is displayed in a table.</span></span>

![Acerca de la página](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="06082-260">Resumen</span><span class="sxs-lookup"><span data-stu-id="06082-260">Summary</span></span>

<span data-ttu-id="06082-261">En este tutorial ha visto cómo realizar la ordenación, filtrado, paginación y agrupación.</span><span class="sxs-lookup"><span data-stu-id="06082-261">In this tutorial you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="06082-262">En el tutorial siguiente aprenderá cómo controlar los cambios del modelo de datos mediante el uso de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="06082-262">In the next tutorial you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="06082-263">[Anterior](crud.md)
[Siguiente](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="06082-263">[Previous](crud.md)
[Next](migrations.md)</span></span>  
