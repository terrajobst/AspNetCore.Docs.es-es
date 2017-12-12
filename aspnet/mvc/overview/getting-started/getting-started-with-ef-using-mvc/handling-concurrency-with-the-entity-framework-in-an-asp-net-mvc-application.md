---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Control de simultaneidad con Entity Framework 6 en una aplicación de MVC de ASP.NET 5 (10 de 12) | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e7b79503a1d297d40c6824f8b2b7bbc1f42b9fca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Control de simultaneidad con Entity Framework 6 en una aplicación de MVC de ASP.NET 5 (10 de 12)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En los tutoriales anteriores aprendió a actualizar los datos. Este tutorial muestra cómo tratar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.

Cambie las páginas web que funcionan con el `Department` entidad para que controlan los errores de simultaneidad. Las ilustraciones siguientes muestran las páginas de índice y Delete, incluidos algunos mensajes que se muestran si se produce un conflicto de simultaneidad.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Cuando un usuario muestra los datos de una entidad con el fin de editarla y, a continuación, otro usuario actualiza los datos de la misma entidad antes de que el primer cambio del usuario se escribe en la base de datos, se produce un conflicto de simultaneidad. Si no habilita la detección de este tipo de conflictos, gana el jugador que actualiza la base de datos en último lugar sobrescribe los cambios del otro usuario. En muchas aplicaciones, el riesgo es aceptable: si hay determinados usuarios o algunas de las actualizaciones, o si no es realmente crítica si se sobrescriben algunos cambios, el costo de la programación para la simultaneidad puede sobrepasar el beneficio obtenido. En ese caso, no tendrá que configurar la aplicación para controlar conflictos de simultaneidad.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidad pesimista (bloqueo)

Si la aplicación necesita evitar la pérdida accidental de datos en escenarios de simultaneidad, una manera de hacerlo es utilizar bloqueos de base de datos. Esto se denomina *simultaneidad pesimista*. Por ejemplo, antes de leer una fila de una base de datos, se solicita un bloqueo de solo lectura o para acceso de actualización. Si bloquea una fila para el acceso de actualización, no se permite ningún otro usuario bloquea la fila de solo lectura o actualizar el acceso, porque se reciben una copia de datos que se están modificando. Si bloquea una fila para el acceso de solo lectura, otras personas también pueden bloquear, para acceso de solo lectura pero no para la actualización.

Administrar bloqueos tiene desventajas. Puede ser bastante complicado programa. Se necesitan recursos de administración de base de datos, y puede provocar problemas de rendimiento como el número de usuarios de una aplicación aumenta. Por estos motivos, no todos los sistemas de administración de bases de datos admiten la simultaneidad pesimista. Entity Framework no proporciona ninguna compatibilidad integrada para él, y este tutorial no muestra cómo se implementa.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

La alternativa a la simultaneidad pesimista es *simultaneidad optimista*. Simultaneidad optimista significa permitir conflictos de simultaneidad que se produzca y reaccionar correctamente si lo hacen. Por ejemplo, Juan ejecuta la página Editar departamentos, cambios de la **presupuesto** cantidad para el departamento de inglés de $350,000.00 $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Antes de que Juan hace clic en **guardar**, Julia ejecuta la misma página y cambios del **Start Date** arrastrándolo desde el 1/9/2007 a 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John hace clic en **guardar** primero y ve su cambio cuando el explorador vuelve a la página de índice, Julia, a continuación, haga clic en **guardar**. ¿Qué sucede después viene determinado por la manera de controlar conflictos de simultaneidad. Algunas de las opciones son las siguientes:

- Puede realizar un seguimiento de la propiedad que se ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos. En el escenario de ejemplo, no se perderían, ningún dato porque se actualizaron las diferentes propiedades por los dos usuarios. La próxima vez que un usuario examina el departamento de inglés, verá los cambios de John y de Julia, una fecha de inicio de 8/8/2013 y un presupuesto de cero dólares.

    Este método de actualización puede reducir el número de conflictos que pueden dar lugar a pérdida de datos, pero no se puede evitar la pérdida de datos si se realizan cambios competencia a la misma propiedad de una entidad. Si Entity Framework funciona de esta manera depende de cómo implementar el código de actualización. A menudo no resulta práctico en una aplicación web, porque puede requerir que mantener grandes cantidades de estado con el fin de realizar un seguimiento de todos los valores de propiedad original de una entidad, así como nuevos valores. Mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor o que deben incluirse en la propia página web (por ejemplo, en los campos ocultos) o en una cookie.
