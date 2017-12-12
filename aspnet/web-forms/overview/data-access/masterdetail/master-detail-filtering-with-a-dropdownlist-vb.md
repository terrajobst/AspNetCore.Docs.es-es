---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Principal-detalle filtrado con DropDownList (VB) | Documentos de Microsoft
author: rick-anderson
description: "En este tutorial veremos cómo mostrar los registros maestros en un control DropDownList y los detalles del elemento de lista seleccionado en un control GridView."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 88e5c65c100bea3cc39b1e08b1aa8a622b4ce7a6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Principal-detalle filtrado con DropDownList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) o [descarga de PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> En este tutorial veremos cómo mostrar los registros maestros en un control DropDownList y los detalles del elemento de lista seleccionado en un control GridView.


## <a name="introduction"></a>Introducción

Un tipo común de informe es la *principal-detalle informe*, en la que comienza el informe mostrando un conjunto de registros "principales". El usuario puede, a continuación, profundizar en uno de los registros maestros, ver, por tanto, "Detalles" de ese registro maestro. Principal-detalle informa es una opción ideal para visualizar las relaciones de uno a varios, como un informe que muestra todas las categorías y, a continuación, lo que permite al usuario seleccionar una categoría determinada y mostrar sus productos asociados. Además, los informes de maestro y detalles son útiles para mostrar información detallada de las tablas especialmente "anchos" (aquellos que tiene una gran cantidad de columnas). Por ejemplo, el nivel de un informe de maestro y detalles "principal" es posible que muestre solo el nombre y la unidad precio del producto de los productos en la base de datos y obtención de detalles de un producto determinado se muestran los campos de producto adicionales (categoría, el proveedor, la cantidad por unidad, y así sucesivamente).

Hay muchas maneras con el que se puede implementar un informe de maestro y detalles. Sobre esto y los tres tutoriales siguientes se examinará una variedad de informes de maestro y detalles. En este tutorial veremos cómo mostrar los registros maestros en un [DropDownList control](https://msdn.microsoft.com/en-us/library/dtx91y0z.aspx) y los detalles del elemento de lista seleccionado en un control GridView. En concreto, informe de maestro y detalles de este tutorial mostrará información de categoría y producto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Paso 1: Mostrar las categorías en DropDownList

Nuestro informe principal-detalle enumerará las categorías de DropDownList, con productos del elemento de lista seleccionado que se muestran más abajo en la página en un control GridView. La primera tarea por delante de nosotros, a continuación, es que las categorías que se muestran en DropDownList. Abra la `FilterByDropDownList.aspx` página en el `Filtering` carpeta, arrastre en DropDownList desde el cuadro de herramientas hasta el Diseñador de la página y establezca su `ID` propiedad `Categories`. A continuación, haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList. Se mostrará al Asistente para configuración de origen de datos.


[![Especifique el origen de datos de DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Figura 1**: especificar el origen de datos de DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Optar por agregar una nueva ObjectDataSource denominado `CategoriesDataSource` que invoca la `CategoriesBLL` la clase `GetCategories()` método.


[![Agregar un nuevo origen ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Figura 2**: agregue un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Elija usar la clase CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Figura 3**: optar por usar la `CategoriesBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Configurar el ObjectDataSource para utilizar el método GetCategories()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Figura 4**: configurar el ObjectDataSource que se utilizan los `GetCategories()` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Después de configurar el ObjectDataSource necesitamos especificar qué campo del origen de datos se debe mostrar en DropDownList y que uno debería estar asociado como el valor para el elemento de lista. Tiene la `CategoryName` campo como la presentación y `CategoryID` como el valor de cada elemento de lista.


[![Tiene la presentación de DropDownList el campo de nombre de categoría y CategoryID de uso como el valor](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Figura 5**: tiene la presentación de DropDownList el `CategoryName` campo y Use `CategoryID` como el valor ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


En este momento tenemos un control DropDownList que se rellena con los registros de la `Categories` tabla (todos lleva a cabo en torno a seis segundos). La figura 6 muestra nuestro progreso hasta el momento cuando se ven a través de un explorador.


[![Un cuadro desplegable enumera las categorías actuales](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Figura 6**: un cuadro desplegable enumera las categorías actuales ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Paso 2: Agregar productos GridView

Este último paso en nuestro informe principal-detalle es enumerar los productos asociados con la categoría seleccionada. Para ello, agregue un control GridView a la página y crear un nuevo origen ObjectDataSource denominado `productsDataSource`. Tiene la `productsDataSource` control restringir sus datos de la `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método.


[![Seleccione el método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Figura 7**: seleccione la `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Después de elegir este método, el Asistente ObjectDataSource nos solicita para el valor para el método  *`categoryID`*  parámetro. Para usar el valor de seleccionado `categories` DropDownList elemento establezca el origen de parámetro para el Control y la ControlID a `Categories`.


[![Establece el parámetro categoryID en el valor de las categorías de DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Figura 8**: establecer el  *`categoryID`*  parámetro en el valor de la `Categories` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Tómese un momento para desproteger nuestro progreso en un explorador. Al visitar primero la página, los productos pertenecen a la categoría seleccionada se muestran (bebidas) (como se muestra en la figura 9), pero cambiar DropDownList no actualiza los datos. Esto es porque se debe producir una devolución de datos del control GridView actualizar. Para ello se tiene dos opciones (ninguno de los cuales requiere escribir código alguno):

- **Establece las categorías de DropDownList**[propiedad AutoPostBack](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**en True.** (Puede hacerlo mediante la comprobación de la opción de Habilitar AutoPostBack en la etiqueta inteligente de DropDownList.) Esto desencadenará una devolución de datos cada vez que seleccione la DropDownList elemento se cambia por el usuario. Por lo tanto, cuando el usuario selecciona una nueva categoría de la lista desplegable un postback surgirán y GridView se actualizará con los productos de la categoría recién seleccionada. (Este es el enfoque que he usado en este tutorial.)
- **Agregar un control de botón Web junto a DropDownList.** Establecer su `Text` propiedad en la actualización o algo similar. Con este enfoque, el usuario deberá seleccionar una nueva categoría y, a continuación, haga clic en el botón. Haga clic en el botón provocará una devolución de datos y actualizar GridView para obtener una lista de los productos de la categoría seleccionada.

Las figuras 9 y 10 se muestra el informe maestro y detalles en acción.


[![Al visitar primero la página, se muestran los productos de bebidas](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Figura 9**: al primero, visite la página, se muestran los productos de bebidas ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Al seleccionar un producto nuevo (crear) automáticamente, una devolución de datos, actualización de GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Figura 10**: seleccionar automáticamente un nuevo producto (producen) provoca una devolución de datos, actualizar GridView ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Agregar un elemento de lista ", elija una categoría:"

Al visitar primero la `FilterByDropDownList.aspx` página las categorías primer elemento de lista del DropDownList (bebidas) está activada de forma predeterminada, que muestra los productos de bebidas en GridView. En lugar de mostrar los productos de la primera categoría, que podríamos desear que en su lugar un elemento de DropDownList seleccionada indica que algo como "--Elija una categoría--".

Para agregar un nuevo elemento de lista a los controles DropDownList, vaya a la ventana Propiedades y haga clic en el botón de puntos suspensivos en el `Items` propiedad. Agregar un nuevo elemento de lista con el `Text` "--Elija una categoría:" y el `Value` `-1`.


[![Agregar a--elija una categoría, elemento de lista](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Figura 11**: agregar un--elija una categoría, elemento de lista ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Como alternativa, puede agregar el elemento de lista, agregue el siguiente marcado al DropDownList:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Además, es necesario establecer el control DropDownList `AppendDataBoundItems` en True porque cuando las categorías se enlazan a los controles DropDownList de ObjectDataSource sobrescribirán los elementos de la lista de agregados manualmente si `AppendDataBoundItems` no es True.


![Establezca la propiedad AppendDataBoundItems en True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Figura 12**: establecer el `AppendDataBoundItems` propiedad en True


Después de estos cambios, cuando en primer lugar, visite la página que está seleccionada la opción "--Elija una categoría:" y ningún producto se muestra.


[![En la carga de la página inicial se muestran los productos](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Figura 13**: se muestran en la inicial página carga No productos ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


La razón no productos se muestran cuando porque se selecciona el elemento de lista ", elija una categoría:" es que su valor es `-1` y no hay productos en la base de datos con un `CategoryID` de `-1`. Si se trata el comportamiento que desee, a continuación, ha terminado en este momento. Si, sin embargo, en el que desea mostrar *todos los* de las categorías cuando se selecciona el elemento de lista ", elija una categoría:", se vuelve a la `ProductsBLL` clase y personalizar el `GetProductsByCategoryID(categoryID)` método por lo que TI, se invoca el `GetProducts()` método si el valor en  *`categoryID`*  parámetro es menor que cero:


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

La técnica que se utiliza aquí es similar al enfoque se usa para mostrar todos los proveedores en el [parámetros declarativos](../basic-reporting/declarative-parameters-cs.md) tutorial, aunque en este ejemplo estamos usando un valor de `-1` para indicar que todos los registros deben estar recuperar en contraposición a `Nothing`. Esto es porque el  *`categoryID`*  parámetro de la `GetProductsByCategoryID(categoryID)` método espera que ha pasado un valor entero, mientras que en el tutorial de parámetros declarativos estábamos pasar en un parámetro de entrada de cadena.

La figura 14 muestra una captura de pantalla de `FilterByDropDownList.aspx` cuando se selecciona la opción "--Elija una categoría--". En este caso, todos los productos se muestran de forma predeterminada, y el usuario puede restringir la visualización seleccionando una categoría específica.


[![Todos los productos están ahora aparece de forma predeterminada](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Figura 14**: todos los productos están ahora aparece de forma predeterminada ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Resumen

Al mostrar datos relacionados con jerárquicamente, suele ayudar a presentar los datos de uso de informes de maestro y detalles, desde el que el usuario puede iniciar examina los datos de la parte superior de la jerarquía y profundizar en los detalles. En este tutorial se examina la creación de un informe de principal-detalle simple que muestra los productos de la categoría seleccionada. Esto se logra usando DropDownList para la lista de categorías y GridView para los productos que pertenecen a la categoría seleccionada.

En el [siguiente tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) le llevaremos el uno paso de interfaz de DropDownList Además, con dos listas desplegables.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
[Siguiente](master-detail-filtering-with-two-dropdownlists-vb.md)
