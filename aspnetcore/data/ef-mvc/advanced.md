---
title: 'ASP.NET Core MVC con EF Core: Avanzado (10 de 10)'
author: rick-anderson
description: En este tutorial se presentan varios temas que le serán de utilidad cuando quiera ir más allá de los conceptos básicos del desarrollo de aplicaciones web ASP.NET Core que usan Entity Framework Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/advanced
ms.openlocfilehash: be44ef115ce72e1571bbdea2c609ea6c53792c59
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093080"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a><span data-ttu-id="0410c-103">ASP.NET Core MVC con EF Core: Avanzado (10 de 10)</span><span class="sxs-lookup"><span data-stu-id="0410c-103">ASP.NET Core MVC with EF Core - Advanced - 10 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0410c-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0410c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0410c-105">En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web de ASP.NET Core MVC con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0410c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="0410c-106">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="0410c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="0410c-107">En el tutorial anterior, se implementó la herencia de tabla por jerarquía.</span><span class="sxs-lookup"><span data-stu-id="0410c-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="0410c-108">En este tutorial se presentan varios temas que es importante tener en cuenta cuando va más allá de los conceptos básicos del desarrollo de aplicaciones web ASP.NET Core que usan Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0410c-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="0410c-109">Consultas SQL sin formato</span><span class="sxs-lookup"><span data-stu-id="0410c-109">Raw SQL Queries</span></span>

<span data-ttu-id="0410c-110">Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado estrechamente a un método concreto de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="0410c-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="0410c-111">Lo consigue mediante la generación de consultas SQL y comandos, lo que también le evita tener que escribirlos usted mismo.</span><span class="sxs-lookup"><span data-stu-id="0410c-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="0410c-112">Pero hay situaciones excepcionales en las que necesita ejecutar consultas específicas de SQL que ha creado manualmente.</span><span class="sxs-lookup"><span data-stu-id="0410c-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="0410c-113">En estos casos, la API de Entity Framework Code First incluye métodos que le permiten pasar comandos SQL directamente a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0410c-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="0410c-114">En EF Core 1.0 dispone de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="0410c-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="0410c-115">Use el método `DbSet.FromSql` para las consultas que devuelven tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="0410c-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="0410c-116">Los objetos devueltos deben ser del tipo esperado por el objeto `DbSet` y se les realiza automáticamente un seguimiento mediante el contexto de base de datos a menos que se [desactive el seguimiento](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="0410c-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="0410c-117">Use `Database.ExecuteSqlCommand` para comandos sin consulta.</span><span class="sxs-lookup"><span data-stu-id="0410c-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="0410c-118">Si tiene que ejecutar una consulta que devuelve tipos que no son entidades, puede usar ADO.NET con la conexión de base de datos proporcionada por EF.</span><span class="sxs-lookup"><span data-stu-id="0410c-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="0410c-119">No se realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si usa este método para recuperar tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="0410c-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="0410c-120">Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques por inyección de código SQL.</span><span class="sxs-lookup"><span data-stu-id="0410c-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="0410c-121">Una manera de hacerlo es mediante consultas parametrizadas para asegurarse de que las cadenas enviadas por una página web no se pueden interpretar como comandos SQL.</span><span class="sxs-lookup"><span data-stu-id="0410c-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="0410c-122">En este tutorial usará las consultas con parámetros al integrar la entrada de usuario en una consulta.</span><span class="sxs-lookup"><span data-stu-id="0410c-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="0410c-123">Llamar a una consulta que devuelve entidades</span><span class="sxs-lookup"><span data-stu-id="0410c-123">Call a query that returns entities</span></span>

