---
title: "Páginas de Razor con EF Core - simultaneidad - 8 de 8"
author: rick-anderson
description: "Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo."
keywords: Simultaneidad de ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 0c49376fd1b602fe03ef2a152d19b58513ae2710
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2017
---
<span data-ttu-id="35df9-104">en-us /</span><span class="sxs-lookup"><span data-stu-id="35df9-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="35df9-105">Controlar los conflictos de simultaneidad - Core EF con páginas de Razor (8 de 8)</span><span class="sxs-lookup"><span data-stu-id="35df9-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="35df9-106">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), y [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="35df9-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="35df9-107">Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan una entidad simultáneamente (al mismo tiempo).</span><span class="sxs-lookup"><span data-stu-id="35df9-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="35df9-108">Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="35df9-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="35df9-109">Conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="35df9-109">Concurrency conflicts</span></span>

<span data-ttu-id="35df9-110">Se produce un conflicto de simultaneidad cuando:</span><span class="sxs-lookup"><span data-stu-id="35df9-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="35df9-111">Un usuario navega a la página de edición para una entidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="35df9-112">Otro usuario actualiza la misma entidad antes de que el primer cambio del usuario se escribe en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="35df9-113">Si no está habilitada la detección de simultaneidad, cuando se produzcan actualizaciones simultáneas:</span><span class="sxs-lookup"><span data-stu-id="35df9-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="35df9-114">La última actualización ganadora.</span><span class="sxs-lookup"><span data-stu-id="35df9-114">The last update wins.</span></span> <span data-ttu-id="35df9-115">Es decir, los últimos valores de actualización se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="35df9-116">La primera de las actualizaciones actuales se pierden.</span><span class="sxs-lookup"><span data-stu-id="35df9-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="35df9-117">Simultaneidad optimista</span><span class="sxs-lookup"><span data-stu-id="35df9-117">Optimistic concurrency</span></span>

<span data-ttu-id="35df9-118">Simultaneidad optimista permite que los conflictos de simultaneidad que se produzca y, a continuación, reacciona correctamente cuando lo hacen.</span><span class="sxs-lookup"><span data-stu-id="35df9-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="35df9-119">Por ejemplo, Julia visita la página de edición de departamento y cambia el presupuesto para el departamento de inglés de $350,000.00 a 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="35df9-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Cambiar la asignación a 0](concurrency/_static/change-budget.png)

<span data-ttu-id="35df9-121">Antes de que Juan hace clic en **guardar**, Juan visita la misma página y cambia el campo de fecha de inicio de 1/9/2007 a 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="35df9-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Cambiar la fecha de inicio a 2013](concurrency/_static/change-date.png)

<span data-ttu-id="35df9-123">Julia hace clic en **guardar** primera y ve su cambiar cuando el explorador muestra la página de índice.</span><span class="sxs-lookup"><span data-stu-id="35df9-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Presupuesto cambiado a cero](concurrency/_static/budget-zero.png)

