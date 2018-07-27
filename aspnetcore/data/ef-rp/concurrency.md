---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Simultaneidad (8 de 8)'
author: rick-anderson
description: Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: ff9e52df63f9c9f47ee659a68beb28b773a114a1
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202697"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="990a6-103">Páginas de Razor con EF Core en ASP.NET Core: Simultaneidad (8 de 8)</span><span class="sxs-lookup"><span data-stu-id="990a6-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="990a6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) y [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="990a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="990a6-105">Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan una entidad de forma simultánea (al mismo tiempo).</span><span class="sxs-lookup"><span data-stu-id="990a6-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="990a6-106">Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="990a6-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="990a6-107">Conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="990a6-107">Concurrency conflicts</span></span>

<span data-ttu-id="990a6-108">Un conflicto de simultaneidad se produce cuando:</span><span class="sxs-lookup"><span data-stu-id="990a6-108">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="990a6-109">Un usuario va a la página de edición de una entidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-109">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="990a6-110">Otro usuario actualiza la misma entidad antes de que el cambio del primer usuario se escriba en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-110">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="990a6-111">Si no está habilitada la detección de simultaneidad, cuando se produzcan actualizaciones simultáneas:</span><span class="sxs-lookup"><span data-stu-id="990a6-111">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="990a6-112">Prevalece la última actualización.</span><span class="sxs-lookup"><span data-stu-id="990a6-112">The last update wins.</span></span> <span data-ttu-id="990a6-113">Es decir, los últimos valores de actualización se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-113">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="990a6-114">La primera de las actualizaciones actuales se pierde.</span><span class="sxs-lookup"><span data-stu-id="990a6-114">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="990a6-115">Simultaneidad optimista</span><span class="sxs-lookup"><span data-stu-id="990a6-115">Optimistic concurrency</span></span>

<span data-ttu-id="990a6-116">La simultaneidad optimista permite que se produzcan conflictos de simultaneidad y luego reacciona correctamente si ocurren.</span><span class="sxs-lookup"><span data-stu-id="990a6-116">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="990a6-117">Por ejemplo, Jane visita la página de edición de Department y cambia el presupuesto para el departamento de inglés de 350.000,00 a 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="990a6-117">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Cambiar el presupuesto a 0](concurrency/_static/change-budget.png)

<span data-ttu-id="990a6-119">Antes de que Jane haga clic en **Save**, John visita la misma página y cambia el campo Start Date de 9/1/2007 a 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="990a6-119">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Cambiar la fecha de inicio a 2013](concurrency/_static/change-date.png)

<span data-ttu-id="990a6-121">Jane hace clic en **Save** primero y ve su cambio cuando el explorador muestra la página de índice.</span><span class="sxs-lookup"><span data-stu-id="990a6-121">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Presupuesto cambiado a cero](concurrency/_static/budget-zero.png)

