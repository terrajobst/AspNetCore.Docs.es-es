---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Uso de TemplateFields en el Control DetailsView (C#) | Documentos de Microsoft
author: rick-anderson
description: Las mismas capacidades de TemplateFields disponibles con GridView también están disponibles con el control DetailsView. En este tutorial se mostrará uno de los productos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f1d2e8312451c0bd1b3aba448963317f5fe06029
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>Uso de TemplateFields en el Control DetailsView (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) o [descarga de PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Las mismas capacidades de TemplateFields disponibles con GridView también están disponibles con el control DetailsView. En este tutorial se mostrará uno de los productos a la vez mediante una DetailsView que contiene TemplateFields.


## <a name="introduction"></a>Introducción

TemplateField ofrece un mayor grado de flexibilidad en la representación de datos que el BoundField, CampoCasillaVerificación, campo Hyperlink y otros controles de campo de datos. En el [tutorial anterior](using-templatefields-in-the-gridview-control-cs.md) analizamos usar TemplateField en un control GridView para:

- Mostrar varios valores de campo de datos en una columna. En concreto, tanto el `FirstName` y `LastName` campos están combinados en una columna de GridView.
- Usar un control Web alternativo para expresar un valor de campo de datos. Hemos visto cómo mostrar la `HiredDate` valor mediante un control de calendario.
- Muestra información de estado en función de los datos subyacentes. Mientras el `Employees` tabla no contiene una columna que devuelve el número de días que un empleado ha estado en el trabajo, hemos podidos mostrar dicha información en el ejemplo de GridView en el tutorial anterior con el uso de un TemplateField y el método de formato.

Las mismas capacidades de TemplateFields disponibles con GridView también están disponibles con el control DetailsView. En este tutorial se mostrará uno de los productos a la vez mediante una DetailsView que contiene dos TemplateFields. El primer TemplateField combinará el `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` campos de datos en una fila de DetailsView. El segundo TemplateField mostrará el valor de la `Discontinued` campo, pero debe utilizar un método de formato para mostrar "Sí" si `Discontinued` es `true`y "NO" en caso contrario.


[![Dos TemplateFields sirven para personalizar la presentación](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figura 1**: TemplateFields dos se utilizan para personalizar la presentación ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


Comencemos.

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Paso 1: Enlace de datos a DetailsView

Como se describe en el tutorial anterior, cuando se trabaja con TemplateFields a menudo resulta más fácil empiece por crear el control de DetailsView que contiene solo BoundFields y, a continuación, agregue TemplateFields nueva o convierta la BoundFields existente en TemplateFields como sea necesario . Por lo tanto, puede iniciar este tutorial agregando un DetailsView a la página a través del diseñador y enlazarlo a un origen ObjectDataSource que devuelve la lista de productos. Estos pasos crearán un DetailsView con BoundFields para cada uno de los campos de valor no booleano del producto y un CampoCasillaVerificación para ese campo valor booleano (suspendido).

Abra la `DetailsViewTemplateField.aspx` página y arrastre un DetailsView del cuadro de herramientas hasta el diseñador. Desde la etiqueta inteligente de DetailsView optar por agregar un nuevo control ObjectDataSource que invoca la `ProductsBLL` la clase `GetProducts()` método.


[![Agregar un nuevo Control ObjectDataSource que invoca el método GetProducts()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figura 2**: agregar un nuevo ObjectDataSource Control ese Invokes el `GetProducts()` método ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


Para este informe quitar el `ProductID`, `SupplierID`, `CategoryID`, y `ReorderLevel` BoundFields. A continuación, volver a ordenar el BoundFields para que la `CategoryName` y `SupplierName` BoundFields aparecen inmediatamente después de la `ProductName` BoundField. No dude en ajustar la `HeaderText` propiedades y propiedades de formato para el BoundFields como considere oportuno. Al igual que con el control GridView, estas modificaciones BoundField nivel pueden realizarse a través del cuadro de diálogo campos (se puede tener acceso haciendo clic en el vínculo Editar los campos de etiqueta inteligente de DetailsView) o a través de la sintaxis declarativa. Por último, borrar el DetailsView `Height` y `Width` valores de propiedad con el fin de permitir la DetailsView controlar para expandir en función de los datos mostrados y Active la casilla Habilitar paginación en la etiqueta inteligente.

Después de realizar estos cambios, el marcado declarativo del control DetailsView debe ser similar al siguiente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Tómese un momento para ver la página a través de un explorador. En este momento debería ver un único producto enumerado (Chai) con filas que muestra el nombre del producto, categoría, proveedor, precio, unidades en existencias, unidades en pedido y su estado no incluida.


[![Detalles del producto se muestran mediante una serie de BoundFields](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figura 3**: detalles del producto se muestran mediante una serie de BoundFields ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Paso 2: Combinar el precio, unidades en existencias y las unidades en el orden en una fila

DetailsView tiene una fila para la `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` campos. Es posible combinar estos campos de datos en una sola fila con TemplateField agregando un nuevo TemplateField o mediante la conversión de una de las existentes `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` BoundFields en TemplateField. Vamos a practicar mientras personalmente prefiero convertir BoundFields existente, mediante la adición de un nuevo TemplateField.

Iniciar, haga clic en el vínculo Editar los campos de etiqueta inteligente de DetailsView para que aparezca el cuadro de diálogo de campos. A continuación, agregue un nuevo TemplateField y establezca su `HeaderText` propiedad "Precio e inventario" y mover la nueva TemplateField por lo que TI está situado encima del `UnitPrice` BoundField.


[![Agregar un nuevo TemplateField al Control DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Figura 4**: agregar un nuevo TemplateField al DetailsView Control ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


Puesto que esta nueva TemplateField contendrá los valores que se muestra actualmente en el `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` BoundFields, vamos a quitarlos.

La última tarea de este paso es definir el `ItemTemplate` marcado para el precio y el inventario TemplateField, lo que puede logra a través de la plantilla de DetailsView edición interfaz en el diseñador o manualmente a través de la sintaxis declarativa del control. Al igual que con el control GridView, se puede acceder a la interfaz de edición de plantilla de DetailsView haciendo clic en el vínculo Editar plantillas en la etiqueta inteligente. Desde aquí puede seleccionar la plantilla para editar en la lista desplegable y, a continuación, agregar los controles Web desde el cuadro de herramientas.

Para este tutorial, empiece por agregar un control de etiqueta para el precio y el del inventario TemplateField `ItemTemplate`. A continuación, haga clic en el vínculo Editar DataBindings de etiquetas inteligentes del control Web Label y enlazar el `Text` propiedad a la `UnitPrice` campo.


[![Enlazar la propiedad de texto de la etiqueta para el campo de datos UnitPrice](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figura 5**: enlazar la etiqueta `Text` propiedad a la `UnitPrice` campo de datos ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Dar formato el precio de una divisa

Con esta adición, el control de etiqueta Web precio y el inventario TemplateField ahora mostrará solo el precio del producto seleccionado. Figura 6 se muestra una captura de pantalla de nuestro progreso hasta el momento cuando se ven a través de un explorador.


[![El precio se muestra el precio y el inventario TemplateField](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figura 6**: el precio y el inventario TemplateField muestra el precio ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


Tenga en cuenta que del precio del producto no tiene el formato como una moneda. Con un BoundField formato es posible estableciendo la `HtmlEncode` propiedad `false` y el `DataFormatString` propiedad a `{0:formatSpecifier}`. Para un TemplateField, sin embargo, las instrucciones de formato deben especificarse en la sintaxis de enlace de datos o mediante el uso de un método de formato definido en alguna parte dentro del código de la aplicación (como en la clase de código subyacente de la página ASP.NET).

Para especificar el formato de la sintaxis de enlace de datos utilizada en el control Web Label, volver al cuadro de diálogo DataBindings haciendo clic en el vínculo Editar DataBindings de etiqueta inteligente de la etiqueta. Puede escribir las instrucciones de formato directamente en la lista desplegable de formato o seleccione una de las cadenas de formato definida. Al igual que con la BoundField `DataFormatString` propiedad, el formato se especifica utilizando `{0:formatSpecifier}`.

Para el `UnitPrice` campo use el formato de moneda especificado seleccionando el valor de la lista desplegable correspondiente o escribiendo en `{0:C}` a mano.


[![Dar formato el precio de una moneda](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figura 7**: dar formato a los precios como una moneda ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


Mediante declaración, la especificación de formato se indica como un segundo parámetro en el `Bind` o `Eval` métodos. La configuración que acaba de crear a través del resultado de diseñador en la siguiente expresión de enlace de datos en el marcado declarativo:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Agregar los campos de datos restantes a TemplateField

En este punto hemos muestra y el formato del `UnitPrice` datos campo en el precio y TemplateField de inventario, pero se deben mostrar el `UnitsInStock` y `UnitsOnOrder` campos. Vamos a mostrar en una línea por debajo del precio y entre paréntesis. Desde la interfaz de edición de plantilla en el diseñador, se puede agregar este tipo marcado situando el cursor dentro de la plantilla y simplemente escribiendo en el texto que se mostrará. Como alternativa, puede escribir este marcado directamente en la sintaxis declarativa.

Agregue el marcado estático, los controles Web Label y sintaxis de enlace de datos para que el precio y el inventario TemplateField muestra la información de inventario y el precio de este modo:

*UnitPrice*  
(**En existencias / en orden:** *UnitsInStock* / *UnitsOnOrder*)

Después de realizar esta tarea marcado declarativo de la DetailsView debe ser similar al siguiente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Con estos cambios, hemos consolidado el precio e información de inventario en una sola fila de DetailsView.


[![El precio y la información de inventario se muestra en una sola fila](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figura 8**: información de inventario y el precio se muestra en una única fila ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Paso 3: Personalizar la información de campo no incluida

El `Products` la tabla `Discontinued` columna es un valor de bit que indica si se ha cancelado el producto. Al enlazar un DetailsView (o GridView) a un control de origen de datos, al igual que los campos de valor booleano, `Discontinued`, se implementan como CheckBoxFields, mientras que los campos de valor no booleanos, como `ProductID`, `ProductName`, y así sucesivamente, se implementan como BoundFields. CheckBoxField se representa como una casilla de verificación deshabilitada que se comprueba si el valor del campo de datos es True y no está activada, en caso contrario.

En lugar de mostrar el CampoCasillaVerificación que podríamos desear mostrar texto que indica en su lugar, independientemente de que el producto ya no está disponible o no. Para lograr esto podríamos quitar el CampoCasillaVerificación de DetailsView y, a continuación, agregue un BoundField cuyo `DataField` propiedad se estableció en `Discontinued`. Tómese un momento para hacerlo. Después de este cambio DetailsView muestra el texto "True" para los productos desusados y "False" para los productos que siguen estando activos.


[![Las cadenas de True y False se usan para mostrar el estado no incluido](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figura 9**: The cadenas True y False se usan para mostrar el estado suspendido ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


Imagine que no queremos que las cadenas "True" o "False" para usarse, pero "YES" y "NO", en su lugar. Dicha personalización puede realizarse con la Ayuda de TemplateField y un método de formato. Un método de formato puede tardar en cualquier número de parámetros de entrada, pero debe devolver el código HTML (como una cadena) para insertar en la plantilla.

Agregar un método de formato para la `DetailsViewTemplateField.aspx` clase de código subyacente de la página denominada `DisplayDiscontinuedAsYESorNO` que acepta un valor booleano como un parámetro de entrada y devuelve una cadena. Como se describe en el tutorial anterior, este método *debe* marcarse como `protected` o `public` para que sea accesible desde la plantilla.


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Este método comprueba el parámetro de entrada (`discontinued`) y devuelve "Sí" si es `true`, "NO" en caso contrario.

> [!NOTE]
> En el método de formato que se examina en la recuperación de tutorial anterior que estábamos pasamos en un campo de datos que puede contener `NULL` s y, por tanto, se necesita comprobar si el empleado `HiredDate` valor de la propiedad tenía una base de datos `NULL` valor antes de obtener acceso a la `EmployeesRow`del `HiredDate` propiedad. Esta comprobación no es necesaria aquí porque el `Discontinued` columna nunca puede tener la base de datos `NULL` valores asignados. Además, este es el motivo de que el método puede aceptar un valor booleano de entrada de parámetro en lugar de tener que aceptar una `ProductsRow` instancia o un parámetro de tipo `object`.


Con este método de formato completo, lo único que queda es para que se llame desde el TemplateField `ItemTemplate`. Para crear el TemplateField quite el `Discontinued` BoundField y agregar un nuevo TemplateField o convertir el `Discontinued` BoundField en TemplateField. A continuación, en la vista de marcado declarativo, editar la TemplateField para que contenga solo un ItemTemplate que invoca la `DisplayDiscontinuedAsYESorNO` método, pasando el valor del elemento actual `ProductRow` la instancia `Discontinued` propiedad. Esto puede obtenerse a través de la `Eval` método. En concreto, el marcado del TemplateField debería ser similar:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Esto hará que la `DisplayDiscontinuedAsYESorNO` invocar al representar el DetailsView, método pasando la `ProductRow` la instancia `Discontinued` valor. Puesto que la `Eval` método devuelve un valor de tipo `object`, pero la `DisplayDiscontinuedAsYESorNO` método espera un parámetro de entrada de tipo `bool`, se convierte la `Eval` métodos devuelven el valor para `bool`. El `DisplayDiscontinuedAsYESorNO` método devolverá "Sí" o "NO" dependiendo del valor recibe. El valor devuelto es lo que se muestra en este DetailsView fila (consulte la figura 10).


[![Los valores Sí o NO son ahora se muestra en la fila no incluidas](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figura 10**: YES o NO valores son ahora se muestra en la fila Discontinued ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>Resumen

TemplateField en el control DetailsView permite un mayor grado de flexibilidad para mostrar los datos que está disponible con los demás controles de campo y son ideales para situaciones donde:

- Varios campos de datos es necesario que se muestren en una columna de GridView
- La fecha se expresa mejor usando un control Web en lugar de texto sin formato
- El resultado depende de los datos subyacentes, como mostrar metadatos, o en volver a formatear los datos

Mientras TemplateFields permitir un mayor grado de flexibilidad en la representación de datos subyacentes de DetailsView, la salida de DetailsView todavía se siente un poco cuadros como cada campo se representa como una fila en un elemento HTML `<table>`.

El control FormView ofrece un mayor grado de flexibilidad para configurar la salida representada. FormView no contiene campos, pero que solo una serie de plantillas (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, y así sucesivamente). Veremos cómo usar FormView para lograr tener un mayor control del diseño representado en nuestro tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Dan Jagers. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-gridview-control-cs.md)
> [Siguiente](using-the-formview-s-templates-cs.md)
