---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Mostrar información de resumen en el pie de página de GridView (VB) | Microsoft Docs
author: rick-anderson
description: A menudo se muestra información resumida en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas podemos pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 4299cf7dc684c1080600789a91349011e495d391
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400644"
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Mostrar información de resumen en el pie de página de GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) o [descargar PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> A menudo se muestra información resumida en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas nos podemos inyectar datos agregados mediante programación. En este tutorial, veremos cómo mostrar datos agregados en esta fila de pie de página.


## <a name="introduction"></a>Introducción

Además de ver cada uno de los precios de los productos, las unidades en existencias, las unidades en orden y los niveles de reordenación, un usuario también es posible que esté interesado en la información de agregado, como el precio medio, el número total de unidades en existencias y así sucesivamente. A menudo se muestra dicha información de resumen en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas nos podemos inyectar datos agregados mediante programación.

Esta tarea presenta tres retos:

1. Configuración del control GridView para mostrar su fila de pie de página
2. Determinación de los datos de resumen ¿es decir, cómo se calcula el precio medio o el total de las unidades en existencias?
3. Inserta los datos de resumen en las celdas de la fila de pie de página adecuadas

En este tutorial, veremos cómo superar estos desafíos. En concreto, vamos a crear una página que enumera las categorías en una lista desplegable con los productos de la categoría seleccionada que se muestran en un control GridView. El control GridView incluirá una fila de pie de página que se muestra el precio medio y el número total de unidades en existencias y en orden para los productos de esa categoría.


[![Información de resumen se muestra en la fila de pie de página de GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: información de resumen se muestra en la fila de pie de página de GridView ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


En este tutorial, con su categoría a la interfaz de maestro y detalles de productos, se basa en los conceptos descritos anteriormente [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial. Si aún no ha trabajado el tutorial anterior, hágalo antes de continuar con ésta.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Paso 1: Agregar la DropDownList categorías y GridView de productos

Antes de que se refieren nosotros mismos con la incorporación de información de resumen al pie de página de GridView, vamos a simplemente cree primero el informe de maestro y detalles. Una vez que hemos completado este primer paso, analizaremos cómo incluir datos de resumen.

Comience abriendo la `SummaryDataInFooter.aspx` página en el `CustomFormatting` carpeta. Agregue un control DropDownList y establezca su `ID` a `Categories`. A continuación, haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList y optar por agregar un nuevo origen ObjectDataSource denominado `CategoriesDataSource` que invoca la `CategoriesBLL` la clase `GetCategories()` método.


[![Agregar un nuevo origen ObjectDataSource denominado CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Figura 2**: agregar un nuevo origen ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![Tiene el origen ObjectDataSource invocar el método de la clase CategoriesBLL GetCategories()](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Figura 3**: que el ObjectDataSource invoque la `CategoriesBLL` la clase `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


Después de configurar el origen ObjectDataSource, el asistente devuelve nos de la configuración de DropDownList de origen de datos se debe mostrar el asistente desde el que se debe especificar qué valor de campo de datos y cuál debe corresponder al valor de la DropDownList `ListItem` s. Tiene la `CategoryName` campo mostrado y use el `CategoryID` como valor.


[![Utilice los campos de CategoryID y CategoryName como el texto y el valor para el ListItems, respectivamente](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Figura 4**: Use el `CategoryName` y `CategoryID` campos como el `Text` y `Value` para el `ListItem` s, respectivamente ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


En este momento tenemos DropDownList (`Categories`) que enumeran las categorías en el sistema. Ahora tenemos que agregar un control GridView que se enumera los productos que pertenecen a la categoría seleccionada. Antes de hacer, sin embargo, dedique un momento para activar la casilla Habilitar AutoPostBack en la etiqueta inteligente de DropDownList. Como se describe en el *filtrado de maestro y detalles con un DropDownList* tutorial estableciendo la DropDownList `AutoPostBack` propiedad `True` la página, se publicarán nuevo cada vez que cambia el valor de DropDownList. Esto hará que el control GridView se actualice, que muestra los productos de la categoría recién seleccionada. Si el `AutoPostBack` propiedad está establecida en `False` (valor predeterminado), cambiar la categoría no causará una devolución de datos y, por tanto, no se actualizan los productos enumerados.


[![Active la casilla de AutoPostBack habilitar en la etiqueta inteligente de DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Figura 5**: Active la casilla de AutoPostBack habilitar en la etiqueta inteligente de DropDownList ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Agregar un control GridView a la página para mostrar los productos de la categoría seleccionada. Establezca la GridView `ID` a `ProductsInCategory` y enlazarlo a un nuevo origen ObjectDataSource denominado `ProductsInCategoryDataSource`.


[![Agregar un nuevo origen ObjectDataSource denominado ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Figura 6**: agregar un nuevo origen ObjectDataSource denominado `ProductsInCategoryDataSource` ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Configurar el origen ObjectDataSource para que invoca el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método.


[![Tiene el origen ObjectDataSource invoca el método GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Figura 7**: que el ObjectDataSource invoque la `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Puesto que el `GetProductsByCategoryID(categoryID)` método toma un parámetro de entrada, podemos especificar el origen del valor del parámetro en el paso final del asistente. Con el fin de mostrar los productos de la categoría seleccionada, tienen un parámetro que se extraen de la `Categories` DropDownList.


[![Obtener el valor del parámetro categoryID de DropDownList categorías seleccionadas](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Figura 8**: obtener el *`categoryID`* valor del parámetro de la DropDownList de categorías seleccionado ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


Después de completar al Asistente para el control GridView tendrá BoundField para cada una de las propiedades del producto. Vamos a limpiar estos BoundFields para que solo el `ProductName`, `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` BoundFields se muestran. Puede agregar cualquier configuración de nivel de campo para los restantes BoundFields (como el formato del `UnitPrice` como una moneda). Después de realizar estos cambios, el marcado declarativo de GridView debe ser similar al siguiente:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

En este momento tenemos un informe de maestro y detalles totalmente operativa que muestra el nombre, precio unitario, las unidades en existencias y unidades pedidas para aquellos productos que pertenecen a la categoría seleccionada.


[![Obtener el valor del parámetro categoryID de DropDownList categorías seleccionadas](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Figura 9**: obtener el *`categoryID`* valor del parámetro de la DropDownList de categorías seleccionado ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Paso 2: Mostrar un pie de página en el control GridView

El control GridView puede mostrar un encabezado y pie de página de fila. Estas filas se muestran según los valores de la `ShowHeader` y `ShowFooter` propiedades, respectivamente, con `ShowHeader` ejecuciones `True` y `ShowFooter` a `False`. Para incluir un pie de página en el control GridView simplemente establece su `ShowFooter` propiedad `True`.


[![Establezca ShowFooter propiedad de GridView en True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Figura 10**: establecer la GridView `ShowFooter` propiedad `True` ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


La fila de pie de página tiene una celda para cada uno de los campos definidos en el control GridView; Sin embargo, estas celdas están vacías de forma predeterminada. Dedique un momento para ver nuestro progreso en un explorador. Con el `ShowFooter` propiedad se establece ahora en `True`, el control GridView incluye una fila de pie de página vacía.


[![GridView ahora incluye una fila de pie de página](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Figura 11**: GridView ahora incluye una fila de pie de página ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


La fila de pie de página en la figura 11 no cabe destacar, ya que tiene un fondo blanco. Vamos a crear un `FooterStyle` clase CSS `Styles.css` que especifica un fondo rojo oscuro y, a continuación, configure el `GridView.skin` skinu en el `DataWebControls` tema para asignar esta clase CSS a la GridView `FooterStyle`del `CssClass` propiedad. Si necesita perfeccionar sus conocimientos sobre las máscaras y temas, vuelva a consultar el [mostrar datos con el origen ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial.

Empiece agregando la siguiente clase CSS a `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

El `FooterStyle` es similar de la clase CSS el `HeaderStyle` clase, aunque la `HeaderStyle`del color de fondo es sutileza más oscuro y su texto se muestra en negrita. Además, el texto del pie de página está alineado a la derecha, mientras que el texto del encabezado está centrado.

A continuación, para asociar esta clase CSS a pie de página de cada GridView, abra el `GridView.skin` de archivos en el `DataWebControls` tema y establezca el `FooterStyle`del `CssClass` propiedad. Después de esta suma marcado del archivo debe ser similar:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

A medida que la captura de pantalla siguiente se muestra, este cambio hace que el pie de página mayor claridad.


[![Fila de pie de página de GridView ahora tiene un Color de fondo rojizo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Figura 12**: fila de pie de página de GridView ahora tiene un Color de fondo rojizo ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Paso 3: Cálculo de los datos de resumen

Con el pie de página de GridView muestra, el siguiente desafío que nos encontramos es cómo calcular los datos de resumen. Hay dos maneras de agregar esta información de proceso:

1. A través de una consulta SQL se podríamos emite una consulta adicional a la base de datos para calcular los datos de resumen para una categoría determinada. SQL incluye una serie de funciones de agregado junto con un `GROUP BY` cláusula para especificar los datos sobre el que se deben resumir los datos. La siguiente consulta SQL podría devolver la información necesaria:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Por supuesto que no desea emitir esta consulta directamente desde el `SummaryDataInFooter.aspx` página, sino mediante la creación de un método en el `ProductsTableAdapter` y `ProductsBLL`.
2. Esta información de proceso, tal como se agrega a la GridView como se describe en [formato basado en datos personalizados](custom-formatting-based-upon-data-cs.md) tutorial, el control GridView `RowDataBound` controlador de eventos se activa una vez para cada fila que se agrega a la GridView después de su estado enlace de datos. Mediante la creación de un controlador de eventos para este evento se pueda mantener un ejecución total de los valores que queremos a agregar. Después de la última fila de datos se ha enlazado a la GridView tenemos los totales y la información necesaria para calcular el promedio.

El segundo enfoque emplea por lo general como ahorra un viaje a la base de datos y el esfuerzo necesario para implementar la funcionalidad de resumen en la capa de acceso a datos y la capa de lógica empresarial, pero cualquier enfoque sería suficiente. Para este tutorial vamos a usar la segunda opción y realizar un seguimiento de la total utilizando el `RowDataBound` controlador de eventos.

Crear un `RowDataBound` controlador de eventos para el control GridView seleccionando GridView en el diseñador, al hacer clic en el icono de rayo desde la ventana Propiedades, y haga doble clic en el `RowDataBound` eventos. Como alternativa, puede seleccionar el control GridView y su evento RowDataBound desde las listas desplegables en la parte superior del archivo de clase de código subyacente ASP.NET. Esto creará un nuevo controlador de eventos denominado `ProductsInCategory_RowDataBound` en el `SummaryDataInFooter.aspx` clase de código subyacente de la página.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Para mantener un total actualizado es necesario definir variables fuera del ámbito del controlador de eventos. Cree las siguientes cuatro variables de nivel de página:

- `_totalUnitPrice`, de tipo `Decimal`
- `_totalNonNullUnitPriceCount`, de tipo `Integer`
- `_totalUnitsInStock`, de tipo `Integer`
- `_totalUnitsOnOrder`, de tipo `Integer`

A continuación, escriba el código para incrementar estas tres variables para cada fila de datos se encuentra en la `RowDataBound` controlador de eventos.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

El `RowDataBound` se inicia el controlador de eventos al asegurar que estamos trabajando con un objeto DataRow. Una vez que se ha establecido, el `Northwind.ProductsRow` instancia que simplemente se enlazó a la `GridViewRow` objeto `e.Row` se almacena en la variable `product`. A continuación, ejecución variables total se incrementan en los valores correspondientes del producto actual (suponiendo que no contienen una base de datos `NULL` valor). Realizar el seguimiento de la ejecución de ambas `UnitPrice` total y el número de que no sean de`NULL` `UnitPrice` registros porque el precio medio es el cociente de estos dos números.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Paso 4: Mostrar los datos de resumen en el pie de página

Con los datos de resumen se suman, el último paso es que se muestre en la fila de pie de página de GridView. Esta tarea, también puede realizarse mediante programación a través del `RowDataBound` controlador de eventos. Recuerde que el `RowDataBound` se activa el controlador de eventos para *cada* fila que está enlazado el control GridView, incluida la fila de pie de página. Por lo tanto, nos podemos aumentar nuestro controlador de eventos para mostrar los datos en la fila de pie de página con el código siguiente:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Puesto que la fila de pie de página se agrega a la GridView después de han agregado todas las filas de datos, que podemos estar seguros de que en el momento ya estamos listos para mostrar los datos de resumen en el pie de página que se haya completado los cálculos totales. A continuación, es el último paso establecer estos valores en las celdas del pie de página.

Para mostrar texto en una celda de pie de página concreto, utilice `e.Row.Cells(index).Text = value`, donde el `Cells` indización comienza en 0. El código siguiente calcula el precio medio (el precio total dividido por el número de productos) y lo muestra junto con el número total de unidades en existencias y unidades en el orden de las celdas de pie de página adecuado del control GridView.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Figura 13 se muestra el informe después de agregar este código. Tenga en cuenta cómo el `ToString("c")` hace que la información de resumen de precio medio tener el formato como una moneda.


[![Fila de pie de página de GridView ahora tiene un Color de fondo rojizo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Figura 13**: fila de pie de página de GridView ahora tiene un Color de fondo rojizo ([haga clic aquí para ver imagen en tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Resumen

Mostrar datos de resumen es un requisito común de informes y el control GridView facilita la tarea debe incluir dicha información en su fila de pie de página. La fila de pie de página se muestra cuando la GridView `ShowFooter` propiedad está establecida en `True` y puede tener el texto en sus celdas establecer mediante programación a través del `RowDataBound` controlador de eventos. Informática de los datos de resumen bien es posible al volver a consultar la base de datos o mediante código de clase de código subyacente de la página ASP.NET para calcular mediante programación los datos de resumen.

En este tutorial concluye nuestro examen de formato personalizado con los controles GridView, DetailsView y FormView. Nuestro siguiente tutorial se inicia la exploración de insertar, actualizar y eliminar datos mediante estos mismos controles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-the-formview-s-templates-vb.md)
