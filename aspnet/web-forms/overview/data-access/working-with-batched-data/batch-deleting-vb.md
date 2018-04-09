---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Lote eliminar (VB) | Documentos de Microsoft
author: rick-anderson
description: Obtenga información acerca de cómo eliminar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se parten un GridView mejorada creado en una anterior tut...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d17ec6ec20be65b3ec9369f1c5a08d5970ec0dd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="batch-deleting-vb"></a>Procesar por lotes eliminando (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) o [descarga de PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Obtenga información acerca de cómo eliminar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se se basan en un GridView mejorada creado en un tutorial anterior. En la capa de acceso a datos se encapsulan las varias operaciones de eliminación en una transacción para asegurarse de que todas las eliminaciones correctamente o se revierten todas las eliminaciones.


## <a name="introduction"></a>Introducción

El [tutorial anterior](batch-updating-vb.md) explorar cómo crear un lote edición interfaz mediante un GridView totalmente editables. En situaciones donde los usuarios son normalmente edición muchos registros a la vez, será necesario un lote en la interfaz de edición muchas menos devoluciones de datos y el contexto de teclado y mouse conmutadores, lo que mejora la eficacia de s de usuario final. Esta técnica es útil del mismo modo para páginas donde es habitual que los usuarios puedan eliminar muchos registros en una sola operación.

Cualquier persona que ha utilizado un cliente de correo electrónico en línea ya está familiarizado con uno del lote más comunes al eliminar interfaces: una casilla de verificación de cada fila en una cuadrícula con una correspondiente eliminar todos los elementos marcados botón (consulte la figura 1). Este tutorial es corto en su lugar porque se ve que se ha hecho todo el disco duro trabajo en los tutoriales anteriores para crear la interfaz basada en web y un método para eliminar una serie de registros como una única operación atómica. En el [agregar una columna de GridView de casillas de verificación](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, creamos un GridView con una columna de casillas de verificación y, en la [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) creamos un método en el tutorial la capa BLL que utilizaría una transacción para eliminar un `List<T>` de `ProductID` valores. En este tutorial, agregaremos parten y mezcla nuestras experiencias para crear un lote de trabajo al eliminar el ejemplo anteriores.


[![Cada fila incluye una casilla de verificación](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: cada fila incluye una casilla de verificación ([haga clic aquí para ver la imagen a tamaño completo](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Paso 1: Crear el lote de eliminación de interfaz

Puesto que ya se creó el lote al eliminar la interfaz en el [agregar una columna de GridView de casillas de verificación](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, podemos simplemente copiaremos a `BatchDelete.aspx` en lugar de crear desde cero. Comience abriendo la `BatchDelete.aspx` página en el `BatchData` carpeta y el `CheckBoxField.aspx` página en el `EnhancedGridView` carpeta. Desde el `CheckBoxField.aspx` página, vaya a la vista del origen y copie el marcado entre la `<asp:Content>` etiquetas tal y como se muestra en la figura 2.


[![Copie el marcado declarativo de CheckBoxField.aspx en el Portapapeles](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: copie el marcado declarativo de `CheckBoxField.aspx` en el Portapapeles ([haga clic aquí para ver la imagen a tamaño completo](batch-deleting-vb/_static/image4.png))


A continuación, vaya a la vista del origen de `BatchDelete.aspx` y pegue el contenido del Portapapeles en el `<asp:Content>` etiquetas. Copiar y pegar el código desde dentro de la clase de código subyacente en `CheckBoxField.aspx.vb` a dentro de la clase de código subyacente en `BatchDelete.aspx.vb` (la `DeleteSelectedProducts` botón s `Click` controlador de eventos, el `ToggleCheckState` método y el `Click` controladores de eventos para el `CheckAll` y `UncheckAll` botones). Después de copiar a través de este contenido, la `BatchDelete.aspx` clase de código subyacente de la página s debería contener el código siguiente:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Una vez copiado en el marcado declarativo y el código fuente, tómese un momento para probar `BatchDelete.aspx` viendo a través de un explorador. Debería ver una lista de los diez primeros productos en un GridView con cada fila Mostrar el nombre de producto s, la categoría y el precio junto con una casilla de verificación de GridView. Debería haber tres botones: compruebe todos los, desactive todas las y eliminar los productos seleccionados. Haga clic en el botón Comprobar todo selecciona todas las casillas, aunque desactive todos los borra todas las casillas. Haga clic en eliminar los productos seleccionados se muestra un mensaje que enumera los `ProductID` valores de los productos seleccionados, pero no se eliminan realmente los productos.


[![La interfaz de CheckBoxField.aspx se ha movido a BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: la interfaz de `CheckBoxField.aspx` se ha movido a `BatchDeleting.aspx` ([haga clic aquí para ver la imagen a tamaño completo](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Paso 2: Eliminar los productos activados mediante transacciones

Con el lote eliminar interfaz copiado correctamente a `BatchDeleting.aspx`, todos los que queda es actualizar el código para que el botón Eliminar productos seleccionado elimina los productos activados mediante el `DeleteProductsWithTransaction` método en la `ProductsBLL` clase. Este método, que se agregó en el [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, acepta como entrada un `List(Of T)` de `ProductID` valores y las eliminaciones correspondiente `ProductID` dentro del ámbito de un transacción.

El `DeleteSelectedProducts` botón s `Click` controlador de eventos utiliza actualmente lo siguiente `For Each` bucle para recorrer en iteración cada fila de GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Para cada fila, el `ProductSelector` control de casilla de verificación Web mediante programación se hace referencia. Si está activada, la fila s `ProductID` se recupera de la `DataKeys` colección y el `DeleteResults` etiqueta s `Text` propiedad se actualiza para incluir un mensaje que indica que la fila se ha seleccionado para su eliminación.

El código anterior no elimina realmente los registros como la llamada a la `ProductsBLL` clase s `Delete` se hace referencia a método. Quisiera que se aplica esta lógica de eliminación, el código eliminaría los productos, pero no dentro de una operación atómica. Es decir, si las eliminaciones de pocos primeras en la secuencia se realizó correctamente, pero no se pudo uno posterior (quizás debido a una infracción de restricción de clave externa), se produciría una excepción, pero esos productos ya eliminados se mantendría eliminados.

Con el fin de garantizar la atomicidad, es necesario usar en su lugar el `ProductsBLL` clase s. `DeleteProductsWithTransaction` método. Dado que este método acepta una lista de `ProductID` valores, es necesario compilar en primer lugar esta lista en la cuadrícula y, a continuación, se pasa como parámetro. Primero creamos una instancia de un `List(Of T)` de tipo `Integer`. En el `For Each` bucle tenemos que agregar los productos seleccionados `ProductID` valores a esta `List(Of T)`. Después del bucle esto `List(Of T)` debe pasarse a la `ProductsBLL` clase s. `DeleteProductsWithTransaction` método. Actualización de la `DeleteSelectedProducts` botón s `Click` controlador de eventos con el código siguiente:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

El código actualizado se crea un `List(Of T)` de tipo `Integer` (`productIDsToDelete`) y lo rellena con la `ProductID` valores para eliminar. Después de la `For Each` de bucle, si hay al menos un producto seleccionado, el `ProductsBLL` clase s. `DeleteProductsWithTransaction` método se llama y pasa esta lista. El `DeleteResults` también se muestra la etiqueta y los datos vuelve a enlazar a GridView (de modo que los registros recién eliminado ya no aparecen como filas en la cuadrícula).

La figura 4 muestra GridView después de haber seleccionado un número de filas para su eliminación. Figura 5 se muestra la pantalla inmediatamente después de que se ha hecho clic el botón Eliminar los productos seleccionados. Tenga en cuenta que en la figura 5 la `ProductID` valores de los registros eliminados se muestran en la etiqueta situada debajo de GridView y las filas ya no están en el control GridView.


[![Los productos seleccionados se eliminarán.](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: The seleccionado productos se eliminarán ([haga clic aquí para ver la imagen a tamaño completo](batch-deleting-vb/_static/image8.png))


[![Los valores de ProductID de productos eliminar son aparecen bajo la GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: los productos eliminado `ProductID` valores son aparecen bajo la GridView ([haga clic aquí para ver la imagen a tamaño completo](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> Para probar el `DeleteProductsWithTransaction` atomicidad de método s, agregar manualmente una entrada para un producto en el `Order Details` de tabla y, a continuación, intenta eliminar ese producto (junto con otras cosas). Recibirá una infracción de restricción de clave externa al intentar eliminar el producto con un orden asociado, pero tenga en cuenta cómo se revierten las eliminaciones de otros productos seleccionados.


## <a name="summary"></a>Resumen

Crear un lote eliminar interfaz implica agregar un control GridView con una columna de casillas de verificación y un botón de control Web de que, al hacer clic en, se eliminarán todas las filas seleccionadas como una única operación atómica. En este tutorial, creamos dicha interfaz reuniendo trabajo realizado en los dos tutoriales anteriores, [agregar una columna de GridView de casillas de verificación](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) y [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md). En el primer tutorial, creamos un GridView con una columna de casillas de verificación y en el último se implementa un método en la capa BLL que, cuando se pasa un `List(Of T)` de `ProductID` valores, los elimina todos los dentro del ámbito de una transacción.

En el siguiente tutorial vamos a crear una interfaz para realizar inserciones por lotes.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Hilton Giesenow y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-updating-vb.md)
> [Siguiente](batch-inserting-vb.md)
