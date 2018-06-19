---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Mostrar varios registros por cada fila con el Control DataList (C#) | Documentos de Microsoft
author: rick-anderson
description: En este breve tutorial exploraremos cómo personalizar el diseño del control DataList a través de sus propiedades RepeatColumns y RepeatDirection.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 308427836c1fef05e66d1f5348c6bd9e80290f9b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891222"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Mostrar varios registros por cada fila con el Control DataList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) o [descarga de PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> En este breve tutorial exploraremos cómo personalizar el diseño del control DataList a través de sus propiedades RepeatColumns y RepeatDirection.


## <a name="introduction"></a>Introducción

Los ejemplos de DataList se ha visto en los últimos dos tutoriales represente cada registro de su origen de datos como una fila en una sola columna HTML `<table>`. Aunque éste es el comportamiento de DataList de forma predeterminada, es muy fácil personalizar la presentación de DataList de forma que los elementos de origen de datos se reparten entre una tabla de varias columna y varias filas. Además, se s posible tener todos los datos del origen de elementos que se muestran en un sola fila, varias columna DataList.

Podemos personalizar el diseño de DataList s a través de su `RepeatColumns` y `RepeatDirection` propiedades, que indican respectivamente, para representar el número de columnas y si los elementos están dispuestos de vertical u horizontalmente. Figura 1, por ejemplo, se muestra a un control DataList que muestra información de producto de una tabla con tres columnas.


[![El control DataList muestra tres productos por fila](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Figura 1**: The DataList muestra tres productos por fila ([haga clic aquí para ver la imagen a tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


Mostrando varios elementos de origen de datos por cada fila, el control DataList más eficazmente puede utilizar espacio en la pantalla horizontal. En este breve tutorial exploraremos estas dos propiedades de DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Paso 1: Mostrar información de producto en un control DataList

Antes de que se examine el `RepeatColumns` y `RepeatDirection` propiedades, permiten s primero debe crear un control DataList en nuestra página que muestre información del producto con el diseño de tabla estándar de una sola columna y varias filas. En este ejemplo, le permiten s mostrar el nombre de s de producto, la categoría y el precio con el siguiente marcado:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Se ha visto cómo enlazar datos a un control DataList en ejemplos anteriores, por lo que podrá mover rápidamente a través de estos pasos. Comience abriendo la `RepeatColumnAndDirection.aspx` página en el `DataListRepeaterBasics` carpeta y arrastre un control DataList desde el cuadro de herramientas hasta el diseñador. Desde la etiqueta inteligente de DataList s, optar por crear un nuevo origen ObjectDataSource y configúrelo para que extraiga los datos de la `ProductsBLL` clase s. `GetProducts` método, elegir el (ninguno) opción desde el Asistente para la s INSERT, UPDATE y eliminar las fichas.

Después de crear y enlazar la nueva ObjectDataSource a DataList, Visual Studio creará automáticamente un `ItemTemplate` que muestra el nombre y valor para cada uno de los campos de datos de producto. Ajustar la `ItemTemplate` directamente a través de marcado declarativo o desde las plantillas Editar opción en la etiqueta inteligente de DataList s para que utilice el marcado que se muestra arriba, reemplazando el *Product Name*, *nombre de categoría*, y *precio* texto con controles de etiqueta que use la sintaxis de enlace de datos adecuada para asignar valores a sus `Text` propiedades. Después de actualizar el `ItemTemplate`, el marcado declarativo de la página s debería ser similar al siguiente:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Observe que se ha incluido un especificador de formato en el `Eval` sintaxis de enlace de datos para el `UnitPrice`, dé formato al valor devuelto como una moneda - `Eval("UnitPrice", "{0:C}").`

Tómese un momento para visitar la página en un explorador. Como se muestra en la figura 2, el control DataList se representa como una tabla de una sola columna y varias filas de productos.


[![De forma predeterminada, los DataList se representan como una tabla de una sola columna y varias filas](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Figura 2**: de forma predeterminada, el DataList se representa como una sola columna, tabla de varias filas ([haga clic aquí para ver la imagen a tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Paso 2: Cambiar la dirección de diseño de DataList s

Mientras el comportamiento predeterminado para el control DataList es mostrar sus elementos verticalmente en una tabla de una sola columna y varias filas, este comportamiento puede modificarse fácilmente a través de DataList s [ `RepeatDirection` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). El `RepeatDirection` propiedad pueda aceptar uno de dos valores posibles: `Horizontal` o `Vertical` (valor predeterminado).

Cambiando el `RepeatDirection` propiedad de `Vertical` a `Horizontal`, el control DataList representa sus registros en una sola fila, crear una columna de cada elemento de origen de datos. Para ilustrar este efecto, haga clic en el control DataList en el diseñador y, a continuación, en la ventana Propiedades, cambie la `RepeatDirection` propiedad de `Vertical` a `Horiztonal`. Inmediatamente de ese modo, el diseñador ajusta el diseño DataList s, crear una interfaz de varias columna de varias filas (consulte la figura 3).


[![Los elementos RepeatDirection propiedad dicta cómo la dirección la s DataList son disponen Out](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Figura 3**: el `RepeatDirection` propiedad dicta cómo son los elementos de dirección la s DataList disponen Out ([haga clic aquí para ver la imagen a tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


Para mostrar pequeñas cantidades de datos, una sola fila y tabla de varias columna podría ser una forma ideal de maximizar el espacio real en pantalla. Sin embargo, para volúmenes más grandes de datos, una sola fila requerirá numerosas columnas, que inserta los elementos que t puede caber en la pantalla de a la derecha. La figura 4 muestra los productos cuando se representan en un sola fila DataList. Dado que hay muchos productos (por encima del 80), el usuario tendrá que desplazarse mucho hacia la derecha para ver información acerca de cada uno de los productos.


[![Para los orígenes de datos lo suficientemente grande, un sola columna DataList requerirá desplazamiento Horizontal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Figura 4**: para suficientemente grandes orígenes de datos, una sola columna DataList le requieren desplazamiento Horizontal ([haga clic aquí para ver la imagen a tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Paso 3: Mostrar datos en una tabla de varias columna y varias filas

Para crear un control DataList de varias columna, varias filas, es necesario establecer la [ `RepeatColumns` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) al número de columnas que desea mostrar. De forma predeterminada, el `RepeatColumns` propiedad se establece en 0, lo que hará que el control DataList mostrar todos sus elementos en una sola fila o una columna (dependiendo del valor de la `RepeatDirection` propiedad).

En nuestro ejemplo, permiten s mostrar tres productos por cada fila de la tabla. Por consiguiente, establecer el `RepeatColumns` propiedad a 3. Después de realizar este cambio, tómese un momento para ver los resultados en un explorador. Como se muestra en la figura 5, los productos se muestran ahora en una tabla de tres columnas y varias filas.


[![Se muestran tres productos por fila](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Figura 5**: tres productos se muestran por fila ([haga clic aquí para ver la imagen a tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


El `RepeatDirection` propiedad afecta a cómo se distribuyen los elementos en el control DataList. La figura 5 muestra los resultados con la `RepeatDirection` propiedad establecida en `Horizontal`. Tenga en cuenta que los tres primeros productos Chai, cambios y Aniseed Syrup están dispuestos de izquierda a derecha y de arriba a abajo. Los siguientes tres productos (a partir de s Chef Anton Cajun Seasoning) aparecen en una fila debajo de las tres primeras. Cambiar el `RepeatDirection` propiedad volver a `Vertical`, sin embargo, distribuye estos productos de arriba a abajo, de izquierda a derecha, como se muestra en la figura 6.


[![En este caso, los productos son disponen verticalmente](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Figura 6**: en este caso, los productos son disponen verticalmente ([haga clic aquí para ver la imagen a tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


El número de filas que se muestran en la tabla resultante depende del número de registros totales enlazado al control DataList. Concretamente, se s el límite superior del número total de elementos de origen de datos dividido por el `RepeatColumns` valor de propiedad. Puesto que la `Products` tabla actualmente tiene 84 productos, que es divisible por 3, hay 28 filas. Si el número de elementos del origen de datos y la `RepeatColumns` valor de propiedad no es divisible, a continuación, la última fila o columna tendrán las celdas en blanco. Si el `RepeatDirection` está establecido en `Vertical`, a continuación, la última columna tendrá las celdas vacías; si `RepeatDirection` es `Horizontal`, la última fila tendrá que las celdas vacías.

## <a name="summary"></a>Resumen

DataList, de forma predeterminada, enumera sus elementos en una tabla de una sola columna y varias filas, que imita la presentación de un control GridView con TemplateField único. Aunque este diseño predeterminado es aceptable, se puede maximizar el espacio de pantalla mostrando varios elementos de origen de datos por cada fila. Para realizar esto es simplemente cuestión de establecer el control DataList s `RepeatColumns` propiedad para el número de columnas que se mostrarán por fila. Además, el control DataList s `RepeatDirection` propiedad puede utilizarse para indicar si el contenido de la tabla de varias columna, varias filas debe ser dispuesto horizontalmente de izquierda a derecha y de arriba a abajo o verticalmente de arriba a abajo, de izquierda a derecha.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era John Suru. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [Siguiente](nested-data-web-controls-cs.md)