<span data-ttu-id="990a6-123">John hace clic en **Save** en una página Edit que sigue mostrando un presupuesto de 350.000,00 USD.</span><span class="sxs-lookup"><span data-stu-id="990a6-123">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="990a6-124">Lo que sucede después viene determinado por cómo controla los conflictos de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-124">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="990a6-125">La simultaneidad optimista incluye las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="990a6-125">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="990a6-126">Puede realizar un seguimiento de la propiedad que ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-126">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="990a6-127">En el escenario, no se perderá ningún dato.</span><span class="sxs-lookup"><span data-stu-id="990a6-127">In the scenario, no data would be lost.</span></span> <span data-ttu-id="990a6-128">Los dos usuarios actualizaron diferentes propiedades.</span><span class="sxs-lookup"><span data-stu-id="990a6-128">Different properties were updated by the two users.</span></span> <span data-ttu-id="990a6-129">La próxima vez que un usuario examine el departamento de inglés, verá los cambios tanto de Jane como de John.</span><span class="sxs-lookup"><span data-stu-id="990a6-129">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="990a6-130">Este método de actualización puede reducir el número de conflictos que pueden dar lugar a una pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-130">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="990a6-131">Este enfoque:</span><span class="sxs-lookup"><span data-stu-id="990a6-131">This approach:</span></span>
 
  * <span data-ttu-id="990a6-132">No puede evitar la pérdida de datos si se realizan cambios paralelos a la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="990a6-132">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="990a6-133">Por lo general, no es práctico en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="990a6-133">Is generally not practical in a web app.</span></span> <span data-ttu-id="990a6-134">Requiere mantener un estado significativo para realizar un seguimiento de todos los valores capturados y nuevos.</span><span class="sxs-lookup"><span data-stu-id="990a6-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="990a6-135">El mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="990a6-135">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="990a6-136">Puede aumentar la complejidad de las aplicaciones en comparación con la detección de simultaneidad en una entidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-136">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="990a6-137">Puede permitir que los cambios de John sobrescriban los cambios de Jane.</span><span class="sxs-lookup"><span data-stu-id="990a6-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="990a6-138">La próxima vez que un usuario examine el departamento de inglés, verá 9/1/2013 y el valor de 350.000,00 USD capturado.</span><span class="sxs-lookup"><span data-stu-id="990a6-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="990a6-139">Este enfoque se denomina un escenario de *Prevalece el cliente* o *Prevalece el último*.</span><span class="sxs-lookup"><span data-stu-id="990a6-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="990a6-140">(Todos los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos). Si no hace ninguna codificación para el control de la simultaneidad, Prevalece el cliente se realizará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="990a6-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="990a6-141">Puede evitar que el cambio de John se actualice en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="990a6-142">Normalmente, la aplicación podría:</span><span class="sxs-lookup"><span data-stu-id="990a6-142">Typically, the app would:</span></span>

  * <span data-ttu-id="990a6-143">Mostrar un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="990a6-143">Display an error message.</span></span>
  * <span data-ttu-id="990a6-144">Mostrar el estado actual de los datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-144">Show the current state of the data.</span></span>
  * <span data-ttu-id="990a6-145">Permitir al usuario volver a aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="990a6-145">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="990a6-146">Esto se denomina un escenario de *Prevalece el almacén*.</span><span class="sxs-lookup"><span data-stu-id="990a6-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="990a6-147">(Los valores del almacén de datos tienen prioridad sobre los valores enviados por el cliente). En este tutorial implementará el escenario de Prevalece el almacén.</span><span class="sxs-lookup"><span data-stu-id="990a6-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="990a6-148">Este método garantiza que ningún cambio se sobrescriba sin que se avise al usuario.</span><span class="sxs-lookup"><span data-stu-id="990a6-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="990a6-149">Administrar la simultaneidad</span><span class="sxs-lookup"><span data-stu-id="990a6-149">Handling concurrency</span></span> 

