---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Simultaneidad (8 de 8)'
author: rick-anderson
description: Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2018
uid: data/ef-rp/concurrency
ms.openlocfilehash: 17ce0c111daabe2c7bbf4795b658856568c85158
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886180"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="36448-103">Páginas de Razor con EF Core en ASP.NET Core: Simultaneidad (8 de 8)</span><span class="sxs-lookup"><span data-stu-id="36448-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="36448-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) y [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="36448-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="36448-105">Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan una entidad de forma simultánea (al mismo tiempo).</span><span class="sxs-lookup"><span data-stu-id="36448-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="36448-106">Si experimenta problemas que no puede resolver, [descargue o vea la aplicación completada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="36448-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="36448-107">[Instrucciones de descarga](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="36448-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="36448-108">Conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="36448-108">Concurrency conflicts</span></span>

<span data-ttu-id="36448-109">Un conflicto de simultaneidad se produce cuando:</span><span class="sxs-lookup"><span data-stu-id="36448-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="36448-110">Un usuario va a la página de edición de una entidad.</span><span class="sxs-lookup"><span data-stu-id="36448-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="36448-111">Otro usuario actualiza la misma entidad antes de que el cambio del primer usuario se escriba en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="36448-112">Si no está habilitada la detección de simultaneidad, cuando se produzcan actualizaciones simultáneas:</span><span class="sxs-lookup"><span data-stu-id="36448-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="36448-113">Prevalece la última actualización.</span><span class="sxs-lookup"><span data-stu-id="36448-113">The last update wins.</span></span> <span data-ttu-id="36448-114">Es decir, los últimos valores de actualización se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="36448-115">La primera de las actualizaciones actuales se pierde.</span><span class="sxs-lookup"><span data-stu-id="36448-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="36448-116">Simultaneidad optimista</span><span class="sxs-lookup"><span data-stu-id="36448-116">Optimistic concurrency</span></span>

<span data-ttu-id="36448-117">La simultaneidad optimista permite que se produzcan conflictos de simultaneidad y luego reacciona correctamente si ocurren.</span><span class="sxs-lookup"><span data-stu-id="36448-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="36448-118">Por ejemplo, Jane visita la página de edición de Department y cambia el presupuesto para el departamento de inglés de 350.000,00 a 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="36448-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Cambiar el presupuesto a 0](concurrency/_static/change-budget.png)

<span data-ttu-id="36448-120">Antes de que Jane haga clic en **Save**, John visita la misma página y cambia el campo Start Date de 9/1/2007 a 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="36448-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Cambiar la fecha de inicio a 2013](concurrency/_static/change-date.png)

<span data-ttu-id="36448-122">Jane hace clic en **Save** primero y ve su cambio cuando el explorador muestra la página de índice.</span><span class="sxs-lookup"><span data-stu-id="36448-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Presupuesto cambiado a cero](concurrency/_static/budget-zero.png)

