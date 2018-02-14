---
title: "Páginas de Razor con EF Core: CRUD (2 de 8)"
author: rick-anderson
description: "Se muestra cómo crear, leer, actualizar y eliminar con EF Core"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="fc368-103">Crear, leer, actualizar y eliminar: EF Core con páginas de Razor (2 de 8)</span><span class="sxs-lookup"><span data-stu-id="fc368-103">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="fc368-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc368-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="fc368-105">En este tutorial, se revisa y personaliza el código CRUD (crear, leer, actualizar y eliminar) con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="fc368-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="fc368-106">Nota: Para minimizar la complejidad y mantener estos tutoriales centrados en EF Core, en los modelos de página de las páginas de Razor se usa código de EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc368-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="fc368-107">Algunos desarrolladores usan un patrón de capa o repositorio de servicio para crear una capa de abstracción entre la interfaz de usuario (las páginas de Razor) y la capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="fc368-108">En este tutorial, se modifican las páginas de Razor Create, Edit, Delete y Details de la carpeta *Student*.</span><span class="sxs-lookup"><span data-stu-id="fc368-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="fc368-109">En el código con scaffolding se usa el modelo siguiente para las páginas Create, Edit y Delete:</span><span class="sxs-lookup"><span data-stu-id="fc368-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="fc368-110">Obtenga y muestre los datos solicitados con el método HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="fc368-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="fc368-111">Guarde los cambios en los datos con el método HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="fc368-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="fc368-112">Las páginas Index y Details obtienen y muestran los datos solicitados con el método HTTP GET `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="fc368-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="fc368-113">Reemplazar SingleOrDefaultAsync por FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="fc368-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="fc368-114">En el código generado se usa [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) para capturar la entidad solicitada.</span><span class="sxs-lookup"><span data-stu-id="fc368-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="fc368-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) es más eficaz para capturar una entidad:</span><span class="sxs-lookup"><span data-stu-id="fc368-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="fc368-116">A menos que el código necesite comprobar que no hay más de una entidad devuelta por la consulta.</span><span class="sxs-lookup"><span data-stu-id="fc368-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="fc368-117">`SingleOrDefaultAsync` captura más datos y realiza trabajo innecesario.</span><span class="sxs-lookup"><span data-stu-id="fc368-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="fc368-118">`SingleOrDefaultAsync` inicia una excepción si hay más de una entidad que se ajuste a la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="fc368-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="fc368-119">`FirstOrDefaultAsync` no inicia una excepción si hay más de una entidad que se ajuste a la parte del filtro.</span><span class="sxs-lookup"><span data-stu-id="fc368-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="fc368-120">Reemplace globalmente `SingleOrDefaultAsync` con `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="fc368-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="fc368-121">`SingleOrDefaultAsync` se usa en cinco lugares:</span><span class="sxs-lookup"><span data-stu-id="fc368-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="fc368-122">`OnGetAsync` en la página Details.</span><span class="sxs-lookup"><span data-stu-id="fc368-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="fc368-123">`OnGetAsync` y `OnPostAsync` en las páginas Edit y Delete.</span><span class="sxs-lookup"><span data-stu-id="fc368-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="fc368-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="fc368-124">FindAsync</span></span>

