---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Patrón de filtrado y detalles con DropDownList (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo mostrar los registros maestros en un control DropDownList y los detalles del elemento de lista seleccionado en un control GridView.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: c2bf3156840c378e554eef3a0629705c059f2777
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833347"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Patrón de filtrado y detalles con DropDownList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) o [descargar PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> En este tutorial, veremos cómo mostrar los registros maestros en un control DropDownList y los detalles del elemento de lista seleccionado en un control GridView.


## <a name="introduction"></a>Introducción

Un tipo común de informe es la *principal-detalle informe*, en que comienza el informe mostrando un conjunto de registros "maestros". El usuario puede, a continuación, profundizar en uno de los registros maestros, ver, por tanto, "Detalles" de ese registro maestro. Informes de maestro y detalles es una opción ideal para visualizar las relaciones uno a varios, como un informe que muestra todas las categorías y, a continuación, lo que permite al usuario seleccionar una categoría determinada y mostrar sus productos asociados. Además, los informes de maestro y detalles son útiles para mostrar información detallada de las tablas especialmente "anchas" (las que tiene una gran cantidad de columnas). Por ejemplo, el nivel "maestro" de un informe de maestro y detalles podría mostrar simplemente el nombre y la unidad de precio del producto de los productos en la base de datos y obtención de detalles de un determinado producto mostraría los campos adicionales del producto (categoría, proveedor, cantidad por unidad, y así sucesivamente).

Hay muchas maneras con las que se puede implementar un informe de maestro y detalles. Sobre esto y los tres tutoriales siguientes analizaremos una variedad de informes de maestro y detalles. En este tutorial veremos cómo mostrar los registros maestros en una [control DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) y los detalles del elemento de lista seleccionado en un control GridView. En concreto, informe de maestro y detalles de este tutorial mostrará información de categoría y producto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Paso 1: Mostrar las categorías en DropDownList

Nuestro informe de maestro y detalles mostrará las categorías de DropDownList, con los productos del elemento de lista seleccionado aparece más abajo en la página en un control GridView. La primera tarea antes que nosotros, a continuación, es que las categorías mostradas en DropDownList. Abrir el `FilterByDropDownList.aspx` página en el `Filtering` carpeta, arrastre en DropDownList del cuadro de herramientas al diseñador de la página y establezca su `ID` propiedad `Categories`. A continuación, haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList. Se mostrará al Asistente para configuración de origen de datos.


[![Especifique el origen de datos de la DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Figura 1**: especificar el origen de datos de la DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Optar por agregar un nuevo origen ObjectDataSource denominado `CategoriesDataSource` que invoca la `CategoriesBLL` la clase `GetCategories()` método.


[![Agregar un nuevo origen ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Figura 2**: agregar un nuevo origen ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![Optar por usar la clase CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Figura 3**: optar por usar la `CategoriesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![Configurar el origen ObjectDataSource para usar el método GetCategories()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Figura 4**: configurar el origen ObjectDataSource que se usarán el `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


Después de configurar ObjectDataSource necesitamos especificar qué campo del origen de datos se debe mostrar en DropDownList y que uno se debe asociar como el valor del elemento de lista. Tiene la `CategoryName` campo como la presentación y `CategoryID` como el valor de cada elemento de lista.


[![Tiene la presentación de DropDownList el campo CategoryName y Use CategoryID como valor](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Figura 5**: tiene la presentación de DropDownList el `CategoryName` campo y Use `CategoryID` como el valor ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


En este momento, tenemos un control DropDownList que se rellena con los registros de la `Categories` tabla (todo lleva a cabo de seis segundos aproximadamente). Figura 6 muestra nuestro progreso hasta ahora, cuando se ve mediante un explorador.


[![Una lista desplegable enumera las categorías actuales](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Figura 6**: un cuadro desplegable enumera las categorías actuales ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Paso 2: Agregar el control GridView de productos

Este último paso en nuestro informe de maestro y detalles es enumerar los productos asociados con la categoría seleccionada. Para ello, agregue un control GridView a la página y crear un nuevo origen ObjectDataSource denominado `productsDataSource`. Tiene la `productsDataSource` control seleccionar sus datos en el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método.


[![Seleccione el método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Figura 7**: seleccione el `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


Después de elegir este método, el ObjectDataSource wizard nos pide el valor para el método *`categoryID`* parámetro. Para usar el valor del seleccionado `categories` DropDownList elemento establezca el origen de parámetro para el Control y la ControlID a `Categories`.


[![Establece el parámetro categoryID en el valor de DropDownList categorías](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Figura 8**: establecer el *`categoryID`* parámetro en el valor de la `Categories` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Dedique un momento para comprobar nuestro progreso en un explorador. Cuando se visita primero la página, esos productos pertenecen a la categoría seleccionada se muestran (bebidas) (como se muestra en la figura 9), pero cambiar DropDownList no actualiza los datos. Esto es porque se debe producir una devolución de datos del control GridView a actualizar. Para lograr esto tenemos dos opciones (ninguno de los cuales requiere escribir ningún código):

- **Establece las categorías de DropDownList**[propiedad AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**en True.** (Puede hacerlo mediante la comprobación de la opción de Habilitar AutoPostBack en la etiqueta inteligente de DropDownList.) Esto desencadenará una devolución de datos cada vez que selecciona la DropDownList elemento se cambia por el usuario. Por lo tanto, cuando el usuario selecciona una nueva categoría de la lista desplegable se produzca una devolución de datos y el control GridView se actualizará con los productos de la categoría recién seleccionada. (Este es el enfoque que he usado en este tutorial.)
- **Agregue un control de botón Web junto a la DropDownList.** Establezca su `Text` propiedad para la actualización o algo similar. Con este enfoque, el usuario deberá seleccionar una nueva categoría y, a continuación, haga clic en el botón. Al hacer clic en el botón se producen un postback y actualizar el control GridView para obtener una lista de los productos de la categoría seleccionada.

Las figuras 9 y 10 se muestra el informe de maestro y detalles en acción.


[![Cuando se visita primero la página, se muestran los productos de bebidas](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Figura 9**: cuando se visita primero la página, se muestran los productos de bebidas ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Seleccionar automáticamente un nuevo producto (generar) produce un PostBack, actualizando el control GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Figura 10**: seleccionar automáticamente un nuevo producto (generar) produce un PostBack, actualizando el control GridView ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Agregar un elemento de lista "--elegir una categoría:"

Cuando se visita primero la `FilterByDropDownList.aspx` página las categorías de primer elemento de lista del DropDownList (bebidas) está seleccionada de forma predeterminada, que muestra los productos de bebidas en el control GridView. En lugar de mostrar productos de la primera categoría, que nos conviene tener en su lugar un elemento de DropDownList seleccionada dice algo parecido a "--elegir una categoría--".

Para agregar un nuevo elemento de lista al control DropDownList, vaya a la ventana Propiedades y haga clic en el botón de puntos suspensivos en el `Items` propiedad. Agregar un nuevo elemento de lista con el `Text` "--elegir una categoría:" y el `Value` `-1`.


[![Agregue--elegir una categoría, elemento de lista](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Figura 11**: agregar--elegir una categoría, elemento de lista ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


Como alternativa, puede agregar el elemento de lista agregando el siguiente marcado al control DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Además, se debe establecer el control DropDownList `AppendDataBoundItems` en True porque cuando las categorías se enlazan al control DropDownList de ObjectDataSource sobrescribirá cualquier elemento agregado manualmente la lista si `AppendDataBoundItems` no es True.


![Establezca la propiedad AppendDataBoundItems en True](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Figura 12**: establecer el `AppendDataBoundItems` propiedad en True


Después de estos cambios, al visitar la página que está seleccionada la opción "--elegir una categoría:" en primer lugar y no hay productos se muestran.


[![En la carga de página inicial se muestran no hay productos](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Figura 13**: se muestran en la página de carga No productos iniciales ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


La razón no hay productos se muestran cuando ya está seleccionado el elemento de lista "--elegir una categoría:" es que su valor es `-1` y hay productos en la base de datos con un `CategoryID` de `-1`. Si se trata el comportamiento que desee, a continuación, ya está en este momento. Si, sin embargo, desea mostrar *todas* de las categorías cuando se selecciona el elemento de lista "--elegir una categoría:", volver a la `ProductsBLL` clase y personalizar el `GetProductsByCategoryID(categoryID)` método por lo que TI invoca el `GetProducts()` método si ha pasado en *`categoryID`* parámetro es menor que cero:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

La técnica empleada aquí es similar al método que se usa para mostrar todos los proveedores en el [parámetros declarativos](../basic-reporting/declarative-parameters-cs.md) tutorial, aunque en este ejemplo estamos usando un valor de `-1` para indicar que todos los registros deben ser recuperar en contraposición a `null`. Esto es porque el *`categoryID`* parámetro de la `GetProductsByCategoryID(categoryID)` método espera que ha pasado un valor entero, mientras que en el tutorial de parámetros declarativos nos estábamos pasando un parámetro de cadena de entrada.

Figura 14 se muestra una captura de pantalla de `FilterByDropDownList.aspx` cuando se selecciona la opción "--elegir una categoría--". En este caso, todos los productos se muestran de forma predeterminada, y el usuario puede restringir la visualización seleccionando una categoría específica.


[![Todos los productos son ahora aparece de forma predeterminada](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Figura 14**: todos los productos son ahora aparece de forma predeterminada ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Resumen

Cuando muestra datos relacionados de forma jerárquica, a menudo ayuda a presentar los datos mediante los informes de maestro/detalle, desde el que el usuario pueda empezar a examinar los datos de la parte superior de la jerarquía y explorar en profundidad los detalles. En este tutorial se examina la creación de un informe sencillo de maestro y detalles que muestre los productos de una categoría seleccionada. Esto se consigue usando DropDownList para la lista de categorías y GridView para los productos que pertenecen a la categoría seleccionada.

En el [siguiente tutorial](master-detail-filtering-with-two-dropdownlists-cs.md) tomaremos el una paso interfaz de DropDownList aún más, con dos DropDownLists.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Siguiente](master-detail-filtering-with-two-dropdownlists-cs.md)