<span data-ttu-id="36448-124">John hace clic en **Save** en una página Edit que sigue mostrando un presupuesto de 350.000,00 USD.</span><span class="sxs-lookup"><span data-stu-id="36448-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="36448-125">Lo que sucede después viene determinado por cómo controla los conflictos de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="36448-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="36448-126">La simultaneidad optimista incluye las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="36448-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="36448-127">Puede realizar un seguimiento de la propiedad que ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="36448-128">En el escenario, no se perderá ningún dato.</span><span class="sxs-lookup"><span data-stu-id="36448-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="36448-129">Los dos usuarios actualizaron diferentes propiedades.</span><span class="sxs-lookup"><span data-stu-id="36448-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="36448-130">La próxima vez que un usuario examine el departamento de inglés, verá los cambios tanto de Jane como de John.</span><span class="sxs-lookup"><span data-stu-id="36448-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="36448-131">Este método de actualización puede reducir el número de conflictos que pueden dar lugar a una pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="36448-132">Este enfoque:</span><span class="sxs-lookup"><span data-stu-id="36448-132">This approach:</span></span>
 
  * <span data-ttu-id="36448-133">No puede evitar la pérdida de datos si se realizan cambios paralelos a la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="36448-133">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="36448-134">Por lo general, no es práctico en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="36448-134">Is generally not practical in a web app.</span></span> <span data-ttu-id="36448-135">Requiere mantener un estado significativo para realizar un seguimiento de todos los valores capturados y nuevos.</span><span class="sxs-lookup"><span data-stu-id="36448-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="36448-136">El mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="36448-136">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="36448-137">Puede aumentar la complejidad de las aplicaciones en comparación con la detección de simultaneidad en una entidad.</span><span class="sxs-lookup"><span data-stu-id="36448-137">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="36448-138">Puede permitir que los cambios de John sobrescriban los cambios de Jane.</span><span class="sxs-lookup"><span data-stu-id="36448-138">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="36448-139">La próxima vez que un usuario examine el departamento de inglés, verá 9/1/2013 y el valor de 350.000,00 USD capturado.</span><span class="sxs-lookup"><span data-stu-id="36448-139">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="36448-140">Este enfoque se denomina un escenario de *Prevalece el cliente* o *Prevalece el último*.</span><span class="sxs-lookup"><span data-stu-id="36448-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="36448-141">(Todos los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos). Si no hace ninguna codificación para el control de la simultaneidad, Prevalece el cliente se realizará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="36448-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="36448-142">Puede evitar que el cambio de John se actualice en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="36448-143">Normalmente, la aplicación podría:</span><span class="sxs-lookup"><span data-stu-id="36448-143">Typically, the app would:</span></span>

  * <span data-ttu-id="36448-144">Mostrar un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="36448-144">Display an error message.</span></span>
  * <span data-ttu-id="36448-145">Mostrar el estado actual de los datos.</span><span class="sxs-lookup"><span data-stu-id="36448-145">Show the current state of the data.</span></span>
  * <span data-ttu-id="36448-146">Permitir al usuario volver a aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="36448-146">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="36448-147">Esto se denomina un escenario de *Prevalece el almacén*.</span><span class="sxs-lookup"><span data-stu-id="36448-147">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="36448-148">(Los valores del almacén de datos tienen prioridad sobre los valores enviados por el cliente). En este tutorial implementará el escenario de Prevalece el almacén.</span><span class="sxs-lookup"><span data-stu-id="36448-148">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="36448-149">Este método garantiza que ningún cambio se sobrescriba sin que se avise al usuario.</span><span class="sxs-lookup"><span data-stu-id="36448-149">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="36448-150">Administrar la simultaneidad</span><span class="sxs-lookup"><span data-stu-id="36448-150">Handling concurrency</span></span> 