- Puede permitir que los cambios de Julia sobrescribir los cambios de John. La próxima vez que un usuario examina el departamento de inglés, verán 8/8/2013 y el valor de $350,000.00 restaurada. Esto se denomina una *cliente Wins* o *el último gana* escenario. (Todos los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos.) Como se mencionó en la introducción a esta sección, si no hace ninguna codificación para el control de simultaneidad, se realizará automáticamente.
- Cambio de Julia puede impedir que se actualiza en la base de datos. Por lo general, debería mostrar un mensaje de error, mostrarle el estado actual de los datos y le permita volver a aplicar sus cambios si todavía desea hacerlos. Esto se denomina una *almacén Wins* escenario. (Los valores de almacén de datos tienen prioridad sobre los valores enviados por el cliente). Implementaremos el escenario de almacén de Wins en este tutorial. Este método garantiza que ningún cambio se sobrescribe sin que un usuario se le avisa a lo que está sucediendo.

### <a name="detecting-concurrency-conflicts"></a>Detectar conflictos de simultaneidad

Puede resolver conflictos de control de [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) las excepciones que inicia de Entity Framework. Para saber cuándo se producen estas excepciones, Entity Framework debe ser capaz de detectar conflictos. Por lo tanto, debe configurar la base de datos y el modelo de datos correctamente. Algunas opciones para habilitar la detección de conflictos son las siguientes:

- En la tabla de base de datos, incluya una columna de seguimiento que puede usarse para determinar si una fila ha cambiado. A continuación, puede configurar el Entity Framework para que incluya esa columna en la `Where` cláusula de SQL `Update` o `Delete` comandos.

    Suele ser el tipo de datos de la columna de seguimiento [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). El [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) valor es un número secuencial que se incrementa cada vez que se actualiza la fila. En un `Update` o `Delete` comando, el `Where` cláusula incluye el valor original de la columna de seguimiento (la versión de fila original). Si ha cambiado por otro usuario, el valor de la fila que se está actualiza el `rowversion` columna es diferente del valor original, por lo que la `Update` o `Delete` instrucción no puede encontrar la fila que se va a actualizar porque la `Where` cláusula. Cuando Entity Framework detecta que se ha actualizado ninguna fila por la `Update` o `Delete` comando (es decir, cuando el número de filas afectadas es cero), que interpreta como un conflicto de simultaneidad.
- Configurar Entity Framework para incluir los valores originales de cada columna en la tabla en la `Where` cláusula de `Update` y `Delete` comandos.

    Como se muestra en la primera opción, si algo en la fila ha cambiado desde que se leyó primero la fila, el `Where` cláusula no devolverá una fila que se va a actualizar, que Entity Framework se interpreta como un conflicto de simultaneidad. Para las tablas de base de datos que tienen muchas columnas, este enfoque puede producir un gran `Where` cláusulas y puede requerir que mantener grandes cantidades de estado. Tal y como se indicó anteriormente, el mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación. Por lo tanto, no por lo general se recomienda este enfoque, y no es el método utilizado en este tutorial.

    Si desea implementar este enfoque para simultaneidad, tendrá que marcar todas las propiedades de clave no principal de la entidad que desea realizar un seguimiento de simultaneidad para agregando el [sea ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) atribuir a ellos. Que cambio permite a Entity Framework incluir todas las columnas en la instrucción SQL `WHERE` cláusula de `UPDATE` instrucciones.

En el resto de este tutorial agregará un [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) tracking (propiedad) para el `Department` entidad, crear un controlador y vistas y para comprobar que todo funciona correctamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Agregar una propiedad de simultaneidad optimista para la entidad Department