<span data-ttu-id="35df9-125">John hace clic en **guardar** en una página de edición que sigue mostrando un presupuesto de 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="35df9-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="35df9-126">¿Qué sucede después viene determinado por la manera de controlar conflictos de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="35df9-127">Simultaneidad optimista incluye las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="35df9-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="35df9-128">Puede realizar un seguimiento de la propiedad que se ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="35df9-129">En el escenario, se perderá ningún dato.</span><span class="sxs-lookup"><span data-stu-id="35df9-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="35df9-130">Se actualizaron las diferentes propiedades por los dos usuarios.</span><span class="sxs-lookup"><span data-stu-id="35df9-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="35df9-131">La próxima vez que un usuario examina el departamento de inglés, verá los cambios de Jane y John Doe.</span><span class="sxs-lookup"><span data-stu-id="35df9-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="35df9-132">Este método de actualización puede reducir el número de conflictos que pueden dar lugar a pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="35df9-133">Este enfoque: * no se puede evitar la pérdida de datos si se realizan cambios competencia a la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="35df9-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="35df9-134">* Es por lo general no es práctico en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="35df9-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="35df9-135">Requiere el mantenimiento de un estado relevante para realizar un seguimiento de capturadas todos los valores y los nuevos valores.</span><span class="sxs-lookup"><span data-stu-id="35df9-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="35df9-136">Mantener grandes cantidades de estado puede afectar al rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="35df9-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="35df9-137">* Puede aumentar la complejidad de las aplicaciones en comparación con la detección de simultaneidad en una entidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="35df9-138">Puede permitir que los cambios de John sobrescribir los cambios de Julia.</span><span class="sxs-lookup"><span data-stu-id="35df9-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="35df9-139">La próxima vez que un usuario examina el departamento de inglés, verán 9/1/2013 y el valor de $350,000.00 capturadas.</span><span class="sxs-lookup"><span data-stu-id="35df9-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="35df9-140">Este enfoque se denomina un *cliente Wins* o *el último gana* escenario.</span><span class="sxs-lookup"><span data-stu-id="35df9-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="35df9-141">(Todos los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos.) Si no hace ninguna codificación para el control de simultaneidad, gana el cliente realiza automáticamente.</span><span class="sxs-lookup"><span data-stu-id="35df9-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="35df9-142">Cambio de John puede impedir que se actualiza en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="35df9-143">Normalmente, la aplicación podría: * mostrar un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="35df9-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="35df9-144">* Mostrar el estado actual de los datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="35df9-145">* Permite al usuario volver a aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="35df9-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="35df9-146">Esto se denomina una *almacén Wins* escenario.</span><span class="sxs-lookup"><span data-stu-id="35df9-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="35df9-147">(Los valores de almacén de datos tienen prioridad sobre los valores enviados por el cliente). Implementar el escenario de almacén de Wins en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="35df9-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="35df9-148">Este método garantiza que ningún cambio se sobrescribe sin que el usuario que se va a una alerta.</span><span class="sxs-lookup"><span data-stu-id="35df9-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="35df9-149">Control de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="35df9-149">Handling concurrency</span></span> 

