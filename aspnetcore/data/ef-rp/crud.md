---
title: 'Páginas de Razor con EF Core en ASP.NET Core: CRUD (2 de 8)'
author: rick-anderson
description: Se muestra cómo crear, leer, actualizar y eliminar con EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 0a8d386104d9c62f37d0a94e66344d3f1e23bae9
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063356"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="a2812-103">Páginas de Razor con EF Core en ASP.NET Core: CRUD (2 de 8)</span><span class="sxs-lookup"><span data-stu-id="a2812-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a2812-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2812-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="a2812-105">En este tutorial, se revisa y personaliza el código CRUD (crear, leer, actualizar y eliminar) con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a2812-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="a2812-106">Para minimizar la complejidad y mantener estos tutoriales centrados en EF Core, en los modelos de página se usa código de EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2812-106">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="a2812-107">Algunos desarrolladores usan un patrón de capa o repositorio de servicio para crear una capa de abstracción entre la interfaz de usuario (las páginas de Razor) y la capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="a2812-108">En este tutorial, se examinan las Razor Pages Create, Edit, Delete y Details de la carpeta *Student*.</span><span class="sxs-lookup"><span data-stu-id="a2812-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are examined.</span></span>

<span data-ttu-id="a2812-109">En el código con scaffolding se usa el modelo siguiente para las páginas Create, Edit y Delete:</span><span class="sxs-lookup"><span data-stu-id="a2812-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="a2812-110">Obtenga y muestre los datos solicitados con el método HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="a2812-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="a2812-111">Guarde los cambios en los datos con el método HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a2812-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="a2812-112">Las páginas Index y Details obtienen y muestran los datos solicitados con el método HTTP GET `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="a2812-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="a2812-113">SingleOrDefaultAsync frente a FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="a2812-113">SingleOrDefaultAsync vs FirstOrDefaultAsync</span></span>

<span data-ttu-id="a2812-114">En el código generado se usa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), que normalmente es preferible a [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="a2812-114">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="a2812-115">`FirstOrDefaultAsync` es más eficaz que `SingleOrDefaultAsync` para capturar una entidad:</span><span class="sxs-lookup"><span data-stu-id="a2812-115">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="a2812-116">A menos que el código necesite comprobar que no hay más de una entidad devuelta por la consulta.</span><span class="sxs-lookup"><span data-stu-id="a2812-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="a2812-117">`SingleOrDefaultAsync` captura más datos y realiza trabajo innecesario.</span><span class="sxs-lookup"><span data-stu-id="a2812-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="a2812-118">`SingleOrDefaultAsync` inicia una excepción si hay más de una entidad que se ajuste a la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="a2812-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="a2812-119">`FirstOrDefaultAsync` no inicia una excepción si hay más de una entidad que se ajuste a la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="a2812-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>

### <a name="findasync"></a><span data-ttu-id="a2812-120">FindAsync</span><span class="sxs-lookup"><span data-stu-id="a2812-120">FindAsync</span></span>

<span data-ttu-id="a2812-121">En gran parte del código con scaffolding, se puede usar [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) en lugar de `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="a2812-121">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="a2812-122">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="a2812-122">`FindAsync`:</span></span>