En *Models\Department.cs*, agregar una propiedad de seguimiento denominada `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

El [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) atributo especifica que esta columna se incluirán en el `Where` cláusula de `Update` y `Delete` comandos enviados a la base de datos. El atributo se denomina [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) porque las versiones anteriores de SQL Server utilizan una instancia de SQL [timestamp](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) tipo de datos antes de la instrucción SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) sustituirla por otra. El tipo .net de [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) es una matriz de bytes.

Si prefiere usar la API fluida, puede usar el [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) método para especificar la propiedad de seguimiento, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Mediante la adición de una propiedad ha cambiado el modelo de base de datos, por lo que necesita realizar otra migración. En la consola de administrador de paquete (PMC), escriba los siguientes comandos:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Modifique el controlador de departamento

En *Controllers\DepartmentController.cs*, agregue un `using` instrucción:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

En el *DepartmentController.cs* de archivo, cambie todas las apariciones de cuatro de "Apellidos" a "FullName" para que las listas desplegables de administrador de departamento va a contener el nombre completo del instructor en lugar de simplemente el apellido.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Reemplace el código existente para la `HttpPost` `Edit` método por el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Si el `FindAsync` método devuelve null, el departamento se ha eliminado por otro usuario. El código muestra usa los valores de formulario expuesto para crear una entidad de departamento, por lo que puede volver a mostrarse la página de edición con un mensaje de error. Como alternativa, no tendrá que volver a crear la entidad department si mostrar sólo un mensaje de error sin volver a mostrar los campos de departamento.

La vista almacena el original `RowVersion` valor en un campo oculto y el método lo recibe en el `rowVersion` parámetro. Antes de llamar a `SaveChanges`, tendrá que colocar original `RowVersion` valor de propiedad en el `OriginalValues` colección para la entidad. A continuación, cuando Entity Framework crea una instancia de SQL `UPDATE` de comandos, que el comando incluirá un `WHERE` cláusula que comprueba si hay una fila que tiene la versión original `RowVersion` valor.

Si no hay ninguna fila se ven afectada por la `UPDATE` comando (ninguna fila tiene la versión original `RowVersion` valor), Entity Framework produce una `DbUpdateConcurrencyException` excepción y el código en el `catch` bloque obtiene los afectados `Department` entidad de la excepción objeto.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Este objeto tiene los nuevos valores especificados por el usuario en su `Entity` propiedad y se pueden obtener los valores leídos de la base de datos mediante una llamada a la `GetDatabaseValues` método.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

El `GetDatabaseValues` método devuelve null si alguien ha eliminado la fila de la base de datos; en caso contrario, tendrá que convertir el objeto devuelto a la `Department` clase para tener acceso a la `Department` propiedades. (Dado que ya se activó para su eliminación, `databaseEntry` sería null solo si se eliminó el departamento tras `FindAsync` se ejecuta y antes de `SaveChanges` se ejecuta.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

A continuación, el código agrega un mensaje de error personalizado para cada columna que tiene valores de base de datos diferentes de lo que el usuario ha escrito en la página Editar:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Un mensaje de error más largo explica lo ocurrido y qué puede hacer al respecto:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Por último, el código establece la `RowVersion` valor de la `Department` objeto en el nuevo valor se recupera de la base de datos. Esta nueva `RowVersion` valor se almacenará en el campo oculto cuando la edición se volverá a abrir la página y la próxima vez que el usuario hace clic en **guardar**, solo los errores de simultaneidad que se producen desde que se detectarán el volver a mostrar de la página de edición.

En *Views\Department\Edit.cshtml*, agregue un campo oculto para guardar la `RowVersion` valor de propiedad, inmediatamente después de un campo oculto para el `DepartmentID` propiedad:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Probar el control de simultaneidad optimista

Ejecute el sitio Web y haga clic en **departamentos**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Haga clic con el **editar** hipervínculo para el departamento de inglés y seleccione **abrir en nueva ficha** , a continuación, haga clic en el **editar** hipervínculo para el departamento de inglés. Las dos pestañas muestran la misma información.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Cambiar un campo en la primera pestaña de explorador y haga clic en **guardar**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

El explorador muestra la página de índice con el valor modificado.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Cambiar un campo en la segunda pestaña de explorador y haga clic en **guardar**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Haga clic en **guardar** en la segunda pestaña de explorador. Verá un mensaje de error:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Haga clic en **guardar** nuevo. El valor especificado en la segunda pestaña de explorador se guarda junto con el valor original de los datos modificados en el Explorador de la primera. Vea los valores guardados cuando aparezca la página de índice.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Actualizar la página de borrado

Para la página de borrado, Entity Framework detecta los conflictos de simultaneidad causados por una persona editar persona del departamento de forma similar. Cuando el `HttpGet` `Delete` método muestra la vista de confirmación, la vista incluye la versión original `RowVersion` valor en un campo oculto. Que el valor, a continuación, está disponible para el `HttpPost` `Delete` método al que se llama cuando el usuario confirma la eliminación. Cuando Entity Framework crea la instrucción SQL `DELETE` de comandos, incluye un `WHERE` cláusula con el original `RowVersion` valor. Si los resultados del comando en cero filas afectadas (es decir, la fila se cambió después de que se muestre la página de confirmación de eliminación), se produce una excepción de simultaneidad y el `HttpGet Delete` método se llama con un indicador de error establecido en `true` con el fin de volver a mostrar la página de confirmación con un mensaje de error. También es posible que se vieron afectados cero filas porque la fila se eliminó por otro usuario, por lo que en ese caso se muestra un mensaje de error diferentes.

En *DepartmentController.cs*, reemplace la `HttpGet` `Delete` método por el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

El método acepta un parámetro opcional que indica si la página se volverá a aparecer después de un error de simultaneidad. Si este indicador es `true`, se envía un mensaje de error a la vista que utiliza un `ViewBag` propiedad.

Reemplace el código de la `HttpPost` `Delete` método (denominado `DeleteConfirmed`) con el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

En el código con scaffolding que acaba de reemplazar, este método acepta un identificador de registro:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Ha cambiado este parámetro para un `Department` la instancia de entidad creada por el enlazador de modelos. Esto le da acceso a la `RowVersion` valor de la propiedad además de la clave de registro.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding denominado el `HttpPost` `Delete` método `DeleteConfirmed` para dar el `HttpPost` método una firma única. (El CLR requiere métodos sobrecargados para que tenga parámetros de método diferente). Ahora que las firmas son únicas, puede pegar con la convención MVC y usar el mismo nombre para el `HttpPost` y `HttpGet` eliminar métodos.

Si se detecta un error de simultaneidad, el código vuelve a mostrar la página de confirmación de eliminación y proporciona una marca que indica que debe mostrar un mensaje de error de simultaneidad.

En *Views\Department\Delete.cshtml*, reemplace el código con scaffolding con el siguiente código que agrega un campo de mensaje de error y los campos ocultos de las propiedades DepartmentID y RowVersion. Los cambios aparecen resaltados.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Este código agrega un mensaje de error entre la `h2` y `h3` encabezados:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Reemplaza `LastName` con `FullName` en el `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Por último, agrega los campos ocultos para el `DepartmentID` y `RowVersion` propiedades después de la `Html.BeginForm` instrucción:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Ejecute la página de índice de departamentos. Haga clic con el **eliminar** hipervínculo para el departamento de inglés y seleccione **abrir en nueva ficha** , a continuación, en la primera ficha, haga clic en el **editar** hipervínculo para el departamento de inglés.

En la primera ventana, cambie uno de los valores y haga clic en **guardar** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La página de índice confirma el cambio.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

En la segunda ficha, haga clic en **eliminar**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Verá el mensaje de error de simultaneidad y se actualizan los valores de departamento con lo que está actualmente en la base de datos.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Si hace clic en **eliminar** nuevo, se le redirigirá a la página de índice, que muestra que se ha eliminado el departamento.

## <a name="summary"></a>Resumen

Con esto finaliza la introducción para controlar los conflictos de simultaneidad. Para obtener información sobre otras formas de controlar diversos escenarios de simultaneidad, vea [patrones de simultaneidad optimista](https://msdn.microsoft.com/en-us/data/jj592904) y [trabajar con valores de propiedad](https://msdn.microsoft.com/en-us/data/jj592677) en MSDN. El siguiente tutorial muestra cómo implementar la herencia de tabla por jerarquía para el `Instructor` y `Student` entidades.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Siguiente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