<span data-ttu-id="990a6-150">Cuando una propiedad se configura como un [token de simultaneidad](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="990a6-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="990a6-151">EF Core comprueba que no se ha modificado la propiedad después de que se capturase.</span><span class="sxs-lookup"><span data-stu-id="990a6-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="990a6-152">La comprobación se produce cuando se llama a [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="990a6-152">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="990a6-153">Si se ha cambiado la propiedad después de haberla capturado, se produce una excepción [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="990a6-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="990a6-154">Deben configurarse el modelo de datos y la base de datos para que admitan producir una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="990a6-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="990a6-155">Detectar conflictos de simultaneidad en una propiedad</span><span class="sxs-lookup"><span data-stu-id="990a6-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="990a6-156">Se pueden detectar conflictos de simultaneidad en el nivel de propiedad con el atributo [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="990a6-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="990a6-157">El atributo se puede aplicar a varias propiedades en el modelo.</span><span class="sxs-lookup"><span data-stu-id="990a6-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="990a6-158">Para obtener más información, consulte [Anotaciones de datos: ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="990a6-158">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="990a6-159">El atributo `[ConcurrencyCheck]` no se usa en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="990a6-159">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="990a6-160">Detectar conflictos de simultaneidad en una fila</span><span class="sxs-lookup"><span data-stu-id="990a6-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="990a6-161">Para detectar conflictos de simultaneidad, se agrega al modelo una columna de seguimiento [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="990a6-161">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="990a6-162">`rowversion`:</span><span class="sxs-lookup"><span data-stu-id="990a6-162">`rowversion` :</span></span>

* <span data-ttu-id="990a6-163">Es específico de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="990a6-163">Is SQL Server specific.</span></span> <span data-ttu-id="990a6-164">Otras bases de datos podrían no proporcionar una característica similar.</span><span class="sxs-lookup"><span data-stu-id="990a6-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="990a6-165">Se usa para determinar que no se ha cambiado una entidad desde que se capturó de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="990a6-166">La base de datos genera un número `rowversion` secuencial que se incrementa cada vez que se actualiza la fila.</span><span class="sxs-lookup"><span data-stu-id="990a6-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="990a6-167">En un comando `Update` o `Delete`, la cláusula `Where` incluye el valor capturado de `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="990a6-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="990a6-168">Si la fila que se está actualizando ha cambiado:</span><span class="sxs-lookup"><span data-stu-id="990a6-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="990a6-169">`rowversion` no coincide con el valor capturado.</span><span class="sxs-lookup"><span data-stu-id="990a6-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="990a6-170">Los comandos `Update` o `Delete` no encuentran una fila porque la cláusula `Where` incluye la `rowversion` capturada.</span><span class="sxs-lookup"><span data-stu-id="990a6-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="990a6-171">Se produce una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="990a6-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="990a6-172">En EF Core, cuando un comando `Update` o `Delete` no han actualizado ninguna fila, se produce una excepción de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="990a6-173">Agregar una propiedad de seguimiento a la entidad Department</span><span class="sxs-lookup"><span data-stu-id="990a6-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="990a6-174">En *Models/Department.cs*, agregue una propiedad de seguimiento denominada RowVersion:</span><span class="sxs-lookup"><span data-stu-id="990a6-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="990a6-175">El atributo [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) especifica que esta columna se incluye en la cláusula `Where` de los comandos `Update` y `Delete`.</span><span class="sxs-lookup"><span data-stu-id="990a6-175">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="990a6-176">El atributo se denomina `Timestamp` porque las versiones anteriores de SQL Server usaban un tipo de datos `timestamp` antes de que el tipo `rowversion` de SQL lo sustituyera por otro.</span><span class="sxs-lookup"><span data-stu-id="990a6-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="990a6-177">La API fluida también puede especificar la propiedad de seguimiento:</span><span class="sxs-lookup"><span data-stu-id="990a6-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="990a6-178">El código siguiente muestra una parte del T-SQL generado por EF Core cuando se actualiza el nombre de Department:</span><span class="sxs-lookup"><span data-stu-id="990a6-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="990a6-179">El código resaltado anteriormente muestra la cláusula `WHERE` que contiene `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="990a6-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="990a6-180">Si la base de datos `RowVersion` no es igual al parámetro `RowVersion` (`@p2`), no se ha actualizado ninguna fila.</span><span class="sxs-lookup"><span data-stu-id="990a6-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="990a6-181">El código resaltado a continuación muestra el T-SQL que comprueba que se actualizó exactamente una fila:</span><span class="sxs-lookup"><span data-stu-id="990a6-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="990a6-182">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) devuelve el número de filas afectadas por la última instrucción.</span><span class="sxs-lookup"><span data-stu-id="990a6-182">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="990a6-183">Si no se actualiza ninguna fila, EF Core produce una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="990a6-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="990a6-184">Puede ver el T-SQL que genera EF Core en la ventana de salida de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="990a6-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="990a6-185">Actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="990a6-185">Update the DB</span></span>

<span data-ttu-id="990a6-186">Agregar la propiedad `RowVersion` cambia el modelo de base de datos, lo que requiere una migración.</span><span class="sxs-lookup"><span data-stu-id="990a6-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="990a6-187">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="990a6-187">Build the project.</span></span> <span data-ttu-id="990a6-188">Escriba lo siguiente en una ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="990a6-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="990a6-189">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="990a6-189">The preceding commands:</span></span>

* <span data-ttu-id="990a6-190">Agregan el archivo de migración *Migrations/{time stamp}_RowVersion.cs*.</span><span class="sxs-lookup"><span data-stu-id="990a6-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="990a6-191">Actualizan el archivo *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="990a6-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="990a6-192">La actualización agrega el siguiente código resaltado al método `BuildModel`:</span><span class="sxs-lookup"><span data-stu-id="990a6-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="990a6-193">Ejecutan las migraciones para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="990a6-194">Aplicar la técnica scaffolding al modelo Departments</span><span class="sxs-lookup"><span data-stu-id="990a6-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="990a6-195">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="990a6-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="990a6-196">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="990a6-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="990a6-197">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="990a6-197">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="990a6-198">El comando anterior aplica scaffolding al modelo `Department`.</span><span class="sxs-lookup"><span data-stu-id="990a6-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="990a6-199">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="990a6-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="990a6-200">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="990a6-200">Build the project.</span></span> <span data-ttu-id="990a6-201">La compilación genera errores similares a los siguientes:</span><span class="sxs-lookup"><span data-stu-id="990a6-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="990a6-202">Cambie globalmente `_context.Department` por `_context.Departments` (es decir, agregue una "s" a `Department`).</span><span class="sxs-lookup"><span data-stu-id="990a6-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="990a6-203">Se encuentran y actualizan siete repeticiones.</span><span class="sxs-lookup"><span data-stu-id="990a6-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="990a6-204">Actualizar la página de índice de Departments</span><span class="sxs-lookup"><span data-stu-id="990a6-204">Update the Departments Index page</span></span>

<span data-ttu-id="990a6-205">El motor de scaffolding creó una columna `RowVersion` para la página de índice, pero ese campo no debería mostrarse.</span><span class="sxs-lookup"><span data-stu-id="990a6-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="990a6-206">En este tutorial, el último byte de la `RowVersion` se muestra para ayudar a entender la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="990a6-207">No se garantiza que el último byte sea único.</span><span class="sxs-lookup"><span data-stu-id="990a6-207">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="990a6-208">Una aplicación real no mostraría `RowVersion` ni el último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="990a6-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="990a6-209">Actualice la página Index:</span><span class="sxs-lookup"><span data-stu-id="990a6-209">Update the Index page:</span></span>

* <span data-ttu-id="990a6-210">Reemplace Index por Departments.</span><span class="sxs-lookup"><span data-stu-id="990a6-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="990a6-211">Reemplace el marcado que contiene `RowVersion` por el último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="990a6-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="990a6-212">Reemplace FirstMidName por FullName.</span><span class="sxs-lookup"><span data-stu-id="990a6-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="990a6-213">El marcado siguiente muestra la página actualizada:</span><span class="sxs-lookup"><span data-stu-id="990a6-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="990a6-214">Actualizar el modelo de la página Edit</span><span class="sxs-lookup"><span data-stu-id="990a6-214">Update the Edit page model</span></span>

<span data-ttu-id="990a6-215">Actualice *pages\departments\edit.cshtml.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="990a6-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="990a6-216">Para detectar un problema de simultaneidad, el [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) se actualiza con el valor `rowVersion` de la entidad de la que se capturó.</span><span class="sxs-lookup"><span data-stu-id="990a6-216">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="990a6-217">EF Core genera un comando UPDATE de SQL con una cláusula WHERE que contiene el valor `RowVersion` original.</span><span class="sxs-lookup"><span data-stu-id="990a6-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="990a6-218">Si no hay ninguna fila afectada por el comando UPDATE (ninguna fila tiene el valor `RowVersion` original), se produce una excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="990a6-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="990a6-219">En el código anterior, `Department.RowVersion` es el valor cuando se capturó la entidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="990a6-220">`OriginalValue` es el valor de la base de datos cuando se llamó a `FirstOrDefaultAsync` en este método.</span><span class="sxs-lookup"><span data-stu-id="990a6-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="990a6-221">El código siguiente obtiene los valores de cliente (es decir, los valores registrados en este método) y los valores de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="990a6-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="990a6-222">El código siguiente agrega un mensaje de error personalizado para cada columna que tiene valores de la base de datos diferentes de lo que se registró en `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="990a6-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="990a6-223">El código resaltado a continuación establece el valor `RowVersion` para el nuevo valor recuperado de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="990a6-224">La próxima vez que el usuario haga clic en **Save**, solo se detectarán los errores de simultaneidad que se produzcan desde la última visualización de la página Edit.</span><span class="sxs-lookup"><span data-stu-id="990a6-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="990a6-225">La instrucción `ModelState.Remove` es necesaria porque `ModelState` tiene el valor `RowVersion` antiguo.</span><span class="sxs-lookup"><span data-stu-id="990a6-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="990a6-226">En la página de Razor, el valor `ModelState` de un campo tiene prioridad sobre los valores de propiedad de modelo cuando ambos están presentes.</span><span class="sxs-lookup"><span data-stu-id="990a6-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="990a6-227">Actualizar la página Edit</span><span class="sxs-lookup"><span data-stu-id="990a6-227">Update the Edit page</span></span>

<span data-ttu-id="990a6-228">Actualice *Pages/Departments/Edit.cshtml* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="990a6-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="990a6-229">El marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="990a6-229">The preceding markup:</span></span>

* <span data-ttu-id="990a6-230">Actualiza la directiva `page` de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="990a6-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="990a6-231">Agrega una versión de fila oculta.</span><span class="sxs-lookup"><span data-stu-id="990a6-231">Adds a hidden row version.</span></span> <span data-ttu-id="990a6-232">Se debe agregar `RowVersion` para que la devolución enlace el valor.</span><span class="sxs-lookup"><span data-stu-id="990a6-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="990a6-233">Muestra el último byte de `RowVersion` para fines de depuración.</span><span class="sxs-lookup"><span data-stu-id="990a6-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="990a6-234">Reemplaza `ViewData` con `InstructorNameSL` fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="990a6-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="990a6-235">Comprobar los conflictos de simultaneidad con la página Edit</span><span class="sxs-lookup"><span data-stu-id="990a6-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="990a6-236">Abra dos instancias de exploradores de Edit en el departamento de inglés:</span><span class="sxs-lookup"><span data-stu-id="990a6-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="990a6-237">Ejecute la aplicación y seleccione Departments.</span><span class="sxs-lookup"><span data-stu-id="990a6-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="990a6-238">Haga clic con el botón derecho en el hipervínculo **Edit** del departamento de inglés y seleccione **Abrir en nueva pestaña**.</span><span class="sxs-lookup"><span data-stu-id="990a6-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="990a6-239">En la primera pestaña, haga clic en el hipervínculo **Edit** del departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="990a6-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="990a6-240">Las dos pestañas del explorador muestran la misma información.</span><span class="sxs-lookup"><span data-stu-id="990a6-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="990a6-241">Cambie el nombre en la primera pestaña del explorador y haga clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="990a6-241">Change the name in the first browser tab and click **Save**.</span></span>

![Página 1 de Department Edit después del cambio](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="990a6-243">El explorador muestra la página de índice con el valor modificado y el indicador rowVersion actualizado.</span><span class="sxs-lookup"><span data-stu-id="990a6-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="990a6-244">Tenga en cuenta el indicador rowVersion actualizado, que se muestra en el segundo postback en la otra pestaña.</span><span class="sxs-lookup"><span data-stu-id="990a6-244">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="990a6-245">Cambie otro campo en la segunda pestaña del explorador.</span><span class="sxs-lookup"><span data-stu-id="990a6-245">Change a different field in the second browser tab.</span></span>

![Página 2 de Department Edit después del cambio](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="990a6-247">Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="990a6-247">Click **Save**.</span></span> <span data-ttu-id="990a6-248">Verá mensajes de error para todos los campos que no coinciden con los valores de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="990a6-248">You see error messages for all fields that don't match the DB values:</span></span>

![Mensaje de error de la página Department Edit](concurrency/_static/edit-error.png)

<span data-ttu-id="990a6-250">Esta ventana del explorador no planeaba cambiar el campo Name.</span><span class="sxs-lookup"><span data-stu-id="990a6-250">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="990a6-251">Copie y pegue el valor actual (Languages) en el campo Name.</span><span class="sxs-lookup"><span data-stu-id="990a6-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="990a6-252">Presione TAB para salir del campo. La validación del lado cliente quita el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="990a6-252">Tab out. Client-side validation removes the error message.</span></span>

![Mensaje de error de la página Department Edit](concurrency/_static/cv.png)

<span data-ttu-id="990a6-254">Vuelva a hacer clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="990a6-254">Click **Save** again.</span></span> <span data-ttu-id="990a6-255">Se guarda el valor especificado en la segunda pestaña del explorador.</span><span class="sxs-lookup"><span data-stu-id="990a6-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="990a6-256">Verá los valores guardados en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="990a6-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="990a6-257">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="990a6-257">Update the Delete page</span></span>

<span data-ttu-id="990a6-258">Actualice el modelo de la página Delete con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="990a6-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="990a6-259">La página Delete detecta los conflictos de simultaneidad cuando la entidad ha cambiado después de que se capturase.</span><span class="sxs-lookup"><span data-stu-id="990a6-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="990a6-260">`Department.RowVersion` es la versión de fila cuando se capturó la entidad.</span><span class="sxs-lookup"><span data-stu-id="990a6-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="990a6-261">Cuando EF Core crea el comando DELETE de SQL, incluye una cláusula WHERE con `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="990a6-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="990a6-262">Si el comando DELETE de SQL tiene como resultado cero filas afectadas:</span><span class="sxs-lookup"><span data-stu-id="990a6-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="990a6-263">La `RowVersion` del comando DELETE de SQL no coincide con la `RowVersion` de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="990a6-264">Se produce una excepción DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="990a6-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="990a6-265">Se llama a `OnGetAsync` con el `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="990a6-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="990a6-266">Actualizar la página Delete</span><span class="sxs-lookup"><span data-stu-id="990a6-266">Update the Delete page</span></span>

<span data-ttu-id="990a6-267">Actualice *Pages/Departments/Delete.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="990a6-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="990a6-268">En el marcado anterior se realizan los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="990a6-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="990a6-269">Se actualiza la directiva `page` de `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="990a6-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="990a6-270">Se agrega un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="990a6-270">Adds an error message.</span></span>
* <span data-ttu-id="990a6-271">Se reemplaza FirstMidName por FullName en el campo **Administrator**.</span><span class="sxs-lookup"><span data-stu-id="990a6-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="990a6-272">Se cambia `RowVersion` para que muestre el último byte.</span><span class="sxs-lookup"><span data-stu-id="990a6-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="990a6-273">Se agrega una versión de fila oculta.</span><span class="sxs-lookup"><span data-stu-id="990a6-273">Adds a hidden row version.</span></span> <span data-ttu-id="990a6-274">Se debe agregar `RowVersion` para que la devolución enlace el valor.</span><span class="sxs-lookup"><span data-stu-id="990a6-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="990a6-275">Comprobar los conflictos de simultaneidad con la página Delete</span><span class="sxs-lookup"><span data-stu-id="990a6-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="990a6-276">Cree un departamento de prueba.</span><span class="sxs-lookup"><span data-stu-id="990a6-276">Create a test department.</span></span>

<span data-ttu-id="990a6-277">Abra dos instancias de exploradores de Delete en el departamento de prueba:</span><span class="sxs-lookup"><span data-stu-id="990a6-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="990a6-278">Ejecute la aplicación y seleccione Departments.</span><span class="sxs-lookup"><span data-stu-id="990a6-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="990a6-279">Haga clic con el botón derecho en el hipervínculo **Delete** del departamento de prueba y seleccione **Abrir en nueva pestaña**.</span><span class="sxs-lookup"><span data-stu-id="990a6-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="990a6-280">Haga clic en el hipervínculo **Edit** del departamento de prueba.</span><span class="sxs-lookup"><span data-stu-id="990a6-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="990a6-281">Las dos pestañas del explorador muestran la misma información.</span><span class="sxs-lookup"><span data-stu-id="990a6-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="990a6-282">Cambie el presupuesto en la primera pestaña del explorador y haga clic en **Save**.</span><span class="sxs-lookup"><span data-stu-id="990a6-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="990a6-283">El explorador muestra la página de índice con el valor modificado y el indicador rowVersion actualizado.</span><span class="sxs-lookup"><span data-stu-id="990a6-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="990a6-284">Tenga en cuenta el indicador rowVersion actualizado, que se muestra en el segundo postback en la otra pestaña.</span><span class="sxs-lookup"><span data-stu-id="990a6-284">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="990a6-285">Elimine el departamento de prueba de la segunda pestaña. Se mostrará un error de simultaneidad con los valores actuales de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="990a6-286">Al hacer clic en **Delete** se elimina la entidad, a menos que se haya actualizado `RowVersion`. El departamento se ha eliminado.</span><span class="sxs-lookup"><span data-stu-id="990a6-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="990a6-287">Vea [Herencia](xref:data/ef-mvc/inheritance) para obtener información sobre cómo se hereda un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="990a6-287">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="990a6-288">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="990a6-288">Additional resources</span></span>

* [<span data-ttu-id="990a6-289">Tokens de simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="990a6-289">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="990a6-290">Controlar la simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="990a6-290">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="990a6-291">Anterior</span><span class="sxs-lookup"><span data-stu-id="990a6-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
