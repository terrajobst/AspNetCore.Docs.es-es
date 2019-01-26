---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Usar procedimientos almacenados y asincrónicos con EF en una aplicación de MVC de ASP.NET'
description: En este tutorial verá cómo implementar el modelo de programación asincrónico y obtenga información sobre cómo utilizar procedimientos almacenados.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9041167af076d80ebf294e054ffe51293d11e888
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889878"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="80a2f-103">Tutorial: Usar procedimientos almacenados y asincrónicos con EF en una aplicación de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="80a2f-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="80a2f-104">En los tutoriales anteriores, ha aprendido cómo leer y actualizar datos mediante el modelo de programación sincrónica.</span><span class="sxs-lookup"><span data-stu-id="80a2f-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="80a2f-105">En este tutorial verá cómo implementar el modelo de programación asincrónico.</span><span class="sxs-lookup"><span data-stu-id="80a2f-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="80a2f-106">El código asincrónico puede ayudar a una aplicación a un mejor rendimiento porque hace un mejor uso de recursos del servidor.</span><span class="sxs-lookup"><span data-stu-id="80a2f-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="80a2f-107">En este tutorial también verá cómo usar procedimientos almacenados para insert, update y las operaciones de eliminación en una entidad.</span><span class="sxs-lookup"><span data-stu-id="80a2f-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="80a2f-108">Por último, volver a implementar la aplicación en Azure, junto con todos los cambios de base de datos que ha implementado desde la primera vez que se ha implementado.</span><span class="sxs-lookup"><span data-stu-id="80a2f-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="80a2f-109">En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.</span><span class="sxs-lookup"><span data-stu-id="80a2f-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Página de los departamentos de TI](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Creación de departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="80a2f-112">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="80a2f-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80a2f-113">Obtenga información sobre el código asincrónico</span><span class="sxs-lookup"><span data-stu-id="80a2f-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="80a2f-114">Crear un controlador de departamento</span><span class="sxs-lookup"><span data-stu-id="80a2f-114">Create a Department controller</span></span>
> * <span data-ttu-id="80a2f-115">Usar procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="80a2f-115">Use stored procedures</span></span>
> * <span data-ttu-id="80a2f-116">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="80a2f-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80a2f-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="80a2f-117">Prerequisites</span></span>

* [<span data-ttu-id="80a2f-118">Actualización de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="80a2f-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="80a2f-119">¿Por qué usar código asincrónico</span><span class="sxs-lookup"><span data-stu-id="80a2f-119">Why use asynchronous code</span></span>

<span data-ttu-id="80a2f-120">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="80a2f-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="80a2f-121">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="80a2f-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="80a2f-122">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="80a2f-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="80a2f-123">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="80a2f-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="80a2f-124">Como resultado, el código asincrónico permite que los recursos de servidor se puede utilizar con mayor eficiencia y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="80a2f-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="80a2f-125">En versiones anteriores de. NET, estaban complejo, escribir y probar el código asincrónico propenso a errores y difícil de depurar.</span><span class="sxs-lookup"><span data-stu-id="80a2f-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="80a2f-126">.NET Framework 4.5, es mucho más fácil que generalmente debería escribir código asincrónico a menos que tenga una razón para no escribir, probar y depurar código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="80a2f-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="80a2f-127">El código asincrónico introduce una pequeña cantidad de sobrecarga, pero para situaciones de poco tráfico la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, es importante la mejora del rendimiento potencial.</span><span class="sxs-lookup"><span data-stu-id="80a2f-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="80a2f-128">Para obtener más información sobre la programación asincrónica, vea [soporte para asincronía para evitar el bloqueo de las llamadas de uso .NET 4.5](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="80a2f-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="80a2f-129">Crear el controlador de departamento</span><span class="sxs-lookup"><span data-stu-id="80a2f-129">Create Department controller</span></span>

<span data-ttu-id="80a2f-130">Crear un controlador de departamento, la misma manera que los controladores anteriores, salvo que esta vez seleccione el **usar acciones de controlador asincrónicas** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="80a2f-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="80a2f-131">Los puntos destacados siguientes muestran lo que se agregó al código sincrónico para el `Index` método para que sea asincrónica:</span><span class="sxs-lookup"><span data-stu-id="80a2f-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="80a2f-132">Cuatro cambios se aplicaron para habilitar la consulta de base de datos de Entity Framework ejecutar de forma asincrónica:</span><span class="sxs-lookup"><span data-stu-id="80a2f-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="80a2f-133">El método está marcado con el `async` palabra clave, que le indica al compilador que genere devoluciones de llamada para partes del cuerpo del método y para crear automáticamente el `Task<ActionResult>` objeto devuelto.</span><span class="sxs-lookup"><span data-stu-id="80a2f-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="80a2f-134">Se ha cambiado el tipo de valor devuelto de `ActionResult` a `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="80a2f-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="80a2f-135">El `Task<T>` tipo representa el trabajo en curso con un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="80a2f-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="80a2f-136">El `await` palabra clave se aplicó a la llamada al servicio web.</span><span class="sxs-lookup"><span data-stu-id="80a2f-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="80a2f-137">Cuando el compilador ve esta palabra clave, en segundo plano divide el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="80a2f-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="80a2f-138">La primera parte termina con la operación que se inicia de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="80a2f-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="80a2f-139">La segunda parte se coloca en un método de devolución de llamada que se llama cuando se complete la operación.</span><span class="sxs-lookup"><span data-stu-id="80a2f-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="80a2f-140">La versión asincrónica de la `ToList` se llamó al método de extensión.</span><span class="sxs-lookup"><span data-stu-id="80a2f-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="80a2f-141">¿Por qué es el `departments.ToList` instrucción pero no puede modificar el `departments = db.Departments` instrucción?</span><span class="sxs-lookup"><span data-stu-id="80a2f-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="80a2f-142">El motivo es que sólo las instrucciones que hacen que las consultas o comandos se envíen a la base de datos se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="80a2f-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="80a2f-143">El `departments = db.Departments` instrucción establece una consulta, pero no se ejecuta la consulta hasta la `ToList` se llama al método.</span><span class="sxs-lookup"><span data-stu-id="80a2f-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="80a2f-144">Por lo tanto, solo el `ToList` método se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="80a2f-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="80a2f-145">En el `Details` método y el `HttpGet` `Edit` y `Delete` métodos, el `Find` método es lo que hace que una consulta para enviarse a la base de datos, por lo que es el método que se ejecuta de forma asincrónica:</span><span class="sxs-lookup"><span data-stu-id="80a2f-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="80a2f-146">En el `Create`, `HttpPost Edit`, y `DeleteConfirmed` métodos, es el `SaveChanges` llamada a método que hace que se ejecuta un comando, no las instrucciones como `db.Departments.Add(department)` lo que solo hacer que las entidades en memoria que se puede modificar.</span><span class="sxs-lookup"><span data-stu-id="80a2f-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="80a2f-147">Abra *Views\Department\Index.cshtml*y reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="80a2f-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="80a2f-148">Este código cambia el título de índice a los departamentos de TI, mueve el nombre de administrador a la derecha y proporciona el nombre completo del administrador.</span><span class="sxs-lookup"><span data-stu-id="80a2f-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="80a2f-149">En el crear, eliminar, detalles y editar vistas, cambie el título de la `InstructorID` campo a la misma manera que cambió el campo de nombre de departamento a "Departamento" en las vistas de Course "administrador".</span><span class="sxs-lookup"><span data-stu-id="80a2f-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="80a2f-150">En la creación y edición de las vistas de usar el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="80a2f-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="80a2f-151">En las vistas Delete y Details, use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="80a2f-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="80a2f-152">Ejecute la aplicación y haga clic en el **departamentos** ficha.</span><span class="sxs-lookup"><span data-stu-id="80a2f-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="80a2f-153">Todo funciona igual que en los demás controladores, pero en este controlador de todas las consultas SQL se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="80a2f-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="80a2f-154">Algunos aspectos que tener en cuenta cuando se utiliza la programación asincrónica con Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="80a2f-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="80a2f-155">El código asincrónico no es seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="80a2f-155">The async code is not thread safe.</span></span> <span data-ttu-id="80a2f-156">En otras palabras, en otras palabras, no intente realizar varias operaciones en paralelo con la misma instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="80a2f-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="80a2f-157">Si quiere aprovechar las ventajas de rendimiento del código asincrónico, asegúrese de que en los paquetes de biblioteca que use (por ejemplo para paginación), también se usa async si llaman a cualquier método de Entity Framework que haga que las consultas se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="80a2f-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="80a2f-158">Usar procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="80a2f-158">Use stored procedures</span></span>

<span data-ttu-id="80a2f-159">Algunos desarrolladores y administradores prefieren usar procedimientos almacenados para el acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="80a2f-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="80a2f-160">En versiones anteriores de Entity Framework puede recuperar datos mediante un procedimiento almacenado por [ejecutando una consulta SQL sin formato](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), pero no se puede indicar a EF que use los procedimientos almacenados para las operaciones de actualización.</span><span class="sxs-lookup"><span data-stu-id="80a2f-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="80a2f-161">En EF 6 es fácil de configurar Code First para utilizar procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="80a2f-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="80a2f-162">En *DAL\SchoolContext.cs*, agregue el código resaltado a la `OnModelCreating` método.</span><span class="sxs-lookup"><span data-stu-id="80a2f-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="80a2f-163">Este código indica a Entity Framework a los procedimientos de uso almacenado para insertar, actualizar y eliminar operaciones en el `Department` entidad.</span><span class="sxs-lookup"><span data-stu-id="80a2f-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="80a2f-164">En la consola de administración de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="80a2f-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="80a2f-165">Abra *migraciones\&lt; marca de tiempo&gt;\_DepartmentSP.cs* para ver el código en el `Up` método que crea Insert, Update y Delete de procedimientos almacenados:</span><span class="sxs-lookup"><span data-stu-id="80a2f-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="80a2f-166">En la consola de administración de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="80a2f-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="80a2f-167">Ejecute la aplicación en modo de depuración, haga clic en el **departamentos** pestaña y, a continuación, haga clic en **crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="80a2f-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="80a2f-168">Escriba los datos de un nuevo departamento y, a continuación, haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="80a2f-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="80a2f-169">En Visual Studio, examine los registros en el **salida** ventana para ver que se usó un procedimiento almacenado para insertar la nueva fila de departamento.</span><span class="sxs-lookup"><span data-stu-id="80a2f-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![SP de inserción de departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="80a2f-171">Código primero crea nombres de procedimientos almacenados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="80a2f-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="80a2f-172">Si usa una base de datos existente, es posible que deba personalizar los nombres de procedimiento almacenado con el fin de utilizar procedimientos almacenados definidos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="80a2f-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="80a2f-173">Para obtener información acerca de cómo hacerlo, consulte [Entity Framework código primera Insertar/Actualizar/Eliminar procedimientos almacenados](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="80a2f-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="80a2f-174">Si desea personalizar lo que genera los procedimientos almacenados, puede editar el código con scaffolding para las migraciones `Up` método que crea el procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="80a2f-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="80a2f-175">De este modo los cambios se reflejan siempre que la migración se ejecuta y se aplicarán a la base de datos de producción cuando las migraciones se ejecuta automáticamente en producción después de la implementación.</span><span class="sxs-lookup"><span data-stu-id="80a2f-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="80a2f-176">Si desea cambiar un procedimiento almacenado existente que se creó en una migración anterior, puede usar el comando Add-Migration para generar una migración en blanco y, a continuación, escribir manualmente el código que llama el [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (método) .</span><span class="sxs-lookup"><span data-stu-id="80a2f-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="80a2f-177">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="80a2f-177">Deploy to Azure</span></span>

<span data-ttu-id="80a2f-178">En esta sección, deberá haber completado opcional **implementar la aplicación en Azure** sección la [implementación y migraciones](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial de esta serie.</span><span class="sxs-lookup"><span data-stu-id="80a2f-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="80a2f-179">Si tenía errores de las migraciones que puede resolver mediante la eliminación de la base de datos en su proyecto local, omita esta sección.</span><span class="sxs-lookup"><span data-stu-id="80a2f-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="80a2f-180">En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="80a2f-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="80a2f-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="80a2f-181">Click **Publish**.</span></span>

    <span data-ttu-id="80a2f-182">Visual Studio implementa la aplicación en Azure y la aplicación se abre en el explorador predeterminado, que se ejecuta en Azure.</span><span class="sxs-lookup"><span data-stu-id="80a2f-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="80a2f-183">Probar la aplicación para comprobar funciona.</span><span class="sxs-lookup"><span data-stu-id="80a2f-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="80a2f-184">La primera vez que ejecute una página que tiene acceso a la base de datos, Entity Framework se ejecuta todas las migraciones `Up` métodos necesarios para la base de datos al día con el modelo de datos actual.</span><span class="sxs-lookup"><span data-stu-id="80a2f-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="80a2f-185">Ahora puede usar todas las páginas web que ha agregado desde la última vez que se ha implementado, incluidas las páginas de departamento que agregó en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="80a2f-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="80a2f-186">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="80a2f-186">Get the code</span></span>

[<span data-ttu-id="80a2f-187">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="80a2f-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="80a2f-188">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="80a2f-188">Additional resources</span></span>

<span data-ttu-id="80a2f-189">Pueden encontrar vínculos a otros recursos de Entity Framework en el [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="80a2f-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="80a2f-190">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="80a2f-190">Next steps</span></span>

<span data-ttu-id="80a2f-191">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="80a2f-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80a2f-192">Aprendió a utilizar código asincrónico</span><span class="sxs-lookup"><span data-stu-id="80a2f-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="80a2f-193">Crea un controlador de departamento</span><span class="sxs-lookup"><span data-stu-id="80a2f-193">Created a Department controller</span></span>
> * <span data-ttu-id="80a2f-194">Usar procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="80a2f-194">Used stored procedures</span></span>
> * <span data-ttu-id="80a2f-195">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="80a2f-195">Deployed to Azure</span></span>

<span data-ttu-id="80a2f-196">Avance al siguiente artículo para obtener información sobre cómo controlar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="80a2f-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="80a2f-197">Control de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="80a2f-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
