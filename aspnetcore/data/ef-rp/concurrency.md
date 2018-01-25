---
title: "Páginas de Razor con EF Core - simultaneidad - 8 de 8"
author: rick-anderson
description: "Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: b36fb71cba058a3409b30a1d9469159fcd027375
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
en-us /

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Controlar los conflictos de simultaneidad - Core EF con páginas de Razor (8 de 8)

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), y [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan una entidad simultáneamente (al mismo tiempo). Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Se produce un conflicto de simultaneidad cuando:

* Un usuario navega a la página de edición para una entidad.
* Otro usuario actualiza la misma entidad antes de que el primer cambio del usuario se escribe en la base de datos.

Si no está habilitada la detección de simultaneidad, cuando se produzcan actualizaciones simultáneas:

* La última actualización ganadora. Es decir, los últimos valores de actualización se guardan en la base de datos.
* La primera de las actualizaciones actuales se pierden.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

Simultaneidad optimista permite que los conflictos de simultaneidad que se produzca y, a continuación, reacciona correctamente cuando lo hacen. Por ejemplo, Julia visita la página de edición de departamento y cambia el presupuesto para el departamento de inglés de $350,000.00 a 0,00 USD.

![Cambiar la asignación a 0](concurrency/_static/change-budget.png)

Antes de que Juan hace clic en **guardar**, Juan visita la misma página y cambia el campo de fecha de inicio de 1/9/2007 a 9/1/2013.

![Cambiar la fecha de inicio a 2013](concurrency/_static/change-date.png)

Julia hace clic en **guardar** primera y ve su cambiar cuando el explorador muestra la página de índice.

![Presupuesto cambiado a cero](concurrency/_static/budget-zero.png)

John hace clic en **guardar** en una página de edición que sigue mostrando un presupuesto de 350,000.00 $. ¿Qué sucede después viene determinado por la manera de controlar conflictos de simultaneidad.

Simultaneidad optimista incluye las siguientes opciones:

* Puede realizar un seguimiento de la propiedad que se ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos.

 En el escenario, se perderá ningún dato. Se actualizaron las diferentes propiedades por los dos usuarios. La próxima vez que un usuario examina el departamento de inglés, verá los cambios de Jane y John Doe. Este método de actualización puede reducir el número de conflictos que pueden dar lugar a pérdida de datos. Este enfoque: * no se puede evitar la pérdida de datos si se realizan cambios competencia a la misma propiedad.
        * Es por lo general no es práctico en una aplicación web. Requiere el mantenimiento de un estado relevante para realizar un seguimiento de capturadas todos los valores y los nuevos valores. Mantener grandes cantidades de estado puede afectar al rendimiento de la aplicación.
        * Puede aumentar la complejidad de las aplicaciones en comparación con la detección de simultaneidad en una entidad.

* Puede permitir que los cambios de John sobrescribir los cambios de Julia.

 La próxima vez que un usuario examina el departamento de inglés, verá 9/1/2013 y el valor de $350,000.00 capturadas. Este enfoque se denomina un *cliente Wins* o *el último gana* escenario. (Todos los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos.) Si no hace ninguna codificación para el control de simultaneidad, gana el cliente realiza automáticamente.

* Cambio de John puede impedir que se actualiza en la base de datos. Normalmente, la aplicación podría: * mostrar un mensaje de error.
        * Mostrar el estado actual de los datos.
        * Permite al usuario volver a aplicar los cambios.

 Esto se denomina una *almacén Wins* escenario. (Los valores de almacén de datos tienen prioridad sobre los valores enviados por el cliente). Implementar el escenario de almacén de Wins en este tutorial. Este método garantiza que ningún cambio se sobrescribe sin que el usuario que se va a una alerta.

## <a name="handling-concurrency"></a>Control de simultaneidad 

Cuando una propiedad se configura como un [token de simultaneidad](https://docs.microsoft.com/ef/core/modeling/concurrency):

* Núcleo EF comprueba que no se han modificado propiedad después de se capturó. La comprobación se produce cuando [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) se llama.
* Si la propiedad ha cambiado desde que se capturó, un [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) se produce. 

El modelo de base de datos y los datos debe configurarse para admitir producir `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Detección de conflictos de simultaneidad en una propiedad

Se pueden detectar conflictos de simultaneidad en el nivel de propiedad con el [sea ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atributo. El atributo se puede aplicar a varias propiedades en el modelo. Para obtener más información, consulte [anotaciones de datos-sea ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).

El `[ConcurrencyCheck]` atributo no se usa en este tutorial.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Detección de conflictos de simultaneidad en una fila

Para detectar conflictos de simultaneidad, un [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) seguimiento de columna se agrega al modelo.  `rowversion` :

* ¿Es SQL Server específico. Otras bases de datos no pueden proporcionar una característica similar.
* Se usa para determinar que una entidad no se ha cambiado desde que se capturó desde la base de datos. 

La base de datos genera un secuencial `rowversion` número que se incrementa cada vez que la fila se actualiza. En un `Update` o `Delete` comando, el `Where` cláusula incluye el valor obtenido de `rowversion`. Si está actualizando la fila ha cambiado:

 * `rowversion`no coincide con el valor obtenido.
 * El `Update` o `Delete` comandos no encuentran una fila porque la `Where` cláusula incluye el capturadas `rowversion`.
 * Un `DbUpdateConcurrencyException` se produce.

En el núcleo de EF, cuando no se haya actualizado ninguna fila por un `Update` o `Delete` de comandos, se produce una excepción de simultaneidad.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Agregar una propiedad de seguimiento a la entidad Department

En *Models/Department.cs*, agregar una propiedad de seguimiento denominada RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

El [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atributo especifica que esta columna se incluye en el `Where` cláusula de `Update` y `Delete` comandos. El atributo se denomina `Timestamp` porque las versiones anteriores de SQL Server utilizan una instancia de SQL `timestamp` tipo de datos antes de la instrucción SQL `rowversion` sustituirla por tipo.

La API fluida también puede especificar la propiedad de seguimiento:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

El código siguiente muestra una parte de la instrucción T-SQL generado por núcleo de EF cuando se actualiza el nombre del departamento:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Anterior resaltada de código muestra la `WHERE` que contiene la cláusula `RowVersion`. Si la base de datos `RowVersion` no es igual a la `RowVersion` parámetro (`@p2`), se ha actualizado ninguna fila.

El código resaltado siguiente muestra el código T-SQL que comprueba que se actualizó exactamente una fila:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) devuelve el número de filas afectadas por la última instrucción. En no se actualizan las filas, Core EF produce un `DbUpdateConcurrencyException`.

Puede ver que el núcleo de EF de T-SQL se genera en la ventana de salida de Visual Studio.

### <a name="update-the-db"></a>Actualizar la base de datos

Agregar el `RowVersion` propiedad cambia el modelo de base de datos, lo que requiere una migración.

Compile el proyecto. Escriba lo siguiente en una ventana de comandos:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Los comandos anteriores:

* Agrega el *migraciones / {tiempo stamp}_RowVersion.cs* archivo de migración.
* Las actualizaciones de la *Migrations/SchoolContextModelSnapshot.cs* archivo. La actualización agrega el código que aparece resaltado a la `BuildModel` método:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Ejecuta las migraciones para actualizar la base de datos.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Aplicar la técnica scaffolding el modelo de departamentos

* Salga de Visual Studio.
* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Ejecute el siguiente comando:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

El scaffolds del comando anterior la `Department` modelo. Abra el proyecto en Visual Studio.

Compile el proyecto. La compilación genera errores similar al siguiente:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Cambiar globalmente `_context.Department` a `_context.Departments` (es decir, agregue una "s" para `Department`). 7 apariciones se encuentra y se actualizan.

### <a name="update-the-departments-index-page"></a>Actualizar la página de índice de departamentos

El motor de scaffolding que crea un `RowVersion` columna para la página de índice, pero ese campo no debe mostrarse. En este tutorial, el último byte de la `RowVersion` se muestra como ayuda para entender la simultaneidad. El último byte no garantiza que es único. No mostrar una aplicación real `RowVersion` o el último byte de `RowVersion`.

Actualice la página de índice:

* Reemplace el índice por departamentos.
* Reemplace el marcado que contiene `RowVersion` con el último byte de `RowVersion`.
* Reemplace FirstMidName con FullName.

El marcado siguiente muestra la página actualizada:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Actualizar el modelo de la página de edición

Actualización *pages\departments\edit.cshtml.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Para detectar un problema de simultaneidad, el [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) se actualiza con la `rowVersion` valor de la entidad se capturó. Núcleo EF genera un comando SQL UPDATE con una cláusula WHERE que contiene la versión original `RowVersion` valor. Si no hay ninguna fila se ven afectada por el comando de actualización (no hay filas tienen la versión original `RowVersion` valor), un `DbUpdateConcurrencyException` se produce la excepción.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

En el código anterior, `Department.RowVersion` es el valor al que se capturó la entidad. `OriginalValue`es el valor de la base de datos cuando `FirstOrDefaultAsync` se llamó a este método.

El código siguiente obtiene los valores de cliente (es decir, los valores registrados en este método) y los valores de la base de datos:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

El código siguiente agrega un mensaje de error personalizado para valores diferentes de lo que se ha registrado para cada columna que tiene la base de datos `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Resaltan de los siguientes conjuntos de código la `RowVersion` valor para el nuevo valor se recupera de la base de datos. La próxima vez que el usuario hace clic en **guardar**, solo los errores de simultaneidad que se producen desde la última visualización de la página de edición se detectarán.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

El `ModelState.Remove` instrucción es necesaria porque `ModelState` tiene la antigua `RowVersion` valor. En la página de Razor, el `ModelState` el valor de un campo tiene prioridad sobre los valores de propiedad de modelo cuando ambos están presentes.

## <a name="update-the-edit-page"></a>Actualizar la página de edición

Actualización *Pages/Departments/Edit.cshtml* con el siguiente marcado:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

El marcado anterior:

* Las actualizaciones de la `page` la directiva de `@page` a `@page "{id:int}"`.
* Agrega una versión de fila está oculta. `RowVersion`debe agregarse para que devolución enlaza el valor.
* Muestra el último byte de `RowVersion` para fines de depuración.
* Reemplaza `ViewData` con fuertemente tipadas `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Conflictos de simultaneidad de prueba con la página de edición

Abra dos instancias de exploradores de edición en el departamento de inglés:

* Ejecute la aplicación y seleccione departamentos.
* Haga clic en el **editar** hipervínculo para el departamento de inglés y seleccione **abrir en nueva ficha**.
* En la primera ficha, haga clic en el **editar** hipervínculo para el departamento de inglés.

Las fichas de dos explorador muestran la misma información.

Cambiar el nombre de la primera pestaña de explorador y haga clic en **guardar**.

![Editar página 1 después de cambio de departamento](concurrency/_static/edit-after-change-1.png)

El explorador muestra la página de índice con el valor cambiado y el indicador de rowVersion actualizada. Tenga en cuenta el indicador rowVersion actualizada, se muestra en la segunda devolución de datos en la ficha otros.

Cambiar un campo diferente en la segunda pestaña de explorador.

![Editar página 2 después de cambio de departamento](concurrency/_static/edit-after-change-2.png)

Haga clic en **Guardar**. Vea mensajes de error para todos los campos que no coinciden con los valores de la base de datos:

![Mensaje de error de página de edición de departamento](concurrency/_static/edit-error.png)

Esta ventana del explorador no va a cambiar el campo de nombre. Copie y pegue el valor actual (idiomas) en el campo nombre. Pestaña salida. Validación del lado cliente quita el mensaje de error.

![Mensaje de error de página de edición de departamento](concurrency/_static/cv.png)

Haga clic en **guardar** nuevo. Se guarda el valor especificado en la segunda pestaña de explorador. Vea los valores guardados en la página de índice.

## <a name="update-the-delete-page"></a>Actualizar la página de borrado

Actualice el modelo de páginas de eliminación con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

La página de borrado detecta los conflictos de simultaneidad cuando la entidad ha cambiado después de se capturó. `Department.RowVersion`es la versión de fila cuando se capturó la entidad. Cuando el núcleo de EF crea el comando SQL DELETE, incluye una cláusula WHERE con `RowVersion`. Si los resultados del comando SQL DELETE en cero filas afectadas:

* El `RowVersion` en DELETE de SQL no coincide con el comando `RowVersion` en la base de datos.
* Se produce una excepción DbUpdateConcurrencyException.
* `OnGetAsync`se llama con la `concurrencyError`.

### <a name="update-the-delete-page"></a>Actualizar la página de borrado

Actualización *Pages/Departments/Delete.cshtml* con el código siguiente:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


El marcado anterior realiza los cambios siguientes:

* Las actualizaciones de la `page` la directiva de `@page` a `@page "{id:int}"`.
* Agrega un mensaje de error.
* Reemplaza FirstMidName por FullName en la **administrador** campo.
* Cambios `RowVersion` para mostrar el último byte.
* Agrega una versión de fila está oculta. `RowVersion`debe agregarse para que devolución enlaza el valor.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Conflictos de simultaneidad de prueba con la página de borrado

Cree un departamento de prueba.

Abra dos instancias de exploradores de eliminación en el departamento de prueba:

* Ejecute la aplicación y seleccione departamentos.
* Haga clic en el **eliminar** hipervínculo para el departamento de prueba y seleccione **abrir en nueva ficha**.
* Haga clic en el **editar** hipervínculo para el departamento de prueba.

Las fichas de dos explorador muestran la misma información.

Cambiar la asignación en la primera pestaña de explorador y haga clic en **guardar**.

El explorador muestra la página de índice con el valor cambiado y el indicador de rowVersion actualizada. Tenga en cuenta el indicador rowVersion actualizada, se muestra en la segunda devolución de datos en la ficha otros.

Elimine el departamento de prueba de la segunda pestaña. Un error de simultaneidad es que se muestran con los valores actuales de la base de datos. Haga clic en **eliminar** elimina la entidad, a menos que `RowVersion` ha sido updated.department se ha eliminado.

Vea [herencia](xref:data/ef-mvc/inheritance) sobre cómo se hereda de un modelo de datos.

### <a name="additional-resources"></a>Recursos adicionales

* [Tokens de simultaneidad en EF Core](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [Control de simultaneidad en EF Core](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/update-related-data)
