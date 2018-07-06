---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Lote de eliminación (VB) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo eliminar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se parten un mejorados de GridView creado en una sesión tut...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: c7a0db8708c95cc348619cadd21514f4bea2e553
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815994"
---
<a name="batch-deleting-vb"></a>Lote de eliminación (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) o [descargar PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Obtenga información sobre cómo eliminar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, se basan en un mejorados de GridView creado en un tutorial anterior. En la capa de acceso a datos se encapsulan las varias operaciones de eliminación en una transacción para asegurarse de que todas las eliminaciones se realizan correctamente o se revierten todas las eliminaciones.


## <a name="introduction"></a>Introducción

El [tutorial anterior](batch-updating-vb.md) explora cómo crear un lote mediante un GridView editable totalmente de interfaz de edición. En situaciones donde los usuarios normalmente edita muchos registros al mismo tiempo, un lote en la interfaz de edición requerirá muchas menos devoluciones de datos y el contexto de teclado para mouse conmutadores, lo que mejora la eficiencia de s del usuario final. Esta técnica es útil de forma similar para las páginas donde es habitual que los usuarios puedan eliminar muchos registros en una sola operación.

Cualquiera que haya usado un cliente de correo electrónico en línea ya está familiarizado con uno del lote más comunes de eliminación de interfaces: botón de una casilla de verificación de cada fila en una cuadrícula con un correspondiente eliminar todos los elementos seleccionados (consulte la figura 1). Este tutorial es bastante corta porque se ve que se ha hecho todo el trabajo duro en los tutoriales anteriores en la creación de la interfaz basada en web y un método para eliminar una serie de registros como una única operación atómica. En el [agregar una columna GridView de casillas de verificación](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, creamos un GridView con una columna de casillas de verificación y, en el [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) creamos un método en el tutorial la capa BLL que usaría una transacción para eliminar un `List<T>` de `ProductID` valores. En este tutorial, se compilará tras y combinar nuestras experiencias para crear un lote de trabajo al eliminar el ejemplo anteriores.


[![Cada fila incluye una casilla de verificación](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: cada fila incluye una casilla de verificación ([haga clic aquí para ver imagen en tamaño completo](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Paso 1: Crear el lote de eliminación de interfaz

Dado que ya creado el lote de eliminación de la interfaz en el [agregar una columna GridView de casillas de verificación](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, nos podemos simplemente cópielo en `BatchDelete.aspx` en lugar de crearla desde cero. Comience abriendo la `BatchDelete.aspx` página en el `BatchData` carpeta y el `CheckBoxField.aspx` página en el `EnhancedGridView` carpeta. Desde el `CheckBoxField.aspx` página, vaya a la vista del origen y copie el marcado entre la `<asp:Content>` etiquetas como se muestra en la figura 2.


[![Copie el marcado declarativo de CheckBoxField.aspx en el Portapapeles](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: copiar el marcado declarativo de `CheckBoxField.aspx` en el Portapapeles ([haga clic aquí para ver imagen en tamaño completo](batch-deleting-vb/_static/image4.png))


A continuación, vaya a la vista del origen en `BatchDelete.aspx` y pegue el contenido del Portapapeles en el `<asp:Content>` etiquetas. También copiar y pegar el código desde dentro de la clase de código subyacente en `CheckBoxField.aspx.vb` a dentro de la clase de código subyacente en `BatchDelete.aspx.vb` (el `DeleteSelectedProducts` botón s `Click` controlador de eventos, el `ToggleCheckState` método y el `Click` controladores de eventos para el `CheckAll` y `UncheckAll` botones). Después de copiar a través de este contenido, el `BatchDelete.aspx` clase de código subyacente de la página s debe contener el código siguiente:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Después de copiar en el marcado declarativo y el código fuente, dedique un momento para probar `BatchDelete.aspx` viendo a través de un explorador. Debería ver un lista de los diez primeros productos en un control GridView con cada fila de mostrar el nombre de producto s, la categoría y el precio, junto con una casilla de verificación de GridView. Debe haber tres botones: comprobar todos los, desactive todas y eliminar productos seleccionados. Al hacer clic en el botón Comprobar todo selecciona todas las casillas, aunque desactive todos los borra todas las casillas. Al hacer clic en eliminar los productos seleccionados se muestra un mensaje que se enumera los `ProductID` los valores de los productos seleccionados, pero no elimina realmente los productos.


[![La interfaz de CheckBoxField.aspx se ha movido a BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: la interfaz de `CheckBoxField.aspx` se ha movido a `BatchDeleting.aspx` ([haga clic aquí para ver imagen en tamaño completo](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Paso 2: Eliminación de los productos activados mediante transacciones

En el lote de eliminación de la interfaz que se copian correctamente en `BatchDeleting.aspx`, todos los que queda es actualizar el código para que el botón Eliminar productos seleccionados elimina los productos activados mediante el `DeleteProductsWithTransaction` método en el `ProductsBLL` clase. Este método, agregada en el [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, se acepta como entrada un `List(Of T)` de `ProductID` valores y la elimina cada uno correspondiente `ProductID` dentro del ámbito de un transacción.

El `DeleteSelectedProducts` botón s `Click` controlador de eventos utiliza actualmente lo `For Each` bucle para recorrer en iteración cada fila GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Para cada fila, el `ProductSelector` control CheckBox Web mediante programación se hace referencia. Si está activada, la fila s `ProductID` se recupera de la `DataKeys` colección y la `DeleteResults` etiqueta s `Text` propiedad se actualiza para incluir un mensaje que indica que la fila se ha seleccionado para su eliminación.

El código anterior no elimina realmente los registros como la llamada a la `ProductsBLL` clase s `Delete` método está marcada como comentario. Quisiera que se aplica esta lógica de eliminación, el código eliminaría los productos, pero no dentro de una operación atómica. Es decir, si se ha realizado correctamente en el primer elimina algunos en la secuencia, pero no se pudo uno posterior (quizás debido a una infracción de restricción de clave externa), se iniciaría una excepción pero eliminados permanecerán aquellos productos que ya se ha eliminado.

Para garantizar la atomicidad, es necesario usar en su lugar el `ProductsBLL` clase s `DeleteProductsWithTransaction` método. Dado que este método acepta una lista de `ProductID` valores, es necesario compilar primero esta lista de la cuadrícula y, a continuación, páselo como un parámetro. Primero creamos una instancia de un `List(Of T)` de tipo `Integer`. Dentro de la `For Each` bucle tenemos que agregar los productos seleccionados `ProductID` valores a este `List(Of T)`. Después del bucle esto `List(Of T)` debe pasarse a la `ProductsBLL` clase s `DeleteProductsWithTransaction` método. Actualización de la `DeleteSelectedProducts` botón s `Click` controlador de eventos con el código siguiente:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

El código actualizado se crea un `List(Of T)` de tipo `Integer` (`productIDsToDelete`) y lo rellena con el `ProductID` para eliminar los valores. Después de la `For Each` bucle, si hay al menos un producto seleccionado, el `ProductsBLL` clase s `DeleteProductsWithTransaction` método se llama y pasa esta lista. El `DeleteResults` también se muestra la etiqueta y los datos vuelve a enlazar el control GridView (de modo que los registros recién eliminados ya no aparecen como filas en la cuadrícula).

Figura 4 muestra el control GridView después de un número de filas se han seleccionado para su eliminación. Figura 5 muestra la pantalla después de que se ha hecho clic el botón Eliminar productos seleccionados. Tenga en cuenta que en la figura 5 la `ProductID` valores de los registros eliminados se muestran en la etiqueta debajo del control GridView y las filas ya no están en GridView.


[![Los productos seleccionados se eliminarán.](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: The seleccionado productos se eliminará ([haga clic aquí para ver imagen en tamaño completo](batch-deleting-vb/_static/image8.png))


[![Los valores de ProductID de productos eliminados son aparece debajo de GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: The productos eliminados `ProductID` los valores son aparece debajo de GridView ([haga clic aquí para ver imagen en tamaño completo](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> Para probar la `DeleteProductsWithTransaction` atomicidad método s, agregue manualmente una entrada para un producto en el `Order Details` de tabla y, a continuación, intenta eliminar ese producto (junto con otros usuarios). Recibirá una infracción de restricción de clave externa cuando se intenta eliminar el producto con un pedido asociados, pero tenga en cuenta cómo se revierten las eliminaciones de otros productos seleccionados.


## <a name="summary"></a>Resumen

Creación de un lote de eliminación de la interfaz implica agregar un control GridView con una columna de casillas de verificación y un botón de control Web de que, al hacer clic en, se eliminarán todas las filas seleccionadas como una única operación atómica. En este tutorial, hemos creado esta interfaz reuniendo trabajo realizado en los dos tutoriales anteriores, [agregar una columna GridView de casillas de verificación](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) y [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md). En el primer tutorial, creamos un GridView con una columna de casillas de verificación y en el último se implementa un método en el nivel de lógica empresarial que, cuando se pasa un `List(Of T)` de `ProductID` valores, eliminarlos todos dentro del ámbito de una transacción.

En el siguiente tutorial, vamos a crear una interfaz para realizar inserciones por lotes.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Hilton Giesenow y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-updating-vb.md)
> [Siguiente](batch-inserting-vb.md)