<span data-ttu-id="0410c-124">La clase `DbSet<TEntity>` proporciona un método que puede usar para ejecutar una consulta que devuelve una entidad de tipo `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="0410c-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="0410c-125">Para ver cómo funciona, cambiará el código en el método `Details` del controlador de departamento.</span><span class="sxs-lookup"><span data-stu-id="0410c-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="0410c-126">En *DepartmentsController.cs*, en el método `Details`, reemplace el código que recupera un departamento con una llamada al método `FromSql`, como se muestra en el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="0410c-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="0410c-127">Para comprobar que el nuevo código funciona correctamente, seleccione la pestaña **Departments** y, después, **Details** para uno de los departamentos.</span><span class="sxs-lookup"><span data-stu-id="0410c-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Detalles del departamento](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="0410c-129">Llamar a una consulta que devuelve otros tipos</span><span class="sxs-lookup"><span data-stu-id="0410c-129">Call a query that returns other types</span></span>

<span data-ttu-id="0410c-130">Anteriormente creó una cuadrícula de estadísticas de alumno de la página About que mostraba el número de alumnos para cada fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="0410c-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="0410c-131">Obtuvo los datos del conjunto de entidades Students (`_context.Students`) y usó LINQ para proyectar los resultados en una lista de objetos de modelo de vista `EnrollmentDateGroup`.</span><span class="sxs-lookup"><span data-stu-id="0410c-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="0410c-132">Suponga que quiere escribir la instrucción SQL propia en lugar de usar LINQ.</span><span class="sxs-lookup"><span data-stu-id="0410c-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="0410c-133">Para ello, necesita ejecutar una consulta SQL que devuelve un valor distinto de objetos entidad.</span><span class="sxs-lookup"><span data-stu-id="0410c-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="0410c-134">En EF Core 1.0, una manera de hacerlo es escribir código de ADO.NET y obtener la conexión de base de datos de EF.</span><span class="sxs-lookup"><span data-stu-id="0410c-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="0410c-135">En *HomeController.cs*, reemplace el método `About` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0410c-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="0410c-136">Agregue una instrucción using:</span><span class="sxs-lookup"><span data-stu-id="0410c-136">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="0410c-137">Ejecute la aplicación y vaya a la página About.</span><span class="sxs-lookup"><span data-stu-id="0410c-137">Run the app and go to the About page.</span></span> <span data-ttu-id="0410c-138">Muestra los mismos datos que anteriormente.</span><span class="sxs-lookup"><span data-stu-id="0410c-138">It displays the same data it did before.</span></span>

![Página About](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="0410c-140">Llamar a una consulta update</span><span class="sxs-lookup"><span data-stu-id="0410c-140">Call an update query</span></span>

<span data-ttu-id="0410c-141">Imagine que los administradores de Contoso University quieren realizar cambios globales en la base de datos, como cambiar el número de créditos para cada curso.</span><span class="sxs-lookup"><span data-stu-id="0410c-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="0410c-142">Si la universidad tiene un gran número de cursos, sería poco eficaz recuperarlos todos como entidades y cambiarlos de forma individual.</span><span class="sxs-lookup"><span data-stu-id="0410c-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="0410c-143">En esta sección implementará una página web que permite al usuario especificar un factor por el cual se va a cambiar el número de créditos para todos los cursos y podrá realizar el cambio mediante la ejecución de una instrucción UPDATE de SQL.</span><span class="sxs-lookup"><span data-stu-id="0410c-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="0410c-144">La página web tendrá el mismo aspecto que la ilustración siguiente:</span><span class="sxs-lookup"><span data-stu-id="0410c-144">The web page will look like the following illustration:</span></span>

![Página Update Course Credits](advanced/_static/update-credits.png)

<span data-ttu-id="0410c-146">En *CoursesContoller.cs*, agregue los métodos UpdateCourseCredits para HttpGet y HttpPost:</span><span class="sxs-lookup"><span data-stu-id="0410c-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="0410c-147">Cuando el controlador procesa una solicitud HttpGet, no se devuelve nada en `ViewData["RowsAffected"]` y la vista muestra un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.</span><span class="sxs-lookup"><span data-stu-id="0410c-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="0410c-148">Cuando se hace clic en el botón **Update**, se llama al método HttpPost y el multiplicador tiene el valor especificado en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="0410c-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="0410c-149">A continuación, el código ejecuta la instrucción SQL que actualiza los cursos y devuelve el número de filas afectadas a la vista en `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="0410c-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="0410c-150">Cuando la vista obtiene un valor `RowsAffected`, muestra el número de filas actualizadas.</span><span class="sxs-lookup"><span data-stu-id="0410c-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="0410c-151">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Views/Courses* y luego haga clic en **Agregar > Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="0410c-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="0410c-152">En el cuadro de diálogo **Agregar nuevo elemento**, haga clic en **ASP.NET** en **Instalado** en el panel izquierdo, haga clic en **Página de la vista de MVC** y nombre la nueva vista *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0410c-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="0410c-153">En *Views/Courses/UpdateCourseCredits.cshtml*, reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0410c-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="0410c-154">Ejecute el método `UpdateCourseCredits` seleccionando la pestaña **Courses**, después, agregue "/UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="0410c-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="0410c-155">Escriba un número en el cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="0410c-155">Enter a number in the text box:</span></span>