* <span data-ttu-id="a2812-123">Busca una entidad con la clave principal (PK).</span><span class="sxs-lookup"><span data-stu-id="a2812-123">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="a2812-124">Si el contexto realiza el seguimiento de una entidad con la clave principal, se devuelve sin una solicitud a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-124">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="a2812-125">Es sencillo y conciso.</span><span class="sxs-lookup"><span data-stu-id="a2812-125">Is simple and concise.</span></span>
* <span data-ttu-id="a2812-126">Está optimizado para buscar una sola entidad.</span><span class="sxs-lookup"><span data-stu-id="a2812-126">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="a2812-127">Puede tener ventajas de rendimiento en algunas situaciones, pero rara vez se produce en aplicaciones web normales.</span><span class="sxs-lookup"><span data-stu-id="a2812-127">Can have perf benefits in some situations, but they rarely happens for typical web apps.</span></span>
* <span data-ttu-id="a2812-128">Usa implícitamente [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) en lugar de [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="a2812-128">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="a2812-129">Sin embargo, si quiere aplicar `Include` a otras entidades, `FindAsync` ya no resulta apropiado.</span><span class="sxs-lookup"><span data-stu-id="a2812-129">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="a2812-130">Esto significa que puede que necesite descartar `FindAsync` y cambiar a una consulta cuando la aplicación progrese.</span><span class="sxs-lookup"><span data-stu-id="a2812-130">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="a2812-131">Personalizar la página de detalles</span><span class="sxs-lookup"><span data-stu-id="a2812-131">Customize the Details page</span></span>

<span data-ttu-id="a2812-132">Vaya a la página `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="a2812-132">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="a2812-133">Los vínculos **Edit**, **Details** y **Delete** son generados por la [Aplicación auxiliar de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Students/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a2812-133">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="a2812-134">Ejecute la aplicación y haga clic en un vínculo **Details**.</span><span class="sxs-lookup"><span data-stu-id="a2812-134">Run the app and select a **Details** link.</span></span> <span data-ttu-id="a2812-135">La dirección URL tiene el formato `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="a2812-135">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="a2812-136">Se pasa Student ID mediante una cadena de consulta (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="a2812-136">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="a2812-137">Actualice las páginas de Razor Edit, Details y Delete para usar la plantilla de ruta `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="a2812-137">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="a2812-138">Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="a2812-138">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="a2812-139">Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya un valor de ruta entero devolverá un error HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="a2812-139">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="a2812-140">Por ejemplo, `http://localhost:5000/Students/Details` devuelve un error 404.</span><span class="sxs-lookup"><span data-stu-id="a2812-140">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="a2812-141">Para que el identificador sea opcional, anexe `?` a la restricción de ruta:</span><span class="sxs-lookup"><span data-stu-id="a2812-141">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="a2812-142">Ejecute la aplicación, haga clic en un vínculo Details y compruebe que la dirección URL pasa el identificador como datos de ruta ( `http://localhost:5000/Students/Details/2` ).</span><span class="sxs-lookup"><span data-stu-id="a2812-142">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="a2812-143">No cambie globalmente `@page` por `@page "{id:int}"`; esta acción rompería los vínculos a las páginas Home y Create.</span><span class="sxs-lookup"><span data-stu-id="a2812-143">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="a2812-144">Agregar datos relacionados</span><span class="sxs-lookup"><span data-stu-id="a2812-144">Add related data</span></span>

<span data-ttu-id="a2812-145">El código con scaffolding de la página Students Index no incluye la propiedad `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="a2812-145">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="a2812-146">En esta sección, se mostrará el contenido de la colección `Enrollments` en la página Details.</span><span class="sxs-lookup"><span data-stu-id="a2812-146">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="a2812-147">El método `OnGetAsync` de *Pages/Students/Details.cshtml.cs* usa el método `FirstOrDefaultAsync` para recuperar una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2812-147">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="a2812-148">Agregue el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2812-148">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="a2812-149">Los métodos [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) y [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) hacen que el contexto cargue la propiedad de navegación `Student.Enrollments` y, dentro de cada inscripción, la propiedad de navegación `Enrollment.Course`.</span><span class="sxs-lookup"><span data-stu-id="a2812-149">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="a2812-150">Estos métodos se examinan con detalle en el tutorial de lectura de datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="a2812-150">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="a2812-151">El método [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) mejora el rendimiento en casos en los que las entidades devueltas no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="a2812-151">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="a2812-152">`AsNoTracking` se describe posteriormente en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a2812-152">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="a2812-153">Mostrar las inscripciones relacionadas en la página Details</span><span class="sxs-lookup"><span data-stu-id="a2812-153">Display related enrollments on the Details page</span></span>

<span data-ttu-id="a2812-154">Abra *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a2812-154">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="a2812-155">Agregue el siguiente código resaltado para mostrar una lista de las inscripciones:</span><span class="sxs-lookup"><span data-stu-id="a2812-155">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="a2812-156">Si la sangría de código no es correcta después de pegar el código, presione CTRL-K-D para corregirlo.</span><span class="sxs-lookup"><span data-stu-id="a2812-156">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="a2812-157">El código anterior recorre en bucle las entidades de la propiedad de navegación `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="a2812-157">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="a2812-158">Para cada inscripción, se muestra el título del curso y la calificación.</span><span class="sxs-lookup"><span data-stu-id="a2812-158">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="a2812-159">El título del curso se recupera de la entidad Course almacenada en la propiedad de navegación `Course` de la entidad Enrollments.</span><span class="sxs-lookup"><span data-stu-id="a2812-159">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="a2812-160">Ejecute la aplicación, haga clic en la pestaña **Students** y después en el vínculo **Details** de un estudiante.</span><span class="sxs-lookup"><span data-stu-id="a2812-160">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="a2812-161">Se muestra la lista de cursos y calificaciones para el alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="a2812-161">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="a2812-162">Actualizar la página Create</span><span class="sxs-lookup"><span data-stu-id="a2812-162">Update the Create page</span></span>

<span data-ttu-id="a2812-163">Actualice el método `OnPostAsync` de *Pages/Students/Create.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2812-163">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a><span data-ttu-id="a2812-164">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="a2812-164">TryUpdateModelAsync</span></span>

<span data-ttu-id="a2812-165">Examine el código de [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):</span><span class="sxs-lookup"><span data-stu-id="a2812-165">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="a2812-166">En el código anterior, `TryUpdateModelAsync<Student>` intenta actualizar el objeto `emptyStudent` mediante los valores de formulario enviados desde la propiedad [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) del [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="a2812-166">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="a2812-167">`TryUpdateModelAsync` solo actualiza las propiedades enumeradas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="a2812-167">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="a2812-168">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="a2812-168">In the preceding sample:</span></span>

* <span data-ttu-id="a2812-169">El segundo argumento (`"student", // Prefix`) es el prefijo que se usa para buscar valores.</span><span class="sxs-lookup"><span data-stu-id="a2812-169">The second argument (`"student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="a2812-170">No distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a2812-170">It's not case sensitive.</span></span>
* <span data-ttu-id="a2812-171">Los valores de formulario enviados se convierten a los tipos del modelo `Student` mediante el [enlace de modelos](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="a2812-171">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>

### <a name="overposting"></a><span data-ttu-id="a2812-172">Publicación excesiva</span><span class="sxs-lookup"><span data-stu-id="a2812-172">Overposting</span></span>

<span data-ttu-id="a2812-173">El uso de `TryUpdateModel` para actualizar campos con valores enviados es un procedimiento recomendado de seguridad porque evita la publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="a2812-173">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="a2812-174">Por ejemplo, suponga que la entidad Student incluye una propiedad `Secret` que esta página web no debe actualizar ni agregar:</span><span class="sxs-lookup"><span data-stu-id="a2812-174">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="a2812-175">Incluso si la aplicación no tiene un campo `Secret` en la de página de Razor de creación o actualización, un hacker podría establecer el valor de `Secret` mediante publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="a2812-175">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="a2812-176">Un hacker podría usar una herramienta como Fiddler, o bien escribir código de JavaScript, para publicar un valor de formulario `Secret`.</span><span class="sxs-lookup"><span data-stu-id="a2812-176">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="a2812-177">El código original no limita los campos que el enlazador de modelos usa cuando crea una instancia Student.</span><span class="sxs-lookup"><span data-stu-id="a2812-177">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="a2812-178">El valor que haya especificado el hacker para el campo de formulario `Secret` se actualiza en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-178">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="a2812-179">En la imagen siguiente se muestra cómo la herramienta Fiddler agrega el campo `Secret` (con el valor "OverPost") a los valores de formulario enviados.</span><span class="sxs-lookup"><span data-stu-id="a2812-179">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Campo Secret agregado por Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="a2812-181">El valor "OverPost" se ha agregado correctamente a la propiedad `Secret` de la fila insertada.</span><span class="sxs-lookup"><span data-stu-id="a2812-181">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="a2812-182">El diseñador de aplicaciones no había previsto que la propiedad `Secret` se estableciera con la página Create.</span><span class="sxs-lookup"><span data-stu-id="a2812-182">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="a2812-183">Modelo de vista</span><span class="sxs-lookup"><span data-stu-id="a2812-183">View model</span></span>

<span data-ttu-id="a2812-184">Normalmente, un modelo de vista contiene un subconjunto de las propiedades incluidas en el modelo que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a2812-184">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="a2812-185">El modelo de aplicación se suele denominar modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="a2812-185">The application model is often called the domain model.</span></span> <span data-ttu-id="a2812-186">El modelo de dominio normalmente contiene todas las propiedades requeridas por la entidad correspondiente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-186">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="a2812-187">El modelo de vista contiene solo las propiedades necesarias para la capa de interfaz de usuario (por ejemplo, la página Create).</span><span class="sxs-lookup"><span data-stu-id="a2812-187">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="a2812-188">Además del modelo de vista, en algunas aplicaciones se usa un modelo de enlace o de entrada para pasar datos entre la clase del modelo de página de las páginas de Razor y el explorador.</span><span class="sxs-lookup"><span data-stu-id="a2812-188">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="a2812-189">Tenga en cuenta el modelo de vista `Student` siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2812-189">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="a2812-190">Los modelos de vista ofrecen una forma alternativa de evitar la publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="a2812-190">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="a2812-191">El modelo de vista contiene solo las propiedades que se van a ver (mostrar) o actualizar.</span><span class="sxs-lookup"><span data-stu-id="a2812-191">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="a2812-192">En el código siguiente se usa el modelo de vista `StudentVM` para crear un alumno:</span><span class="sxs-lookup"><span data-stu-id="a2812-192">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="a2812-193">El método [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) establece los valores de este objeto mediante la lectura de otro objeto [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="a2812-193">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="a2812-194">`SetValues` usa la coincidencia de nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="a2812-194">`SetValues` uses property name matching.</span></span> <span data-ttu-id="a2812-195">No es necesario que el tipo de modelo de vista esté relacionado con el tipo de modelo, basta con que tenga propiedades que coincidan.</span><span class="sxs-lookup"><span data-stu-id="a2812-195">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="a2812-196">El uso de `StudentVM` requiere que se actualice [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) para usar `StudentVM` en lugar de `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2812-196">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="a2812-197">En las páginas de Razor, la clase derivada `PageModel` es el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="a2812-197">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="a2812-198">Actualizar la página Edit</span><span class="sxs-lookup"><span data-stu-id="a2812-198">Update the Edit page</span></span>

<span data-ttu-id="a2812-199">Actualice el modelo de página para la página Edit.</span><span class="sxs-lookup"><span data-stu-id="a2812-199">Update the page model for the Edit page.</span></span> <span data-ttu-id="a2812-200">Los cambios más importantes aparecen resaltados:</span><span class="sxs-lookup"><span data-stu-id="a2812-200">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="a2812-201">Los cambios de código son similares a la página Create con algunas excepciones:</span><span class="sxs-lookup"><span data-stu-id="a2812-201">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="a2812-202">`OnPostAsync` tiene un parámetro `id` opcional.</span><span class="sxs-lookup"><span data-stu-id="a2812-202">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="a2812-203">El estudiante actual se obtiene de la base de datos, en lugar de crear un estudiante vacío.</span><span class="sxs-lookup"><span data-stu-id="a2812-203">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="a2812-204">`FirstOrDefaultAsync` se ha reemplazado con [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="a2812-204">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="a2812-205">`FindAsync` es una buena elección cuando se selecciona una entidad de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="a2812-205">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="a2812-206">Vea [FindAsync](#FindAsync) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a2812-206">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="a2812-207">Probar las páginas Edit y Create</span><span class="sxs-lookup"><span data-stu-id="a2812-207">Test the Edit and Create pages</span></span>

<span data-ttu-id="a2812-208">Cree y modifique algunas entidades Student.</span><span class="sxs-lookup"><span data-stu-id="a2812-208">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="a2812-209">Estados de entidad</span><span class="sxs-lookup"><span data-stu-id="a2812-209">Entity States</span></span>

<span data-ttu-id="a2812-210">El contexto de base de datos realiza el seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-210">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="a2812-211">La información de sincronización del contexto de base de datos determina qué ocurre cuando se llama a [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="a2812-211">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="a2812-212">Por ejemplo, cuando se pasa una entidad nueva al método [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync), el estado de esa entidad se establece en [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="a2812-212">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="a2812-213">Cuando se llama a `SaveChangesAsync`, el contexto de base de datos emite un comando INSERT de SQL.</span><span class="sxs-lookup"><span data-stu-id="a2812-213">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="a2812-214">Una entidad puede estar en uno de los [estados siguientes](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span><span class="sxs-lookup"><span data-stu-id="a2812-214">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="a2812-215">`Added`: la entidad no existe todavía en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-215">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="a2812-216">El método `SaveChanges` emite una instrucción INSERT.</span><span class="sxs-lookup"><span data-stu-id="a2812-216">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="a2812-217">`Unchanged`: no es necesario guardar cambios con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="a2812-217">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="a2812-218">Una entidad tiene este estado cuando se lee desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-218">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="a2812-219">`Modified`: se han modificado algunos o todos los valores de propiedad de la entidad.</span><span class="sxs-lookup"><span data-stu-id="a2812-219">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="a2812-220">El método `SaveChanges` emite una instrucción UPDATE.</span><span class="sxs-lookup"><span data-stu-id="a2812-220">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="a2812-221">`Deleted`: la entidad se ha marcado para su eliminación.</span><span class="sxs-lookup"><span data-stu-id="a2812-221">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="a2812-222">El método `SaveChanges` emite una instrucción DELETE.</span><span class="sxs-lookup"><span data-stu-id="a2812-222">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="a2812-223">`Detached`: el contexto de base de datos no está realizando el seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="a2812-223">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="a2812-224">En una aplicación de escritorio, los cambios de estado normalmente se establecen de forma automática.</span><span class="sxs-lookup"><span data-stu-id="a2812-224">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="a2812-225">Se lee una entidad, se realizan cambios y el estado de la entidad se cambia automáticamente a `Modified`.</span><span class="sxs-lookup"><span data-stu-id="a2812-225">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="a2812-226">La llamada a `SaveChanges` genera una instrucción UPDATE de SQL que solo actualiza las propiedades modificadas.</span><span class="sxs-lookup"><span data-stu-id="a2812-226">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="a2812-227">En una aplicación web, el `DbContext` que lee una entidad y muestra los datos se elimina después de representar una página.</span><span class="sxs-lookup"><span data-stu-id="a2812-227">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="a2812-228">Cuando se llama al método `OnPostAsync` de una página, se realiza una nueva solicitud web con una instancia nueva de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a2812-228">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="a2812-229">Volver a leer la entidad en ese contexto nuevo simula el procesamiento de escritorio.</span><span class="sxs-lookup"><span data-stu-id="a2812-229">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="a2812-230">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="a2812-230">Update the Delete page</span></span>

<span data-ttu-id="a2812-231">En esta sección, se agrega código para implementar un mensaje de error personalizado cuando se produce un error en la llamada a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a2812-231">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="a2812-232">Agregue una cadena para contener los posibles mensajes de error:</span><span class="sxs-lookup"><span data-stu-id="a2812-232">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="a2812-233">Reemplace el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2812-233">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="a2812-234">El código anterior contiene el parámetro opcional `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="a2812-234">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="a2812-235">`saveChangesError` indica si se llamó al método después de un error al eliminar el objeto Student.</span><span class="sxs-lookup"><span data-stu-id="a2812-235">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="a2812-236">Es posible que se produzca un error en la operación de eliminación debido a problemas de red transitorios.</span><span class="sxs-lookup"><span data-stu-id="a2812-236">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="a2812-237">Los errores de red transitorios son más probables en la nube.</span><span class="sxs-lookup"><span data-stu-id="a2812-237">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="a2812-238">`saveChangesError` es false cuando se llama a `OnGetAsync` de la página Delete desde la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a2812-238">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="a2812-239">Cuando `OnPostAsync` llama a `OnGetAsync` (debido a un error en la operación de eliminación), el parámetro `saveChangesError` es true.</span><span class="sxs-lookup"><span data-stu-id="a2812-239">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="a2812-240">El método OnPostAsync de las páginas Delete</span><span class="sxs-lookup"><span data-stu-id="a2812-240">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="a2812-241">Reemplace `OnPostAsync` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2812-241">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="a2812-242">En el código anterior se recupera la entidad seleccionada y después se llama al método [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) para establecer el estado de la entidad en `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="a2812-242">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="a2812-243">Cuando se llama a `SaveChanges`, se genera un comando DELETE de SQL.</span><span class="sxs-lookup"><span data-stu-id="a2812-243">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="a2812-244">Si se produce un error en `Remove`:</span><span class="sxs-lookup"><span data-stu-id="a2812-244">If `Remove` fails:</span></span>

* <span data-ttu-id="a2812-245">Se detecta la excepción de base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2812-245">The DB exception is caught.</span></span>
* <span data-ttu-id="a2812-246">Se llama al método `OnGetAsync` de las páginas Delete con `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="a2812-246">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="a2812-247">Actualizar la página de Razor Delete</span><span class="sxs-lookup"><span data-stu-id="a2812-247">Update the Delete Razor Page</span></span>

<span data-ttu-id="a2812-248">Agregue el siguiente mensaje de error resaltado a la página de Razor Delete.</span><span class="sxs-lookup"><span data-stu-id="a2812-248">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="a2812-249">Pruebe Delete.</span><span class="sxs-lookup"><span data-stu-id="a2812-249">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="a2812-250">Errores comunes</span><span class="sxs-lookup"><span data-stu-id="a2812-250">Common errors</span></span>

<span data-ttu-id="a2812-251">Student/Index u otros vínculos no funcionan:</span><span class="sxs-lookup"><span data-stu-id="a2812-251">Student/Index or other links don't work:</span></span>

<span data-ttu-id="a2812-252">Compruebe que la página de Razor contiene la directiva `@page` correcta.</span><span class="sxs-lookup"><span data-stu-id="a2812-252">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="a2812-253">Por ejemplo, la página de Razor Student/Index **no** debe contener una plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="a2812-253">For example, The Student/Index Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="a2812-254">Cada página de Razor debe incluir la directiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="a2812-254">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="a2812-255">[Anterior](xref:data/ef-rp/intro)
> [Siguiente](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="a2812-255">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
