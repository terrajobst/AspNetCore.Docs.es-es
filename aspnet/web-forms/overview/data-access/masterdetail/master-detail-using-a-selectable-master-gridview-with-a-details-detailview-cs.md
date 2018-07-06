---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial tendrá un control GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón de selección. Al hacer clic en el botón Seleccionar para un particu...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: ea1bb03e2ea39198b70cf71c1105b3e6134e3971
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825761"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) o [descargar PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> Este tutorial tendrá un control GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón de selección. Al hacer clic en el botón de selección de un determinado producto hará que su información completa para mostrarse en un control DetailsView en la misma página.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-across-two-pages-cs.md) vimos cómo crear un informe de maestro y detalles mediante dos páginas web: una página web "principal", desde el que se muestra la lista de proveedores; y una página web "Detalles" que se enumeran los productos proporcionados por el texto seleccionado proveedor. Este formato de informe de dos páginas puede se comprimen en una sola página. Este tutorial tendrá un control GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón de selección. Al hacer clic en el botón de selección de un determinado producto hará que su información completa para mostrarse en un control DetailsView en la misma página.


[![Al hacer clic en el botón de selección muestra los detalles del producto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Figura 1**: al hacer clic en el botón de selección muestra los detalles del producto ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Paso 1: Crear un control GridView seleccionable

Recuerde que en el maestro y detalles de dos páginas de informes que cada registro maestro incluirá un hipervínculo que, al hacer clic, envía el usuario a la página de detalles, pasando la fila donde ha hecho clic `SupplierID` valor en la cadena de consulta. Se agregó un hipervínculo de este tipo para cada fila GridView con un campo Hyperlink. Para el informe de maestro y detalles de página única, necesitamos un botón para cada control GridView de filas que, al hacer clic, muestra los detalles. El control GridView se puede configurar para que incluya un botón Seleccionar para cada fila que se produce un postback y marca esa fila como la GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Empiece agregando un control GridView a la `DetailsBySelecting.aspx` página en el `Filtering` carpeta, establecer su `ID` propiedad `ProductsGrid`. A continuación, agregue un nuevo origen ObjectDataSource denominado `AllProductsDataSource` que invoca la `ProductsBLL` la clase `GetProducts()` método.


[![Crear un origen ObjectDataSource denominado AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Figura 2**: crear un origen ObjectDataSource denominado `AllProductsDataSource` ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![Utilice la clase ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Figura 3**: Use el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![Configurar el origen ObjectDataSource para invocar el método GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Figura 4**: configurar el origen ObjectDataSource a Invoke la `GetProducts()` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


Editar campos de GridView quitando todos excepto el `ProductName` y `UnitPrice` BoundFields. Además, no dude en personalizar estos BoundFields según sea necesario, como el formato el `UnitPrice` BoundField como moneda y cambiando el `HeaderText` propiedades de la BoundFields. Estos pasos pueden realizarse gráficamente, haciendo clic en el vínculo Editar columnas de etiqueta inteligente de GridView, o mediante la configuración manual de la sintaxis declarativa.


[![Quitar todo salvo la ProductName y UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Figura 5**: quite todos menos el `ProductName` y `UnitPrice` BoundFields ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


El marcado final para el control GridView es:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

A continuación, se debe marcar GridView como seleccionable, que se agregará un botón Seleccionar para cada fila. Para lograr esto, simplemente marque la casilla de verificación Habilitar selección en la etiqueta inteligente de GridView.


[![Que se pueda seleccionar las filas de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Figura 6**: asegúrese filas seleccionables de GridView ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


Comprobación de la opción de habilitar la selección agrega un CommandField a la `ProductsGrid` GridView con su `ShowSelectButton` propiedad establecida en True. Esto da como resultado un botón Seleccionar para cada fila del control GridView, como se muestra en la figura 6. De forma predeterminada, los botones de selección se representan como LinkButtons, pero puede usar los botones o ImageButtons en su lugar a través de la CommandField `ButtonType` propiedad.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Cuando se hace clic en el botón Seleccionar de una fila GridView que habrá trastornos una devolución de datos y la GridView `SelectedRow` se actualiza la propiedad. Además el `SelectedRow` propiedad, el control GridView proporciona el [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), y [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) propiedades. El `SelectedIndex` propiedad devuelve el índice de la fila seleccionada, mientras que el `SelectedValue` y `SelectedDataKey` propiedades devuelven valores según la GridView [propiedad DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

El `DataKeyNames` propiedad se utiliza para asociar una o más campos de datos con cada fila y normalmente se utilizan para atribuir la información de identificación de forma exclusiva los datos subyacentes con cada fila GridView. El `SelectedValue` propiedad devuelve el valor del primer `DataKeyNames` campo de datos de la fila seleccionada mientras que el `SelectedDataKey` propiedad devuelve la fila seleccionada `DataKey` objeto, que contiene todos los valores para los campos de clave de datos especificado para esa fila.

El `DataKeyNames` propiedad se establece automáticamente en los campos de datos de identificación de forma exclusiva al enlazar un origen de datos a un control GridView, DetailsView o FormView a través del diseñador. Aunque esta propiedad se ha establecido para que podamos automáticamente en los tutoriales anteriores, los ejemplos se habría funcionado sin el `DataKeyNames` propiedad especificada. Sin embargo, para el control GridView seleccionable en este tutorial, así como tutoriales futuros en el que se obtendrá examinando Insertar, actualizar y eliminar, el `DataKeyNames` propiedad debe establecerse correctamente. Dedique un momento para asegurarse de que la GridView `DataKeyNames` propiedad está establecida en `ProductID`.

Vamos a ver nuestro progreso hasta ahora a través de un explorador. Tenga en cuenta que el control GridView muestra el nombre y el precio de todos los productos, junto con un control LinkButton seleccione. Al hacer clic en el botón de selección, se produce un postback. En el paso 2, veremos cómo hacer que una respuesta DetailsView a esta devolución de datos al mostrar los detalles del producto seleccionado.


[![Cada fila del producto contiene un control LinkButton Select](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Figura 7**: cada fila del producto contiene un control LinkButton seleccione ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>Resaltado de la fila seleccionada

El `ProductsGrid` GridView tiene un `SelectedRowStyle` propiedad que se puede utilizar para dictar el estilo visual de la fila seleccionada. Utiliza correctamente, esto puede mejorar la experiencia del usuario presentando más claramente qué fila del control GridView está seleccionada actualmente. Para este tutorial, vamos a tener la fila seleccionada se resalta con un fondo amarillo.

Al igual que con nuestros tutoriales anteriores, vamos a intentar mantener la configuración relacionada con la estética definida como clases CSS. Por lo tanto, cree una nueva clase CSS en `Styles.css` denominado `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Para aplicar esta clase CSS para el `SelectedRowStyle` propiedad de *todos los* GridViews en nuestra serie de tutoriales, editar la `GridView.skin` máscara en el `DataWebControls` tema para incluir el `SelectedRowStyle` opciones como se muestra a continuación:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Con esta adición, la fila seleccionada de GridView aparece resaltada con un color de fondo amarillo.


[![Personalizar la apariencia de la fila seleccionada mediante SelectedRowStyle propiedad de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Figura 8**: personalizar la apariencia de usando la fila seleccionada la GridView `SelectedRowStyle` propiedad ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Paso 2: Mostrar los detalles del producto seleccionado en DetailsView

Con la `ProductsGrid` completar GridView, todo lo que queda es agregar un DetailsView que muestra información sobre el producto específico seleccionado. Agregar un control DetailsView por encima del control GridView y crear un nuevo origen ObjectDataSource denominado `ProductDetailsDataSource`. Puesto que deseamos que este DetailsView para mostrar información concreta sobre el producto seleccionado, configure el `ProductDetailsDataSource` para usar el `ProductsBLL` la clase `GetProductByProductID(productID)` método.


[![Invocar el método de la clase ProductsBLL GetProductByProductID(productID)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Figura 9**: invocar la `ProductsBLL` la clase `GetProductByProductID(productID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


Tiene la *`productID`* valor del parámetro obtenida del control GridView `SelectedValue` propiedad. Como se explicó anteriormente, la GridView `SelectedValue` propiedad devuelve el valor de la fila seleccionada de la clave de los primeros datos. Por lo tanto, es imprescindible que la GridView `DataKeyNames` propiedad está establecida en `ProductID`, de modo que la fila seleccionada `ProductID` valor devuelto por `SelectedValue`.


[![Establezca el parámetro productID a propiedad de SelectedValue prvku GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Figura 10**: establecer el *`productID`* parámetro a la GridView `SelectedValue` propiedad ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


Una vez el `productDetailsDataSource` ObjectDataSource ha sido configurado correctamente y se ha enlazado a DetailsView, este tutorial está completo. Cuando primero se visita la página no se selecciona ninguna fila, por lo que del control GridView `SelectedValue` propiedad devuelve `null`. Puesto que hay productos con un `NULL` `ProductID` valor, se devuelve ningún registro por el `GetProductByProductID(productID)` método, lo que significa que no se muestra el DetailsView (consulte la figura 11). Al hacer clic en botón Seleccionar de una fila GridView, una devolución de datos que habrá trastornos y DetailsView se actualiza. Esta vez la GridView `SelectedValue` propiedad devuelve el `ProductID` de la fila seleccionada, el `GetProductByProductID(productID)` método devuelve un `ProductsDataTable` con información acerca de ese producto en particular y DetailsView muestra estos detalles (consulte la figura 12).


[![Cuando se muestre la primera visita, sólo el control GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Figura 11**: cuando se visita en primer lugar, se muestra solo el control GridView ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![Al seleccionar una fila, se muestran los detalles del producto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Figura 12**: al seleccionar una fila, se muestran los detalles del producto ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>Resumen

En este y los tres tutoriales anteriores, hemos visto una serie de técnicas para mostrar los informes de maestro y detalles. En este tutorial que se examinan mediante un GridView seleccionable para alojar los registros maestros y DetailsView para mostrar los detalles sobre el registro maestro seleccionado en la misma página. En los tutoriales anteriores hemos examinado cómo mostrar informes de maestro y detalles con listas desplegables y mostrar registros maestros en una página web y registros de detalle en otro.

En este tutorial concluye nuestro examen de los informes de maestro y detalles. A partir de la siguiente tutorialwe comenzará la exploración de formato personalizado con el control GridView, DetailsView y FormView. Veremos cómo personalizar la apariencia de estos controles en función de los datos enlazados a ellas, cómo resumir los datos en el pie de página de GridView y cómo usar plantillas para obtener un mayor grado de control sobre el diseño.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-across-two-pages-cs.md)
> [Siguiente](master-detail-filtering-with-a-dropdownlist-vb.md)
