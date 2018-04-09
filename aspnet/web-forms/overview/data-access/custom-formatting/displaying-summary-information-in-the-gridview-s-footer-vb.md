---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Mostrar información de resumen en el pie de página de GridView (VB) | Documentos de Microsoft
author: rick-anderson
description: A menudo se muestra información de resumen en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas podemos pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: d9a1a3f3c680f367395f984254da6cdcdd3c08d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Mostrar información de resumen en el pie de página de GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) o [descarga de PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> A menudo se muestra información de resumen en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas se pueden insertar mediante programación datos agregados. En este tutorial veremos cómo mostrar datos agregados en esta fila de pie de página.


## <a name="introduction"></a>Introducción

Además de ver cada uno de los precios de los productos, las unidades en existencias, unidades en orden y los niveles de nuevo pedido, un usuario también podría estar interesado en la información de agregado, como el precio medio, el número total de unidades en existencia y así sucesivamente. Esta información de resumen se suele mostrar en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas se pueden insertar mediante programación datos agregados.

Esta tarea nos presenta con tres retos:

1. Configuración del control GridView para mostrar la fila de pie de página
2. Determinar los datos de resumen; ¿es decir, cómo se calcula el precio medio ni el total de las unidades en existencias?
3. Insertar los datos de resumen en las celdas apropiadas de la fila de pie de página

En este tutorial veremos cómo superar estos desafíos. En concreto, vamos a crear una página que enumera las categorías en una lista desplegable con los productos de la categoría seleccionada que se muestran en un control GridView. GridView incluirá una fila de pie de página que muestra el precio medio y el número total de unidades en existencia y en orden para los productos de esa categoría.


[![Información de resumen se muestra en la fila de pie de página de GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: información de resumen se muestra en la fila de pie de página de GridView ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


Este tutorial, con su categoría a la interfaz de maestro y detalles de productos, se basa en los conceptos descritos en la anterior [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial. Si aún no ha trabajado el tutorial anterior, hágalo antes de continuar con ésta.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Paso 1: Agregar las categorías de DropDownList y productos GridView

Antes de relativas a nosotros mismos con la adición de información de resumen al pie de página de GridView, vamos a simplemente compilar primero el informe maestro y detalles. Una vez que hemos completado este primer paso, analizaremos cómo incluir datos de resumen.

Comience abriendo la `SummaryDataInFooter.aspx` página en el `CustomFormatting` carpeta. Agregue un control DropDownList y establezca su `ID` a `Categories`. A continuación, haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList y optar por agregar una nueva ObjectDataSource denominado `CategoriesDataSource` que invoca la `CategoriesBLL` la clase `GetCategories()` método.


[![Agregar un nuevo origen ObjectDataSource denominado CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Figura 2**: agregue un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![Tiene el ObjectDataSource Invoke (método) de la clase CategoriesBLL GetCategories()](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Figura 3**: tiene la invocación de ObjectDataSource el `CategoriesBLL` la clase `GetCategories()` método ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


Después de configurar el ObjectDataSource, el asistente volverá nos configuración del DropDownList de origen de datos se debe mostrar el asistente desde el que es preciso especificar qué valor de campo de datos y cuál debe corresponder al valor de la DropDownList `ListItem` s. Tiene la `CategoryName` campo que se muestra y use la `CategoryID` como el valor.


[![Utilice los campos de CategoryID y CategoryName como el texto y el valor para el ListItems, respectivamente](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Figura 4**: Use la `CategoryName` y `CategoryID` campos como el `Text` y `Value` para el `ListItem` s, respectivamente ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


En este momento tenemos una DropDownList (`Categories`) que enumeran las categorías en el sistema. Ahora tenemos que agregar un control GridView que muestra los productos que pertenecen a la categoría seleccionada. Antes de hacer, sin embargo, tómese un momento para comprobar la casilla Habilitar AutoPostBack de etiqueta inteligente de DropDownList. Como se describe en el *filtrado de maestro y detalles con un DropDownList* tutorial, estableciendo el DropDownList `AutoPostBack` propiedad `True` la página se publicarán nuevo cada vez que se cambia el valor de DropDownList. Esto hará que el control GridView se actualice, que muestra los productos de la categoría recién seleccionada. Si el `AutoPostBack` propiedad está establecida en `False` (valor predeterminado), cambiar la categoría no provocarán una devolución de datos y, por tanto, no se actualizan los productos enumerados.


[![Active la casilla de AutoPostBack habilitar en la etiqueta inteligente de DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Figura 5**: Active la casilla de AutoPostBack habilitar en la etiqueta inteligente de DropDownList ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Agregar un control GridView a la página para mostrar los productos de la categoría seleccionada. Establecer la GridView `ID` a `ProductsInCategory` y enlazarla a un nuevo ObjectDataSource denominado `ProductsInCategoryDataSource`.


[![Agregar un nuevo origen ObjectDataSource denominado ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Figura 6**: agregue un nuevo ObjectDataSource denominado `ProductsInCategoryDataSource` ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Configurar el ObjectDataSource para que invoca el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método.


[![Tiene el ObjectDataSource invocar el método GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Figura 7**: tiene la invocación de ObjectDataSource el `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Puesto que la `GetProductsByCategoryID(categoryID)` método toma un parámetro de entrada, en el paso final del asistente se puede especificar el origen del valor del parámetro. Para mostrar los productos de la categoría seleccionada, tienen un parámetro extraído de la `Categories` DropDownList.


[![Obtener el valor del parámetro categoryID de la lista desplegable categorías seleccionadas](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Figura 8**: obtener el *`categoryID`* valor del parámetro de la lista desplegable categorías seleccionadas ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


Después de completar al Asistente GridView tendrá un BoundField para cada una de las propiedades del producto. Vamos a limpiar estos BoundFields para que solo el `ProductName`, `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` BoundFields se muestran. No dude en agregar cualquier configuración de nivel de campo a la BoundFields restantes (como el formato del `UnitPrice` como una moneda). Después de realizar estos cambios, el marcado declarativo de GridView debe ser similar al siguiente:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

En este momento tenemos un informe principal-detalle totalmente operativa que muestra el nombre, precio unitario, unidades en existencia y unidades pedidas para aquellos productos que pertenecen a la categoría seleccionada.


[![Obtener el valor del parámetro categoryID de la lista desplegable categorías seleccionadas](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Figura 9**: obtener el *`categoryID`* valor del parámetro de la lista desplegable categorías seleccionadas ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Paso 2: Mostrar un pie de página en el control GridView

El control GridView puede mostrar un encabezado y pie de página de fila. Estas filas se muestran según los valores de la `ShowHeader` y `ShowFooter` propiedades, respectivamente, con `ShowHeader` volverá al valor predeterminado `True` y `ShowFooter` a `False`. Para incluir un pie de página en el control GridView simplemente establece su `ShowFooter` propiedad `True`.


[![Establezca ShowFooter propiedad de GridView en True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Figura 10**: establecer la GridView `ShowFooter` propiedad `True` ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


La fila de pie de página tiene una celda para cada uno de los campos definidos en el control GridView; Sin embargo, estas celdas están vacías de forma predeterminada. Tómese un momento para ver el progreso en un explorador. Con el `ShowFooter` propiedad ahora se establece en `True`, GridView incluye una fila de pie de página vacía.


[![GridView ahora incluye una fila de pie de página](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Figura 11**: GridView ahora incluye una fila de pie de página ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


La fila de pie de página en la figura 11 no se resaltan, ya que tiene un fondo blanco. Vamos a crear un `FooterStyle` clase CSS `Styles.css` que especifica un fondo rojo oscuro y, a continuación, configurar la `GridView.skin` archivo de máscara en el `DataWebControls` tema para asignar esta clase CSS a la GridView `FooterStyle`del `CssClass` propiedad. Si desea perfeccionar sus conocimientos sobre máscaras y temas, hacen referencia a la [mostrar datos con el ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial.

Empiece agregando la siguiente clase CSS para `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

El `FooterStyle` es similar en su estilo de clase CSS el `HeaderStyle` de la clase, aunque la `HeaderStyle`del color de fondo es detalle más oscuro y el texto se muestra en negrita. Además, el texto en el pie de página está alineado a la derecha, mientras que el texto del encabezado está centrado.

A continuación, para asociar esta clase CSS pie de página de cada GridView, abra el `GridView.skin` un archivo en el `DataWebControls` tema y establezca el `FooterStyle`del `CssClass` propiedad. Tras la adición de este marcado del archivo debe ser similar:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

A medida que la captura de pantalla siguiente muestra, este cambio hace que el pie de página mostrará con mayor claridad.


[![Fila de pie de página de GridView ahora tiene un Color de fondo rojizo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Figura 12**: fila de pie de página de GridView la tiene ahora un Color de fondo rojizo ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Paso 3: Cálculo de los datos de resumen

Con pie de página del GridView muestra el siguiente desafío nos encontramos es cómo calcular los datos de resumen. Hay dos maneras de calcular esta información de agregado:

1. A través de una consulta SQL, puede emitir una consulta adicional a la base de datos para calcular los datos de resumen para una categoría concreta. SQL incluye una serie de funciones de agregado junto con un `GROUP BY` cláusula para especificar los datos sobre el que deben que se resuman los datos. La siguiente consulta SQL podría devolver la información necesaria:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Por supuesto, no deseará emite esta consulta directamente desde la `SummaryDataInFooter.aspx` página, sino que mediante la creación de un método en el `ProductsTableAdapter` y `ProductsBLL`.
2. Esta información de proceso, tal como se agrega a la GridView como se describe en [formato basado en datos personalizados](custom-formatting-based-upon-data-cs.md) del tutorial, GridView `RowDataBound` controlador de eventos se desencadena una vez para cada fila que se agrega a la GridView después de su sido enlace de datos. Mediante la creación de un controlador de eventos para este evento podemos conservar una duración total de los valores que desea agregar. Después de la última fila de datos se ha enlazado a la GridView tenemos los totales y la información necesaria para calcular el promedio.

Normalmente aplico el segundo enfoque tal y como guarda un viaje a la base de datos y el esfuerzo necesario para implementar la funcionalidad de resumen en la capa de acceso a datos y la capa de lógica empresarial, pero cualquier enfoque sería suficiente. Para este tutorial vamos a usar la segunda opción y realizar un seguimiento de la ejecución total utilizando el `RowDataBound` controlador de eventos.

Crear un `RowDataBound` controlador de eventos del control GridView seleccionando GridView en el diseñador, haga clic en el icono de rayo desde la ventana Propiedades y haga doble clic en el `RowDataBound` eventos. Como alternativa, puede seleccionar GridView y su evento RowDataBound desde las listas desplegables en la parte superior del archivo de clase de código subyacente ASP.NET. Esto creará un nuevo controlador de eventos denominado `ProductsInCategory_RowDataBound` en la `SummaryDataInFooter.aspx` clase de código subyacente de la página.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Para mantener un total actualizado necesitamos definir variables fuera del ámbito del controlador de eventos. Crear las cuatro variables de nivel de página siguientes:

- `_totalUnitPrice`, de tipo `Decimal`
- `_totalNonNullUnitPriceCount`, de tipo `Integer`
- `_totalUnitsInStock`, de tipo `Integer`
- `_totalUnitsOnOrder`, de tipo `Integer`

A continuación, escribir el código para incrementar estos tres variables para cada fila de datos se encuentra en la `RowDataBound` controlador de eventos.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

El `RowDataBound` controlador de eventos inicia asegurándose de que estamos trabajando con una fila de datos. Una vez que se ha establecido, el `Northwind.ProductsRow` instancia que solo se enlazó a la `GridViewRow` objeto `e.Row` se almacena en la variable `product`. A continuación, ejecución variables total se incrementan en los valores correspondientes del producto actual (suponiendo que no contienen una base de datos `NULL` valor). Se realice un seguimiento de la ejecución de ambos `UnitPrice` total y el número de no -`NULL` `UnitPrice` registros porque el precio medio es el cociente de estos dos números.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Paso 4: Mostrar los datos de resumen en el pie de página

Con los datos de resumen se suman, el último paso es para que se muestre en la fila de pie de página de GridView. Esta tarea, también se puede lograr mediante programación a través del `RowDataBound` controlador de eventos. Recuerde que el `RowDataBound` controlador de eventos se desencadena para *cada* fila que está enlazado el control GridView, incluida la fila de pie de página. Por lo tanto, se puede aumentar el controlador de eventos para mostrar los datos en la fila de pie de página con el código siguiente:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Puesto que la fila de pie de página se agrega a la GridView después de han agregado todas las filas de datos, se podemos estar seguros de que cuando se está listos para mostrar los datos de resumen en el pie de página que se haya completado la ejecución total de cálculos. A continuación, es el último paso establecer estos valores en las celdas del pie de página.

Para mostrar texto en una celda de pie de página determinado, utilice `e.Row.Cells(index).Text = value`, donde el `Cells` indización comienza en 0. El código siguiente calcula el precio medio (el precio total dividido por el número de productos) y lo muestra junto con el número total de unidades en existencias y unidades en el orden de las celdas de pie de página adecuado de GridView.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Figura 13 muestra el informe después de que se ha agregado este código. Tenga en cuenta cómo el `ToString("c")` , la información de resumen de precio medio se da formato como una moneda.


[![Fila de pie de página de GridView ahora tiene un Color de fondo rojizo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Figura 13**: fila de pie de página de GridView la tiene ahora un Color de fondo rojizo ([haga clic aquí para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Resumen

Mostrar los datos de resumen es un requisito común de informes y el control GridView facilita el proceso incluir esta información en su fila de pie de página. Se muestra la fila de pie de página cuando la GridView `ShowFooter` propiedad está establecida en `True` y puede tener el texto en sus celdas que se establecen mediante programación a través del `RowDataBound` controlador de eventos. Calcular los datos de resumen bien es posible volver a consultar la base de datos o mediante código de clase de código subyacente de la página ASP.NET para calcular mediante programación los datos de resumen.

Este tutorial concluye el examen de formato personalizado con los controles GridView, DetailsView y FormView. Nuestro tutorial siguiente inicia la exploración de insertar, actualizar y eliminar datos mediante estos mismos controles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-the-formview-s-templates-vb.md)
