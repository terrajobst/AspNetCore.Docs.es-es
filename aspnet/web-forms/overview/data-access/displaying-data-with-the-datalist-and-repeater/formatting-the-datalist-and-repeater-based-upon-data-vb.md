---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Aplicar formato a los controles DataList y Repeater en función de datos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial le guiaré a través de ejemplos de cómo se dar formato a la apariencia de los controles DataList y Repeater, ya sea mediante el uso de funciones de formato con...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d16f808101475b66b347e0e464800f247b3a611
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837191"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Aplicar formato a los controles DataList y Repeater en función de datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) o [descargar PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> En este tutorial le guiaré a través de ejemplos de cómo se dar formato a la apariencia de los controles DataList y Repeater, ya sea mediante el uso de funciones de formato en plantillas o controlando el evento enlazado a datos.


## <a name="introduction"></a>Introducción

Como hemos visto en el tutorial anterior, el control DataList ofrece una serie de propiedades relacionadas con el estilo que afectan a su apariencia. En concreto, hemos visto cómo asignar clases CSS de valor predeterminado para el control DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, y `SelectedItemStyle` propiedades. Además de estas cuatro propiedades, el control DataList incluye una serie de otras propiedades relacionadas con el estilo, como `Font`, `ForeColor`, `BackColor`, y `BorderWidth`, por nombrar algunos. El control Repeater no contiene las propiedades relacionadas con el estilo. Cualquier configuración de estilo de este tipo debe realizarse directamente en el marcado en las plantillas de s Repeater.

A menudo, sin embargo, ¿cómo se deben aplicar el formato de datos depende de los propios datos. Por ejemplo, cuando se muestran los productos que podría desear mostrar la información del producto en un color gris claro si ya no está disponible, o quizás deseamos resaltar el `UnitsInStock` valor si es cero. Como hemos visto en los tutoriales anteriores, el control GridView, DetailsView y FormView ofrecen dos formas distintas para dar formato a su apariencia en función de sus datos:

