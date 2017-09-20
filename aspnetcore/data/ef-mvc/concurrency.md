---
title: "Núcleo de ASP.NET MVC con EF Core - simultaneidad - 8 de 10"
author: tdykstra
description: "Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo."
keywords: Simultaneidad de ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: fc6b218034183a9153c1ef22c99d920a942d2d09
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>Controlar los conflictos de simultaneidad - Core EF con el tutorial de MVC de ASP.NET Core (8 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En los tutoriales anteriores, aprendió a actualizar los datos. Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.

Podrá crear páginas web que funcionan con la entidad Department y controlar los errores de simultaneidad. Las ilustraciones siguientes muestran las páginas de editar y eliminar, incluidos algunos mensajes que se muestran si se produce un conflicto de simultaneidad.

![Página de edición de departamento](concurrency/_static/edit-error.png)

![Página de borrado de departamento](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Cuando un usuario muestra los datos de una entidad con el fin de editarla y, a continuación, otro usuario actualiza los datos de la misma entidad antes de que el primer cambio del usuario se escribe en la base de datos, se produce un conflicto de simultaneidad. Si no habilita la detección de este tipo de conflictos, gana el jugador que actualiza la base de datos en último lugar sobrescribe los cambios del otro usuario. En muchas aplicaciones, el riesgo es aceptable: si hay determinados usuarios o algunas de las actualizaciones, o si no es realmente crítica si se sobrescriben algunos cambios, el costo de la programación para la simultaneidad puede sobrepasar el beneficio obtenido. En ese caso, no tendrá que configurar la aplicación para controlar conflictos de simultaneidad.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidad pesimista (bloqueo)

Si la aplicación necesita evitar la pérdida accidental de datos en escenarios de simultaneidad, una manera de hacerlo es utilizar bloqueos de base de datos. Esto se denomina simultaneidad pesimista. Por ejemplo, antes de leer una fila de una base de datos, se solicita un bloqueo de solo lectura o para acceso de actualización. Si bloquea una fila para el acceso de actualización, no se permite ningún otro usuario bloquea la fila de solo lectura o actualizar el acceso, porque se reciben una copia de datos que se están modificando. Si bloquea una fila para el acceso de solo lectura, otras personas también pueden bloquear, para acceso de solo lectura pero no para la actualización.

Administrar bloqueos tiene desventajas. Puede ser bastante complicado programa. Se necesitan recursos de administración de base de datos, y puede provocar problemas de rendimiento como el número de usuarios de una aplicación aumenta. Por estos motivos, no todos los sistemas de administración de bases de datos admiten la simultaneidad pesimista. Entity Framework Core no proporciona ninguna compatibilidad integrada para él, y este tutorial no muestra cómo se implementa.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

La alternativa a la simultaneidad pesimista es simultaneidad optimista. Simultaneidad optimista significa permitir conflictos de simultaneidad que se produzca y reaccionar correctamente si lo hacen. Por ejemplo, Julia visita la página Editar departamento y cambia la cantidad de presupuesto para el departamento de inglés de $350,000.00 a 0,00 USD.

![Cambiar la asignación a 0](concurrency/_static/change-budget.png)

Antes de que Juan hace clic en **guardar**, Juan visita la misma página y cambia el campo de fecha de inicio de 1/9/2007 a 9/1/2013.

![Cambiar la fecha de inicio a 2013](concurrency/_static/change-date.png)

Julia hace clic en **guardar** primera y ve su cambiar cuando el explorador vuelve a la página de índice.

![Presupuesto cambiado a cero](concurrency/_static/budget-zero.png)

A continuación, John hace clic en **guardar** en una página de edición que sigue mostrando un presupuesto de 350,000.00 $. ¿Qué sucede después viene determinado por la manera de controlar conflictos de simultaneidad.

Algunas de las opciones son las siguientes:

* Puede realizar un seguimiento de la propiedad que se ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos.

     En el escenario de ejemplo, no se perderían, ningún dato porque se actualizaron las diferentes propiedades por los dos usuarios. La próxima vez que un usuario examina el departamento de inglés, verá los cambios de Jane y John Doe--una fecha de inicio de 9/1/2013 y un presupuesto de cero dólares. Este método de actualización puede reducir el número de conflictos que pueden dar lugar a pérdida de datos, pero no se puede evitar la pérdida de datos si se realizan cambios competencia a la misma propiedad de una entidad. Si Entity Framework funciona de esta manera depende de cómo implementar el código de actualización. A menudo no resulta práctico en una aplicación web, porque puede requerir que mantener grandes cantidades de estado con el fin de realizar un seguimiento de todos los valores de propiedad original de una entidad, así como nuevos valores. Mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor o que deben incluirse en la propia página web (por ejemplo, en los campos ocultos) o en una cookie.

* Puede permitir que los cambios de John sobrescribir los cambios de Julia.

     La próxima vez que un usuario examina el departamento de inglés, verán 9/1/2013 y el valor de $350,000.00 restaurada. Esto se denomina una *cliente Wins* o *el último gana* escenario. (Todos los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos.) Como se mencionó en la introducción a esta sección, si no hace ninguna codificación para el control de simultaneidad, se realizará automáticamente.

* Cambio de John puede impedir que se actualiza en la base de datos.

     Por lo general, debería mostrar un mensaje de error, le muestra el estado actual de los datos y le permitan volver a aplicar los cambios si quiere que estén. Esto se denomina una *almacén Wins* escenario. (Los valores de almacén de datos tienen prioridad sobre los valores enviados por el cliente). Implementaremos el escenario de almacén de Wins en este tutorial. Este método garantiza que ningún cambio se sobrescribe sin que un usuario se le avisa a lo que está sucediendo.

### <a name="detecting-concurrency-conflicts"></a>Detectar conflictos de simultaneidad

Puede resolver conflictos de control de `DbConcurrencyException` las excepciones que inicia de Entity Framework. Para saber cuándo se producen estas excepciones, Entity Framework debe ser capaz de detectar conflictos. Por lo tanto, debe configurar la base de datos y el modelo de datos correctamente. Algunas opciones para habilitar la detección de conflictos son las siguientes:

* En la tabla de base de datos, incluya una columna de seguimiento que puede usarse para determinar si una fila ha cambiado. A continuación, puede configurar el Entity Framework para incluir esa columna en el lugar en la cláusula de actualización de SQL o los comandos Delete.

     Suele ser el tipo de datos de la columna de seguimiento `rowversion`. El `rowversion` valor es un número secuencial que se incrementa cada vez que se actualiza la fila. En un comando Update o Delete, Where cláusula incluye el valor original de la columna de seguimiento (la versión de fila original). Si ha cambiado por otro usuario, el valor de la fila que se está actualiza la `rowversion` columna es diferente del valor original, por lo que la instrucción Update o Delete no puede encontrar la fila que se va a actualizar a causa de Where cláusula. Cuando la encuentra Entity Framework que no se haya actualizado ninguna fila mediante la actualización o eliminación de comandos (es decir, cuando el número de filas afectadas es cero), interpreta como un conflicto de simultaneidad.

* Configurar Entity Framework para incluir los valores originales de cada columna en la tabla en el lugar en la cláusula de comandos Update y Delete.

     Como se muestra en la primera opción, si algo en la fila ha cambiado desde que se leyó primero la fila, Where cláusula no devolverá una fila que se va a actualizar, que Entity Framework se interpreta como un conflicto de simultaneidad. Para las tablas de base de datos que tienen muchas columnas, puede dar lugar a este enfoque en muy grande, donde las cláusulas y puede requerir que mantener grandes cantidades de estado. Tal y como se indicó anteriormente, el mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación. Por lo tanto, no por lo general se recomienda este enfoque, y no es el método utilizado en este tutorial.

     Si desea implementar este enfoque para simultaneidad, tendrá que marcar todas las propiedades de clave no principal de la entidad que desea realizar un seguimiento de simultaneidad para agregando el `ConcurrencyCheck` atribuir a ellos. Este cambio permite a Entity Framework incluir todas las columnas en la cláusula Where de SQL de las instrucciones Update y Delete.

En el resto de este tutorial agregará un `rowversion` propiedad de seguimiento para la entidad Department, crear un controlador y vistas y para comprobar que todo funciona correctamente.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Agregar una propiedad de seguimiento a la entidad Department

En *Models/Department.cs*, agregar una propiedad de seguimiento denominada RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

El `Timestamp` atributo especifica que esta columna se incluirá en Where cláusula de comandos Update y Delete se envía a la base de datos. El atributo se denomina `Timestamp` porque las versiones anteriores de SQL Server utilizan una instancia de SQL `timestamp` tipo de datos antes de la instrucción SQL `rowversion` sustituirla por otra. El tipo .NET de `rowversion` es una matriz de bytes.

Si prefiere usar la API fluida, puede usar el `IsConcurrencyToken` (método) (en *Data/SchoolContext.cs*) para especificar la propiedad de seguimiento, tal como se muestra en el ejemplo siguiente:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Mediante la adición de una propiedad ha cambiado el modelo de base de datos, por lo que necesita realizar otra migración.

Guardar los cambios, compile el proyecto y, a continuación, escriba los siguientes comandos en la ventana de comandos:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Crear un controlador de departamentos y vistas

Aplicar la técnica scaffolding un controlador de departamentos y vistas como lo hizo anteriormente para estudiantes, cursos e instructores.

![Departamento de scaffolding](concurrency/_static/add-departments-controller.png)

En el *DepartmentsController.cs* de archivo, cambie todas las apariciones de cuatro de "FirstMidName" a "FullName" para que las listas desplegables de administrador de departamento va a contener el nombre completo del instructor en lugar de simplemente el apellido.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Actualizar la vista de índice de departamentos

El motor de scaffolding creó una columna RowVersion en la vista de índice, pero ese campo no debe mostrarse.

Reemplace el código de *Views/Departments/Index.cshtml* con el código siguiente.

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Esto cambia el encabezado en "Departamentos", elimina la columna RowVersion y muestra el nombre completo en lugar del nombre para el administrador.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Actualizar los métodos de edición en el controlador de departamentos

En ambos el HttpGet `Edit` método y la `Details` método, agregue `AsNoTracking`. En el HttpGet `Edit` método, agregue carga diligente para el administrador.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Reemplace el código existente para el HttpPost `Edit` método por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

El código comienza cuando se intenta leer el departamento de actualizarse. Si el `SingleOrDefaultAsync` método devuelve null, el departamento se ha eliminado por otro usuario. En ese caso, el código usa los valores de formulario expuesto para crear una entidad de departamento, por lo que puede volver a mostrarse la página de edición con un mensaje de error. Como alternativa, no tendrá que volver a crear la entidad department si mostrar sólo un mensaje de error sin volver a mostrar los campos de departamento.

La vista almacena el original `RowVersion` valor en un campo oculto y este método recibe ese valor en el `rowVersion` parámetro. Antes de llamar a `SaveChanges`, tendrá que colocar original `RowVersion` valor de propiedad en el `OriginalValues` colección para la entidad.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Cuando Entity Framework crea un comando SQL UPDATE, ese comando incluirá una cláusula WHERE que comprueba si hay una fila que tiene la versión original `RowVersion` valor. Si ninguna fila se ven afectada por el comando de actualización (no hay filas tienen la versión original `RowVersion` valor), Entity Framework produce un `DbUpdateConcurrencyException` excepción.

El código del bloque catch para esa excepción Obtiene la entidad afectada de departamento que tiene los valores actualizados de los `Entries` propiedad del objeto de excepción.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

El `Entries` colección contará con una sola `EntityEntry` objeto.  Puede utilizar dicho objeto para obtener los nuevos valores especificados por el usuario y los valores de la base de datos actual.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

El código agrega un mensaje de error personalizado para cada columna que tiene valores de base de datos diferentes de lo que el usuario especificado en el menú Edición página (sólo un campo se muestra aquí por razones de brevedad).

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Por último, el código establece la `RowVersion` valor de la `departmentToUpdate` para el nuevo valor recuperadas de la base de datos. Esta nueva `RowVersion` valor se almacenará en el campo oculto cuando la edición se volverá a abrir la página y la próxima vez que el usuario hace clic en **guardar**, solo los errores de simultaneidad que se producen desde que se detectarán el volver a mostrar de la página de edición.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

El `ModelState.Remove` instrucción es necesaria porque `ModelState` tiene la antigua `RowVersion` valor. En la vista, la `ModelState` el valor de un campo tiene prioridad sobre los valores de propiedad de modelo cuando ambos están presentes.

## <a name="update-the-department-edit-view"></a>Actualizar la vista de edición de departamento

En *Views/Departments/Edit.cshtml*, realice los cambios siguientes:

* Agregar un campo oculto para guardar la `RowVersion` valor de propiedad, inmediatamente después de un campo oculto para el `DepartmentID` propiedad.

* Agregar una opción "Seleccionar Administrador" a la lista desplegable.

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Conflictos de simultaneidad de prueba en la página Editar

Ejecute la aplicación y vaya a la página de índice de departamentos. Haga clic en el **editar** hipervínculo para el departamento de inglés y seleccione **abrir en nueva ficha**, a continuación, haga clic en el **editar** hipervínculo para el departamento de inglés. Las pestañas del dos explorador ahora muestran la misma información.

Cambiar un campo en la primera pestaña de explorador y haga clic en **guardar**.

![Editar página 1 después de cambio de departamento](concurrency/_static/edit-after-change-1.png)

El explorador muestra la página de índice con el valor modificado.

Cambiar un campo en la segunda pestaña de explorador.

![Editar página 2 después de cambio de departamento](concurrency/_static/edit-after-change-2.png)

Haga clic en **Guardar**. Verá un mensaje de error:

![Mensaje de error de página de edición de departamento](concurrency/_static/edit-error.png)

Haga clic en **guardar** nuevo. Se guarda el valor especificado en la segunda pestaña de explorador. Vea los valores guardados cuando aparezca la página de índice.

## <a name="update-the-delete-page"></a>Actualizar la página de borrado

Para la página de borrado, Entity Framework detecta los conflictos de simultaneidad causados por una persona editar persona del departamento de forma similar. Cuando el HttpGet `Delete` método muestra la vista de confirmación, la vista incluye la versión original `RowVersion` valor en un campo oculto. Que el valor, a continuación, está disponible para el HttpPost `Delete` método al que se llama cuando el usuario confirma la eliminación. Cuando Entity Framework crea el comando SQL DELETE, incluye una cláusula WHERE con la versión original `RowVersion` valor. Si los resultados del comando en cero filas afectadas (es decir, la fila se cambió después de que se muestre la página de confirmación de eliminación), se produce una excepción de simultaneidad y el HttpGet `Delete` método se llama con una error marca establecida en true para volver a mostrar la página de confirmación con un mensaje de error. También es posible que se vieron afectados cero filas porque la fila se eliminó por otro usuario, por lo que en ese caso no se muestra ningún mensaje de error.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Actualizar los métodos de eliminación en el controlador de departamentos

En *DepartmentsController.cs*, reemplace el HttpGet `Delete` método por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

El método acepta un parámetro opcional que indica si la página se volverá a aparecer después de un error de simultaneidad. Si esta marca es true y el departamento especificado ya no existe, se eliminó por otro usuario. En ese caso, el código se redirige a la página de índice.  Si existe el departamento y esta marca es true, se cambió por otro usuario. En ese caso, el código envía un mensaje de error a la vista mediante `ViewData`.  

Reemplace el código en el HttpPost `Delete` método (denominado `DeleteConfirmed`) con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

En el código con scaffolding que acaba de reemplazar, este método acepta un identificador de registro:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Ha cambiado este parámetro en una instancia de entidad de departamento creada por el enlazador de modelos. Esto proporciona acceso EF con el valor de la propiedad RowVersion además de la clave de registro.

```csharp
public async Task<IActionResult> Delete(Department department)
```

También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding usa el nombre `DeleteConfirmed` para proporcionar el método HttpPost una firma única. (El CLR requiere métodos sobrecargados para que tenga parámetros de método diferente). Ahora que las firmas son únicas, puede pegar con la convención MVC y usar el mismo nombre para los métodos de eliminación HttpPost y HttpGet.

Si ya se ha eliminado el departamento, el `AnyAsync` método devuelve false y la aplicación simplemente vuelve al método de índice.

Si se detecta un error de simultaneidad, el código vuelve a mostrar la página de confirmación de eliminación y proporciona una marca que indica que debe mostrar un mensaje de error de simultaneidad.

### <a name="update-the-delete-view"></a>Actualizar la vista de eliminación

En *Views/Departments/Delete.cshtml*, reemplace el código con scaffolding con el siguiente código que agrega un campo de mensaje de error y los campos ocultos de las propiedades DepartmentID y RowVersion. Los cambios aparecen resaltados.

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Esto hace que los cambios siguientes:

* Agrega un mensaje de error entre la `h2` y `h3` encabezados.

* Reemplaza FirstMidName por FullName en la **administrador** campo.

* Quita el campo RowVersion.

* Agrega un campo oculto para el `RowVersion` propiedad.

Ejecute la aplicación y vaya a la página de índice de departamentos. Haga clic en el **eliminar** hipervínculo para el departamento de inglés y seleccione **abrir en nueva ficha**, a continuación, en la primera ficha, haga clic en el **editar** hipervínculo para el departamento de inglés.

En la primera ventana, cambie uno de los valores y haga clic en **guardar**:

![Página de edición de departamento después de cambio antes de eliminar](concurrency/_static/edit-after-change-for-delete.png)

En la segunda ficha, haga clic en **eliminar**. Verá el mensaje de error de simultaneidad y se actualizan los valores de departamento con lo que está actualmente en la base de datos.

![Página de confirmación de eliminación de departamento con error de simultaneidad](concurrency/_static/delete-error.png)

Si hace clic en **eliminar** nuevo, se le redirigirá a la página de índice, que muestra que se ha eliminado el departamento.

## <a name="update-details-and-create-views"></a>Detalles de la actualización y crear vistas

Si lo desea puede limpiar con scaffolding código en los detalles y crear vistas.

Reemplace el código de *Views/Departments/Details.cshtml* para eliminar la columna RowVersion y muestra el nombre completo del administrador.

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Reemplace el código de *Views/Departments/Create.cshtml* para agregar una opción que seleccione en la lista desplegable.

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Resumen

Con esto finaliza la introducción para controlar los conflictos de simultaneidad. Para obtener más información acerca de cómo administrar la simultaneidad en EF Core, vea [conflictos de simultaneidad](https://docs.microsoft.com/ef/core/saving/concurrency). El siguiente tutorial muestra cómo implementar la herencia de tabla por jerarquía para las entidades Instructor y Student.

>[!div class="step-by-step"]
[Anterior](update-related-data.md)
[Siguiente](inheritance.md)  
