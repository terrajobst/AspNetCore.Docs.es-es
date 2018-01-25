---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: "Dar formato al DataList y repetidor basándose en datos (VB) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial guiaremos a través de ejemplos de cómo se dar formato a la apariencia de los controles DataList y repetidor, ya sea mediante funciones de formato con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 460fc36062f3338ffd178aceda2b3b224752a089
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Dar formato al DataList y repetidor basándose en datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) o [descarga de PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> En este tutorial guiaremos a través de ejemplos de cómo se dar formato a la apariencia de los controles DataList y repetidor, mediante las funciones de formato en plantillas o controlando el evento de enlace de datos.


## <a name="introduction"></a>Introducción

Como vimos en el tutorial anterior, el control DataList ofrece una serie de propiedades relacionadas con el estilo que afectan a su apariencia. En concreto, hemos visto cómo asignar predeterminado clases CSS para el control DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, y `SelectedItemStyle` propiedades. Además de estas cuatro propiedades, el control DataList incluye una serie de otras propiedades relacionadas con el estilo, como `Font`, `ForeColor`, `BackColor`, y `BorderWidth`, por nombrar algunos. El control de repetidor no contiene las propiedades relacionadas con el estilo. Cualquier configuración de estilo de este tipo debe realizarse directamente en el marcado en las plantillas de s de repetidor.

A menudo, sin embargo, ¿cómo se deben aplicar el formato de datos depende de los datos en Sí. Por ejemplo, al enumerar los productos que tengamos que deseamos que aparezca la información del producto en un color de fuente de color gris claro si ya no está disponible, o puede que convenga resaltar el `UnitsInStock` el valor si es cero. Como hemos visto en tutoriales anteriores, la GridView, DetailsView y FormView ofrecen dos formas distintas para dar formato a su apariencia en función de sus datos:

- **El `DataBound` eventos** crear un controlador de eventos adecuado `DataBound` evento, que se desencadena después de que los datos se ha enlazado a cada elemento (del control GridView era el `RowDataBound` eventos; para el DataList y repetidor es el `ItemDataBound`evento). En ese evento controlador, el enlazado a datos solo se puede examinar y realizan decisiones de formato. Examinamos esta técnica en el [formato basado en datos personalizados](../custom-formatting/custom-formatting-based-upon-data-vb.md) tutorial.
- **Aplicar formato a las funciones en las plantillas de** al uso de TemplateFields en los controles de DetailsView o GridView, o una plantilla en el control FormView, podemos agregar una función de formato a la clase de código subyacente ASP.NET page s, la capa de lógica de negocios ni a ninguna otra biblioteca de clases que sea accesible desde la aplicación web. Esta función de formato puede aceptar un número arbitrario de parámetros de entrada, pero debe devolver el código HTML que se representa en la plantilla. Funciones de formato se examinan en primer lugar en la [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial.

Ambas de estas técnicas de formato están disponibles con los controles DataList y repetidor. En este tutorial guiaremos a través de ejemplos que utilizan ambas técnicas para ambos controles.

## <a name="using-theitemdataboundevent-handler"></a>Mediante el`ItemDataBound`controlador de eventos

Cuando los datos se enlazan a un control DataList, desde un control de origen de datos o mediante la asignación mediante programación los datos al control s `DataSource` propiedad y llamar a su `DataBind()` método, el control DataList s `DataBinding` evento activa, el origen de datos de tipo enumerado y cada registro de datos está enlazada al control DataList. Para cada registro del origen de datos, el control DataList crea un [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) objeto que es, a continuación, enlazar el registro actual. Durante este proceso, el control DataList genera dos eventos:

- **`ItemCreated`**se activa tras la `DataListItem` se ha creado
- **`ItemDataBound`**se desencadena después de que el registro actual se ha enlazado a la`DataListItem`

Los siguientes pasos describen el proceso de enlace de datos para el control DataList.

1. El control DataList s [ `DataBinding` evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) se activa
2. Los datos se enlazan al control DataList  
  
 Para cada registro del origen de datos 

    1. Crear un `DataListItem` objeto
    2. Activar la [ `ItemCreated` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Enlazar el registro de la`DataListItem`
    4. Activar la [ `ItemDataBound` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Agregar el `DataListItem` a la `Items` colección

Al enlazar datos al control de repetidor, se avanza a través de la misma secuencia exacta de pasos. La única diferencia es que en lugar de `DataListItem` repetidor de instancias que se está creados, usa [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> El lector astuto quizás haya observado una ligera anomalías entre la secuencia de pasos que ocurren cuando el DataList y repetidor se enlazan a datos frente a cuando el control GridView está enlazado a datos. En la parte final del proceso de enlace de datos, genera GridView el `DataBound` eventos; sin embargo, el DataList ni repetidor control tiene uno de esos eventos. Esto es porque los controles DataList y repetidor se crearon en el período de tiempo ASP.NET 1.x, antes de que el patrón de controlador de eventos previas y posteriores al nivel se había convertido en común.


Al igual que con el control GridView, es una opción para aplicar formato en función de los datos crear un controlador de eventos para el `ItemDataBound` eventos. Este controlador de eventos debería inspeccionar los datos que tenían que solo se ha enlazado a la `DataListItem` o `RepeaterItem` y afectar al formato del control según sea necesario.

Para el control DataList, cambios de formato para el elemento completo puede implementarse utilizando la `DataListItem` propiedades relacionadas con el estilo de s, que incluyen el estándar `Font`, `ForeColor`, `BackColor`, `CssClass`, y así sucesivamente. Para afectar al formato de los controles Web determinados dentro de la plantilla de DataList s, es necesario tener acceso mediante programación y modificar el estilo de los controles Web. Hemos visto cómo llevar a cabo esta copia en el *formato basado en datos personalizados* tutorial. Al igual que el control de repetidor, el `RepeaterItem` clase no tiene ninguna propiedad relacionadas con el estilo; por lo tanto, todos los cambios relacionados con el estilo realizan en un `RepeaterItem` en el `ItemDataBound` controlador de eventos debe realizarse mediante programación accediendo y actualizar los controles Web de la plantilla.

Puesto que la `ItemDataBound` formato técnica para el DataList y repetidor son prácticamente idénticas, centraremos nuestro ejemplo sobre cómo utilizar el control DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Paso 1: Mostrar información del producto en el control DataList

Antes de que nos preocupamos por la aplicación de formato, s permiten crear una página que usa un control DataList para mostrar información de producto. En el [tutorial anterior](displaying-data-with-the-datalist-and-repeater-controls-vb.md) creamos un DataList cuyo `ItemTemplate` muestra cada nombre de producto s, categoría, proveedor, cantidad por unidad y el precio. Permiten s Repita esta funcionalidad aquí en este tutorial. Para lograr esto, o bien puede volver a DataList y su ObjectDataSource desde el principio o puede copiar a través de los controles de la página creada en el tutorial anterior (`Basics.aspx`) y péguelos en la página para este tutorial (`Formatting.aspx`).

Una vez que ha replicado la funcionalidad de DataList y ObjectDataSource desde `Basics.aspx` en `Formatting.aspx`, tómese un momento para cambiar el control DataList s `ID` propiedad de `DataList1` a más descriptivo `ItemDataBoundFormattingExample`. A continuación, ver al control DataList en un explorador. Como se muestra en la figura 1, la única diferencia formato entre cada producto es que alterna el color de fondo.


[![Los productos se muestran en el Control DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Figura 1**: los productos se muestran en el Control DataList ([haga clic aquí para ver la imagen a tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Para este tutorial, permiten s formato a DataList de forma que todos los productos con un precio menor que $20.00 tendrá tanto su nombre y el precio unitario amarillo resaltado.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Paso 2: Determinar mediante programación el valor de los datos en el controlador de evento ItemDataBound

Dado que solo los productos con un precio bajo $20.00 will tienen el formato personalizado aplicado, debemos ser capaces de determinar cada precio s del producto. Cuando se enlazan datos a un control DataList, DataList enumera los registros de su origen de datos y, para cada registro, crea un `DataListItem` instancia, enlace el registro de origen de datos a la `DataListItem`. Después del registro determinado s datos se ha enlazado a la corriente `DataListItem` objeto, el control DataList s `ItemDataBound` desencadena el evento. Podemos crear un controlador de eventos para este evento inspeccionar los valores de datos actuales `DataListItem` y, según esos valores, realice los cambios de formato necesarios.

Crear un `ItemDataBound` eventos para el control DataList y agregue el código siguiente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Aunque el concepto y la semántica de DataList s `ItemDataBound` controlador de eventos son las mismas que las utilizadas por las operaciones de asignación GridView `RowDataBound` controlador de eventos en el *formato basado en datos personalizados* difiere de la sintaxis de tutorial, ligeramente. Cuando el `ItemDataBound` desencadena el evento, el `DataListItem` simplemente enlazado a datos se haya pasado al controlador de eventos correspondiente a través de `e.Item` (en lugar de `e.Row`, al igual que con las operaciones de asignación GridView `RowDataBound` controlador de eventos). El control DataList s `ItemDataBound` controlador de eventos se desencadena para *cada* fila agregada al control DataList, como filas de encabezado, las filas de pie de página y filas de separador. Sin embargo, la información de producto solo se enlaza a las filas de datos. Por lo tanto, cuando se usa el `ItemDataBound` evento para inspeccionar los datos enlazados al control DataList, necesitamos primero asegúrese de que se está trabajando con un elemento de datos. Esto puede realizarse mediante la comprobación de la `DataListItem` s [ `ItemType` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), que puede tener uno de [los siguientes valores de ocho](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Ambos `Item` y `AlternatingItem``DataListItem` elementos de datos de composición de las operaciones de asignación DataList s. Suponiendo que se está trabajando con un `Item` o `AlternatingItem`, tenemos acceso a los datos reales `ProductsRow` instancia que estaba enlazado actual `DataListItem`. El `DataListItem` s [ `DataItem` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contiene una referencia a la `DataRowView` objeto cuya `Row` propiedad proporciona una referencia a los datos reales `ProductsRow` objeto.

A continuación, comprobamos la `ProductsRow` instancia s `UnitPrice` propiedad. Desde la tabla Products s `UnitPrice` campo permite `NULL` valores antes de intentar tener acceso a la `UnitPrice` propiedad deberíamos primero comprobamos para ver si tiene un `NULL` valor mediante la `IsUnitPriceNull()` (método). Si el `UnitPrice` valor no es `NULL`, a continuación, compruebe para ver si se es menor que $20.00. Si se trata realmente en $20.00, necesitamos, a continuación, aplique el formato personalizado.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Paso 3: Resaltar el nombre del producto s y el precio

Una vez que se sabe que un precio del producto s es menor que $20.00, todo lo que queda es resalte su nombre y el precio. Para lograr esto, se deben en primer lugar mediante programación hacen referencia a los controles de etiqueta en el `ItemTemplate` que muestra el nombre de producto s y el precio. A continuación, necesitamos hacer que muestren un fondo amarillo. Esta información de formato se puede aplicar modificando directamente las etiquetas `BackColor` propiedades (`LabelID.BackColor = Color.Yellow`); lo ideal es que, sin embargo, se deben expresar todos los asuntos relacionados con la pantalla a través de las hojas de estilos en cascada. De hecho, ya tenemos una hoja de estilos que proporciona el formato deseado definido en `Styles.css`  -  `AffordablePriceEmphasis`, que se creó y se describe en el *formato basado en datos personalizados* tutorial.

Para aplicar el formato, basta con establecer los dos controles Web Label `CssClass` propiedades para `AffordablePriceEmphasis`, tal y como se muestra en el código siguiente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Con el `ItemDataBound` completado de controlador de eventos, volver a visitar la `Formatting.aspx` página en un explorador. Como se muestra en la figura 2, los productos que tengan un precio en $20.00 tienen su nombre y el precio resaltado.


[![Se resaltan los productos menos de $20.00](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Figura 2**: se resaltan los productos menos de $20.00 ([haga clic aquí para ver la imagen a tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Puesto que el control DataList se representa como HTML `<table>`, sus `DataListItem` instancias tienen propiedades relacionadas con el estilo que se pueden establecer para aplicar un estilo específico para el elemento completo. Por ejemplo, si deseamos resaltar el *todo* elemento amarillo cuando su precio era menor que $20.00, podríamos haber reemplazamos el código que hace referencia a las etiquetas y establezca sus `CssClass` propiedades con la siguiente línea de código: `e.Item.CssClass = "AffordablePriceEmphasis"` (consulte la figura 3).


El `RepeaterItem` s que componen el control de repetidor, sin embargo, don t ofrecen estas propiedades de nivel de estilo. Por lo tanto, aplicar formato personalizado a repetidor requiere que la aplicación de propiedades de estilo para los controles Web dentro de las plantillas de repetidor s, al igual que hicimos en la figura 2.


[![Se resalta el elemento de producto completo de productos en $20.00](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Figura 3**: se resalta el elemento de producto completo para los productos en $20.00 ([haga clic aquí para ver la imagen a tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Uso de funciones de formato desde dentro de la plantilla

En el *utilizando TemplateFields en el GridView Control* tutorial hemos visto cómo usar una función de formato dentro de GridView TemplateField para aplicar un formato personalizado en función de los datos enlazados a las filas de s GridView. Una función de formato es un método que se puede invocar desde una plantilla y devuelve el código HTML que se va a emitir en su lugar. Funciones de formato puede residir en la clase de código subyacente ASP.NET page s o puede centralizar en archivos de clase en el `App_Code` carpeta o en un proyecto de biblioteca de clases independiente. Mover la función de formato fuera de la clase de código subyacente ASP.NET page s es ideal si planea usar la misma función de formato en varias páginas ASP.NET o en otras aplicaciones web ASP.NET.

Para mostrar las funciones de formato, permiten s dispone de la información de producto incluyen el texto [DISCONTINUED] junto al nombre del producto s si lo s suspendido. Además, permiten s tiene el if amarillo resaltado de precio, s $20.00 menor (como hicimos el `ItemDataBound` ejemplo del controlador de eventos); si el precio es $20.00 o superior, deje que s no permite mostrar el precio real, pero en su lugar el texto, visite llama para una oferta de precio. La figura 4 muestra una captura de pantalla de los productos con estas reglas de formato que se aplican.


[![Para productos caros, el precio se reemplaza con el texto, llame a para una oferta de precio](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Figura 4**: para productos caros, el precio se reemplaza con el texto, llame a para una oferta de precio ([haga clic aquí para ver la imagen a tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Paso 1: Crear las funciones de formato

En este ejemplo se necesitan dos funciones de formato, uno que muestra el nombre del producto junto con el texto [DISCONTINUED], si es necesario y otro que muestra un precio de resaltado si lo s menor $20.00, o el texto, llame a para una oferta de precio en caso contrario. Permiten s crear estas funciones en la clase de código subyacente ASP.NET page s y asignarles nombre `DisplayProductNameAndDiscontinuedStatus` y `DisplayPrice`. Ambos métodos deben devolver el código HTML que se representan como una cadena y ambos deben marcarse `Protected` (o `Public`) para que se pueden invocar desde la parte de la sintaxis declarativa de página s ASP.NET. Sigue el código de estos dos métodos:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Tenga en cuenta que la `DisplayProductNameAndDiscontinuedStatus` método acepta los valores de la `productName` y `discontinued` campos de datos como valores escalares, mientras que la `DisplayPrice` método acepta un `ProductsRow` instancia (en lugar de un `unitPrice` valor escalar). Cualquiera de los enfoques funcionará; Sin embargo, si la función de formato es trabajar con valores escalares que pueden contener la base de datos `NULL` valores (como `UnitPrice`; ni `ProductName` ni `Discontinued` permitir `NULL` valores), debe tener especial cuidado en administrar estos entradas escalares.

En concreto, el parámetro de entrada debe ser de tipo `Object` puesto que el valor de entrada puede ser un `DBNull` instancia en lugar del tipo de datos esperado. Además, se debe realizar una comprobación para determinar si el valor de entrada es una base de datos `NULL` valor. Es decir, si deseamos la `DisplayPrice` método para aceptar el precio como un valor escalar, se d tiene que usar el código siguiente:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Tenga en cuenta que la `unitPrice` parámetro de entrada es de tipo `Object` y que se ha modificado la instrucción condicional para determinar si `unitPrice` es `DBNull` o no. Además, desde el `unitPrice` parámetro de entrada se pasa como un `Object`, debe convertirse en un valor decimal.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Paso 2: Llamar a la función de formato desde el ItemTemplate DataList s

Con las funciones de formato que se agrega a la clase de código subyacente de la página s ASP.NET, lo único que queda es invocar estas funciones desde el control DataList s de formato `ItemTemplate`. Para llamar a una función de formato desde una plantilla, coloque la llamada de función dentro de la sintaxis de enlace de datos:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

En el control DataList s `ItemTemplate` el `ProductNameLabel` control Web Label actualmente muestra el nombre del producto s mediante la asignación de su `Text` propiedad el resultado de `<%# Eval("ProductName") %>`. Para que aparezca en ella el nombre junto con el texto [DISCONTINUED], si es necesario, actualice la sintaxis declarativa para que asigna en su lugar el `Text` el valor de la propiedad de la `DisplayProductNameAndDiscontinuedStatus` método. Al hacer esto, debemos pasar en el nombre de producto s y los valores no incluidos con el `Eval("columnName")` sintaxis. `Eval`Devuelve un valor de tipo `Object`, pero la `DisplayProductNameAndDiscontinuedStatus` método espera parámetros de entrada de tipo `String` y `Boolean`; por lo tanto, nos debemos convierte los valores devueltos por la `Eval` método a los tipos de parámetro de entrada esperada, de este modo:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Para mostrar el precio, podemos establecer simplemente la `UnitPriceLabel` etiqueta s `Text` propiedad en el valor devuelto por la `DisplayPrice` método, tal como se hizo para mostrar el nombre del producto s y [DESPARECIDO] texto. Sin embargo, en lugar de pasar el `UnitPrice` como un parámetro de entrada escalar, se pasa en su lugar de toda la `ProductsRow` instancia:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Con las llamadas a las funciones de formato en su lugar, tómese un momento para ver el progreso en un explorador. La pantalla debe ser similar a la figura 5, con los productos desusados incluido el texto [DISCONTINUED] y los productos que cuestan más de $20.00 tiene su precio reemplazan por el texto, la llamada para una oferta de precio.


[![Para productos caros, el precio se reemplaza con el texto, llame a para una oferta de precio](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Figura 5**: para productos caros, el precio se reemplaza con el texto, llame a para una oferta de precio ([haga clic aquí para ver la imagen a tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Resumen

Aplica formato al contenido de un control DataList o repetidor en función de los datos puede realizarse mediante dos técnicas. La primera técnica consiste en crear un controlador de eventos para el `ItemDataBound` evento, que se desencadena cuando se enlaza cada registro del origen de datos a una nueva `DataListItem` o `RepeaterItem`. En el `ItemDataBound` controlador de eventos, se pueden examinar los datos del elemento s actual y, a continuación, aplicar formato se puede aplicar al contenido de la plantilla, o bien, para `DataListItem` s a todo el elemento.

Como alternativa, formatos personalizados pueden generarse a través de las funciones de formato. Una función de formato es un método que se puede invocar desde el control DataList repetidor s plantillas o que devuelve el código HTML para emitir en su lugar. A menudo, el código HTML devuelto por una función de formato se determina por los valores que se enlaza al elemento actual. Estos valores se pueden pasar a la función de formato, como valores escalares o al pasar todo el objeto que se enlaza al elemento (por ejemplo, como el `ProductsRow` instancia).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Yaakov Ellis, Randy Schmidt y Liz Shulok. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
[Siguiente](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