<span data-ttu-id="fc368-125">En gran parte del código con scaffolding, se puede usar [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) en lugar de `FirstOrDefaultAsync` o `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="fc368-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="fc368-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="fc368-126">`FindAsync`:</span></span>

* <span data-ttu-id="fc368-127">Busca una entidad con la clave principal (PK).</span><span class="sxs-lookup"><span data-stu-id="fc368-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="fc368-128">Si el contexto realiza el seguimiento de una entidad con la clave principal, se devuelve sin una solicitud a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="fc368-129">Es sencillo y conciso.</span><span class="sxs-lookup"><span data-stu-id="fc368-129">Is simple and concise.</span></span>
* <span data-ttu-id="fc368-130">Está optimizado para buscar una sola entidad.</span><span class="sxs-lookup"><span data-stu-id="fc368-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="fc368-131">Puede tener ventajas de rendimiento en algunas situaciones, pero rara vez entra en juego para escenarios web normales.</span><span class="sxs-lookup"><span data-stu-id="fc368-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="fc368-132">Usa implícitamente [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) en lugar de [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="fc368-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="fc368-133">Pero si quiere incluir otras entidades, Find ya no resulta apropiado.</span><span class="sxs-lookup"><span data-stu-id="fc368-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="fc368-134">Esto significa que puede que necesite descartar Find y cambiar a una consulta cuando la aplicación progrese.</span><span class="sxs-lookup"><span data-stu-id="fc368-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="fc368-135">Personalizar la página de detalles</span><span class="sxs-lookup"><span data-stu-id="fc368-135">Customize the Details page</span></span>

<span data-ttu-id="fc368-136">Vaya a la página `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="fc368-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="fc368-137">Los vínculos **Edit**, **Details** y **Delete** son generados por la [Aplicación auxiliar de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Students/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc368-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="fc368-138">Haga clic en un vínculo Details.</span><span class="sxs-lookup"><span data-stu-id="fc368-138">Select a Details link.</span></span> <span data-ttu-id="fc368-139">La dirección URL tiene el formato `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="fc368-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="fc368-140">Se pasa Student ID mediante una cadena de consulta (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="fc368-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="fc368-141">Actualice las páginas de Razor Edit, Details y Delete para usar la plantilla de ruta `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="fc368-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="fc368-142">Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="fc368-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="fc368-143">Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya un valor de ruta entero devolverá un error HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="fc368-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="fc368-144">Por ejemplo, `http://localhost:5000/Students/Details` devuelve un error 404.</span><span class="sxs-lookup"><span data-stu-id="fc368-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="fc368-145">Para que el identificador sea opcional, anexe `?` a la restricción de ruta:</span><span class="sxs-lookup"><span data-stu-id="fc368-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="fc368-146">Ejecute la aplicación, haga clic en un vínculo Details y compruebe que la dirección URL pasa el identificador como datos de ruta (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="fc368-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="fc368-147">No cambie globalmente `@page` por `@page "{id:int}"`; esta acción rompería los vínculos a las páginas Home y Create.</span><span class="sxs-lookup"><span data-stu-id="fc368-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="fc368-148">Agregar datos relacionados</span><span class="sxs-lookup"><span data-stu-id="fc368-148">Add related data</span></span>

<span data-ttu-id="fc368-149">El código con scaffolding de la página Students Index no incluye la propiedad `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="fc368-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="fc368-150">En esta sección, se mostrará el contenido de la colección `Enrollments` en la página Details.</span><span class="sxs-lookup"><span data-stu-id="fc368-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="fc368-151">El método `OnGetAsync` de *Pages/Students/Details.cshtml.cs* usa el método `FirstOrDefaultAsync` para recuperar una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="fc368-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="fc368-152">Agregue el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc368-152">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="fc368-153">Los métodos `Include` y `ThenInclude` hacen que el contexto cargue la propiedad de navegación `Student.Enrollments` y, dentro de cada inscripción, la propiedad de navegación `Enrollment.Course`.</span><span class="sxs-lookup"><span data-stu-id="fc368-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="fc368-154">Estos métodos se examinan con detalle en el tutorial de lectura de datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="fc368-154">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="fc368-155">El método `AsNoTracking` mejora el rendimiento en casos en los que las entidades devueltas no se actualizan en el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="fc368-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="fc368-156">`AsNoTracking` se describe posteriormente en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="fc368-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="fc368-157">Mostrar las inscripciones relacionadas en la página Details</span><span class="sxs-lookup"><span data-stu-id="fc368-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="fc368-158">Abra *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc368-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="fc368-159">Agregue el siguiente código resaltado para mostrar una lista de las inscripciones:</span><span class="sxs-lookup"><span data-stu-id="fc368-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="fc368-160">Si la sangría de código no es correcta después de pegar el código, presione CTRL-K-D para corregirlo.</span><span class="sxs-lookup"><span data-stu-id="fc368-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="fc368-161">El código anterior recorre en bucle las entidades de la propiedad de navegación `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="fc368-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="fc368-162">Para cada inscripción, se muestra el título del curso y la calificación.</span><span class="sxs-lookup"><span data-stu-id="fc368-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="fc368-163">El título del curso se recupera de la entidad Course almacenada en la propiedad de navegación `Course` de la entidad Enrollments.</span><span class="sxs-lookup"><span data-stu-id="fc368-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="fc368-164">Ejecute la aplicación, haga clic en la pestaña **Students** y después en el vínculo **Details** de un estudiante.</span><span class="sxs-lookup"><span data-stu-id="fc368-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="fc368-165">Se muestra la lista de cursos y calificaciones para el alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="fc368-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="fc368-166">Actualizar la página Create</span><span class="sxs-lookup"><span data-stu-id="fc368-166">Update the Create page</span></span>

<span data-ttu-id="fc368-167">Actualice el método `OnPostAsync` de *Pages/Students/Create.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc368-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="fc368-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="fc368-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="fc368-169">Examine el código de [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):</span><span class="sxs-lookup"><span data-stu-id="fc368-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="fc368-170">En el código anterior, `TryUpdateModelAsync<Student>` intenta actualizar el objeto `emptyStudent` mediante los valores de formulario enviados desde la propiedad [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) del [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="fc368-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="fc368-171">`TryUpdateModelAsync` solo actualiza las propiedades enumeradas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="fc368-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="fc368-172">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="fc368-172">In the preceding sample:</span></span>

* <span data-ttu-id="fc368-173">El segundo argumento (` "student", // Prefix`) es el prefijo que se usa para buscar valores.</span><span class="sxs-lookup"><span data-stu-id="fc368-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="fc368-174">No distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fc368-174">It's not case sensitive.</span></span>
* <span data-ttu-id="fc368-175">Los valores de formulario enviados se convierten a los tipos del modelo `Student` mediante el [enlace de modelos](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="fc368-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="fc368-176">Publicación excesiva</span><span class="sxs-lookup"><span data-stu-id="fc368-176">Overposting</span></span>

<span data-ttu-id="fc368-177">El uso de `TryUpdateModel` para actualizar campos con valores enviados es un procedimiento recomendado de seguridad porque evita la publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="fc368-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="fc368-178">Por ejemplo, suponga que la entidad Student incluye una propiedad `Secret` que esta página web no debe actualizar ni agregar:</span><span class="sxs-lookup"><span data-stu-id="fc368-178">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="fc368-179">Incluso si la aplicación no tiene un campo `Secret` en la de página de Razor de creación o actualización, un hacker podría establecer el valor de `Secret` mediante publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="fc368-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="fc368-180">Un hacker podría usar una herramienta como Fiddler, o bien escribir código de JavaScript, para publicar un valor de formulario `Secret`.</span><span class="sxs-lookup"><span data-stu-id="fc368-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="fc368-181">El código original no limita los campos que el enlazador de modelos usa cuando crea una instancia Student.</span><span class="sxs-lookup"><span data-stu-id="fc368-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="fc368-182">El valor que haya especificado el hacker para el campo de formulario `Secret` se actualiza en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="fc368-183">En la imagen siguiente se muestra cómo la herramienta Fiddler agrega el campo `Secret` (con el valor "OverPost") a los valores de formulario enviados.</span><span class="sxs-lookup"><span data-stu-id="fc368-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Campo Secret agregado por Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="fc368-185">El valor "OverPost" se ha agregado correctamente a la propiedad `Secret` de la fila insertada.</span><span class="sxs-lookup"><span data-stu-id="fc368-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="fc368-186">El diseñador de aplicaciones no había previsto que la propiedad `Secret` se estableciera con la página Create.</span><span class="sxs-lookup"><span data-stu-id="fc368-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="fc368-187">Modelo de vista</span><span class="sxs-lookup"><span data-stu-id="fc368-187">View model</span></span>

<span data-ttu-id="fc368-188">Normalmente, un modelo de vista contiene un subconjunto de las propiedades incluidas en el modelo que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc368-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="fc368-189">El modelo de aplicación se suele denominar modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="fc368-189">The application model is often called the domain model.</span></span> <span data-ttu-id="fc368-190">El modelo de dominio normalmente contiene todas las propiedades requeridas por la entidad correspondiente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="fc368-191">El modelo de vista contiene solo las propiedades necesarias para la capa de interfaz de usuario (por ejemplo, la página Create).</span><span class="sxs-lookup"><span data-stu-id="fc368-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="fc368-192">Además del modelo de vista, en algunas aplicaciones se usa un modelo de enlace o de entrada para pasar datos entre la clase del modelo de página de las páginas de Razor y el explorador.</span><span class="sxs-lookup"><span data-stu-id="fc368-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="fc368-193">Tenga en cuenta el modelo de vista `Student` siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc368-193">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="fc368-194">Los modelos de vista ofrecen una forma alternativa de evitar la publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="fc368-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="fc368-195">El modelo de vista contiene solo las propiedades que se van a ver (mostrar) o actualizar.</span><span class="sxs-lookup"><span data-stu-id="fc368-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="fc368-196">En el código siguiente se usa el modelo de vista `StudentVM` para crear un alumno:</span><span class="sxs-lookup"><span data-stu-id="fc368-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="fc368-197">El método [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) establece los valores de este objeto mediante la lectura de otro objeto [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="fc368-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="fc368-198">`SetValues` usa la coincidencia de nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="fc368-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="fc368-199">No es necesario que el tipo de modelo de vista esté relacionado con el tipo de modelo, basta con que tenga propiedades que coincidan.</span><span class="sxs-lookup"><span data-stu-id="fc368-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="fc368-200">El uso de `StudentVM` requiere que se actualice [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) para usar `StudentVM` en lugar de `Student`.</span><span class="sxs-lookup"><span data-stu-id="fc368-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="fc368-201">En las páginas de Razor, la clase derivada `PageModel` es el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="fc368-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="fc368-202">Actualizar la página Edit</span><span class="sxs-lookup"><span data-stu-id="fc368-202">Update the Edit page</span></span>

<span data-ttu-id="fc368-203">Actualice el modelo de página para la página Edit:</span><span class="sxs-lookup"><span data-stu-id="fc368-203">Update the page model for the Edit page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="fc368-204">Los cambios de código son similares a la página Create con algunas excepciones:</span><span class="sxs-lookup"><span data-stu-id="fc368-204">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="fc368-205">`OnPostAsync` tiene un parámetro `id` opcional.</span><span class="sxs-lookup"><span data-stu-id="fc368-205">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="fc368-206">El estudiante actual se obtiene de la base de datos, en lugar de crear un estudiante vacío.</span><span class="sxs-lookup"><span data-stu-id="fc368-206">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="fc368-207">`FirstOrDefaultAsync` se ha reemplazado con [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="fc368-207">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="fc368-208">`FindAsync` es una buena elección cuando se selecciona una entidad de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="fc368-208">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="fc368-209">Vea [FindAsync](#FindAsync) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="fc368-209">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="fc368-210">Probar las páginas Edit y Create</span><span class="sxs-lookup"><span data-stu-id="fc368-210">Test the Edit and Create pages</span></span>

<span data-ttu-id="fc368-211">Cree y modifique algunas entidades Student.</span><span class="sxs-lookup"><span data-stu-id="fc368-211">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="fc368-212">Estados de entidad</span><span class="sxs-lookup"><span data-stu-id="fc368-212">Entity States</span></span>

<span data-ttu-id="fc368-213">El contexto de base de datos realiza el seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-213">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="fc368-214">La información de sincronización del contexto de base de datos determina qué ocurre cuando se llama a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="fc368-214">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="fc368-215">Por ejemplo, cuando se pasa una nueva entidad al método `Add`, el estado de esa entidad se establece en `Added`.</span><span class="sxs-lookup"><span data-stu-id="fc368-215">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="fc368-216">Cuando se llama a `SaveChanges`, el contexto de base de datos emite un comando INSERT de SQL.</span><span class="sxs-lookup"><span data-stu-id="fc368-216">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="fc368-217">Una entidad puede estar en uno de los estados siguientes:</span><span class="sxs-lookup"><span data-stu-id="fc368-217">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="fc368-218">`Added`: la entidad no existe todavía en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-218">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="fc368-219">El método `SaveChanges` emite una instrucción INSERT.</span><span class="sxs-lookup"><span data-stu-id="fc368-219">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="fc368-220">`Unchanged`: no es necesario guardar cambios con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="fc368-220">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="fc368-221">Una entidad tiene este estado cuando se lee desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-221">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="fc368-222">`Modified`: se han modificado algunos o todos los valores de propiedad de la entidad.</span><span class="sxs-lookup"><span data-stu-id="fc368-222">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="fc368-223">El método `SaveChanges` emite una instrucción UPDATE.</span><span class="sxs-lookup"><span data-stu-id="fc368-223">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="fc368-224">`Deleted`: la entidad se ha marcado para su eliminación.</span><span class="sxs-lookup"><span data-stu-id="fc368-224">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="fc368-225">El método `SaveChanges` emite una instrucción DELETE.</span><span class="sxs-lookup"><span data-stu-id="fc368-225">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="fc368-226">`Detached`: el contexto de base de datos no está realizando el seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="fc368-226">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="fc368-227">En una aplicación de escritorio, los cambios de estado normalmente se establecen de forma automática.</span><span class="sxs-lookup"><span data-stu-id="fc368-227">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="fc368-228">Se lee una entidad, se realizan cambios y el estado de la entidad se cambia automáticamente a `Modified`.</span><span class="sxs-lookup"><span data-stu-id="fc368-228">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="fc368-229">La llamada a `SaveChanges` genera una instrucción UPDATE de SQL que solo actualiza las propiedades modificadas.</span><span class="sxs-lookup"><span data-stu-id="fc368-229">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="fc368-230">En una aplicación web, el `DbContext` que lee una entidad y muestra los datos se elimina después de representar una página.</span><span class="sxs-lookup"><span data-stu-id="fc368-230">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="fc368-231">Cuando se llama al método `OnPostAsync` de una página, se realiza una nueva solicitud web con una instancia nueva de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fc368-231">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="fc368-232">Volver a leer la entidad en ese contexto nuevo simula el procesamiento de escritorio.</span><span class="sxs-lookup"><span data-stu-id="fc368-232">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="fc368-233">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="fc368-233">Update the Delete page</span></span>

<span data-ttu-id="fc368-234">En esta sección, se agrega código para implementar un mensaje de error personalizado cuando se produce un error en la llamada a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="fc368-234">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="fc368-235">Agregue una cadena para contener los posibles mensajes de error:</span><span class="sxs-lookup"><span data-stu-id="fc368-235">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="fc368-236">Reemplace el método `OnGetAsync` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc368-236">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="fc368-237">El código anterior contiene el parámetro opcional `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="fc368-237">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="fc368-238">`saveChangesError` indica si se llamó al método después de un error al eliminar el objeto Student.</span><span class="sxs-lookup"><span data-stu-id="fc368-238">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="fc368-239">Es posible que se produzca un error en la operación de eliminación debido a problemas de red transitorios.</span><span class="sxs-lookup"><span data-stu-id="fc368-239">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="fc368-240">Los errores de red transitorios son más probables en la nube.</span><span class="sxs-lookup"><span data-stu-id="fc368-240">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="fc368-241">`saveChangesError` es false cuando se llama a `OnGetAsync` de la página Delete desde la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="fc368-241">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="fc368-242">Cuando `OnPostAsync` llama a `OnGetAsync` (debido a un error en la operación de eliminación), el parámetro `saveChangesError` es true.</span><span class="sxs-lookup"><span data-stu-id="fc368-242">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="fc368-243">El método OnPostAsync de las páginas Delete</span><span class="sxs-lookup"><span data-stu-id="fc368-243">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="fc368-244">Reemplace `OnPostAsync` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc368-244">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="fc368-245">En el código anterior se recupera la entidad seleccionada y después se llama al método `Remove` para establecer el estado de la entidad en `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="fc368-245">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="fc368-246">Cuando se llama a `SaveChanges`, se genera un comando DELETE de SQL.</span><span class="sxs-lookup"><span data-stu-id="fc368-246">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="fc368-247">Si se produce un error en `Remove`:</span><span class="sxs-lookup"><span data-stu-id="fc368-247">If `Remove` fails:</span></span>

* <span data-ttu-id="fc368-248">Se detecta la excepción de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc368-248">The DB exception is caught.</span></span>
* <span data-ttu-id="fc368-249">Se llama al método `OnGetAsync` de las páginas Delete con `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="fc368-249">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="fc368-250">Actualizar la página de Razor Delete</span><span class="sxs-lookup"><span data-stu-id="fc368-250">Update the Delete Razor Page</span></span>

<span data-ttu-id="fc368-251">Agregue el siguiente mensaje de error resaltado a la página de Razor Delete.</span><span class="sxs-lookup"><span data-stu-id="fc368-251">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="fc368-252">Pruebe Delete.</span><span class="sxs-lookup"><span data-stu-id="fc368-252">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="fc368-253">Errores comunes</span><span class="sxs-lookup"><span data-stu-id="fc368-253">Common errors</span></span>

<span data-ttu-id="fc368-254">Student/Home u otros vínculos no funcionan:</span><span class="sxs-lookup"><span data-stu-id="fc368-254">Student/Home or other links don't work:</span></span>

<span data-ttu-id="fc368-255">Compruebe que la página de Razor contiene la directiva `@page` correcta.</span><span class="sxs-lookup"><span data-stu-id="fc368-255">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="fc368-256">Por ejemplo, la página de Razor Student/Home **no** debe contener una plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="fc368-256">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="fc368-257">Cada página de Razor debe incluir la directiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="fc368-257">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fc368-258">[Anterior](xref:data/ef-rp/intro)
[Siguiente](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="fc368-258">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
