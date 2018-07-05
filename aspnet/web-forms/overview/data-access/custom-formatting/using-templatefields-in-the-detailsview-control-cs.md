---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Uso de TemplateFields en el Control DetailsView (C#) | Microsoft Docs
author: rick-anderson
description: Las mismas funcionalidades disponibles con el control GridView de TemplateFields también están disponibles con el control DetailsView. En este tutorial se mostrará uno de los productos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 33f8e86ad15559440b31b975f9db565d7f331fed
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368449"
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>Uso de TemplateFields en el Control DetailsView (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) o [descargar PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Las mismas funcionalidades disponibles con el control GridView de TemplateFields también están disponibles con el control DetailsView. En este tutorial se mostrará uno de los productos a la vez mediante un DetailsView que contiene TemplateFields.


## <a name="introduction"></a>Introducción

TemplateField ofrece un mayor grado de flexibilidad en la representación de datos que el BoundField, CampoCasillaVerificación, campo Hyperlink y otros controles de campo de datos. En el [tutorial anterior](using-templatefields-in-the-gridview-control-cs.md) contemplamos usar TemplateField en un control GridView para:

- Mostrar varios valores de campo de datos en una columna. En concreto, tanto el `FirstName` y `LastName` campos se combinan en una columna de GridView.
- Usar un control Web alternativo para expresar un valor de campo de datos. Hemos visto cómo mostrar la `HiredDate` valor mediante un control de calendario.
- Mostrar información de estado en función de los datos subyacentes. Mientras el `Employees` tabla no contiene una columna que devuelve el número de días que un empleado ha sido en el trabajo, fuimos capaces de mostrar dicha información en el ejemplo de GridView en el tutorial anterior con el uso de un método de formato y TemplateField.

Las mismas funcionalidades disponibles con el control GridView de TemplateFields también están disponibles con el control DetailsView. En este tutorial se mostrará uno de los productos a la vez mediante un DetailsView que contiene dos TemplateFields. El primer TemplateField combinará el `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` campos de datos en una fila de DetailsView. El segundo TemplateField mostrará el valor de la `Discontinued` campo, pero debe utilizar un método de formato para mostrar "Sí" si `Discontinued` es `true`y "NO" en caso contrario.


[![Dos TemplateFields se usan para personalizar la presentación](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figura 1**: TemplateFields dos se usan para personalizar la presentación ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


Comencemos.

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Paso 1: Enlace de datos a DetailsView

Como se describe en el tutorial anterior, cuando se trabaja con TemplateFields a menudo resulta más fácil para empiece por crear el control DetailsView que contiene solo BoundFields y, a continuación, agregar nuevos TemplateFields o convertir el BoundFields existente en TemplateFields como sea necesario . Por lo tanto, puede empezar este tutorial agregando un DetailsView en la página mediante el diseñador y enlazarlo a un origen ObjectDataSource que devuelve la lista de productos. Estos pasos crearán un DetailsView con BoundFields para cada uno de los campos de valor no booleano del producto y un CampoCasillaVerificación para ese campo valor booleano (suspendido).

Abra el `DetailsViewTemplateField.aspx` página y arrastre un DetailsView desde el cuadro de herramientas hasta el diseñador. Desde la etiqueta inteligente de DetailsView optar por agregar un nuevo control ObjectDataSource que invoca la `ProductsBLL` la clase `GetProducts()` método.


[![Agregar un nuevo Control ObjectDataSource que invoca el método GetProducts()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figura 2**: agregar un nuevo ObjectDataSource Control ese Invokes el `GetProducts()` método ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


Para este informe, quite el `ProductID`, `SupplierID`, `CategoryID`, y `ReorderLevel` BoundFields. A continuación, volver a ordenar el BoundFields para que el `CategoryName` y `SupplierName` BoundFields aparecen inmediatamente después de la `ProductName` BoundField. No dude en ajustar la `HeaderText` propiedades y las propiedades de formato para el BoundFields como considere oportuno. Al igual que con el control GridView, estas modificaciones BoundField nivel pueden realizarse a través del cuadro de diálogo campos (accesible, haga clic en el vínculo Editar campos de etiqueta inteligente de DetailsView) o a través de la sintaxis declarativa. Por último, borre el DetailsView `Height` y `Width` los valores de propiedad con el fin de permitir DetailsView controlar para expandir en función de los datos mostrados y Active la casilla Habilitar paginación en la etiqueta inteligente.

Después de realizar estos cambios, el marcado declarativo del control DetailsView debe ser similar al siguiente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Dedique un momento para ver la página mediante un explorador. En este momento debería ver un único producto enumerado (Chai) con las filas que se muestra el nombre del producto, categoría, proveedor, precio, unidades en existencias, las unidades en orden y su estado no incluida.


[![Se muestran los detalles del producto mediante una serie de BoundFields](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figura 3**: detalles del producto de la que se muestran mediante una serie de BoundFields ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Paso 2: Combinar el precio, unidades en existencias y las unidades en el orden en una fila

DetailsView tiene una fila para el `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` campos. Es posible combinar estos campos de datos en una sola fila con un TemplateField agregando un nuevo TemplateField o convirtiendo una de las existentes `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` BoundFields en TemplateField. Aunque personalmente prefiero convertir BoundFields existente, vamos a practicar agregando un nuevo TemplateField.

Iniciar, haga clic en el vínculo Editar campos de etiqueta inteligente de ovládacího prvku DetailsView. para que aparezca el cuadro de diálogo campos. A continuación, agregue un nuevo TemplateField y establezca su `HeaderText` propiedad "Precio e inventario" y movimiento TemplateField nuevo, por lo que TI se coloca sobre el `UnitPrice` BoundField.


[![Agregar un nuevo TemplateField en el Control DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Figura 4**: agregar un nuevo TemplateField en el DetailsView Control ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


Puesto que este nuevo TemplateField contendrá los valores que se muestra actualmente en el `UnitPrice`, `UnitsInStock`, y `UnitsOnOrder` BoundFields, vamos a quitarlos.

La última tarea de este paso es definir el `ItemTemplate` marcado para el precio y TemplateField de inventario, lo que puede logra a través de la plantilla de DetailsView editar interfaz en el diseñador o manualmente a través de la sintaxis declarativa del control. Al igual que con el control GridView, interfaz de edición de plantilla de DetailsView puede tener acceso haciendo clic en el vínculo Editar plantillas en la etiqueta inteligente. Desde aquí puede seleccionar la plantilla para editar en la lista desplegable y, a continuación, agregar los controles Web desde el cuadro de herramientas.

Para este tutorial, empiece por agregar un control de etiqueta para el precio y del inventario TemplateField `ItemTemplate`. A continuación, haga clic en el vínculo Editar DataBindings de etiqueta inteligente del control de etiqueta Web y enlazar el `Text` propiedad a la `UnitPrice` campo.


[![Enlazar la propiedad Text de etiqueta para el campo de datos UnitPrice](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figura 5**: enlazar la etiqueta `Text` propiedad a la `UnitPrice` campo de datos ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>El precio de formato como una moneda

Con esta versión, el control de etiqueta Web precio y el inventario TemplateField ahora muestran sólo el precio del producto seleccionado. Figura 6 muestra una captura de pantalla de nuestro progreso hasta ahora, cuando se ve mediante un explorador.


[![El precio se muestra el precio y el inventario TemplateField](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figura 6**: el precio se muestra el precio y el inventario TemplateField ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


Tenga en cuenta que los precios del producto no tiene el formato como una moneda. Con BoundField, formato es posible estableciendo el `HtmlEncode` propiedad `false` y el `DataFormatString` propiedad a `{0:formatSpecifier}`. Para un TemplateField, sin embargo, las instrucciones de formato deben especificarse en la sintaxis de enlace de datos o mediante el uso de un método de formato definido en alguna parte dentro del código de la aplicación (como en la clase de código subyacente de la página ASP.NET).

Para especificar el formato de la sintaxis de enlace de datos utilizada en el control Web Label, volver al cuadro de diálogo DataBindings haciendo clic en el vínculo Editar DataBindings de etiqueta inteligente de la etiqueta. Puede escribir las instrucciones de formato directamente en la lista desplegable de formato o seleccione una de las cadenas de formato definida. Al igual que con el BoundField `DataFormatString` propiedad, el formato se especifica mediante `{0:formatSpecifier}`.

Para el `UnitPrice` campo use el formato de moneda se especifica seleccionando el valor de la lista desplegable correspondiente o escribiendo en `{0:C}` a mano.


[![Dar formato el precio de una moneda](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figura 7**: dar formato el precio como una moneda ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


Mediante declaración, la especificación de formato se indica como segundo parámetro en el `Bind` o `Eval` métodos. La configuración que acaba de crear a través del resultado del diseñador en la siguiente expresión de enlace de datos en el marcado declarativo:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Agregar los campos de datos restantes a TemplateField

En este punto hemos muestra y da formato el `UnitPrice` datos de campo en el precio y TemplateField de inventario, pero aún se necesitan mostrar el `UnitsInStock` y `UnitsOnOrder` campos. Vamos a mostrar en una línea por debajo del precio y entre paréntesis. Desde la interfaz de edición de plantilla en el diseñador, se puede agregar tal marcado colocando el cursor dentro de la plantilla y simplemente escribiendo en el texto que se mostrará. Como alternativa, puede escribir este marcado directamente en la sintaxis declarativa.

Agregue el marcado estático, los controles Web de la etiqueta y sintaxis de enlace de datos para que el precio y el inventario TemplateField muestra la información de inventario y el precio de este modo:

*precio por unidad*  
(**En existencias / en orden:** *UnitsInStock* / *UnitsOnOrder*)

Después de realizar esta tarea marcado declarativo de su DetailsView debe ser similar al siguiente:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Con estos cambios se ha consolidado la precios e información de inventario en una sola fila DetailsView.


[![El precio y la información de inventario se muestra en una única fila](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figura 8**: información de inventario y el precio se muestra en una única fila ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Paso 3: Personalizar la información de campo no incluida

El `Products` la tabla `Discontinued` columna es un valor de bits que indica si se ha dejado de usar el producto. Al enlazar un DetailsView (o GridView) a un control de origen de datos, como los campos de valor booleano, `Discontinued`, se implementan como CheckBoxFields, mientras que los campos de valor no booleano como `ProductID`, `ProductName`, y así sucesivamente, se implementan como BoundFields. CheckBoxField se representa como una casilla deshabilitada en el que se comprueba si el valor del campo de datos es True y la existencia de lo contrario.

En lugar de mostrar el CampoCasillaVerificación nos conviene en su lugar mostrará el texto que indica si el producto no está disponible. Para realizar esta acción podríamos quitar el CampoCasillaVerificación DetailsView y, a continuación, agregue un BoundField cuyo `DataField` propiedad se estableció en `Discontinued`. Dedique un momento para hacerlo. Después de este cambio DetailsView muestra el texto "True" para productos discontinuados y "False" para los productos que siguen estando activos.


[![Las cadenas True y False se usan para mostrar el estado no incluido](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figura 9**: The cadenas True y False se usan para mostrar el estado suspendido ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


Imagine que no queremos que las cadenas "True" o "False" para utilizar, pero "YES" y "NO", en su lugar. Dicha personalización puede realizarse con la Ayuda de TemplateField y un método de formato. Un método de formato puede tardar en cualquier número de parámetros de entrada, pero debe devolver el código HTML (como una cadena) para insertar en la plantilla.

Agregar un método de formato para el `DetailsViewTemplateField.aspx` clase de código subyacente de la página denominada `DisplayDiscontinuedAsYESorNO` que acepta un valor booleano como un parámetro de entrada y devuelve una cadena. Como se describe en el tutorial anterior, este método *debe* marcarse como `protected` o `public` con el fin de ser accesibles desde la plantilla.


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Este método comprueba el parámetro de entrada (`discontinued`) y devuelve "Sí" si es `true`, "NO" en caso contrario.

> [!NOTE]
> En el método de formato que se examinan en la recuperación de tutorial anterior que nos estábamos pasando un campo de datos que puede contener `NULL` s y, por tanto, es necesario para comprobar si el empleado `HiredDate` valor de la propiedad tenía una base de datos `NULL` valor antes obtener acceso a la `EmployeesRow`del `HiredDate` propiedad. Esta comprobación no es necesario aquí porque el `Discontinued` columna nunca puede tener la base de datos `NULL` valores asignados. Además, es por esta razón el método puede aceptar un valor booleano de entrada parámetro en lugar de tener que aceptar una `ProductsRow` instancia o un parámetro de tipo `object`.


Con este método de formato completo, todo lo que queda es llamar desde el TemplateField `ItemTemplate`. Para crear TemplateField quite el `Discontinued` BoundField y agregar un nuevo TemplateField o convertir el `Discontinued` BoundField en TemplateField. A continuación, en la vista de marcado declarativo, editar TemplateField para que contenga sólo una ItemTemplate que invoca la `DisplayDiscontinuedAsYESorNO` método, pasando el valor del elemento actual `ProductRow` la instancia `Discontinued` propiedad. Esto se puede acceder mediante el `Eval` método. En concreto, el marcado del TemplateField debería ser similar:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Esto hará que el `DisplayDiscontinuedAsYESorNO` método que se invocará cuando representación DetailsView, pasando el `ProductRow` la instancia `Discontinued` valor. Puesto que la `Eval` método devuelve un valor de tipo `object`, pero la `DisplayDiscontinuedAsYESorNO` método espera un parámetro de entrada de tipo `bool`, se puede convertir el `Eval` métodos devuelven el valor a `bool`. El `DisplayDiscontinuedAsYESorNO` método, a continuación, devolverá "Sí" o "NO" dependiendo del valor recibe. El valor devuelto es lo que se muestra en este DetailsView de fila (consulte la figura 10).


[![Los valores Sí o NO son ahora se muestra en la fila Discontinued](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figura 10**: valores Sí o NO son ahora se muestra en la fila Discontinued ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>Resumen

TemplateField en el control DetailsView permite un mayor grado de flexibilidad al mostrar los datos que está disponible con los otros controles de campo y son ideales para las situaciones donde:

- Necesitan varios campos de datos que se mostrará en una columna de GridView
- Los datos se expresan mejor mediante un control Web en lugar de texto sin formato
- El resultado depende de los datos subyacentes, como mostrar los metadatos o en volver a formatear los datos

Mientras TemplateFields permiten un mayor grado de flexibilidad en la representación de datos subyacentes de DetailsView, la salida de DetailsView todavía se siente un poco anguloso como cada campo se representa como una fila en un elemento HTML `<table>`.

El control FormView ofrece un mayor grado de flexibilidad para configurar la salida representada. FormView no contiene campos sino simplemente una serie de plantillas (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, y así sucesivamente). Veremos cómo usar FormView para lograr tener un mayor control del diseño representado en nuestro tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Dan Jagers. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-gridview-control-cs.md)
> [Siguiente](using-the-formview-s-templates-cs.md)
