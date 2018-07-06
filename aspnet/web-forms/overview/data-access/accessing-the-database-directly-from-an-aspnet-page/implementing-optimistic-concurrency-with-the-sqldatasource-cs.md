---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implementar la simultaneidad optimista con SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial revisaremos los aspectos básicos de control de simultaneidad optimista y, a continuación, explore cómo se implementa mediante el control SqlDataSource.
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 2907e05dfa82d44d38217b90d85d5c2192613560
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811470"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementar la simultaneidad optimista con SqlDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) o [descargar PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> En este tutorial revisaremos los aspectos básicos de control de simultaneidad optimista y, a continuación, explore cómo se implementa mediante el control SqlDataSource.


## <a name="introduction"></a>Introducción

En el tutorial anterior, hemos visto cómo agregar, insertar, actualizar y eliminar capacidades para el control SqlDataSource. En resumen, para proporcionar estas características se necesita especificar correspondiente `INSERT`, `UPDATE`, o `DELETE` instrucción SQL en el control s `InsertCommand`, `UpdateCommand`, o `DeleteCommand` propiedades, junto con la correspondiente los parámetros en el `InsertParameters`, `UpdateParameters`, y `DeleteParameters` colecciones. Aunque estas propiedades y las colecciones se pueden especificar manualmente, el botón de opciones avanzadas de s de Asistente para configurar origen de datos ofrece una generar `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones que se creará automáticamente estas instrucciones según el `SELECT` instrucción.

Junto con la generar `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones, el cuadro de diálogo Opciones de generación SQL avanzadas incluye una opción de usar simultaneidad optimista (consulte la figura 1). Cuando está activada, el `WHERE` cláusulas en el autogenerados `UPDATE` y `DELETE` se modifican las instrucciones para realizar solo la actualización o eliminación si el t de preguntas de datos de base de datos subyacente se ha modificado desde que el usuario por última vez carga los datos en la cuadrícula.