- **El `DataBound` eventos** crear un controlador de eventos adecuado `DataBound` eventos, que se desencadena después de que los datos se ha enlazado a cada elemento (para el control GridView era el `RowDataBound` eventos; para los controles DataList y Repeater es el `ItemDataBound`eventos). En ese caso, controlador, el enlazado a datos solo se puede examinar y decisiones de formato hace. Hemos visto esta técnica en el [formato basado en datos personalizados](../custom-formatting/custom-formatting-based-upon-data-vb.md) tutorial.
- **Aplicar formato a las funciones de plantillas** al uso de TemplateFields en los controles DetailsView o GridView, o una plantilla en el control FormView, podemos agregar una función de formato a la clase de código subyacente ASP.NET página s, la capa de lógica empresarial o cualquiera otra biblioteca de clases que sea accesible desde la aplicación web. Esta función de formato que puede aceptar un número arbitrario de parámetros de entrada, pero debe devolver el código HTML para representar en la plantilla. Funciones de formato se examinaron primero en el [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial.

Ambas técnicas de formato están disponibles con los controles DataList y Repeater. En este tutorial le guiaré a través de ejemplos de ambas técnicas para ambos controles.

## <a name="using-theitemdataboundevent-handler"></a>Mediante el`ItemDataBound`controlador de eventos

Cuando los datos están enlazados a un control DataList, o desde un control de origen de datos mediante la asignación mediante programación los datos al control s `DataSource` propiedad y una llamada a su `DataBind()` método, el control DataList s `DataBinding` evento desencadena, el origen de datos de tipo enumerado y cada registro de datos está enlazado al control DataList. Para cada registro del origen de datos, se crea el control DataList un [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) objeto que se luego enlazado en el registro actual. Durante este proceso, el control DataList genera dos eventos:

- **`ItemCreated`** se activa tras la `DataListItem` creada
- **`ItemDataBound`** se desencadena después de que el registro actual se ha enlazado a la `DataListItem`

Los siguientes pasos describen el proceso de enlace de datos para el control DataList.

1. El control DataList s [ `DataBinding` evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) se activa
2. Los datos se enlazan al control DataList  
  
   Para cada registro en el origen de datos 

    1. Crear un `DataListItem` objeto
    2. Incendio el [ `ItemCreated` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Enlazar el registro para el `DataListItem`
    4. Incendio el [ `ItemDataBound` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Agregar el `DataListItem` a la `Items` colección

Al enlazar datos al control Repeater, que progresa a través de la misma secuencia exacta de pasos. La única diferencia es que en lugar de `DataListItem` instancias que se está creadas, se utiliza el control Repeater [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> El lector astuto quizá ha notado una anomalía ligera entre la secuencia de pasos que tienen lugar cuando los controles DataList y Repeater se enlazan a datos frente a cuando el control GridView se enlaza a datos. En la parte final del proceso de enlace de datos, el control GridView provoca la `DataBound` eventos; sin embargo, el control DataList ni Repeater control tienen uno de esos eventos. Esto es porque los controles DataList y Repeater se crearon en el marco ASP.NET 1.x, antes de que se había vuelto el patrón de controlador de eventos previos y posteriores al nivel común.


Al igual que con el control GridView, una opción de formato según los datos es para crear un controlador de eventos para el `ItemDataBound` eventos. Este controlador de eventos podría inspeccionar los datos que solo tenían ha enlazado la `DataListItem` o `RepeaterItem` y afectan al formato del control según sea necesario.

Para el control DataList, cambios de formato para todo el elemento se puede implementar mediante el `DataListItem` propiedades relacionadas con el estilo de s, que incluyen el estándar `Font`, `ForeColor`, `BackColor`, `CssClass`, y así sucesivamente. Para afectar al formato de los controles Web determinados dentro de la plantilla de s DataList, necesitamos acceso mediante programación y modificar el estilo de los controles Web. Hemos visto cómo realizar esta copia en el *formato basado en datos personalizados* tutorial. Al igual que el control Repeater, el `RepeaterItem` clase no tiene ninguna propiedad relacionada con el estilo; por tanto, todos los cambios relacionados con el estilo se realizan a un `RepeaterItem` en el `ItemDataBound` controlador de eventos debe realizarse mediante el acceso mediante programación y actualizar los controles Web dentro de la plantilla.

Puesto que el `ItemDataBound` técnica de formato para los controles DataList y Repeater son prácticamente idénticos, centraremos nuestro ejemplo sobre cómo utilizar el control DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Paso 1: Mostrar información de producto en el control DataList

Antes de que preocuparse sobre el formato, s permiten crear una página que usa un control DataList para mostrar información de producto. En el [tutorial anterior](displaying-data-with-the-datalist-and-repeater-controls-vb.md) creamos un control DataList cuyo `ItemTemplate` muestra cada nombre de producto s, categoría, proveedor, cantidad por unidad y el precio. Permiten s Repita esta funcionalidad aquí en este tutorial. Para lograr esto, ya sea puede volver a crear el control DataList y su origen ObjectDataSource desde cero, o puede copiar a través de esos controles desde la página creada en el tutorial anterior (`Basics.aspx`) y péguelos en la página para este tutorial (`Formatting.aspx`).

Una vez que se ha replicado desde la funcionalidad de DataList y ObjectDataSource `Basics.aspx` en `Formatting.aspx`, dedique un momento para cambiar el control DataList s `ID` propiedad desde `DataList1` al más descriptivo `ItemDataBoundFormattingExample`. A continuación, puede ver al control DataList en un explorador. Como se muestra en la figura 1, la única diferencia formato entre cada producto es que alterna el color de fondo.


[![Los productos se muestran en el Control DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Figura 1**: se enumeran los productos en el Control DataList ([haga clic aquí para ver imagen en tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Para este tutorial, deje que s el control DataList de formato tal que todos los productos con un precio menor 20,00 USD tendrá tanto su nombre y el precio unitario resaltado en amarillo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Paso 2: Determinar mediante programación el valor de los datos en el controlador de eventos ItemDataBound

Dado que solo los productos con un precio bajo 20,00 USD will tienen el formato personalizado aplicado, debemos ser capaces de determinar cada precio s del producto. Al enlazar datos a un control DataList, el control DataList enumera los registros en su origen de datos y, para cada registro, crea un `DataListItem` instancia, enlace el registro de origen de datos a la `DataListItem`. Después del registro determinado s se ha enlazado datos al actual `DataListItem` objeto, el control DataList s `ItemDataBound` desencadena el evento. Podemos crear un controlador de eventos para este evento inspeccionar los valores de datos actual `DataListItem` y, en función de esos valores, realice los cambios de formato necesarios.

Crear un `ItemDataBound` eventos para el control DataList y agregue el código siguiente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Aunque el concepto y la semántica de DataList s `ItemDataBound` controlador de eventos son las mismas que las utilizadas por las operaciones de asignación GridView `RowDataBound` controlador de eventos en el *formato basado en datos personalizados* tutorial, la sintaxis difiere ligeramente. Cuando el `ItemDataBound` desencadena el evento, el `DataListItem` sólo enlazado a datos se pasa a un controlador de eventos correspondiente a través de `e.Item` (en lugar de `e.Row`, al igual que con las operaciones de asignación GridView `RowDataBound` controlador de eventos). El control DataList s `ItemDataBound` se activa el controlador de eventos para *cada* fila agregada a DataList, incluidas las filas de encabezado, pie de página filas y filas de separador. Sin embargo, la información del producto solo está enlazada a las filas de datos. Por lo tanto, cuando se usa el `ItemDataBound` enlazado el control DataList evento para inspeccionar los datos, es necesario asegurarse de que se está trabajando con un elemento de datos. Esto puede realizarse mediante la comprobación de la `DataListItem` s [ `ItemType` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), que puede tener uno de [los siguientes valores de ocho](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Ambos `Item` y `AlternatingItem``DataListItem` elementos de datos de composición la s DataList s. Suponiendo que se está trabajando con un `Item` o `AlternatingItem`, tenemos acceso a los datos reales `ProductsRow` instancia que se enlazó a la actual `DataListItem`. El `DataListItem` s [ `DataItem` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contiene una referencia a la `DataRowView` objeto cuya propiedad `Row` propiedad proporciona una referencia a los datos reales `ProductsRow` objeto.

A continuación, comprobamos el `ProductsRow` instancia s `UnitPrice` propiedad. Desde la tabla Products s `UnitPrice` campo permite `NULL` valores antes de intentar tener acceso a la `UnitPrice` propiedad deberíamos primero comprobamos si tiene un `NULL` valor utilizando la `IsUnitPriceNull()` método. Si el `UnitPrice` valor no es `NULL`, nos, comprobamos si lo s menor 20,00 USD. Si es en realidad 20,00 USD, a continuación, necesitamos aplicar el formato personalizado.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Paso 3: Resalte el nombre del producto y el precio

Una vez que sepamos que un precio del producto s es inferior a 20,00 USD, todo lo que queda es resaltar su nombre y el precio. Para lograr esto, nos debemos primero mediante programación hacen referencia a los controles de etiqueta en el `ItemTemplate` que muestran el nombre de producto s y el precio. A continuación, se debe hacer que muestren un fondo amarillo. Esta información de formato se puede aplicar modificando directamente las etiquetas `BackColor` propiedades (`LabelID.BackColor = Color.Yellow`); lo ideal es que, sin embargo, se deberían expresar todos los asuntos relacionados con la pantalla a través de las hojas de estilos en cascada. De hecho, ya tenemos una hoja de estilos que proporciona el formato deseado definidas en `Styles.css`  -  `AffordablePriceEmphasis`, que se creó y se describe en el *formato basado en datos personalizados* tutorial.

Para aplicar el formato, basta con establecer los dos controles Web Label `CssClass` propiedades a `AffordablePriceEmphasis`, como se muestra en el código siguiente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Con el `ItemDataBound` completado del controlador de eventos, repase la `Formatting.aspx` página en un explorador. Como se muestra en la figura 2, los productos que tengan un precio bajo 20,00 USD tienen su nombre y el precio de resaltado.


[![Se resaltan en esos productos inferior 20,00 USD](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Figura 2**: se resaltan los productos menos 20,00 USD ([haga clic aquí para ver imagen en tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Puesto que el control DataList se representa como HTML `<table>`, sus `DataListItem` las instancias tienen propiedades relacionadas con el estilo que se pueden establecer para aplicar un estilo específico para el elemento completo. Por ejemplo, si deseamos resaltar el *todo* elemento amarillo cuando su precio era inferior a 20,00 USD, podríamos haber reemplazamos el código que hace referencia a las etiquetas y establezca sus `CssClass` propiedades con la siguiente línea de código: `e.Item.CssClass = "AffordablePriceEmphasis"` (consulte la figura 3).


El `RepeaterItem` s que componen el control Repeater, sin embargo, don t ofrecen estas propiedades de nivel de estilo. Por lo tanto, aplicar formato personalizado para el control Repeater requiere que la aplicación de propiedades de estilo a los controles Web dentro de las plantillas de Repeater s, tal como se hizo en la figura 2.


[![Se resalta el elemento de producto completo de productos en 20,00 USD](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Figura 3**: se resalta el elemento de producto completo de productos en 20,00 USD ([haga clic aquí para ver imagen en tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Uso de funciones de formato desde la plantilla

En el *uso de TemplateFields en el GridView Control* tutorial hemos visto cómo usar una función de formato de GridView TemplateField para aplicar formato personalizado en función de los datos enlazados a las filas de s GridView. Una función de formato es un método que se puede invocar desde una plantilla y devuelve el código HTML que se va a emitir en su lugar. Funciones de formato pueden residir en la clase de código subyacente ASP.NET página s o puede ser centralizadas en archivos de clase en el `App_Code` carpeta o en un proyecto de biblioteca de clases independiente. Mover la función de formato fuera de la clase de código subyacente ASP.NET page s es ideal si planea usar la misma función de formato en varias páginas ASP.NET o en otras aplicaciones web ASP.NET.

Para demostrar las funciones de formato, permiten s tiene la información del producto incluyen el texto [DISCONTINUED] junto al nombre del producto s si lo s discontinuado. Además, permiten s tiene if amarillo resaltado precio s menor 20,00 USD (como hicimos en el `ItemDataBound` ejemplo del controlador de eventos); si el precio es 20,00 USD o superior, permiten s no muestran el precio real, pero en su lugar, el texto, debe llamar a para una cotización. Figura 4 muestra una captura de pantalla de los productos con estas reglas de formato aplicadas.


[![Para los productos costosa, el precio se sustituirá con el texto, llame a para una cotización](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Figura 4**: productos costosa, el precio se sustituirá con el texto, llame a para una cotización ([haga clic aquí para ver imagen en tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Paso 1: Creación de las funciones de formato

En este ejemplo se necesitan dos funciones de formato, que muestra el nombre del producto junto con el texto [DISCONTINUED], si es necesario y otra que muestra un precio de resaltado si lo s menor 20,00 USD o el texto, llame a para una cotización en caso contrario. Permiten s crear estas funciones en la clase de código subyacente ASP.NET página s y Denomínelos `DisplayProductNameAndDiscontinuedStatus` y `DisplayPrice`. Ambos métodos deben devolver el código HTML para representar como una cadena y ambos deben marcarse `Protected` (o `Public`) para que se pueden invocar desde la parte de la sintaxis declarativa s de la página ASP.NET. Sigue el código para estos dos métodos:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Tenga en cuenta que el `DisplayProductNameAndDiscontinuedStatus` método acepta los valores de la `productName` y `discontinued` campos de datos como valores escalares, mientras que el `DisplayPrice` método acepta un `ProductsRow` instancia (en lugar de un `unitPrice` valor escalar). Cualquiera de los enfoques funcionará; Sin embargo, si la función de formato es trabajar con valores escalares que pueden contener la base de datos `NULL` valores (como `UnitPrice`; ni `ProductName` ni `Discontinued` permitir `NULL` valores), debe tener especial cuidado en el control de estos entradas escalares.

En concreto, el parámetro de entrada debe ser de tipo `Object` puesto que el valor de entrada puede ser un `DBNull` instancia en lugar del tipo de datos esperados. Además, se debe realizar una comprobación para determinar si el valor de entrada es una base de datos `NULL` valor. Es decir, si quisiéramos el `DisplayPrice` método para aceptar el precio como un valor escalar, d se debe usar el código siguiente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Tenga en cuenta que el `unitPrice` parámetro de entrada es de tipo `Object` y que se ha modificado la instrucción condicional para determinar si `unitPrice` es `DBNull` o no. Además, desde el `unitPrice` parámetro de entrada se pasa como un `Object`, se debe convertir en un valor decimal.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Paso 2: Llamar a la función de formato desde el ItemTemplate DataList s

Con las funciones de formato agregadas a la clase de código subyacente ASP.NET página s, todo lo que queda es invocar estas funciones desde el control DataList s de formato `ItemTemplate`. Para llamar a una función de formato desde una plantilla, coloque la llamada de función dentro de la sintaxis de enlace de datos:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

En el control DataList s `ItemTemplate` el `ProductNameLabel` control Web Label actualmente muestra el nombre del producto s asignando su `Text` el resultado de la propiedad de `<%# Eval("ProductName") %>`. Con el fin de que aparezca en ella el nombre y el texto [DISCONTINUED], si es necesario, actualice la sintaxis declarativa para que en su lugar, se asigna el `Text` el valor de propiedad de la `DisplayProductNameAndDiscontinuedStatus` método. Al hacerlo, debemos pasar el nombre de producto s y los valores no incluidos con el `Eval("columnName")` sintaxis. `Eval` Devuelve un valor de tipo `Object`, pero la `DisplayProductNameAndDiscontinuedStatus` método espera parámetros de entrada de tipo `String` y `Boolean`; por lo tanto, nos debemos convertir los valores devueltos por la `Eval` método a los tipos de parámetro de entrada esperada, de este modo:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Para mostrar el precio, podemos establecer simplemente la `UnitPriceLabel` etiqueta s `Text` propiedad en el valor devuelto por la `DisplayPrice` método, tal como se hizo para mostrar el nombre del producto s y no [incluye] texto. Sin embargo, en lugar de pasar el `UnitPrice` como un parámetro de entrada escalar, se pasa en su lugar en todo el `ProductsRow` instancia:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Con las llamadas a las funciones de formato en su lugar, tómese un momento para ver nuestro progreso en un explorador. La pantalla debe ser similar a la figura 5, con los productos discontinuados incluido el texto [DISCONTINUED] y esos productos cuyo costo es más de $20,00 tiene su precio reemplazan por el texto, la llamada para una cotización.


[![Para los productos costosa, el precio se sustituirá con el texto, llame a para una cotización](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Figura 5**: productos costosa, el precio se sustituirá con el texto, llame a para una cotización ([haga clic aquí para ver imagen en tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Resumen

Formato del contenido de un control DataList o Repeater en función de los datos puede realizarse mediante dos técnicas. La primera técnica consiste en crear un controlador de eventos para el `ItemDataBound` eventos, que se desencadenan cuando se enlaza cada registro del origen de datos a un nuevo `DataListItem` o `RepeaterItem`. En el `ItemDataBound` controlador de eventos, se pueden examinar los datos del elemento s actual y, a continuación, aplicar formato a se pueden aplicar al contenido de la plantilla, o bien, para `DataListItem` s a todo el elemento.

Como alternativa, el formato personalizado puede llevarse a cabo a través de las funciones de formato. Una función de formato es un método que se puede invocar desde el control DataList o plantillas de s Repeater que devuelve el código HTML para emitir en su lugar. A menudo, el código HTML devuelto por una función de formato que viene determinada por los valores que se enlaza al elemento actual. Estos valores pueden pasarse a la función de formato, como los valores escalares o pasando el objeto completo que se enlaza al elemento (como el `ProductsRow` instancia).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Liz Shulok, Yaakov Ellis y Randy Schmidt. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [Siguiente](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