![Página Update Course Credits](advanced/_static/update-credits.png)

<span data-ttu-id="0410c-157">Haga clic en **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="0410c-157">Click **Update**.</span></span> <span data-ttu-id="0410c-158">Verá el número de filas afectadas:</span><span class="sxs-lookup"><span data-stu-id="0410c-158">You see the number of rows affected:</span></span>

![Filas afectadas de la página Update Course Credits](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="0410c-160">Haga clic en **Volver a la lista** para ver la lista de cursos con el número de créditos revisado.</span><span class="sxs-lookup"><span data-stu-id="0410c-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="0410c-161">Tenga en cuenta que el código de producción garantiza que las actualizaciones siempre dan como resultado datos válidos.</span><span class="sxs-lookup"><span data-stu-id="0410c-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="0410c-162">El código simplificado que se muestra a continuación podría multiplicar el número de créditos lo suficiente para que el resultado sea un número superior a 5.</span><span class="sxs-lookup"><span data-stu-id="0410c-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="0410c-163">(La propiedad `Credits` tiene un atributo `[Range(0, 5)]`). La consulta update podría funcionar, pero los datos no válidos podrían provocar resultados inesperados en otras partes del sistema que asumen que el número de créditos es igual o inferior a 5.</span><span class="sxs-lookup"><span data-stu-id="0410c-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="0410c-164">Para obtener más información sobre las consultas SQL básicas, vea [Consultas SQL básicas](https://docs.microsoft.com/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="0410c-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="0410c-165">Examinar el código SQL enviado a la base de datos</span><span class="sxs-lookup"><span data-stu-id="0410c-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="0410c-166">A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0410c-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="0410c-167">EF Core usa automáticamente la funcionalidad de registro integrada para ASP.NET Core para escribir los registros que contienen el código SQL para las consultas y actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="0410c-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="0410c-168">En esta sección podrá ver algunos ejemplos de registros de SQL.</span><span class="sxs-lookup"><span data-stu-id="0410c-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="0410c-169">Abra *StudentsController.cs* y, en el método `Details`, establezca un punto de interrupción en la instrucción `if (student == null)`.</span><span class="sxs-lookup"><span data-stu-id="0410c-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="0410c-170">Ejecute la aplicación en modo de depuración y vaya a la página Details de un alumno.</span><span class="sxs-lookup"><span data-stu-id="0410c-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="0410c-171">Vaya a la ventana **Salida** que muestra la salida de depuración y verá la consulta:</span><span class="sxs-lookup"><span data-stu-id="0410c-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="0410c-172">Aquí verá algo que podría sorprenderle: la instrucción SQL selecciona hasta 2 filas (`TOP(2)`) de la tabla Person.</span><span class="sxs-lookup"><span data-stu-id="0410c-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="0410c-173">El método `SingleOrDefaultAsync` no se resuelve en 1 fila en el servidor.</span><span class="sxs-lookup"><span data-stu-id="0410c-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="0410c-174">El motivo es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="0410c-174">Here's why:</span></span>

* <span data-ttu-id="0410c-175">Si la consulta devolvería varias filas, el método devuelve NULL.</span><span class="sxs-lookup"><span data-stu-id="0410c-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="0410c-176">Para determinar si la consulta devolvería varias filas, EF tiene que comprobar si devuelve al menos 2.</span><span class="sxs-lookup"><span data-stu-id="0410c-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="0410c-177">Tenga en cuenta que no tiene que usar el modo de depuración y parar en un punto de interrupción para obtener los resultados del registro en la ventana **Salida**.</span><span class="sxs-lookup"><span data-stu-id="0410c-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="0410c-178">Es una manera cómoda de detener el registro en el punto que quiere ver en la salida.</span><span class="sxs-lookup"><span data-stu-id="0410c-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="0410c-179">Si no lo hace, el registro continúa y tiene que desplazarse hacia atrás para encontrar los elementos que le interesan.</span><span class="sxs-lookup"><span data-stu-id="0410c-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="0410c-180">Patrones de repositorio y de unidad de trabajo</span><span class="sxs-lookup"><span data-stu-id="0410c-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="0410c-181">Muchos desarrolladores escriben código para implementar el repositorio y una unidad de patrones de trabajo como un contenedor en torno al código que funciona con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0410c-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="0410c-182">Estos patrones están previstos para crear una capa de abstracción entre la capa de acceso de datos y la capa de lógica de negocios de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="0410c-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="0410c-183">Implementar estos patrones puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar la realización de pruebas unitarias automatizadas o el desarrollo controlado por pruebas (TDD).</span><span class="sxs-lookup"><span data-stu-id="0410c-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="0410c-184">Pero escribir código adicional para implementar estos patrones no siempre es la mejor opción para las aplicaciones que usan EF, por varias razones:</span><span class="sxs-lookup"><span data-stu-id="0410c-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="0410c-185">La propia clase de contexto de EF aísla el código del código específico del almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="0410c-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="0410c-186">La clase de contexto de EF puede actuar como una clase de unidad de trabajo para las actualizaciones de base de datos que hace con EF.</span><span class="sxs-lookup"><span data-stu-id="0410c-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="0410c-187">EF incluye características para implementar TDD sin escribir código de repositorio.</span><span class="sxs-lookup"><span data-stu-id="0410c-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="0410c-188">Para obtener información sobre cómo implementar los patrones de repositorio y de unidad de trabajo, consulte [la versión de Entity Framework 5 de esta serie de tutoriales](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="0410c-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="0410c-189">Entity Framework Core implementa un proveedor de base de datos en memoria que puede usarse para realizar pruebas.</span><span class="sxs-lookup"><span data-stu-id="0410c-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="0410c-190">Para más información, vea [Pruebas con InMemory](/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="0410c-190">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="0410c-191">Detección de cambios automática</span><span class="sxs-lookup"><span data-stu-id="0410c-191">Automatic change detection</span></span>

<span data-ttu-id="0410c-192">Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que hay que enviar a la base de datos) comparando los valores actuales de una entidad con los valores originales.</span><span class="sxs-lookup"><span data-stu-id="0410c-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="0410c-193">Cuando se consulta o se adjunta la entidad, se almacenan los valores originales.</span><span class="sxs-lookup"><span data-stu-id="0410c-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="0410c-194">Algunos de los métodos que provocan la detección de cambios automática son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="0410c-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="0410c-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="0410c-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="0410c-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="0410c-196">DbContext.Entry</span></span>

* <span data-ttu-id="0410c-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="0410c-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="0410c-198">Si está realizando el seguimiento de un gran número de entidades y llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas si desactiva temporalmente la detección de cambios automática mediante la propiedad `ChangeTracker.AutoDetectChangesEnabled`.</span><span class="sxs-lookup"><span data-stu-id="0410c-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="0410c-199">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0410c-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="0410c-200">Código fuente y planes de desarrollo de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0410c-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="0410c-201">El origen de Entity Framework Core está en [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="0410c-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="0410c-202">El repositorio de EF Core contiene las compilaciones nocturnas, el seguimiento de problemas, las especificaciones de características, las notas de las reuniones de diseño y [el plan de desarrollo futuro](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="0410c-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="0410c-203">Puede archivar o buscar errores y contribuir.</span><span class="sxs-lookup"><span data-stu-id="0410c-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="0410c-204">Aunque el código fuente es abierto, Entity Framework Core es totalmente compatible como producto de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0410c-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="0410c-205">El equipo de Microsoft Entity Framework mantiene el control sobre qué contribuciones se aceptan y comprueba todos los cambios de código para garantizar la calidad de cada versión.</span><span class="sxs-lookup"><span data-stu-id="0410c-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="0410c-206">Ingeniería inversa desde la base de datos existente</span><span class="sxs-lookup"><span data-stu-id="0410c-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="0410c-207">Para usar técnicas de ingeniería inversa a un modelo de datos, incluidas las clases de entidad de una base de datos existente, use el comando [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="0410c-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="0410c-208">Consulte el [tutorial de introducción](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="0410c-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="0410c-209">Usar LINQ dinámico para simplificar la ordenación del código de selección</span><span class="sxs-lookup"><span data-stu-id="0410c-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="0410c-210">El [tercer tutorial de esta serie](sort-filter-page.md) muestra cómo escribir código LINQ mediante el codificado de forma rígida de los nombres de columna en una instrucción `switch`.</span><span class="sxs-lookup"><span data-stu-id="0410c-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="0410c-211">Con dos columnas entre las que elegir, esto funciona bien, pero si tiene muchas columnas, el código podría considerarse detallado.</span><span class="sxs-lookup"><span data-stu-id="0410c-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="0410c-212">Para solucionar este problema, puede usar el método `EF.Property` para especificar el nombre de la propiedad como una cadena.</span><span class="sxs-lookup"><span data-stu-id="0410c-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="0410c-213">Para probar este enfoque, reemplace el método `Index` en `StudentsController` con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="0410c-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="0410c-214">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0410c-214">Next steps</span></span>

<span data-ttu-id="0410c-215">Con esto finaliza esta serie de tutoriales sobre cómo usar Entity Framework Core en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0410c-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="0410c-216">Para obtener más información sobre EF Core, consulte la [documentación de Entity Framework Core](https://docs.microsoft.com/ef/core).</span><span class="sxs-lookup"><span data-stu-id="0410c-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="0410c-217">También está disponible un libro: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action) (Entity Framework Core en acción, en inglés).</span><span class="sxs-lookup"><span data-stu-id="0410c-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="0410c-218">Para obtener información sobre cómo implementar una aplicación web, consulte [Hospedaje e implementación](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="0410c-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="0410c-219">Para obtener información sobre otros temas relacionados con ASP.NET Core MVC, como la autenticación y autorización, consulte la [documentación de ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="0410c-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](xref:index).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="0410c-220">Agradecimientos</span><span class="sxs-lookup"><span data-stu-id="0410c-220">Acknowledgments</span></span>

<span data-ttu-id="0410c-221">Tom Dykstra y Rick Anderson (Twitter @RickAndMSFT) escribieron este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0410c-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="0410c-222">Rowan Miller, Diego Vega y otros miembros del equipo de Entity Framework participaron con revisiones de código y ayudaron a depurar problemas que surgieron mientras se estaba escribiendo el código para los tutoriales.</span><span class="sxs-lookup"><span data-stu-id="0410c-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="0410c-223">Errores comunes</span><span class="sxs-lookup"><span data-stu-id="0410c-223">Common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="0410c-224">ContosoUniversity.dll usado por otro proceso</span><span class="sxs-lookup"><span data-stu-id="0410c-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="0410c-225">Mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="0410c-225">Error message:</span></span>

> <span data-ttu-id="0410c-226">No se puede abrir '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' para escribir: El proceso no puede obtener acceso al archivo '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll', otro proceso lo está usando.</span><span class="sxs-lookup"><span data-stu-id="0410c-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="0410c-227">Solución:</span><span class="sxs-lookup"><span data-stu-id="0410c-227">Solution:</span></span>

<span data-ttu-id="0410c-228">Detenga el sitio en IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0410c-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="0410c-229">Vaya a la bandeja del sistema de Windows, busque IIS Express y haga clic con el botón derecho en su icono; seleccione el sitio de Contoso University y, después, haga clic en **Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="0410c-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="0410c-230">La migración aplicó la técnica scaffolding sin código en los métodos Up y Down</span><span class="sxs-lookup"><span data-stu-id="0410c-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="0410c-231">Causa posible:</span><span class="sxs-lookup"><span data-stu-id="0410c-231">Possible cause:</span></span>

<span data-ttu-id="0410c-232">Los comandos de la CLI de EF no cierran y guardan automáticamente los archivos de código.</span><span class="sxs-lookup"><span data-stu-id="0410c-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="0410c-233">Si no ha guardado los cambios cuando ejecuta el comando `migrations add`, EF no encontrará los cambios.</span><span class="sxs-lookup"><span data-stu-id="0410c-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="0410c-234">Solución:</span><span class="sxs-lookup"><span data-stu-id="0410c-234">Solution:</span></span>

<span data-ttu-id="0410c-235">Ejecute el comando `migrations remove`, guarde los cambios de código y vuelva a ejecutar el comando `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="0410c-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="0410c-236">Errores durante la ejecución de la actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="0410c-236">Errors while running database update</span></span>

<span data-ttu-id="0410c-237">Al hacer cambios en el esquema, se pueden generar otros errores en una base de datos que contenga los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="0410c-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="0410c-238">Si se producen errores de migración que no se pueden resolver, puede cambiar el nombre de la base de datos en la cadena de conexión o eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0410c-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="0410c-239">Con una base de datos nueva, no hay ningún dato para migrar y es mucho más probable que el comando de actualización de base de datos se complete sin errores.</span><span class="sxs-lookup"><span data-stu-id="0410c-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="0410c-240">El enfoque más sencillo consiste en cambiar el nombre de la base de datos en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0410c-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="0410c-241">La próxima vez que ejecute `database update`, se creará una base de datos.</span><span class="sxs-lookup"><span data-stu-id="0410c-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="0410c-242">Para eliminar una base de datos en SSOX, haga clic con el botón derecho en la base de datos, haga clic en **Eliminar** y, después, en el cuadro de diálogo **Eliminar base de datos**, seleccione **Cerrar conexiones existentes** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0410c-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="0410c-243">Para eliminar una base de datos mediante el uso de la CLI, ejecute el comando de la CLI `database drop`:</span><span class="sxs-lookup"><span data-stu-id="0410c-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="0410c-244">Error al buscar la instancia de SQL Server</span><span class="sxs-lookup"><span data-stu-id="0410c-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="0410c-245">Mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="0410c-245">Error Message:</span></span>

> <span data-ttu-id="0410c-246">Error relacionado con la red o específico de la instancia mientras se establecía una conexión con el servidor SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0410c-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="0410c-247">No se encontró el servidor o éste no estaba accesible.</span><span class="sxs-lookup"><span data-stu-id="0410c-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="0410c-248">Compruebe que el nombre de la instancia es correcto y que SQL Server está configurado para admitir conexiones remotas.</span><span class="sxs-lookup"><span data-stu-id="0410c-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="0410c-249">(proveedor: interfaces de red de SQL, error: 26 -	Error al buscar el servidor o instancia especificado)</span><span class="sxs-lookup"><span data-stu-id="0410c-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="0410c-250">Solución:</span><span class="sxs-lookup"><span data-stu-id="0410c-250">Solution:</span></span>

<span data-ttu-id="0410c-251">Compruebe la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="0410c-251">Check the connection string.</span></span> <span data-ttu-id="0410c-252">Si ha eliminado manualmente el archivo de base de datos, cambie el nombre de la base de datos en la cadena de construcción para volver a empezar con una base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="0410c-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="0410c-253">Anterior</span><span class="sxs-lookup"><span data-stu-id="0410c-253">Previous</span></span>](inheritance.md)