![Puede agregar compatibilidad con la simultaneidad optimista de la avanzada cuadro de diálogo Opciones de generación de SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Figura 1**: puede agregar compatibilidad con la simultaneidad optimista de la avanzada cuadro de diálogo Opciones de generación de SQL


En el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial se examinan los fundamentos del control de simultaneidad optimista y cómo agregarlo a ObjectDataSource. En este tutorial crearemos Retocar en las operaciones básicas de control de simultaneidad optimista y, a continuación, explore cómo implementarla mediante SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Un resumen de la simultaneidad optimista

Para las aplicaciones web que permiten a varios usuarios simultáneos para editar o eliminar los mismos datos, existe la posibilidad de que un usuario podría sobrescribir accidentalmente otra cambios s. En el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial proporciona en el ejemplo siguiente:

Imagine que dos usuarios, Jisun y Sam, visitan una página en una aplicación que permite a los visitantes actualizar y eliminar productos a través de un control GridView en ambos. Haga clic en el botón Editar para Chai aproximadamente al mismo tiempo. Jisun cambia el nombre del producto a Chai té y hace clic en el botón Actualizar. El resultado neto es un `UPDATE` instrucción que se envía a la base de datos, que establece *todas* de los campos actualizables de producto s (aunque Jisun solo actualiza un campo, `ProductName`). En este momento, la base de datos tiene los valores de té Chai, la categoría Bebidas, los líquidos exóticas de proveedor, y así sucesivamente para este producto en particular. Sin embargo, el control GridView en pantalla de Sam s sigue mostrando el nombre del producto en la fila GridView editable como Chai. Unos pocos segundos después de que se han realizado cambios de s Jisun, Sam actualizaciones de la categoría condimentos y hace clic en actualizar. Esto da como resultado un `UPDATE` instrucción enviada a la base de datos que establece el nombre del producto a Chai, la `CategoryID` para el Id. de categoría condimentos correspondiente y así sucesivamente. Se han sobrescrito los cambios de s Jisun al nombre del producto.

Figura 2 ilustra esta interacción.


[![Cuando dos usuarios simultáneamente actualizan un registro existe potencial s para un usuario s cambia esta opción para sobrescribir los otros elementos](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Figura 2**: cuando dos usuarios al mismo tiempo la actualización cambia de un registro existe s potencial para un usuario s para sobrescribir los otros elementos ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Para evitar que este escenario como el desarrollo, una forma de [control de simultaneidad](http://en.wikipedia.org/wiki/Concurrency_control) debe implementarse. [Simultaneidad optimista](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) el enfoque de este tutorial funciona en la suposición de que, aunque puede haber conflictos de simultaneidad vez, surgen la mayoría del tiempo tales conflictos ganaron t. Por lo tanto, si surge un conflicto, el control de simultaneidad optimista simplemente informa al usuario que su t de cambios puede se puede guardar porque otro usuario modificó los mismos datos.

> [!NOTE]
> Para las aplicaciones donde se supone que habrá muchos conflictos de simultaneidad o si este tipo de conflictos no es válidos, a continuación, el control de simultaneidad pesimista puede utilizarse en su lugar. Vuelva a consultar el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial para obtener una explicación más completa en el control de simultaneidad pesimista.


Control de simultaneidad optimista funciona asegurándose de que el registro que se va a actualizar o eliminar tiene los mismos valores que tenía cuando se inició la actualización o eliminación de proceso. Por ejemplo, al hacer clic en el botón Editar de un control GridView editable, los valores del registro s se leen de la base de datos y muestran en cuadros de texto y otros controles Web. Estos valores originales se guardan por el control GridView. Más adelante, cuando el usuario hace que sus cambios y hace clic en el botón de actualización, el `UPDATE` instrucción utilizada debe tener en cuenta los valores originales además de los valores nuevos y solo actualiza el registro de base de datos subyacente, si los valores que el usuario comenzó a editar el original son idénticos a los valores en la base de datos. Figura 3 muestra esta secuencia de eventos.


[![Para que la actualización o eliminación se realice correctamente, los valores originales deben ser iguales a los valores actuales de la base de datos](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: para la actualización o eliminación que sea un éxito, el Original valores debe ser igual que los valores actuales de la base de datos ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Hay varios enfoques para implementar la simultaneidad optimista (consulte [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic simultaneidad actualizando lógica](http://www.eggheadcafe.com/articles/20050719.asp) para un breve vistazo a una serie de opciones). La técnica empleada por SqlDataSource (así como por los DataSets con tipo ADO.NET que se utiliza en la capa de acceso a datos) aumenta la `WHERE` cláusula para incluir una comparación de todos los valores originales. La siguiente `UPDATE` instrucción, por ejemplo, actualiza el nombre y el precio de un producto sólo si los valores actuales de la base de datos son iguales a los valores que se recuperaron originalmente al actualizar el registro en el control GridView. El `@ProductName` y `@UnitPrice` parámetros contienen los nuevos valores especificados por el usuario, mientras que `@original_ProductName` y `@original_UnitPrice` contienen los valores que se cargaron originalmente en el control GridView cuando se hace clic en el botón Editar:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Como veremos en este tutorial, la habilitación del control de simultaneidad optimista con SqlDataSource es tan sencillo como activar una casilla.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Paso 1: Crear un SqlDataSource que admite la simultaneidad optimista

Comience abriendo la `OptimisticConcurrency.aspx` página desde la `SqlDataSource` carpeta. Arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador, la configuración de su `ID` propiedad `ProductsDataSourceWithOptimisticConcurrency`. A continuación, haga clic en el vínculo Configurar origen de datos de la etiqueta inteligente del control s. En la primera pantalla del asistente, elija para que funcione con el `NORTHWINDConnectionString` y haga clic en siguiente.


[![Elija para trabajar con el NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: elegir para trabajar con el `NORTHWINDConnectionString` ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


En este ejemplo se agregará un control GridView que permite a los usuarios editar el `Products` tabla. Por lo tanto, en la configuración de la pantalla de la instrucción Select, elija el `Products` de tabla en la lista desplegable y seleccione el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` columnas, como se muestra en la figura 5.


[![De la tabla Products, devolver el ProductID, ProductName, UnitPrice y las columnas no incluidas](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Figura 5**: desde el `Products` de tabla, se devuelve el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` columnas ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Después de elegir las columnas, haga clic en el botón Opciones avanzadas para que aparezca el cuadro de diálogo Opciones de generación SQL avanzadas. Compruebe la generar `INSERT`, `UPDATE`, y `DELETE` instrucciones y utilice las casillas de verificación de la simultaneidad optimista y haga clic en Aceptar (consulte atrás en la figura 1 para una captura de pantalla). Complete el Asistente haciendo clic en siguiente y luego en Finalizar.

Después de completar el Asistente para configurar orígenes de datos, dedique un momento a examinar los resultados `DeleteCommand` y `UpdateCommand` propiedades y el `DeleteParameters` y `UpdateParameters` colecciones. La manera más fácil de hacerlo es hacer clic en la ficha de origen en la esquina inferior izquierda para ver la sintaxis declarativa de s de página. Allí encontrará un `UpdateCommand` valor:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Con siete parámetros en el `UpdateParameters` colección:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

De forma similar, el `DeleteCommand` propiedad y `DeleteParameters` colección debe ser similar al siguiente:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Además de ampliar la `WHERE` cláusulas de la `UpdateCommand` y `DeleteCommand` propiedades (y agregar los parámetros adicionales a las colecciones de parámetros respectivos), seleccionar el uso optimista de opción de simultaneidad ajusta a otras dos propiedades:

- Los cambios del [ `ConflictDetection` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) desde `OverwriteChanges` (valor predeterminado) para `CompareAllValues`
- Los cambios del [ `OldValuesParameterFormatString` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) desde {0} (valor predeterminado) a original\_ {0} .

Cuando los datos de control Web invoca la s SqlDataSource `Update()` o `Delete()` método, pasa en los valores originales. Si la s SqlDataSource `ConflictDetection` propiedad está establecida en `CompareAllValues`, estos valores originales se agregan al comando. El `OldValuesParameterFormatString` propiedad proporciona el patrón de nomenclatura que se usan para estos parámetros de valor original. El Asistente para configurar orígenes de datos usa original\_ {0} y los nombres de cada parámetro original en el `UpdateCommand` y `DeleteCommand` propiedades y `UpdateParameters` y `DeleteParameters` colecciones según corresponda.

> [!NOTE]
> Puesto que nos re no usa la s de control SqlDataSource insertar funcionalidades, no dude en quitar el `InsertCommand` propiedad y su `InsertParameters` colección.


## <a name="correctly-handlingnullvalues"></a>Para controlar correctamente`NULL`valores

Por desgracia, aumentada `UPDATE` y `DELETE` instrucciones genera automáticamente el Asistente para configurar origen de datos cuando se utiliza simultaneidad optimista es *no* funcionan con los registros que contienen `NULL` valores. Para ver por qué, considere la posibilidad de nuestro s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

El `UnitPrice` columna en el `Products` tabla puede tener `NULL` valores. Si tiene un registro determinado un `NULL` valor `UnitPrice`, el `WHERE` parte de la cláusula `[UnitPrice] = @original_UnitPrice` le *siempre* se evalúan como False porque `NULL = NULL` siempre devuelve False. Por lo tanto, los registros que contienen `NULL` valores no se pueden modificar ni eliminar, como el `UPDATE` y `DELETE` instrucciones `WHERE` cláusulas ganaron t devuelven las filas para actualizar o eliminar.

> [!NOTE]
> Este error se notificó primero a Microsoft en junio de 2004 en [SqlDataSource genera SQL las instrucciones incorrectas](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) y al parecer está programada se ha solucionado en la próxima versión de ASP.NET.


Para solucionar este problema, tenemos que actualizar manualmente el `WHERE` cláusulas tanto en el `UpdateCommand` y `DeleteCommand` las propiedades de **todos los** columnas que pueden tener `NULL` valores. En general, cambiar `[ColumnName] = @original_ColumnName` para:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Esta modificación se puede realizar directamente mediante el marcado declarativo, mediante las opciones UpdateQuery o DeleteQuery desde la ventana Propiedades, o a través de la actualización y eliminar las pestañas en la especificación de una instrucción SQL personalizada o la opción de procedimiento almacenado en los datos de configuración Asistente de origen. Nuevamente, esta modificación debe efectuarse en *cada* columna en el `UpdateCommand` y `DeleteCommand` s `WHERE` cláusula que pueda contener `NULL` valores.

Si se aplica a nuestro ejemplo da como resultado la siguiente modificada `UpdateCommand` y `DeleteCommand` valores:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Paso 2: Agregar un control GridView con Editar y opciones de eliminación

Con SqlDataSource configurado para admitir la simultaneidad optimista, todo lo que queda es agregar un control Web de datos a la página que utiliza este control de simultaneidad. Para este tutorial, permiten s Agregar un control GridView que proporciona ambos edición y la funcionalidad de eliminación. Para ello, arrastre un control GridView del cuadro de herramientas en el diseñador y el conjunto de sus `ID` a `Products`. En la etiqueta inteligente s GridView, enlazarlo a la `ProductsDataSourceWithOptimisticConcurrency` control SqlDataSource agregado en el paso 1. Por último, compruebe las opciones Habilitar edición y habilitar la eliminación de la etiqueta inteligente.


[![Enlazar el control GridView a SqlDataSource y habilitar la edición y eliminación](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Figura 6**: enlazar el control GridView a SqlDataSource y Habilitar edición y eliminación ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Después de agregar el control GridView, configurar su apariencia mediante la eliminación de la `ProductID` BoundField, cambiar el `ProductName` BoundField s `HeaderText` propiedad al producto y actualice el `UnitPrice` BoundField para que su `HeaderText` es propiedad simplemente precio. Idealmente, d mejoramos la interfaz de edición para incluir un control RequiredFieldValidator para el `ProductName` valor y un control CompareValidator para los `UnitPrice` valor (para asegurarse de que un valor numérico con el formato correcto de s). Hacer referencia a la [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial para una información más detallada sobre la personalización de las operaciones de asignación GridView interfaz de edición.

> [!NOTE]
> GridView debe estar habilitado el estado de vista de s puesto que los valores originales que se pasan desde el control GridView de SqlDataSource son almacenados en la vista estado.


Después de realizar estas modificaciones en el control GridView, el marcado declarativo de GridView y SqlDataSource debe ser similar al siguiente:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Para ver el control de simultaneidad optimista en acción, abra dos ventanas del explorador y cargar la `OptimisticConcurrency.aspx` página en ambos. Haga clic en los botones de edición para el primer producto de los dos exploradores. En un explorador, cambie el nombre de producto y haga clic en actualizar. El explorador se postback y GridView volverá a su modo de edición previamente, que muestra el nuevo nombre de producto para el registro que acaba de modificar.

En la segunda ventana del explorador, cambie el precio (pero deje el nombre del producto como su valor original) y haga clic en actualizar. En el postback, la cuadrícula se devuelve al modo de edición previamente, pero no se registra el cambio en el precio. El segundo explorador muestra el mismo valor que la primera de ellas en el nuevo nombre de producto con el precio anterior. Se perdieron los cambios realizados en la segunda ventana del explorador. Además, los cambios se han perdido silenciosamente en su lugar, porque se ha producido ninguna excepción o mensaje que indica que acaba de producirse una infracción de simultaneidad.


[![Los cambios en la segunda ventana del explorador se perdieron en modo silencioso](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Figura 7**: los cambios en el segundo explorador ventana se silenciosamente perdieron ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


La razón de por qué no se confirmaron los cambios del explorador s segundo fue que la `UPDATE` instrucción s `WHERE` cláusula filtran todos los registros y, por tanto, no afectó a ninguna fila. Permiten s mirar el `UPDATE` instrucción de nuevo:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Cuando la segunda ventana del explorador actualiza el registro, el nombre original del producto especificado en el `WHERE` cláusula t coincidencia con el nombre de producto existente (ya que se ha cambiado el primer explorador). Por lo tanto, la instrucción `[ProductName] = @original_ProductName` devuelve False y el `UPDATE` no afecta a todos los registros.

> [!NOTE]
> Eliminar trabajos de la misma manera. Con dos ventanas del explorador abiertas, empiece por edición de un producto determinado con uno y, a continuación, guarde sus cambios. Después de guardar los cambios en un explorador, haga clic en el botón Eliminar para el mismo producto en el otro. Puesto que el original don valores t coincidan en el `DELETE` instrucción s `WHERE` cláusula, la eliminación silenciosa genera un error.


Desde la perspectiva de s del usuario final en la segunda ventana del explorador, tras hacer clic en el botón Actualizar la cuadrícula se devuelve al modo de edición previamente, pero sus cambios se han perdido. Sin embargo, allí s ningún indicador visual que seguir su t de los cambios. Lo ideal es que, si cambia de un usuario s se pierden a una infracción de simultaneidad, d envíeles una notificación y, quizás, mantenga la cuadrícula en modo de edición. Permiten s vistazo a cómo realizar esta tarea.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Paso 3: Determinar cuándo se ha producido una infracción de simultaneidad

Puesto que una infracción de simultaneidad rechaza los cambios que ha realizado uno, sería bueno avisar al usuario cuando se ha producido una infracción de simultaneidad. Para avisar al usuario, s permiten agregar un control Web de la etiqueta a la parte superior de la página llamada `ConcurrencyViolationMessage` cuyo `Text` propiedad muestra el siguiente mensaje: ha intentado actualizar o eliminar un registro que se ha actualizado simultáneamente por otro usuario. Revise los cambios del otro usuario y la actualización de puesta al día o eliminar. Establecer el control de etiqueta s `CssClass` propiedad a la advertencia, que es una clase CSS definida en `Styles.css` que muestra texto en una fuente roja, cursiva, negrita y grande. Por último, establezca la etiqueta s `Visible` y `EnableViewState` propiedades a `false`. Esto ocultará la etiqueta excepto solo esas devoluciones de datos que se establece explícitamente su `Visible` propiedad `true`.


[![Agregar un Control etiqueta a la página para mostrar la advertencia](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Figura 8**: agregar un Control etiqueta a la página para mostrar la advertencia ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Al realizar una actualización o eliminación, la s GridView `RowUpdated` y `RowDeleted` controladores de eventos se activan después de su control de origen de datos haya realizado la actualización solicitada o delete. Podemos determinar el número de filas afectado por la operación de estos controladores de eventos. Si cero filas afectadas, queremos mostrar el `ConcurrencyViolationMessage` etiqueta.

Cree un controlador de eventos para ambos el `RowUpdated` y `RowDeleted` eventos y agregue el código siguiente:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

En ambos controladores de eventos comprobamos la `e.AffectedRows` propiedad y, si es igual a 0, establezca el `ConcurrencyViolationMessage` etiqueta s `Visible` propiedad `true`. En el `RowUpdated` controlador de eventos, se instruir a GridView para permanecer en modo de edición estableciendo el `KeepInEditMode` propiedad en true. Al hacerlo, debemos enlazar los datos a la cuadrícula para que los demás datos de usuario s se cargan en la interfaz de edición. Esto se logra mediante una llamada a la s GridView `DataBind()` método.

Como se muestra en la figura 9, estos dos controladores de eventos, se muestra un mensaje muy notable siempre que se produzca una infracción de simultaneidad.


[![Se muestra un mensaje en caso de una infracción de simultaneidad](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Figura 9**: se muestra un mensaje en caso de una infracción de simultaneidad ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Resumen

Al crear una aplicación web donde simultánea de varios usuarios pueden estar editando los mismos datos, es importante tener en cuenta las opciones de control de simultaneidad. De forma predeterminada, los controles Web de los datos de ASP.NET y controles de origen de datos no emplean cualquier control de simultaneidad. Como hemos visto en este tutorial, implementar el control de simultaneidad optimista con SqlDataSource es relativamente rápido y sencillo. SqlDataSource controla la mayoría de los diversos preparativos para la adición de aumentada `WHERE` cláusulas a la autogenerados `UPDATE` y `DELETE` , pero no hay instrucciones son algunas sutilezas en el control de `NULL` valor columnas, como se describe en el Para controlar correctamente `NULL` sección de valores.

En este tutorial concluye nuestro examen de SqlDataSource. Nuestros tutoriales restantes volverá a trabajar con datos mediante el origen ObjectDataSource y arquitectura en capas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [Siguiente](querying-data-with-the-sqldatasource-control-vb.md)