<span data-ttu-id="35df9-150">Cuando una propiedad se configura como un [token de simultaneidad](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="35df9-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="35df9-151">Núcleo EF comprueba que no se han modificado propiedad después de se capturó.</span><span class="sxs-lookup"><span data-stu-id="35df9-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="35df9-152">La comprobación se produce cuando [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) se llama.</span><span class="sxs-lookup"><span data-stu-id="35df9-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="35df9-153">Si la propiedad ha cambiado desde que se capturó, un [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) se produce.</span><span class="sxs-lookup"><span data-stu-id="35df9-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="35df9-154">El modelo de base de datos y los datos debe configurarse para admitir producir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="35df9-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="35df9-155">Detección de conflictos de simultaneidad en una propiedad</span><span class="sxs-lookup"><span data-stu-id="35df9-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="35df9-156">Se pueden detectar conflictos de simultaneidad en el nivel de propiedad con el [sea ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atributo.</span><span class="sxs-lookup"><span data-stu-id="35df9-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="35df9-157">El atributo se puede aplicar a varias propiedades en el modelo.</span><span class="sxs-lookup"><span data-stu-id="35df9-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="35df9-158">Para obtener más información, consulte [anotaciones de datos-sea ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="35df9-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="35df9-159">El `[ConcurrencyCheck]` atributo no se utiliza en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="35df9-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="35df9-160">Detección de conflictos de simultaneidad en una fila</span><span class="sxs-lookup"><span data-stu-id="35df9-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="35df9-161">Para detectar conflictos de simultaneidad, un [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) seguimiento de columna se agrega al modelo.</span><span class="sxs-lookup"><span data-stu-id="35df9-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="35df9-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="35df9-162">`rowversion` :</span></span>

* <span data-ttu-id="35df9-163">¿Es SQL Server específico.</span><span class="sxs-lookup"><span data-stu-id="35df9-163">Is SQL Server specific.</span></span> <span data-ttu-id="35df9-164">Otras bases de datos no pueden proporcionar una característica similar.</span><span class="sxs-lookup"><span data-stu-id="35df9-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="35df9-165">Se usa para determinar que una entidad no se ha cambiado desde que se capturó desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="35df9-166">La base de datos genera un secuencial `rowversion` número que se incrementa cada vez que la fila se actualiza.</span><span class="sxs-lookup"><span data-stu-id="35df9-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="35df9-167">En un `Update` o `Delete` comando, el `Where` cláusula incluye el valor obtenido de `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="35df9-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="35df9-168">Si está actualizando la fila ha cambiado:</span><span class="sxs-lookup"><span data-stu-id="35df9-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="35df9-169">`rowversion`no coincide con el valor obtenido.</span><span class="sxs-lookup"><span data-stu-id="35df9-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="35df9-170">El `Update` o `Delete` comandos no encuentran una fila porque la `Where` cláusula incluye el capturadas `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="35df9-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="35df9-171">Un `DbUpdateConcurrencyException` se produce.</span><span class="sxs-lookup"><span data-stu-id="35df9-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="35df9-172">En el núcleo de EF, cuando no se haya actualizado ninguna fila por un `Update` o `Delete` de comandos, se produce una excepción de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="35df9-173">Agregar una propiedad de seguimiento a la entidad Department</span><span class="sxs-lookup"><span data-stu-id="35df9-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="35df9-174">En *Models/Department.cs*, agregar una propiedad de seguimiento denominada RowVersion:</span><span class="sxs-lookup"><span data-stu-id="35df9-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="35df9-175">El [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atributo especifica que esta columna se incluye en el `Where` cláusula de `Update` y `Delete` comandos.</span><span class="sxs-lookup"><span data-stu-id="35df9-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="35df9-176">El atributo se denomina `Timestamp` porque las versiones anteriores de SQL Server utilizan una instancia de SQL `timestamp` tipo de datos antes de la instrucción SQL `rowversion` sustituirla por tipo.</span><span class="sxs-lookup"><span data-stu-id="35df9-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="35df9-177">La API fluida también puede especificar la propiedad de seguimiento:</span><span class="sxs-lookup"><span data-stu-id="35df9-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="35df9-178">El código siguiente muestra una parte de la instrucción T-SQL generado por núcleo de EF cuando se actualiza el nombre del departamento:</span><span class="sxs-lookup"><span data-stu-id="35df9-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="35df9-179">Anterior resaltada de código muestra la `WHERE` que contiene la cláusula `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="35df9-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="35df9-180">Si la base de datos `RowVersion` no es igual a la `RowVersion` parámetro (`@p2`), se ha actualizado ninguna fila.</span><span class="sxs-lookup"><span data-stu-id="35df9-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="35df9-181">El código resaltado siguiente muestra el código T-SQL que comprueba que se actualizó exactamente una fila:</span><span class="sxs-lookup"><span data-stu-id="35df9-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="35df9-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) devuelve el número de filas afectadas por la última instrucción.</span><span class="sxs-lookup"><span data-stu-id="35df9-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="35df9-183">En no se actualizan las filas, Core EF produce un `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="35df9-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="35df9-184">Puede ver que el núcleo de EF de T-SQL se genera en la ventana de salida de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35df9-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="35df9-185">Actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="35df9-185">Update the DB</span></span>

<span data-ttu-id="35df9-186">Agregar el `RowVersion` propiedad cambia el modelo de base de datos, lo que requiere una migración.</span><span class="sxs-lookup"><span data-stu-id="35df9-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="35df9-187">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="35df9-187">Build the project.</span></span> <span data-ttu-id="35df9-188">Escriba lo siguiente en una ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="35df9-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="35df9-189">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="35df9-189">The preceding commands:</span></span>

* <span data-ttu-id="35df9-190">Agrega el *migraciones / {tiempo stamp}_RowVersion.cs* archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="35df9-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="35df9-191">Las actualizaciones de la *Migrations/SchoolContextModelSnapshot.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="35df9-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="35df9-192">La actualización agrega el código que aparece resaltado a la `BuildModel` método:</span><span class="sxs-lookup"><span data-stu-id="35df9-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="35df9-193">Ejecuta las migraciones para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="35df9-194">Aplicar la técnica scaffolding el modelo de departamentos</span><span class="sxs-lookup"><span data-stu-id="35df9-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="35df9-195">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35df9-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="35df9-196">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="35df9-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="35df9-197">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="35df9-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="35df9-198">El scaffolds del comando anterior la `Department` modelo.</span><span class="sxs-lookup"><span data-stu-id="35df9-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="35df9-199">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35df9-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="35df9-200">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="35df9-200">Build the project.</span></span> <span data-ttu-id="35df9-201">La compilación genera errores similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="35df9-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="35df9-202">Cambiar globalmente `_context.Department` a `_context.Departments` (es decir, agregue una "s" para `Department`).</span><span class="sxs-lookup"><span data-stu-id="35df9-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="35df9-203">7 apariciones se encuentra y se actualizan.</span><span class="sxs-lookup"><span data-stu-id="35df9-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="35df9-204">Actualizar la página de índice de departamentos</span><span class="sxs-lookup"><span data-stu-id="35df9-204">Update the Departments Index page</span></span>

<span data-ttu-id="35df9-205">El motor de scaffolding que crea un `RowVersion` columna para la página de índice, pero ese campo no debe mostrarse.</span><span class="sxs-lookup"><span data-stu-id="35df9-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="35df9-206">En este tutorial, el último byte de la `RowVersion` se muestra como ayuda para entender la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="35df9-207">El último byte se garantiza que no sean únicos.</span><span class="sxs-lookup"><span data-stu-id="35df9-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="35df9-208">No mostrar una aplicación real `RowVersion` o el último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="35df9-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="35df9-209">Actualice la página de índice:</span><span class="sxs-lookup"><span data-stu-id="35df9-209">Update the Index page:</span></span>

* <span data-ttu-id="35df9-210">Reemplace el índice por departamentos.</span><span class="sxs-lookup"><span data-stu-id="35df9-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="35df9-211">Reemplace el marcado que contiene `RowVersion` con el último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="35df9-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="35df9-212">Reemplace FirstMidName con FullName.</span><span class="sxs-lookup"><span data-stu-id="35df9-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="35df9-213">El marcado siguiente muestra la página actualizada:</span><span class="sxs-lookup"><span data-stu-id="35df9-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="35df9-214">Actualizar el modelo de la página de edición</span><span class="sxs-lookup"><span data-stu-id="35df9-214">Update the Edit page model</span></span>

<span data-ttu-id="35df9-215">Actualización *pages\departments\edit.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35df9-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="35df9-216">Para detectar un problema de simultaneidad, el [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) se actualiza con la `rowVersion` valor de la entidad era se capturó.</span><span class="sxs-lookup"><span data-stu-id="35df9-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity was it was fetched.</span></span> <span data-ttu-id="35df9-217">Núcleo EF genera un comando SQL UPDATE con una cláusula WHERE que contiene la versión original `RowVersion` valor.</span><span class="sxs-lookup"><span data-stu-id="35df9-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="35df9-218">Si no hay ninguna fila se ven afectada por el comando de actualización (no hay filas tienen la versión original `RowVersion` valor), un `DbUpdateConcurrencyException` se produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="35df9-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="35df9-219">En el código anterior, `Department.RowVersion` es el valor al que se capturó la entidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="35df9-220">`OriginalValue`es el valor de la base de datos cuando `FirstOrDefaultAsync` se llamó a este método.</span><span class="sxs-lookup"><span data-stu-id="35df9-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="35df9-221">El código siguiente obtiene los valores de cliente (es decir, los valores registrados en este método) y los valores de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="35df9-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="35df9-222">El código siguiente agrega un mensaje de error personalizado para valores diferentes de lo que se ha registrado para cada columna que tiene la base de datos `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="35df9-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="35df9-223">Resaltan de los siguientes conjuntos de código la `RowVersion` valor para el nuevo valor se recupera de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="35df9-224">La próxima vez que el usuario hace clic en **guardar**, solo los errores de simultaneidad que se producen desde la última visualización de la página de edición se detectarán.</span><span class="sxs-lookup"><span data-stu-id="35df9-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="35df9-225">El `ModelState.Remove` instrucción es necesaria porque `ModelState` tiene la antigua `RowVersion` valor.</span><span class="sxs-lookup"><span data-stu-id="35df9-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="35df9-226">En la página de Razor, el `ModelState` el valor de un campo tiene prioridad sobre los valores de propiedad de modelo cuando ambos están presentes.</span><span class="sxs-lookup"><span data-stu-id="35df9-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="35df9-227">Actualizar la página de edición</span><span class="sxs-lookup"><span data-stu-id="35df9-227">Update the Edit page</span></span>

<span data-ttu-id="35df9-228">Actualización *Pages/Departments/Edit.cshtml* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="35df9-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="35df9-229">El marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="35df9-229">The preceding markup:</span></span>

* <span data-ttu-id="35df9-230">Las actualizaciones de la `page` la directiva de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="35df9-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="35df9-231">Agrega una versión de fila está oculta.</span><span class="sxs-lookup"><span data-stu-id="35df9-231">Adds a hidden row version.</span></span> <span data-ttu-id="35df9-232">`RowVersion`debe agregarse para que devolución enlaza el valor.</span><span class="sxs-lookup"><span data-stu-id="35df9-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="35df9-233">Muestra el último byte de `RowVersion` para fines de depuración.</span><span class="sxs-lookup"><span data-stu-id="35df9-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="35df9-234">Reemplaza `ViewData` con fuertemente tipadas `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="35df9-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="35df9-235">Conflictos de simultaneidad de prueba con la página de edición</span><span class="sxs-lookup"><span data-stu-id="35df9-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="35df9-236">Abra dos instancias de exploradores de edición en el departamento de inglés:</span><span class="sxs-lookup"><span data-stu-id="35df9-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="35df9-237">Ejecute la aplicación y seleccione departamentos.</span><span class="sxs-lookup"><span data-stu-id="35df9-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="35df9-238">Haga clic en el **editar** hipervínculo para el departamento de inglés y seleccione **abrir en nueva ficha**.</span><span class="sxs-lookup"><span data-stu-id="35df9-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="35df9-239">En la primera ficha, haga clic en el **editar** hipervínculo para el departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="35df9-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="35df9-240">Las fichas de dos explorador muestran la misma información.</span><span class="sxs-lookup"><span data-stu-id="35df9-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="35df9-241">Cambiar el nombre de la primera pestaña de explorador y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="35df9-241">Change the name in the first browser tab and click **Save**.</span></span>

![Editar página 1 después de cambio de departamento](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="35df9-243">El explorador muestra la página de índice con el valor cambiado y el indicador de rowVersion actualizada.</span><span class="sxs-lookup"><span data-stu-id="35df9-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="35df9-244">Tenga en cuenta el indicador rowVersion actualizada, se muestra en la segunda devolución de datos en la ficha otros.</span><span class="sxs-lookup"><span data-stu-id="35df9-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="35df9-245">Cambiar un campo diferente en la segunda pestaña de explorador.</span><span class="sxs-lookup"><span data-stu-id="35df9-245">Change a different field in the second browser tab.</span></span>

![Editar página 2 después de cambio de departamento](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="35df9-247">Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="35df9-247">Click **Save**.</span></span> <span data-ttu-id="35df9-248">Vea mensajes de error para todos los campos que no coinciden con los valores de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="35df9-248">You see error messages for all fields that don't match the DB values:</span></span>

![Mensaje de error de página de edición de departamento](concurrency/_static/edit-error.png)

<span data-ttu-id="35df9-250">Esta ventana del explorador no pretendía cambiar el campo de nombre.</span><span class="sxs-lookup"><span data-stu-id="35df9-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="35df9-251">Copie y pegue el valor actual (idiomas) en el campo nombre.</span><span class="sxs-lookup"><span data-stu-id="35df9-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="35df9-252">Pestaña salida. Validación del lado cliente quita el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="35df9-252">Tab out. Client-side validation removes the error message.</span></span>

![Mensaje de error de página de edición de departamento](concurrency/_static/cv.png)

<span data-ttu-id="35df9-254">Haga clic en **guardar** nuevo.</span><span class="sxs-lookup"><span data-stu-id="35df9-254">Click **Save** again.</span></span> <span data-ttu-id="35df9-255">Se guarda el valor especificado en la segunda pestaña de explorador.</span><span class="sxs-lookup"><span data-stu-id="35df9-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="35df9-256">Vea los valores guardados en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="35df9-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="35df9-257">Actualizar la página de borrado</span><span class="sxs-lookup"><span data-stu-id="35df9-257">Update the Delete page</span></span>

<span data-ttu-id="35df9-258">Actualice el modelo de páginas de eliminación con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35df9-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="35df9-259">La página de borrado detecta los conflictos de simultaneidad cuando la entidad ha cambiado después de se capturó.</span><span class="sxs-lookup"><span data-stu-id="35df9-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="35df9-260">`Department.RowVersion`es la versión de fila cuando se capturó la entidad.</span><span class="sxs-lookup"><span data-stu-id="35df9-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="35df9-261">Cuando el núcleo de EF crea el comando SQL DELETE, incluye una cláusula WHERE con `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="35df9-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="35df9-262">Si los resultados del comando SQL DELETE en cero filas afectadas:</span><span class="sxs-lookup"><span data-stu-id="35df9-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="35df9-263">El `RowVersion` en DELETE de SQL no coincide con el comando `RowVersion` en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="35df9-264">Se produce una excepción DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="35df9-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="35df9-265">`OnGetAsync`se llama con la `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="35df9-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="35df9-266">Actualizar la página de borrado</span><span class="sxs-lookup"><span data-stu-id="35df9-266">Update the Delete page</span></span>

<span data-ttu-id="35df9-267">Actualización *Pages/Departments/Delete.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35df9-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="35df9-268">El marcado anterior realiza los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="35df9-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="35df9-269">Las actualizaciones de la `page` la directiva de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="35df9-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="35df9-270">Agrega un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="35df9-270">Adds an error message.</span></span>
* <span data-ttu-id="35df9-271">Reemplaza FirstMidName por FullName en la **administrador** campo.</span><span class="sxs-lookup"><span data-stu-id="35df9-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="35df9-272">Cambios `RowVersion` para mostrar el último byte.</span><span class="sxs-lookup"><span data-stu-id="35df9-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="35df9-273">Agrega una versión de fila está oculta.</span><span class="sxs-lookup"><span data-stu-id="35df9-273">Adds a hidden row version.</span></span> <span data-ttu-id="35df9-274">`RowVersion`debe agregarse para que devolución enlaza el valor.</span><span class="sxs-lookup"><span data-stu-id="35df9-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="35df9-275">Conflictos de simultaneidad de prueba con la página de borrado</span><span class="sxs-lookup"><span data-stu-id="35df9-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="35df9-276">Cree un departamento de prueba.</span><span class="sxs-lookup"><span data-stu-id="35df9-276">Create a test department.</span></span>

<span data-ttu-id="35df9-277">Abra dos instancias de exploradores de eliminación en el departamento de prueba:</span><span class="sxs-lookup"><span data-stu-id="35df9-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="35df9-278">Ejecute la aplicación y seleccione departamentos.</span><span class="sxs-lookup"><span data-stu-id="35df9-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="35df9-279">Haga clic en el **eliminar** hipervínculo para el departamento de prueba y seleccione **abrir en nueva ficha**.</span><span class="sxs-lookup"><span data-stu-id="35df9-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="35df9-280">Haga clic en el **editar** hipervínculo para el departamento de prueba.</span><span class="sxs-lookup"><span data-stu-id="35df9-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="35df9-281">Las fichas de dos explorador muestran la misma información.</span><span class="sxs-lookup"><span data-stu-id="35df9-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="35df9-282">Cambiar la asignación en la primera pestaña de explorador y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="35df9-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="35df9-283">El explorador muestra la página de índice con el valor cambiado y el indicador de rowVersion actualizada.</span><span class="sxs-lookup"><span data-stu-id="35df9-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="35df9-284">Tenga en cuenta el indicador rowVersion actualizada, se muestra en la segunda devolución de datos en la ficha otros.</span><span class="sxs-lookup"><span data-stu-id="35df9-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="35df9-285">Elimine el departamento de prueba de la segunda pestaña. Un error de simultaneidad es que se muestran con los valores actuales de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="35df9-286">Haga clic en **eliminar** elimina la entidad, a menos que `RowVersion` ha sido updated.department se ha eliminado.</span><span class="sxs-lookup"><span data-stu-id="35df9-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="35df9-287">Vea [herencia](xref:data/ef-mvc/inheritance) para obtener instrucciones sobre cómo la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="35df9-287">See [Inheritance](xref:data/ef-mvc/inheritance) for instruction on how to inheritance in the data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="35df9-288">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="35df9-288">Additional resources</span></span>

* [<span data-ttu-id="35df9-289">Tokens de simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="35df9-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="35df9-290">Control de simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="35df9-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="35df9-291">Anterior</span><span class="sxs-lookup"><span data-stu-id="35df9-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
