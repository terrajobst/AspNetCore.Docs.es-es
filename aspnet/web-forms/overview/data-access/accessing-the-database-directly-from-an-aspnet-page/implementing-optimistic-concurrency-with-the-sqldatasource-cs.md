---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: "Implementación de simultaneidad optimista con SqlDataSource (C#) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial se revisión las operaciones básicas de control de simultaneidad optimista y, a continuación, explorar cómo se implementa mediante el control SqlDataSource."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b089a0b25aa5a520f3e20af8ec5212072ad7c7bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementación de simultaneidad optimista con SqlDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) o [descarga de PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> En este tutorial se revisión las operaciones básicas de control de simultaneidad optimista y, a continuación, explorar cómo se implementa mediante el control SqlDataSource.


## <a name="introduction"></a>Introducción

En el tutorial anterior, se examina cómo agregar Insertar, actualizar y eliminar funciones para el control SqlDataSource. En resumen, para proporcionar estas características necesitábamos especificar el correspondiente `INSERT`, `UPDATE`, o `DELETE` instrucción SQL en el control s `InsertCommand`, `UpdateCommand`, o `DeleteCommand` propiedades, junto con la correspondiente parámetros de la `InsertParameters`, `UpdateParameters`, y `DeleteParameters` las colecciones. Aunque estas propiedades y las colecciones pueden especificarse manualmente, el botón Avanzadas de la s de Asistente para configurar origen de datos ofrece una generar `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones que se creará automáticamente estas instrucciones en función de la `SELECT` instrucción.

Junto con Generate `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de instrucciones, el cuadro de diálogo Opciones de generación SQL avanzadas incluye un uso de la opción de simultaneidad optimista (consulte la figura 1). Al activar esta opción, el `WHERE` cláusulas en la autogenerado `UPDATE` y `DELETE` se modifican las instrucciones para llevar a cabo sólo la actualización o eliminación si el t de todavía no de datos de base de datos subyacente se ha modificado desde que el usuario última carga los datos en la cuadrícula.


![Puede agregar compatibilidad con la simultaneidad optimista de la avanzada cuadro de diálogo Opciones de generación de SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Figura 1**: puede agregar compatibilidad con la simultaneidad optimista de la avanzada cuadro de diálogo Opciones de generación de SQL


En el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial examinamos los fundamentos del control de simultaneidad optimista y cómo agregarlo a ObjectDataSource. En este tutorial comenzaremos Retocar en las operaciones básicas de control de simultaneidad optimista y, a continuación, explorar cómo se implementa mediante el SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Resumen de la simultaneidad optimista

Para aplicaciones web que permiten a varios usuarios simultáneos para editar o eliminar los mismos datos, existe la posibilidad de que un usuario podría sobrescribir accidentalmente los cambios de otro s. En el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial proporciona en el siguiente ejemplo:

Imagine que dos usuarios, Jisun y Sam, visitan una página en una aplicación que permite a los visitantes actualizar y eliminar productos a través de un control GridView en ambos. Haga clic en el botón Editar para Chai aproximadamente al mismo tiempo. Jisun cambia el nombre de producto a té Chai y hace clic en el botón de actualización. El resultado neto es un `UPDATE` instrucción que se envía a la base de datos, que establece *todos los* de los campos actualizables producto s (aunque Jisun solo actualiza un campo, `ProductName`). En este momento, la base de datos tiene los valores de té Chai, la categoría Bebidas, el proveedor Exotic Liquids, y así sucesivamente para este producto en particular. Sin embargo, el control GridView en pantalla de Sam s sigue mostrando el nombre del producto en la fila de GridView editable como Chai. Unos pocos segundos después de que se han realizado cambios de s Jisun, Sam actualizaciones de la categoría condimentos y hace clic en actualizar. Esto da como resultado un `UPDATE` instrucción enviada a la base de datos que establece el nombre del producto para Chai, la `CategoryID` para el Id. de categoría de condimentos correspondiente y así sucesivamente. Cambios de s Jisun al nombre del producto se han sobrescrito.

Figura 2 muestra esta interacción.


[![Cuando dos usuarios simultáneamente actualización un registro existe potencial s para un usuario s cambia para sobrescribir las otras operaciones de asignación](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Figura 2**: cuando dos usuarios al mismo tiempo la actualización un registro existe s potencial para un usuario s cambia a sobrescribir las otras operaciones de asignación ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Para evitar que este escenario como el desarrollo, una forma de [control de simultaneidad](http://en.wikipedia.org/wiki/Concurrency_control) debe implementarse. [Simultaneidad optimista](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) el foco de este tutorial funciona en la suposición aunque puede haber conflictos de simultaneidad cada ahora y, a continuación, la mayoría de las veces tales conflictos won t surgen. Por lo tanto, si surge un conflicto, control de simultaneidad optimista simplemente informa al usuario que puede guardar su t de can cambios porque otro usuario ha modificado los mismos datos.

> [!NOTE]
> Para las aplicaciones donde se supone que habrá muchos conflictos de simultaneidad o si tales conflictos no son válidos, a continuación, el control de simultaneidad pesimista puede utilizarse en su lugar. Hacen referencia a la [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial para obtener una explicación más exhaustiva sobre el control de simultaneidad pesimista.


Control de simultaneidad optimista funciona asegurándose de que el registro que se va a actualizar o eliminar tiene los mismos valores que tenía cuando se inició la actualización o eliminación de proceso. Por ejemplo, al hacer clic en el botón de edición en un control GridView editable, los valores del registro s se leer desde la base de datos y se muestran en cuadros de texto y otros controles Web. Estos valores originales se guardan por GridView. Más adelante, cuando el usuario hace que sus cambios y haga clic en el botón de actualización, la `UPDATE` instrucción utilizada debe tener en cuenta los valores originales y los nuevos valores y actualizar solo el registro de base de datos subyacente si los valores originales que el usuario inició la edición son idénticos a los valores en la base de datos. La figura 3 muestra esta secuencia de eventos.


[![Para que la actualización o eliminación sea correcta, los valores originales deben ser iguales a los valores actuales de la base de datos](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: para la actualización o eliminación como correcto, el Original valores debe ser igual a los valores actuales de la base de datos ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Hay varios enfoques para implementar la simultaneidad optimista (vea [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic simultaneidad actualizando lógica](http://www.eggheadcafe.com/articles/20050719.asp) para una breve descripción de una serie de opciones). Técnica utilizada por el SqlDataSource (así como el ADO.NET escrito los conjuntos de datos utilizados en la capa de acceso a datos) aumenta la `WHERE` cláusula para incluir una comparación de todos los valores originales. El siguiente `UPDATE` instrucción, por ejemplo, actualiza el nombre y el precio de un producto sólo si los valores actuales de la base de datos son iguales a los valores que se recuperaron originalmente cuando se actualiza el registro en GridView. El `@ProductName` y `@UnitPrice` parámetros contienen los nuevos valores especificados por el usuario, mientras que `@original_ProductName` y `@original_UnitPrice` contienen los valores que se cargaron originalmente en GridView cuando se hace clic en el botón de edición:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Como veremos más adelante en este tutorial, habilitar el control de simultaneidad optimista con SqlDataSource es tan simple como una casilla de verificación.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Paso 1: Crear un SqlDataSource que admite la simultaneidad optimista

Comience abriendo la `OptimisticConcurrency.aspx` página desde el `SqlDataSource` carpeta. Arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador, la configuración de su `ID` propiedad `ProductsDataSourceWithOptimisticConcurrency`. A continuación, haga clic en el vínculo Configurar origen de datos de la etiqueta inteligente de control s. En la primera pantalla del asistente, elija para que funcione con el `NORTHWINDConnectionString` y haga clic en siguiente.


[![Elija para trabajar con la conexión NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: elegir para trabajar con la `NORTHWINDConnectionString` ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


En este ejemplo iremos agregando un control GridView que permite a los usuarios editar los `Products` tabla. Por lo tanto, en la pantalla de la instrucción Select de configurar, elija la `Products` de tabla en la lista desplegable y seleccione el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` las columnas, como se muestra en la figura 5.


[![De la tabla Products, devolver ProductID, ProductName, UnitPrice y columnas no incluidas](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Figura 5**: desde la `Products` tabla, devolver el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` columnas ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Después de elegir las columnas, haga clic en el botón Opciones avanzadas para abrir el cuadro de diálogo Opciones de generación SQL avanzadas. Compruebe la generar `INSERT`, `UPDATE`, y `DELETE` las instrucciones y utilizar las casillas de verificación de la simultaneidad optimista y haga clic en Aceptar (hacen referencia a la figura 1 para una captura de pantalla). Complete el Asistente haciendo clic en siguiente y, a continuación, finalizar.

Después de completar el Asistente para configurar orígenes de datos, tómese un momento para examinar los resultados `DeleteCommand` y `UpdateCommand` propiedades y la `DeleteParameters` y `UpdateParameters` colecciones. La manera más fácil de hacerlo es hacer clic en la ficha de origen en la esquina inferior izquierda para ver la sintaxis declarativa de s de página. Allí encontrará un `UpdateCommand` valo:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Con siete parámetros en la `UpdateParameters` colección:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

De forma similar, el `DeleteCommand` propiedad y `DeleteParameters` colección debe ser similar al siguiente:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Además de aumentar la `WHERE` cláusulas de la `UpdateCommand` y `DeleteCommand` propiedades (y agregar los parámetros adicionales a las colecciones de parámetros respectivos), al seleccionar el uso optimista de opción de simultaneidad ajusta otras dos propiedades:

- Cambios de la [ `ConflictDetection` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) de `OverwriteChanges` (el valor predeterminado) a`CompareAllValues`
- Cambios de la [ `OldValuesParameterFormatString` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) de {0} (el valor predeterminado) para original\_{0}.

Cuando los datos de control Web invoca las operaciones de asignación SqlDataSource `Update()` o `Delete()` método, pasa en los valores originales. Si las operaciones de asignación SqlDataSource `ConflictDetection` propiedad está establecida en `CompareAllValues`, estos valores originales se agregan al comando. El `OldValuesParameterFormatString` propiedad proporciona el patrón de nomenclatura que se usa para estos parámetros de valor original. El Asistente para configurar orígenes de datos usa original\_{0} y el nombre de cada parámetro original en el `UpdateCommand` y `DeleteCommand` propiedades y `UpdateParameters` y `DeleteParameters` colecciones en consecuencia.

> [!NOTE]
> Puesto que se re no usa la s de control SqlDataSource insertar capacidades, no dude en quitar el `InsertCommand` propiedad y su `InsertParameters` colección.


## <a name="correctly-handlingnullvalues"></a>Para controlar correctamente`NULL`valores

Por desgracia, aumentada `UPDATE` y `DELETE` genera automáticamente las instrucciones del Asistente para configurar origen de datos cuando se utiliza simultaneidad optimista es *no* funcionan con los registros que contienen `NULL` valores. Para ver por qué, considere la posibilidad de nuestro s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

El `UnitPrice` columna en el `Products` tabla puede tener `NULL` valores. Si tiene un registro en particular una `NULL` valor `UnitPrice`, el `WHERE` parte de la cláusula `[UnitPrice] = @original_UnitPrice` le *siempre* se evalúan como False porque `NULL = NULL` siempre devuelve False. Por lo tanto, los registros que contienen `NULL` valores no se pueden editar ni eliminar, como la `UPDATE` y `DELETE` instrucciones `WHERE` cláusulas won t devuelven las filas para actualizar o eliminar.

> [!NOTE]
> Este error se notificó primero a Microsoft en junio de 2004 en [SqlDataSource genera SQL las instrucciones incorrectas](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) y al parecer está programado que se solucionará en la próxima versión de ASP.NET.


Para solucionar este problema, tenemos que actualizar manualmente el `WHERE` cláusulas tanto en el `UpdateCommand` y `DeleteCommand` propiedades de **todos los** columnas que pueden tener `NULL` valores. En general, cambiar `[ColumnName] = @original_ColumnName` para:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Esta modificación se puede realizar directamente a través del marcado declarativo, mediante las opciones UpdateQuery o DeleteQuery desde la ventana Propiedades, o a través de la actualización y eliminar las pestañas en especificar una instrucción SQL personalizada o la opción de procedimiento almacenado en los datos de configurar Asistente de origen. De nuevo, se debe realizar esta modificación para *cada* columna en el `UpdateCommand` y `DeleteCommand` s `WHERE` cláusula que puede contener `NULL` valores.

Cuando se aplica esto a nuestro ejemplo da como resultado en la siguiente modificada `UpdateCommand` y `DeleteCommand` valores:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Paso 2: Agregar un control GridView con Editar y eliminar opciones

Con SqlDataSource configurado para admitir la simultaneidad optimista, todo lo que queda es agregar un control Web de datos a la página que utiliza este control de simultaneidad. Para este tutorial, permiten s Agregar un control GridView que proporciona ambos editar y eliminar funciones. Para ello, arrastre un control GridView en el cuadro de herramientas en el diseñador y el conjunto de sus `ID` a `Products`. Desde la etiqueta inteligente de GridView s, enlácelo al `ProductsDataSourceWithOptimisticConcurrency` control SqlDataSource agregado en el paso 1. Por último, compruebe las opciones Habilitar edición y habilitar la eliminación de la etiqueta inteligente.


[![Enlazar el control GridView a SqlDataSource y habilitar la edición y eliminación](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Figura 6**: enlazar el control GridView SqlDataSource y Habilitar edición y eliminación ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Después de agregar el control GridView, configurar su apariencia mediante la eliminación de la `ProductID` BoundField, cambiar el `ProductName` BoundField s `HeaderText` propiedad al producto y actualice la `UnitPrice` BoundField para que su `HeaderText` propiedad es simplemente precio. Idealmente, se d mejorar la interfaz de edición para incluir un control RequiredFieldValidator para la `ProductName` valor y un control CompareValidator para el `UnitPrice` valor (para asegurarse de que un valor numérico con el formato correcto de s). Hacer referencia a la [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial para obtener información más detallada en Personalizar las operaciones de asignación GridView edición de interfaz.

> [!NOTE]
> GridView debe estar habilitado el estado de vista de s ya que son los valores originales que se pasa desde el control GridView SqlDataSource almacenados en la vista estado.


Después de realizar estas modificaciones a GridView, el marcado declarativo GridView y SqlDataSource debe ser similar al siguiente:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Para ver el control de simultaneidad optimista en acción, abra dos ventanas del explorador y cargar la `OptimisticConcurrency.aspx` página en ambos. Haga clic en los botones de edición para el primer producto de dos exploradores. En un explorador, cambie el nombre del producto y haga clic en actualizar. El explorador se devolución y GridView volverá a su modo de edición previamente, que muestra el nuevo nombre de producto que acaba de modificar el registro.

En la segunda ventana del explorador, cambie el precio (pero deje el nombre del producto como su valor original) y haga clic en actualizar. En la devolución de datos, la cuadrícula se vuelve a su modo de edición previamente, pero no se registra el cambio en el precio. El segundo explorador muestra el mismo valor que la primera de ellas en el nuevo nombre de producto con el precio antiguo. Se perderán los cambios realizados en la segunda ventana del explorador. Además, los cambios se han perdido silenciosamente en su lugar, tal y como se ha producido ninguna excepción o mensaje que indica que acaba de producirse una infracción de simultaneidad.


[![Los cambios en la segunda ventana del explorador se perdieron en modo silencioso](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Figura 7**: los cambios en el segundo explorador ventana se en modo silencioso perdieron ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


La razón por qué no se confirmaron los cambios de explorador s segundo fue que la `UPDATE` instrucción s `WHERE` cláusula filtran todos los registros y, por tanto, no afectó a ninguna fila. Permiten s mirar el `UPDATE` instrucción de nuevo:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Cuando la segunda ventana del explorador actualiza el registro, el nombre original del producto especificado en el `WHERE` cláusula t coincidencia con el nombre de producto existente (desde que se ha cambiado el primer explorador). Por lo tanto, la instrucción `[ProductName] = @original_ProductName` devuelve un valor falso y el `UPDATE` no afecta a ningún registro.

> [!NOTE]
> Eliminar trabajos de la misma manera. Con dos ventanas del explorador abiertas, empiece por edición de un producto determinado con uno y, a continuación, guardar sus cambios. Después de guardar los cambios en un explorador, haga clic en el botón Eliminar para el mismo producto en la otra. Puesto que el original don valores t coincidan en el `DELETE` instrucción s `WHERE` cláusula, la eliminación silenciosa se produce un error.


Desde la perspectiva de s de usuario final en la segunda ventana del explorador, tras hacer clic en el botón de actualización devuelve la cuadrícula para el modo de edición previamente, pero los cambios se han perdido. Sin embargo, hay s ningún indicador visual que pegar su t Professionals de cambios. Lo ideal es que, si un usuario s cambios se pierden una infracción de simultaneidad, se d envíeles una notificación y, quizás, mantener la cuadrícula en modo de edición. Permiten s mire cómo lograr este objetivo.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Paso 3: Determinar cuándo se ha producido una infracción de simultaneidad

Puesto que una infracción de simultaneidad rechaza las modificaciones realizadas por uno, sería bueno avisar al usuario cuando se ha producido una infracción de simultaneidad. Para generar una alerta al usuario, s permiten agregar un control Web Label a la parte superior de la página con el nombre `ConcurrencyViolationMessage` cuyo `Text` propiedad muestra el siguiente mensaje: se ha intentado actualizar o eliminar un registro que se ha actualizado al mismo tiempo por otro usuario. Revisar cambios de otro usuario y, a continuación, la actualización de puesta al día o eliminar. Establecer el control de etiqueta s `CssClass` propiedad a Warning, que es una clase CSS definida en `Styles.css` que muestra texto en una fuente de color rojo, cursiva, negrita y grande. Por último, establezca la etiqueta s `Visible` y `EnableViewState` propiedades para `false`. Este modo ocultará la etiqueta excepto solo esas devoluciones de datos donde se establece explícitamente su `Visible` propiedad `true`.


[![Agregar un Control de etiqueta a la página para mostrar la advertencia](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Figura 8**: agregar un Control de etiqueta a la página para mostrar la advertencia ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Al realizar una actualización o eliminación, las operaciones de asignación GridView `RowUpdated` y `RowDeleted` controladores de eventos se activan después de su control de origen de datos ha realizado la actualización solicitada o delete. Podemos determinar el número de filas afectado por la operación de estos controladores de eventos. Si se vieron afectados cero filas, queremos mostrar el `ConcurrencyViolationMessage` etiqueta.

Cree un controlador de eventos para ambos el `RowUpdated` y `RowDeleted` eventos y agregue el código siguiente:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

En ambos controladores de eventos comprobamos la `e.AffectedRows` propiedad y, si es igual a 0, establezca el `ConcurrencyViolationMessage` etiqueta s `Visible` propiedad `true`. En el `RowUpdated` controlador de eventos, se ordena también GridView debe permanecer en modo de edición estableciendo la `KeepInEditMode` propiedad en true. Si lo hace, es necesario volver a enlazar los datos a la cuadrícula para que los demás datos de usuario s se cargan en la interfaz de edición. Esto se logra mediante una llamada a las operaciones de asignación GridView `DataBind()` método.

Como se muestra en la figura 9, estos dos controladores de eventos, se muestra un mensaje muy apreciable cuando se produce una infracción de simultaneidad.


[![Se muestra un mensaje en el caso de una infracción de simultaneidad](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Figura 9**: se muestra un mensaje en el caso de una infracción de simultaneidad ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Resumen

Al crear una aplicación web donde simultánea de varios usuarios pueden modificar los mismos datos, es importante tener en cuenta las opciones de control de simultaneidad. De forma predeterminada, los controles Web de datos ASP.NET y controles de origen de datos no emplear cualquier control de simultaneidad. Como hemos visto en este tutorial, implementar el control de simultaneidad optimista con SqlDataSource es relativamente rápido y sencillo. SqlDataSource controla la mayoría del trabajo duro para aumentar la adición de `WHERE` cláusulas a la autogenerado `UPDATE` y `DELETE` pero las instrucciones son unos matices en el control de `NULL` valor columnas, como se describe en el Para controlar correctamente `NULL` valores de sección.

Este tutorial concluye el examen de SqlDataSource. El resto de los tutoriales volverá a trabajar con datos mediante la ObjectDataSource y la arquitectura en capas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
[Siguiente](querying-data-with-the-sqldatasource-control-vb.md)