<span data-ttu-id="36448-151">Cuando una propiedad se configura como un [token de simultaneidad](/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="36448-151">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="36448-152">EF Core comprueba que no se ha modificado la propiedad después de que se capturase.</span><span class="sxs-lookup"><span data-stu-id="36448-152">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="36448-153">La comprobación se produce cuando se llama a [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="36448-153">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="36448-154">Si se ha cambiado la propiedad después de haberla capturado, se produce una excepción [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="36448-154">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="36448-155">Deben configurarse el modelo de datos y la base de datos para que admitan producir una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="36448-155">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="36448-156">Detectar conflictos de simultaneidad en una propiedad</span><span class="sxs-lookup"><span data-stu-id="36448-156">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="36448-157">Se pueden detectar conflictos de simultaneidad en el nivel de propiedad con el atributo [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="36448-157">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="36448-158">El atributo se puede aplicar a varias propiedades en el modelo.</span><span class="sxs-lookup"><span data-stu-id="36448-158">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="36448-159">Para obtener más información, consulte [Anotaciones de datos: ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="36448-159">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="36448-160">El atributo `[ConcurrencyCheck]` no se usa en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="36448-160">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="36448-161">Detectar conflictos de simultaneidad en una fila</span><span class="sxs-lookup"><span data-stu-id="36448-161">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="36448-162">Para detectar conflictos de simultaneidad, se agrega al modelo una columna de seguimiento [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="36448-162">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="36448-163">`rowversion`:</span><span class="sxs-lookup"><span data-stu-id="36448-163">`rowversion` :</span></span>

* <span data-ttu-id="36448-164">Es específico de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="36448-164">Is SQL Server specific.</span></span> <span data-ttu-id="36448-165">Otras bases de datos podrían no proporcionar una característica similar.</span><span class="sxs-lookup"><span data-stu-id="36448-165">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="36448-166">Se usa para determinar que no se ha cambiado una entidad desde que se capturó de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-166">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="36448-167">La base de datos genera un número `rowversion` secuencial que se incrementa cada vez que se actualiza la fila.</span><span class="sxs-lookup"><span data-stu-id="36448-167">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="36448-168">En un comando `Update` o `Delete`, la cláusula `Where` incluye el valor capturado de `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="36448-168">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="36448-169">Si la fila que se está actualizando ha cambiado:</span><span class="sxs-lookup"><span data-stu-id="36448-169">If the row being updated has changed:</span></span>

* <span data-ttu-id="36448-170">`rowversion` no coincide con el valor capturado.</span><span class="sxs-lookup"><span data-stu-id="36448-170">`rowversion` doesn't match the fetched value.</span></span>
* <span data-ttu-id="36448-171">Los comandos `Update` o `Delete` no encuentran una fila porque la cláusula `Where` incluye la `rowversion` capturada.</span><span class="sxs-lookup"><span data-stu-id="36448-171">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
* <span data-ttu-id="36448-172">Se produce una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="36448-172">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="36448-173">En EF Core, cuando un comando `Update` o `Delete` no han actualizado ninguna fila, se produce una excepción de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="36448-173">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="36448-174">Agregar una propiedad de seguimiento a la entidad Department</span><span class="sxs-lookup"><span data-stu-id="36448-174">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="36448-175">En *Models/Department.cs*, agregue una propiedad de seguimiento denominada RowVersion:</span><span class="sxs-lookup"><span data-stu-id="36448-175">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="36448-176">El atributo [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) especifica que esta columna se incluye en la cláusula `Where` de los comandos `Update` y `Delete`.</span><span class="sxs-lookup"><span data-stu-id="36448-176">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="36448-177">El atributo se denomina `Timestamp` porque las versiones anteriores de SQL Server usaban un tipo de datos `timestamp` antes de que el tipo `rowversion` de SQL lo sustituyera por otro.</span><span class="sxs-lookup"><span data-stu-id="36448-177">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="36448-178">La API fluida también puede especificar la propiedad de seguimiento:</span><span class="sxs-lookup"><span data-stu-id="36448-178">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="36448-179">El código siguiente muestra una parte del T-SQL generado por EF Core cuando se actualiza el nombre de Department:</span><span class="sxs-lookup"><span data-stu-id="36448-179">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="36448-180">El código resaltado anteriormente muestra la cláusula `WHERE` que contiene `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="36448-180">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="36448-181">Si la base de datos `RowVersion` no es igual al parámetro `RowVersion` (`@p2`), no se ha actualizado ninguna fila.</span><span class="sxs-lookup"><span data-stu-id="36448-181">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="36448-182">El código resaltado a continuación muestra el T-SQL que comprueba que se actualizó exactamente una fila:</span><span class="sxs-lookup"><span data-stu-id="36448-182">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="36448-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) devuelve el número de filas afectadas por la última instrucción.</span><span class="sxs-lookup"><span data-stu-id="36448-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="36448-184">Si no se actualiza ninguna fila, EF Core produce una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="36448-184">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="36448-185">Puede ver el T-SQL que genera EF Core en la ventana de salida de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36448-185">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="36448-186">Actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="36448-186">Update the DB</span></span>

<span data-ttu-id="36448-187">Agregar la propiedad `RowVersion` cambia el modelo de base de datos, lo que requiere una migración.</span><span class="sxs-lookup"><span data-stu-id="36448-187">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="36448-188">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="36448-188">Build the project.</span></span> <span data-ttu-id="36448-189">Escriba lo siguiente en una ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="36448-189">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="36448-190">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="36448-190">The preceding commands:</span></span>

* <span data-ttu-id="36448-191">Agregan el archivo de migración *Migrations/{time stamp}_RowVersion.cs*.</span><span class="sxs-lookup"><span data-stu-id="36448-191">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="36448-192">Actualizan el archivo *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="36448-192">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="36448-193">La actualización agrega el siguiente código resaltado al método `BuildModel`:</span><span class="sxs-lookup"><span data-stu-id="36448-193">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="36448-194">Ejecutan las migraciones para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-194">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a><span data-ttu-id="36448-195">Aplicar la técnica scaffolding al modelo Departments</span><span class="sxs-lookup"><span data-stu-id="36448-195">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="36448-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36448-196">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="36448-197">Siga las instrucciones que encontrará en [Aplicación de scaffolding al modelo de alumnos](xref:data/ef-rp/intro#scaffold-the-student-model) y use `Department` para la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="36448-197">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Department` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="36448-198">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="36448-198">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="36448-199">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="36448-199">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="36448-200">El comando anterior aplica scaffolding al modelo `Department`.</span><span class="sxs-lookup"><span data-stu-id="36448-200">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="36448-201">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36448-201">Open the project in Visual Studio.</span></span>

<span data-ttu-id="36448-202">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="36448-202">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="36448-203">Actualizar la página de índice de Departments</span><span class="sxs-lookup"><span data-stu-id="36448-203">Update the Departments Index page</span></span>

<span data-ttu-id="36448-204">El motor de scaffolding creó una columna `RowVersion` para la página de índice, pero ese campo no debería mostrarse.</span><span class="sxs-lookup"><span data-stu-id="36448-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="36448-205">En este tutorial, el último byte de la `RowVersion` se muestra para ayudar a entender la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="36448-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="36448-206">No se garantiza que el último byte sea único.</span><span class="sxs-lookup"><span data-stu-id="36448-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="36448-207">Una aplicación real no mostraría `RowVersion` ni el último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="36448-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="36448-208">Actualice la página Index:</span><span class="sxs-lookup"><span data-stu-id="36448-208">Update the Index page:</span></span>

* <span data-ttu-id="36448-209">Reemplace Index por Departments.</span><span class="sxs-lookup"><span data-stu-id="36448-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="36448-210">Reemplace el marcado que contiene `RowVersion` por el último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="36448-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="36448-211">Reemplace FirstMidName por FullName.</span><span class="sxs-lookup"><span data-stu-id="36448-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="36448-212">El marcado siguiente muestra la página actualizada:</span><span class="sxs-lookup"><span data-stu-id="36448-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="36448-213">Actualizar el modelo de la página Edit</span><span class="sxs-lookup"><span data-stu-id="36448-213">Update the Edit page model</span></span>

<span data-ttu-id="36448-214">Actualice *pages\departments\edit.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="36448-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="36448-215">Para detectar un problema de simultaneidad, el [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) se actualiza con el valor `rowVersion` de la entidad de la que se capturó.</span><span class="sxs-lookup"><span data-stu-id="36448-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="36448-216">EF Core genera un comando UPDATE de SQL con una cláusula WHERE que contiene el valor `RowVersion` original.</span><span class="sxs-lookup"><span data-stu-id="36448-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="36448-217">Si no hay ninguna fila afectada por el comando UPDATE (ninguna fila tiene el valor `RowVersion` original), se produce una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="36448-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="36448-218">En el código anterior, `Department.RowVersion` es el valor cuando se capturó la entidad.</span><span class="sxs-lookup"><span data-stu-id="36448-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="36448-219">`OriginalValue` es el valor de la base de datos cuando se llamó a `FirstOrDefaultAsync` en este método.</span><span class="sxs-lookup"><span data-stu-id="36448-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="36448-220">El código siguiente obtiene los valores de cliente (es decir, los valores registrados en este método) y los valores de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="36448-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="36448-221">El código siguiente agrega un mensaje de error personalizado para cada columna que tiene valores de la base de datos diferentes de lo que publicado en `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="36448-221">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="36448-222">El código resaltado a continuación establece el valor `RowVersion` para el nuevo valor recuperado de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="36448-223">La próxima vez que el usuario haga clic en **Save**, solo se detectarán los errores de simultaneidad que se produzcan desde la última visualización de la página Edit.</span><span class="sxs-lookup"><span data-stu-id="36448-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="36448-224">La instrucción `ModelState.Remove` es necesaria porque `ModelState` tiene el valor `RowVersion` antiguo.</span><span class="sxs-lookup"><span data-stu-id="36448-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="36448-225">En la página de Razor, el valor `ModelState` de un campo tiene prioridad sobre los valores de propiedad de modelo cuando ambos están presentes.</span><span class="sxs-lookup"><span data-stu-id="36448-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="36448-226">Actualizar la página Edit</span><span class="sxs-lookup"><span data-stu-id="36448-226">Update the Edit page</span></span>

<span data-ttu-id="36448-227">Actualice *Pages/Departments/Edit.cshtml* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="36448-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="36448-228">El marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="36448-228">The preceding markup:</span></span>

* <span data-ttu-id="36448-229">Actualiza la directiva `page` de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="36448-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="36448-230">Agrega una versión de fila oculta.</span><span class="sxs-lookup"><span data-stu-id="36448-230">Adds a hidden row version.</span></span> <span data-ttu-id="36448-231">Se debe agregar `RowVersion` para que la devolución enlace el valor.</span><span class="sxs-lookup"><span data-stu-id="36448-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="36448-232">Muestra el último byte de `RowVersion` para fines de depuración.</span><span class="sxs-lookup"><span data-stu-id="36448-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="36448-233">Reemplaza `ViewData` con `InstructorNameSL` fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="36448-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="36448-234">Comprobar los conflictos de simultaneidad con la página Edit</span><span class="sxs-lookup"><span data-stu-id="36448-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="36448-235">Abra dos instancias de exploradores de Edit en el departamento de inglés:</span><span class="sxs-lookup"><span data-stu-id="36448-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="36448-236">Ejecute la aplicación y seleccione Departments.</span><span class="sxs-lookup"><span data-stu-id="36448-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="36448-237">Haga clic con el botón derecho en el hipervínculo **Edit** del departamento de inglés y seleccione **Abrir en nueva pestaña**.</span><span class="sxs-lookup"><span data-stu-id="36448-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="36448-238">En la primera pestaña, haga clic en el hipervínculo **Edit** del departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="36448-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="36448-239">Las dos pestañas del explorador muestran la misma información.</span><span class="sxs-lookup"><span data-stu-id="36448-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="36448-240">Cambie el nombre en la primera pestaña del explorador y haga clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="36448-240">Change the name in the first browser tab and click **Save**.</span></span>

![Página 1 de Department Edit después del cambio](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="36448-242">El explorador muestra la página de índice con el valor modificado y el indicador rowVersion actualizado.</span><span class="sxs-lookup"><span data-stu-id="36448-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="36448-243">Tenga en cuenta el indicador rowVersion actualizado, que se muestra en el segundo postback en la otra pestaña.</span><span class="sxs-lookup"><span data-stu-id="36448-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="36448-244">Cambie otro campo en la segunda pestaña del explorador.</span><span class="sxs-lookup"><span data-stu-id="36448-244">Change a different field in the second browser tab.</span></span>

![Página 2 de Department Edit después del cambio](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="36448-246">Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="36448-246">Click **Save**.</span></span> <span data-ttu-id="36448-247">Verá mensajes de error para todos los campos que no coinciden con los valores de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="36448-247">You see error messages for all fields that don't match the DB values:</span></span>

![Mensaje de error de la página Department Edit](concurrency/_static/edit-error.png)

<span data-ttu-id="36448-249">Esta ventana del explorador no planeaba cambiar el campo Name.</span><span class="sxs-lookup"><span data-stu-id="36448-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="36448-250">Copie y pegue el valor actual (Languages) en el campo Name.</span><span class="sxs-lookup"><span data-stu-id="36448-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="36448-251">Presione TAB para salir del campo. La validación del lado cliente quita el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="36448-251">Tab out. Client-side validation removes the error message.</span></span>

![Mensaje de error de la página Department Edit](concurrency/_static/cv.png)

<span data-ttu-id="36448-253">Vuelva a hacer clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="36448-253">Click **Save** again.</span></span> <span data-ttu-id="36448-254">Se guarda el valor especificado en la segunda pestaña del explorador.</span><span class="sxs-lookup"><span data-stu-id="36448-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="36448-255">Verá los valores guardados en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="36448-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="36448-256">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="36448-256">Update the Delete page</span></span>

<span data-ttu-id="36448-257">Actualice el modelo de la página Delete con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="36448-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="36448-258">La página Delete detecta los conflictos de simultaneidad cuando la entidad ha cambiado después de que se capturase.</span><span class="sxs-lookup"><span data-stu-id="36448-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="36448-259">`Department.RowVersion` es la versión de fila cuando se capturó la entidad.</span><span class="sxs-lookup"><span data-stu-id="36448-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="36448-260">Cuando EF Core crea el comando DELETE de SQL, incluye una cláusula WHERE con `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="36448-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="36448-261">Si el comando DELETE de SQL tiene como resultado cero filas afectadas:</span><span class="sxs-lookup"><span data-stu-id="36448-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="36448-262">La `RowVersion` del comando DELETE de SQL no coincide con la `RowVersion` de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="36448-263">Se produce una excepción DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="36448-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="36448-264">Se llama a `OnGetAsync` con el `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="36448-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="36448-265">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="36448-265">Update the Delete page</span></span>

<span data-ttu-id="36448-266">Actualice *Pages/Departments/Delete.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="36448-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="36448-267">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="36448-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="36448-268">Se actualiza la directiva `page` de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="36448-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="36448-269">Se agrega un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="36448-269">Adds an error message.</span></span>
* <span data-ttu-id="36448-270">Se reemplaza FirstMidName por FullName en el campo **Administrator**.</span><span class="sxs-lookup"><span data-stu-id="36448-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="36448-271">Se cambia `RowVersion` para que muestre el último byte.</span><span class="sxs-lookup"><span data-stu-id="36448-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="36448-272">Se agrega una versión de fila oculta.</span><span class="sxs-lookup"><span data-stu-id="36448-272">Adds a hidden row version.</span></span> <span data-ttu-id="36448-273">Se debe agregar `RowVersion` para que la devolución enlace el valor.</span><span class="sxs-lookup"><span data-stu-id="36448-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="36448-274">Comprobar los conflictos de simultaneidad con la página Delete</span><span class="sxs-lookup"><span data-stu-id="36448-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="36448-275">Cree un departamento de prueba.</span><span class="sxs-lookup"><span data-stu-id="36448-275">Create a test department.</span></span>

<span data-ttu-id="36448-276">Abra dos instancias de exploradores de Delete en el departamento de prueba:</span><span class="sxs-lookup"><span data-stu-id="36448-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="36448-277">Ejecute la aplicación y seleccione Departments.</span><span class="sxs-lookup"><span data-stu-id="36448-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="36448-278">Haga clic con el botón derecho en el hipervínculo **Delete** del departamento de prueba y seleccione **Abrir en nueva pestaña**.</span><span class="sxs-lookup"><span data-stu-id="36448-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="36448-279">Haga clic en el hipervínculo **Edit** del departamento de prueba.</span><span class="sxs-lookup"><span data-stu-id="36448-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="36448-280">Las dos pestañas del explorador muestran la misma información.</span><span class="sxs-lookup"><span data-stu-id="36448-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="36448-281">Cambie el presupuesto en la primera pestaña del explorador y haga clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="36448-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="36448-282">El explorador muestra la página de índice con el valor modificado y el indicador rowVersion actualizado.</span><span class="sxs-lookup"><span data-stu-id="36448-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="36448-283">Tenga en cuenta el indicador rowVersion actualizado, que se muestra en el segundo postback en la otra pestaña.</span><span class="sxs-lookup"><span data-stu-id="36448-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="36448-284">Elimine el departamento de prueba de la segunda pestaña. Se mostrará un error de simultaneidad con los valores actuales de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="36448-285">Al hacer clic en **Delete** se elimina la entidad, a menos que se haya actualizado `RowVersion`. El departamento se ha eliminado.</span><span class="sxs-lookup"><span data-stu-id="36448-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="36448-286">Vea [Herencia](xref:data/ef-mvc/inheritance) para obtener información sobre cómo se hereda un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="36448-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="36448-287">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="36448-287">Additional resources</span></span>

* [<span data-ttu-id="36448-288">Tokens de simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="36448-288">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="36448-289">Controlar la simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="36448-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="36448-290">Versión de YouTube de este tutorial (gestión de conflictos de simultaneidad)</span><span class="sxs-lookup"><span data-stu-id="36448-290">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="36448-291">Versión en YouTube de este tutorial (parte 2)</span><span class="sxs-lookup"><span data-stu-id="36448-291">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="36448-292">Versión en YouTube de este tutorial (parte 3)</span><span class="sxs-lookup"><span data-stu-id="36448-292">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [<span data-ttu-id="36448-293">Anterior</span><span class="sxs-lookup"><span data-stu-id="36448-293">Previous</span></span>](xref:data/ef-rp/update-related-data)
