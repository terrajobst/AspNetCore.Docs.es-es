---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Controlar la simultaneidad con Entity Framework en una aplicación ASP.NET MVC (7 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eadadd1ac98191d0f802dc742294938eb48d86b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809010"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Controlar la simultaneidad con Entity Framework en una aplicación ASP.NET MVC (7 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En los dos tutoriales anteriores, ha trabajado con los datos relacionados. Este tutorial muestra cómo controlar la simultaneidad. Podrá crear páginas web que funcionan con el `Department` entidad y las páginas que edición y eliminación `Department` entidades controlará los errores de simultaneidad. Las ilustraciones siguientes muestran las páginas de índice y Delete, incluidos algunos mensajes que se muestran si se produce un conflicto de simultaneidad.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Los conflictos de simultaneidad ocurren cuando un usuario muestra los datos de una entidad para editarlos y, después, otro usuario actualiza los datos de la misma entidad antes de que el primer cambio del usuario se escriba en la base de datos. Si no habilita la detección de este tipo de conflictos, quien actualice la base de datos en último lugar sobrescribe los cambios del otro usuario. En muchas aplicaciones, el riesgo es aceptable: si hay pocos usuarios o pocas actualizaciones, o si no es realmente importante si se sobrescriben algunos cambios, el costo de programación para la simultaneidad puede superar el beneficio obtenido. En ese caso, no tendrá que configurar la aplicación para que controle los conflictos de simultaneidad.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidad pesimista (bloqueo)

Si la aplicación necesita evitar la pérdida accidental de datos en casos de simultaneidad, una manera de hacerlo es usar los bloqueos de base de datos. Esto se denomina *simultaneidad pesimista*. Por ejemplo, antes de leer una fila de una base de datos, solicita un bloqueo de solo lectura o para acceso de actualización. Si bloquea una fila para acceso de actualización, no se permite que ningún otro usuario bloquee la fila como solo lectura o para acceso de actualización, porque recibirían una copia de los datos que se están modificando. Si bloquea una fila para acceso de solo lectura, otras personas también pueden bloquearla para acceso de solo lectura pero no para actualización.

Administrar los bloqueos tiene desventajas. Puede ser bastante complicado de programar. Requiere recursos de administración de base de datos significativos y puede provocar problemas de rendimiento como el número de usuarios de una aplicación aumenta (es decir, no se escala bien). Por estos motivos, no todos los sistemas de administración de bases de datos admiten la simultaneidad pesimista. Entity Framework no proporciona ninguna compatibilidad integrada para ello, y en este tutorial no muestra cómo implementarlo.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

La alternativa a la simultaneidad pesimista es *simultaneidad optimista*. La simultaneidad optimista implica permitir que se produzcan conflictos de simultaneidad y reaccionar correctamente si ocurren. Por ejemplo, Juan ejecuta la página de edición de los departamentos de TI, los cambios del **presupuesto** cantidad del departamento de inglés de 350.000,00 a 0,00 USD.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Antes de John hace clic en **guardar**, Julia ejecuta la misma página y los cambios del **Start Date** arrastrándolo desde el 9/1/2007 a 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John hace clic en **guardar** primero y ve su cambio cuando el explorador vuelve a la página de índice, a continuación, Juan hace clic en **guardar**. Lo que sucede después viene determinado por cómo controla los conflictos de simultaneidad. Algunas de las opciones se exponen a continuación:

- Puede realizar un seguimiento de la propiedad que ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos. En el escenario de ejemplo, no se perdería ningún dato porque los dos usuarios actualizaron diferentes propiedades. La próxima vez que un usuario examine el departamento de inglés, verá los cambios de John y Jane: una fecha de inicio de 8/8/2013 y un presupuesto de cero dólares.

    Este método de actualización puede reducir el número de conflictos que pueden dar lugar a una pérdida de datos, pero no puede evitar la pérdida de datos si se realizan cambios paralelos a la misma propiedad de una entidad. Si Entity Framework funciona de esta manera o no, depende de cómo implemente el código de actualización. A menudo no resulta práctico en una aplicación web, porque puede requerir mantener grandes cantidades de estado con el fin de realizar un seguimiento de todos los valores de propiedad originales de una entidad, así como los valores nuevos. Mantener grandes cantidades de estado puede afectar al rendimiento de la aplicación a porque requiere recursos del servidor o deben incluirse en la propia página web (por ejemplo, en los campos ocultos).
- Puede permitir que los cambios de Jane sobrescribir el cambio de John. La próxima vez que un usuario examine el departamento de inglés, verá 8/8/2013 y el valor de 350.000,00 USD restaurado. Esto se denomina un escenario de *Prevalece el cliente* o *Prevalece el último*. (Los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos). Como se mencionó en la introducción de esta sección, si no hace ninguna codificación para el control de la simultaneidad, se realizará automáticamente.
- Cambio de Julia puede impedir que se actualiza en la base de datos. Por lo general, podría mostrar un mensaje de error, mostrarle el estado actual de los datos y que le permita volver a aplicar sus cambios si desea que estén. Esto se denomina un escenario de *Prevalece el almacén*. (Los valores del almacén de datos tienen prioridad sobre los valores enviados por el cliente). En este tutorial implementará el escenario de Prevalece el almacén. Este método garantiza que ningún cambio se sobrescriba sin que se avise al usuario de lo que está sucediendo.

### <a name="detecting-concurrency-conflicts"></a>Detectar conflictos de simultaneidad

Puede resolver los conflictos controlando [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) las excepciones que inicia Entity Framework. Para saber cuándo se producen dichas excepciones, Entity Framework debe ser capaz de detectar conflictos. Por lo tanto, debe configurar correctamente la base de datos y el modelo de datos. Algunas opciones para habilitar la detección de conflictos son las siguientes:

- En la tabla de la base de datos, incluya una columna de seguimiento que pueda usarse para determinar si una fila ha cambiado. A continuación, puede configurar Entity Framework para que incluya esa columna en el `Where` cláusula SQL `Update` o `Delete` comandos.

    Suele ser el tipo de datos de la columna de seguimiento [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). El [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) valor es un número secuencial que se incrementa cada vez que se actualiza la fila. En un `Update` o `Delete` comando, el `Where` cláusula incluye el valor original de la columna de seguimiento (la versión original). Si ha cambiado por otro usuario, el valor de la fila que se está actualiza el `rowversion` columna es diferente del valor original, por lo que la `Update` o `Delete` instrucción no encuentra la fila para actualizar debido la `Where` cláusula. Cuando Entity Framework encuentra que no se ha actualizado ninguna fila por la `Update` o `Delete` comando (es decir, cuando el número de filas afectadas es cero), lo interpreta como un conflicto de simultaneidad.
- Configurar Entity Framework para incluir los valores originales de cada columna en la tabla en la `Where` cláusula de `Update` y `Delete` comandos.

    Como se muestra en la primera opción, si algo en la fila ha cambiado desde que se leyó primero la fila, el `Where` cláusula no devolverá una fila para la actualización, que Entity Framework interpreta como un conflicto de simultaneidad. Para las tablas de base de datos que tienen muchas columnas, este enfoque puede provocar gran `Where` cláusulas y puede requerir mantener grandes cantidades de estado. Como se indicó anteriormente, mantener grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor o deben incluirse en la propia página web. Por lo tanto, este enfoque generalmente no se recomienda y no es el método utilizado en este tutorial.

    Si desea implementar este enfoque para simultaneidad, tendrá que marcar todas las propiedades de clave no principal de la entidad que desea realizar un seguimiento de simultaneidad para agregando el [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) atributo a ellos. Que cambio permite a Entity Framework incluir todas las columnas en el código SQL `WHERE` cláusula de `UPDATE` instrucciones.

En el resto de este tutorial, agregará un [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking (propiedad) para el `Department` entidad, cree un controlador y vistas y probar para comprobar que todo funciona correctamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Agregar una propiedad de simultaneidad optimista para la entidad Department

En *Models\Department.cs*, agregue una propiedad de seguimiento denominada `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

El [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) atributo especifica que esta columna se incluirá en el `Where` cláusula de `Update` y `Delete` los comandos enviados a la base de datos. El atributo se denomina [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) porque las versiones anteriores de SQL Server utilizan una instancia de SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) tipo de datos antes de que el SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ha reemplazado. El tipo .net de `rowversion` es una matriz de bytes. Si prefiere usar la API fluida, puede usar el [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) método para especificar la propiedad de seguimiento, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Al agregar una propiedad cambió el modelo de base de datos, por lo que necesita realizar otra migración. En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Crear un controlador de departamento

Crear un `Department` controlador y vistas de la misma manera que los demás controladores, mediante las siguientes opciones:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

En *Controllers\DepartmentController.cs*, agregue un `using` instrucción:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Cambio "LastName" a "FullName" en todas partes de este archivo (cuatro repeticiones) para que se enumera en la lista desplegable de administrador de departamento contendrá el nombre completo del instructor en lugar de simplemente el apellido.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Reemplace el código existente para el `HttpPost` `Edit` método con el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

La vista almacenará original `RowVersion` valor en un campo oculto. Cuando se crea el enlazador de modelos la `department` instancia, ese objeto tendrá original `RowVersion` valor de propiedad y los nuevos valores para las demás propiedades, como los introducidos por el usuario en la página de edición. A continuación, cuando Entity Framework crea una instancia de SQL `UPDATE` comando, que incluirá comando un `WHERE` cláusula que busca una fila que tenga el original `RowVersion` valor.

Si no hay ninguna fila afectada por la `UPDATE` comando (ninguna fila tiene el original `RowVersion` valor), inicia Entity Framework un `DbUpdateConcurrencyException` excepción y el código en el `catch` bloque obtiene afectado `Department` entidad de la excepción objeto. Esta entidad tiene los valores leídos desde la base de datos y los nuevos valores especificados por el usuario:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

A continuación, el código agrega un mensaje de error personalizado para cada columna que tiene valores de la base de datos diferentes de lo que el usuario que especificó en la página de edición:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

¿Qué ha ocurrido y qué hacer al respecto, explica un mensaje de error más largo:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Por último, el código establece la `RowVersion` valor de la `Department` recupera el objeto en el nuevo valor de la base de datos. Este nuevo valor `RowVersion` se almacenará en el campo oculto cuando se vuelva a mostrar la página Edit y, la próxima vez que el usuario haga clic en **Save**, solo se detectarán los errores de simultaneidad que se produzcan desde que se vuelva a mostrar la página Edit.

En *Views\Department\Edit.cshtml*, agregue un campo oculto para guardar la `RowVersion` valor de propiedad, inmediatamente después del campo oculto para el `DepartmentID` propiedad:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

En *Views\Department\Index.cshtml*, reemplace el código existente por el siguiente código al mover vínculos de la fila a la izquierda y cambiar los encabezados de columna y título de página para mostrar `FullName` en lugar de `LastName` en el **Administrador** columna:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Las pruebas de control de simultaneidad optimista

Ejecute el sitio Web y haga clic en **departamentos**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Haga clic en el **editar** hipervínculo para Kim Abercrombie y seleccione **abrir en nueva pestaña** , a continuación, haga clic en el **editar** hipervínculo para Kim Abercrombie. Las dos ventanas muestran la misma información.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Cambiar un campo en la primera ventana de explorador y haga clic en **guardar**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

El explorador muestra la página de índice con el valor modificado.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Cambiar el cualquier campo en la segunda ventana del explorador y haga clic en **guardar**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Haga clic en **guardar** en la segunda ventana del explorador. Verá un mensaje de error:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Vuelva a hacer clic en **Save**. El valor especificado en el Explorador de segundo se guarda junto con el valor original de los datos que se cambian en el primer explorador. Verá los valores guardados cuando aparezca la página de índice.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Actualizar la página Delete

Para la página Delete, Entity Framework detecta los conflictos de simultaneidad causados por una persona que edita el departamento de forma similar. Cuando el `HttpGet` `Delete` método muestra la vista de confirmación, la vista incluye original `RowVersion` valor en un campo oculto. Dicho valor está entonces disponible para el `HttpPost` `Delete` método que se llama cuando el usuario confirma la eliminación. Cuando Entity Framework crea el código SQL `DELETE` de comandos, incluye un `WHERE` cláusula con el original `RowVersion` valor. Si el comando tiene como resultado cero filas afectadas (es decir, la fila se cambió después de que se muestre la página de confirmación de eliminación), se produce una excepción de simultaneidad y el `HttpGet Delete` se llama al método con un indicador de error establecido en `true` con el fin de volver a mostrar el página de confirmación con un mensaje de error. También es posible que el cero filas afectadas porque la fila fue eliminada por otro usuario, por lo que en ese caso se muestra un mensaje de error diferentes.

En *DepartmentController.cs*, reemplace el `HttpGet` `Delete` método con el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

El método acepta un parámetro opcional que indica si la página volverá a aparecer después de un error de simultaneidad. Si esta marca es `true`, se envía un mensaje de error a la vista mediante una `ViewBag` propiedad.

Reemplace el código en el `HttpPost` `Delete` método (denominado `DeleteConfirmed`) con el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

En el código al que se aplicó la técnica scaffolding que acaba de reemplazar, este método solo acepta un identificador de registro:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Ha cambiado este parámetro para un `Department` instancia de entidad creada por el enlazador de modelos. Esto le otorga acceso a la `RowVersion` valor de propiedad además de la clave de registro.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El nombre de código con scaffolding la `HttpPost` `Delete` método `DeleteConfirmed` para dar el `HttpPost` método una firma única. (El CLR requiere métodos sobrecargados para tener parámetros de método diferentes). Ahora que las firmas son únicas, puede seguir la convención de MVC y usar el mismo nombre para el `HttpPost` y `HttpGet` elimina métodos.

Si se detecta un error de simultaneidad, el código vuelve a mostrar la página de confirmación de Delete y proporciona una marca que indica que se debería mostrar un mensaje de error de simultaneidad.

En *Views\Department\Delete.cshtml*, reemplace el código con scaffolding con el siguiente código que hace que sea parte del formato cambia y agrega un campo de mensaje de error. Los cambios aparecen resaltados.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Este código agrega un mensaje de error entre el `h2` y `h3` encabezados:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Reemplaza `LastName` con `FullName` en el `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Por último, agrega los campos ocultos para los `DepartmentID` y `RowVersion` propiedades después de la `Html.BeginForm` instrucción:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ejecute la página de índice de Departments. Haga clic en el **eliminar** hipervínculo para el departamento de inglés y seleccione **abrir en ventana nueva,** , a continuación, en la primera ventana, haga clic en el **editar** hipervínculo para el inglés departamento.

En la primera ventana, cambie uno de los valores y haga clic en **guardar** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La página de índice confirma el cambio.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

En la segunda ventana, haga clic en **eliminar**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Verá el mensaje de error de simultaneidad y se actualizarán los valores de Department con lo que está actualmente en la base de datos.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Si vuelve a hacer clic en **Delete**, se le redirigirá a la página de índice, que muestra que se ha eliminado el departamento.

## <a name="summary"></a>Resumen

Con esto finaliza la introducción para el control de los conflictos de simultaneidad. Para obtener información sobre otras formas de controlar los distintos escenarios de simultaneidad, consulte [patrones de simultaneidad optimista](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) y [trabajar con valores de propiedad](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) en el blog del equipo de Entity Framework. El siguiente tutorial muestra cómo implementar la herencia de tabla por jerarquía para el `Instructor` y `Student` entidades.

Pueden encontrar vínculos a otros recursos de Entity Framework en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
