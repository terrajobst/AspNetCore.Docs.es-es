---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles (VB) | Documentos de Microsoft
author: rick-anderson
description: "Este tutorial tendrá un GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón de selección. Al hacer clic en el botón Seleccionar para un particu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0db9bf25ce61c31dd8258aaebadf42e7738473ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) o [descarga de PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Este tutorial tendrá un GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón de selección. Haga clic en el botón Seleccionar para un determinado producto hará que su información completa para mostrarse en un control DetailsView en la misma página.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-across-two-pages-vb.md) hemos visto cómo crear un informe de maestro y detalles mediante dos páginas web: una página web "principal", desde el que se muestra la lista de proveedores; y una página web "Detalles" que enumeran los productos suministrados por el texto seleccionado proveedor. Este formato de informe de dos páginas puede se comprimen en una sola página. Este tutorial tendrá un GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón de selección. Haga clic en el botón Seleccionar para un determinado producto hará que su información completa para mostrarse en un control DetailsView en la misma página.


[![Haga clic en el botón Seleccionar muestra detalles del producto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Figura 1**: haga clic en el botón Seleccionar muestra detalles del producto ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Paso 1: Crear un control GridView seleccionable

Recuerde que en la dos páginas principal-detalle informar de que cada registro maestro incluye un hipervínculo que, al hacer clic, envía al usuario a la página de detalles que se pasa a la fila donde ha hecho clic `SupplierID` valor en la cadena de consulta. Se agregó un hipervínculo de este tipo para cada fila de GridView utilizando un campo Hyperlink. Para el informe de página maestra/detalles, necesitamos un botón para cada GridView de filas que, al hacer clic, muestra los detalles. El control GridView puede configurarse para incluir un botón Seleccionar para cada fila que provoca una devolución de datos y marca esa fila como la GridView [SelectedRow](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Empiece agregando un control GridView a la `DetailsBySelecting.aspx` página en el `Filtering` carpeta, establecer su `ID` propiedad `ProductsGrid`. A continuación, agregue un nuevo ObjectDataSource denominado `AllProductsDataSource` que invoca la `ProductsBLL` la clase `GetProducts()` método.


[![Crear un ObjectDataSource denominado AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Figura 2**: crear un ObjectDataSource denominado `AllProductsDataSource` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Utilice la clase ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Figura 3**: Use la `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Configurar el ObjectDataSource para invocar el método GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Figura 4**: configurar el ObjectDataSource a Invoke la `GetProducts()` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Editar los campos de GridView quitar todos menos el `ProductName` y `UnitPrice` BoundFields. Además, no dude en personalizar estos BoundFields según sea necesario, como el formato del `UnitPrice` BoundField como una moneda y cambiar la `HeaderText` propiedades de la BoundFields. Estos pasos pueden realizarse gráficamente, haciendo clic en el vínculo Editar columnas de etiqueta inteligente de GridView, o mediante la configuración manual de la sintaxis declarativa.


[![Quitar todos excepto el ProductName y UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Figura 5**: quitar todos menos el `ProductName` y `UnitPrice` BoundFields ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Es el marcado final del control GridView:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

A continuación, es necesario marcar GridView como seleccionable, que se agregará un botón Seleccionar para cada fila. Para lograr esto, solo tiene que activar la casilla de verificación Habilitar selección en la etiqueta inteligente de GridView.


[![Que se pueda seleccionar filas de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Figura 6**: realizar filas seleccionable la GridView ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Activar la opción Habilitar selección agrega un CommandField a la `ProductsGrid` GridView con su `ShowSelectButton` propiedad establecida en True. Esto da como resultado un botón Seleccionar para cada fila del control GridView, como se muestra en la figura 6. De forma predeterminada, los botones de selección se representan como LinkButton, pero puede usar botones o ImageButtons en su lugar a través de la CommandField `ButtonType` propiedad.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Cuando se hace clic en el botón de selección de una fila de GridView que habrá trastornos una devolución de datos y la GridView `SelectedRow` se actualiza la propiedad. Además el `SelectedRow` propiedad, GridView proporciona el [SelectedIndex](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), y [SelectedDataKey](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) propiedades. El `SelectedIndex` propiedad devuelve el índice de la fila seleccionada, mientras que la `SelectedValue` y `SelectedDataKey` propiedades devuelven valores según el GridView [propiedad DataKeyNames](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

El `DataKeyNames` propiedad se utiliza para asociar uno o más campos de datos valores con cada fila y se usan normalmente para la información de identificación exclusiva de los datos subyacentes con cada fila de GridView de atributos. El `SelectedValue` propiedad devuelve el valor del primer `DataKeyNames` campo de datos para la fila seleccionada where como la `SelectedDataKey` propiedad devuelve la fila seleccionada `DataKey` objeto, que contiene todos los valores para los campos de clave de datos especificado para esa fila.

El `DataKeyNames` propiedad se establece automáticamente en los campos de datos identifica de forma única al enlazar un origen de datos a un control GridView, DetailsView o FormView a través del diseñador. Aunque esta propiedad se ha establecido para que podamos automáticamente en los tutoriales anteriores, los ejemplos habría funcionado sin el `DataKeyNames` propiedad especificada. Sin embargo, para el control GridView seleccionable en este tutorial, así como para ver los tutoriales futuros en el que se podrá examinar Insertar, actualizar y eliminar, el `DataKeyNames` propiedad debe establecerse correctamente. Tómese un momento para asegurarse de que su GridView `DataKeyNames` propiedad está establecida en `ProductID`.

Vamos a ver nuestro progreso hasta el momento a través de un explorador. Tenga en cuenta que el control GridView muestra el nombre y el precio de todos los productos junto con un control LinkButton seleccione. Al hacer clic en el botón Seleccionar provoca una devolución de datos. En el paso 2, veremos cómo hacer que un responden DetailsView a esta devolución de datos, mostrando los detalles del producto seleccionado.


[![Cada fila del producto contiene un LinkButton Select](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Figura 7**: cada fila del producto contiene un control LinkButton seleccione ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Resaltado de la fila seleccionada

El `ProductsGrid` GridView tiene un `SelectedRowStyle` propiedad que se puede utilizar para dictar el estilo visual de la fila seleccionada. Utiliza correctamente, esto puede mejorar la experiencia del usuario presentando más claramente qué fila de GridView está seleccionada actualmente. Para este tutorial, vamos a tener la fila seleccionada se resalta con un fondo amarillo.

Al igual que con nuestros tutoriales anteriores, vamos a intentar mantener la configuración relacionada con estéticos definida como clases CSS. Por lo tanto, cree una nueva clase CSS en `Styles.css` denominado `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Para aplicar esta clase CSS para el `SelectedRowStyle` propiedad de *todos los* GridView en nuestra serie de tutoriales, editar la `GridView.skin` de máscara en el `DataWebControls` tema que se debe incluir el `SelectedRowStyle` configuración tal y como se muestra a continuación:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Con esta adición, la fila seleccionada de GridView está ahora resaltada con un color de fondo amarillo.


[![Personalizar la apariencia de la fila seleccionada mediante SelectedRowStyle propiedad de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Figura 8**: personalizar la apariencia de mediante la fila de seleccionados la GridView `SelectedRowStyle` propiedad ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Paso 2: Mostrar detalles del producto seleccionado en un DetailsView

Con la `ProductsGrid` completar GridView, todo lo que queda es agregar un DetailsView que muestra información sobre el producto concreto seleccionado. Agregar un control DetailsView anteriormente GridView y crear un nuevo origen ObjectDataSource denominado `ProductDetailsDataSource`. Puesto que deseamos que este DetailsView para mostrar información específica acerca del producto seleccionado, configure la `ProductDetailsDataSource` para usar el `ProductsBLL` la clase `GetProductByProductID(productID)` método.


[![Invoke (método) de la clase ProductsBLL GetProductByProductID(productID)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Figura 9**: invocar la `ProductsBLL` la clase `GetProductByProductID(productID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Tiene la  *`productID`*  valor del parámetro que se obtiene desde el control GridView `SelectedValue` propiedad. Como se explicó anteriormente, la GridView `SelectedValue` propiedad devuelve el valor de la fila seleccionada de la clave de los primeros datos. Por lo tanto, es imperativo que la GridView `DataKeyNames` propiedad está establecida en `ProductID`, de modo que la fila seleccionada `ProductID` valor devuelto por `SelectedValue`.


[![Establecer el parámetro productID a propiedad de SelectedValue de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Figura 10**: establecer el  *`productID`*  parámetro a la GridView `SelectedValue` propiedad ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Una vez el `productDetailsDataSource` ObjectDataSource se ha configurado correctamente y se ha enlazado a DetailsView, este tutorial está completo. Cuando se visita la página por primera vez no se selecciona ninguna fila, por lo que el de GridView `SelectedValue` propiedad devuelve `Nothing`. Puesto que no hay productos con un `NULL` `ProductID` valor, se devuelve ningún registro por el `GetProductByProductID(productID)` método, lo que significa que no se muestra la DetailsView (consulte la figura 11). Al hacer clic en el botón de selección de una fila de GridView, seguidamente, tiene lugar una devolución de datos y se actualiza el DetailsView. En esta ocasión el GridView `SelectedValue` propiedad devuelve el `ProductID` de la fila seleccionada, la `GetProductByProductID(productID)` método devuelve un `ProductsDataTable` con información acerca de ese producto en concreto y DetailsView muestra estos detalles (consulte la figura 12).


[![Cuando se muestre la primera visita, solo GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Figura 11**: cuando se visita por primera vez, se muestra solo GridView ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Al seleccionar una fila, se muestran los detalles del producto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Figura 12**: al seleccionar una fila, se muestran los detalles del producto ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Resumen

En este y los tres tutoriales anteriores hemos visto una serie de técnicas para mostrar los informes de maestro y detalles. En este tutorial que se examinan utilizando un GridView seleccionable para albergar los registros maestros y DetailsView para mostrar detalles sobre el registro maestro seleccionado en la misma página. En los tutoriales anteriores explicamos cómo mostrar informes de maestro y detalles mediante listas desplegables y visualización de registros maestros en una página web y registros de detalle en otro.

Este tutorial concluye nuestra examen de los informes de maestro y detalles. Empezando por el siguiente tutorial comenzaremos nuestra exploración de formato personalizado con GridView, DetailsView y FormView. Veremos cómo personalizar el aspecto de estos controles en función de los datos enlazados a ellos, cómo resumir los datos en el pie de página de GridView y cómo utilizar plantillas para obtener un mayor control sobre el diseño.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-across-two-pages-vb.md)
