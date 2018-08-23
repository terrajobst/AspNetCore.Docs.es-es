---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Mostrar varios registros por fila con el Control DataList (C#) | Microsoft Docs
author: rick-anderson
description: En este breve tutorial exploraremos cómo personalizar el diseño de DataList a través de sus propiedades RepeatColumns y RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f79c446a0c9407309ab65cd993df544e883afb22
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839120"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Mostrar varios registros por fila con el Control DataList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) o [descargar PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> En este breve tutorial exploraremos cómo personalizar el diseño de DataList a través de sus propiedades RepeatColumns y RepeatDirection.


## <a name="introduction"></a>Introducción

Los ejemplos de DataList se ve que se muestra en los dos últimos tutoriales impiden que cada registro de su origen de datos como una fila en una sola columna HTML `<table>`. Aunque este es el comportamiento de DataList de forma predeterminada, es muy fácil personalizar la presentación de DataList de forma que los elementos de origen de datos se reparten entre una tabla de varias columna y varias filas. Además, se s posible tener todos los datos del origen de elementos que se muestran en un sola fila, varias columna DataList.

Podemos personalizar el diseño de DataList s a través de su `RepeatColumns` y `RepeatDirection` propiedades, que, respectivamente, indican cuántas columnas se representan y si esos elementos se colocan verticalmente u horizontalmente. Figura 1, por ejemplo, se muestra a un control DataList que muestra información de producto en una tabla con tres columnas.


[![El control DataList muestra tres productos por fila](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Figura 1**: el control DataList muestra tres productos por fila ([haga clic aquí para ver imagen en tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


Mostrando varios elementos de origen de datos por fila, el control DataList puede utilizar más eficazmente el espacio de pantalla horizontal. En este breve tutorial exploraremos estas dos propiedades de DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Paso 1: Mostrar información de producto en un control DataList

Antes de examinar el `RepeatColumns` y `RepeatDirection` propiedades, s permiten crear un control DataList en nuestra página que muestra información de producto mediante el diseño de tabla estándar de una sola columna, varias filas. En este ejemplo, permiten s mostrar el nombre del producto, categoría y precio con el siguiente marcado:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Se ve visto cómo enlazar datos a un control DataList en ejemplos anteriores, por lo que seguiremos estos pasos rápidamente. Comience abriendo la `RepeatColumnAndDirection.aspx` página en el `DataListRepeaterBasics` carpetas y arrastre un control DataList desde el cuadro de herramientas hasta el diseñador. En la etiqueta inteligente de DataList s, optar por crear un nuevo origen ObjectDataSource y configurarlo para extraer los datos de la `ProductsBLL` clase s `GetProducts` método, al elegir el (ninguno) opción desde el Asistente para la s INSERT, UPDATE y eliminar las fichas.

Después de crear y enlazar el control DataList nuevo origen ObjectDataSource, Visual Studio creará automáticamente un `ItemTemplate` que muestra el nombre y valor para cada uno de los campos de datos del producto. Ajustar la `ItemTemplate` directamente a través de marcado declarativo o desde las plantillas Editar opción en la etiqueta inteligente de DataList s para que utilice el marcado que se muestra arriba, reemplazando el *Product Name*, *nombre de categoría*, y *precio* texto con controles de etiqueta que use la sintaxis de enlace de datos adecuada para asignar valores a sus `Text` propiedades. Después de actualizar el `ItemTemplate`, el marcado declarativo de la página s debe ser similar al siguiente:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Tenga en cuenta que ve incluye un especificador de formato en el `Eval` sintaxis de enlace de datos para el `UnitPrice`, dar formato al valor devuelto como una moneda: `Eval("UnitPrice", "{0:C}").`

Dedique un momento para visitar la página en un explorador. Como se muestra en la figura 2, el control DataList representa como una tabla de una sola columna y varias filas de productos.


[![De forma predeterminada, los controles DataList se representan como una tabla de una sola columna y varias filas](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Figura 2**: de forma predeterminada, el control DataList que se representa como una sola columna, tabla de varias filas ([haga clic aquí para ver imagen en tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Paso 2: Cambiar la dirección del diseño s DataList

Mientras el comportamiento predeterminado para el control DataList consiste en disponer sus elementos verticalmente en una tabla de una sola columna y varias filas, este comportamiento puede modificarse fácilmente mediante el control DataList s [ `RepeatDirection` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). El `RepeatDirection` propiedad puede aceptar uno de dos valores posibles: `Horizontal` o `Vertical` (valor predeterminado).

Cambiando el `RepeatDirection` propiedad desde `Vertical` a `Horizontal`, el control DataList representa sus registros en una sola fila, la creación de una columna por cada elemento de origen de datos. Para ilustrar este efecto, haga clic en el control DataList en el diseñador y, a continuación, en la ventana Propiedades, cambie la `RepeatDirection` propiedad desde `Vertical` a `Horiztonal`. Inmediatamente tras hacerlo, el diseñador ajusta el control DataList s diseño, creación de una sola fila, varias columnas (consulte la figura 3).


[![Los elementos de RepeatDirection propiedad dicta cómo la dirección la s DataList son colocan horizontalmente](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Figura 3**: el `RepeatDirection` propiedad dicta cómo los elementos de dirección la s DataList son colocan horizontalmente ([haga clic aquí para ver imagen en tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


Al mostrar pequeñas cantidades de datos, una sola fila y tabla de varias columnas podría ser una forma ideal para maximizar el espacio en pantalla. Sin embargo, para mayores volúmenes de datos, una sola fila requerirá muchas columnas, que inserta los elementos que t puede caber en la pantalla de a la derecha. Figura 4 muestra los productos cuando se representan en un control DataList de fila única. Dado que existen muchos productos (más de 80), el usuario tendrá que desplazarse hacia la distancia a la derecha para ver información acerca de cada uno de los productos.


[![Para orígenes de datos lo suficientemente grande, será necesario un control DataList de columna único desplazamiento Horizontal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Figura 4**: para suficientemente grandes orígenes de datos, una sola columna DataList le requiere desplazamiento Horizontal ([haga clic aquí para ver imagen en tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Paso 3: Mostrar datos en una tabla de varias columna y varias filas

Para crear un control DataList de varias columna, varias filas, es necesario establecer la [ `RepeatColumns` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) al número de columnas que desea mostrar. De forma predeterminada, el `RepeatColumns` propiedad está establecida en 0, lo que hará que el control DataList mostrar todos sus elementos en una sola fila o columna (dependiendo del valor de la `RepeatDirection` propiedad).

En nuestro ejemplo, deje que s muestre tres productos por cada fila de tabla. Por consiguiente, establecer el `RepeatColumns` propiedad a 3. Después de realizar este cambio, dedique un momento para ver los resultados en un explorador. Como se muestra en la figura 5, los productos se muestran ahora en una tabla con tres columnas y varias filas.


[![Se muestran tres productos por fila](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Figura 5**: tres productos se muestran por fila ([haga clic aquí para ver imagen en tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


El `RepeatDirection` propiedad afecta a cómo se colocan los elementos en el control DataList. La figura 5 muestra los resultados con el `RepeatDirection` propiedad establecida en `Horizontal`. Tenga en cuenta que se distribuyen los tres primeros productos Chai, Chang y Aniseed jarabe de izquierda a derecha, arriba a abajo. Los siguientes tres productos (empezando por s Chef Antón Cajun Seasoning) aparecen en una fila debajo de las tres primeras. Cambiar el `RepeatDirection` propiedad nuevo a `Vertical`, sin embargo, dispone de estos productos de arriba a abajo, de izquierda a derecha, como se muestra en la figura 6.


[![En este caso, los productos son colocan verticalmente](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Figura 6**: en este caso, los productos son colocan verticalmente ([haga clic aquí para ver imagen en tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


El número de filas que se muestran en la tabla resultante depende del número de registros total enlazado al control DataList. Concretamente, se s el límite máximo del número total de elementos de origen de datos dividido por el `RepeatColumns` valor de propiedad. Puesto que el `Products` tabla actualmente tiene 84 productos, que es divisible por 3, 28 de filas. Si el número de elementos del origen de datos y el `RepeatColumns` el valor de propiedad no es divisible, a continuación, la última fila o columna tendrán las celdas en blanco. Si el `RepeatDirection` está establecido en `Vertical`, a continuación, la última columna tendrá las celdas vacías; si `RepeatDirection` es `Horizontal`, la última fila tendrá las celdas vacías.

## <a name="summary"></a>Resumen

El control DataList, de forma predeterminada, muestra sus elementos en una tabla de una sola columna, varias filas, que imita la presentación de un control GridView con un único TemplateField. Aunque este diseño predeterminado es aceptable, se puede maximizar el espacio de pantalla mostrando varios elementos de origen de datos por cada fila. Para realizar esto es simplemente una cuestión de configurar el control DataList s `RepeatColumns` propiedad para el número de columnas que se muestran por fila. Además, el control DataList s `RepeatDirection` propiedad puede usarse para indicar si el contenido de la tabla de varias columna, varias filas debe disponerse horizontal de izquierda a derecha, arriba a abajo o vertical de arriba a abajo, de izquierda a derecha.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era John Suru. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [Siguiente](nested-data-web-controls-cs.md)
