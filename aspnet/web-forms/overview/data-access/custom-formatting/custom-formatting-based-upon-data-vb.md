---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Formato personalizado basado en datos (VB) | Microsoft Docs
author: rick-anderson
description: Ajuste del formato de la GridView, DetailsView o FormView basado en los datos enlazados a él puede realizarse de varias maneras. En este tutorial vamos a l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ed55c181534e18bbbb4447ce9a4e5c19ff3717b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366205"
---
<a name="custom-formatting-based-upon-data-vb"></a>Formato personalizado basado en datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) o [descargar PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Ajuste del formato de la GridView, DetailsView o FormView basado en los datos enlazados a él puede realizarse de varias maneras. En este tutorial, buscaremos cómo lograr el formato mediante el uso de los controladores de eventos DataBound y RowDataBound enlazado a datos.


## <a name="introduction"></a>Introducción

La apariencia de los controles GridView, DetailsView y FormView puede personalizarse mediante un gran número de propiedades relacionadas con el estilo. Propiedades como `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, y `Height`, entre otros, dictar la apariencia general de la representación del control. Las propiedades incluidas `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, y otras permiten estos mismos valores de estilo que se aplicará a secciones específicas. Del mismo modo, se puede aplicar esta configuración de estilo en el nivel de campo.

En muchos escenarios, los requisitos de formato dependen del valor de los datos mostrados. Por ejemplo, para llamar la atención para fuera de los productos de cotizaciones, un informe que incluya información del producto puede establecer el color de fondo como amarillo para esos productos cuya `UnitsInStock` y `UnitsOnOrder` campos son iguales a 0. Para resaltar los productos más caros, podemos queremos mostrar los precios de los productos cuyo costo es más de $75,00 en negrita.

Ajuste del formato de la GridView, DetailsView o FormView basado en los datos enlazados a él puede realizarse de varias maneras. En este tutorial, echaremos un vistazo cómo lograr enlazado a datos de formato mediante el uso de la `DataBound` y `RowDataBound` controladores de eventos. En el siguiente tutorial exploraremos una solución alternativa.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Mediante el Control DetailsView`DataBound`controlador de eventos

Cuando los datos están enlazados a un DetailsView, o desde un control de origen de datos mediante la asignación mediante programación de datos para el control `DataSource` propiedad y una llamada a su `DataBind()` método, que se produce la siguiente secuencia de pasos:

1. Del control Web de datos `DataBinding` desencadena el evento.
2. Los datos se enlazan a los datos de control Web.
3. Del control Web de datos `DataBound` desencadena el evento.

Inmediatamente después de los pasos 1 y 3 a través de un controlador de eventos se puede insertar lógica personalizada. Mediante la creación de un controlador de eventos para el `DataBound` evento podemos determinar mediante programación los datos que se ha enlazado al control Web de datos y ajustar el formato según sea necesario. Para ilustrar esto, vamos a crear un DetailsView que mostrará la información general acerca de un producto, sino que mostrará el `UnitPrice` valor en un ***fuente negrita, cursiva*** si supera 75,00 $.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Paso 1: Mostrar la información del producto en DetailsView

Abra el `CustomColors.aspx` página en el `CustomFormatting` carpeta, arrastre un control DetailsView desde el cuadro de herramientas hasta el diseñador, establezca su `ID` valor de propiedad `ExpensiveProductsPriceInBoldItalic`y enlazarlo a un nuevo control ObjectDataSource que invoca la `ProductsBLL` la clase `GetProducts()` método. Los pasos detallados para llevar a cabo esto se omiten aquí por razones de brevedad puesto que se describe detalladamente en los tutoriales anteriores.

Una vez que se ha enlazado al origen ObjectDataSource a DetailsView, dedique un momento para modificar la lista de campos. He optado por eliminar la `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, y `Discontinued` BoundFields y cambia el nombre y volver a formatear el BoundFields restantes. También se ha retirado la `Width` y `Height` configuración. Puesto que DetailsView muestra sólo un único registro, es necesario habilitar la paginación con el fin de permitir que el usuario final ver todos los productos. Hacerlo activando la casilla Habilitar paginación en la etiqueta inteligente de DetailsView.


[![Figura 1: Active la casilla de verificación Habilitar paginación en la etiqueta inteligente de DetailsView](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Figura 1**: figura 1: Active la casilla de la paginación de habilitar en la etiqueta inteligente de DetailsView ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image3.png))


Después de estos cambios, el marcado DetailsView será:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Dedique un momento para probar esta página en el explorador.


[![El Control DetailsView muestra uno de los productos a la vez](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Figura 2**: The DetailsView Control muestra un producto a la vez ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Paso 2: Determinar mediante programación el valor de los datos en el controlador de eventos de enlace de datos

Con el fin de mostrar el precio de una fuente en negrita, cursiva aquellos productos cuyo `UnitPrice` supera el valor $75,00, necesitamos poder determinar mediante programación el `UnitPrice` valor. Para DetailsView, esto puede realizarse en el `DataBound` controlador de eventos. Para crear el evento de controlador haga clic en DetailsView en el diseñador y vaya a la ventana Propiedades. Presione F4 para activarla, si no está visible o vaya al menú Ver y seleccione la opción de menú de la ventana Propiedades. En la ventana Propiedades, haga clic en el icono de rayo para mostrar eventos de DetailsView. A continuación, o bien haga doble clic en el `DataBound` eventos o escriba el nombre, el controlador de eventos que desea crear.


![Crear un controlador de eventos para el evento enlazado a datos](custom-formatting-based-upon-data-vb/_static/image7.png)

**Figura 3**: crear un controlador de eventos para el `DataBound` eventos


> [!NOTE]
> También puede crear un controlador de eventos de la parte del código de la página ASP.NET. Allí encontrará dos listas desplegables en la parte superior de la página. Seleccione el objeto de la lista desplegable izquierda y el evento que desea crear un controlador para desde la lista desplegable derecha y Visual Studio creará automáticamente el controlador de eventos adecuado.


Si lo hace, creará automáticamente el controlador de eventos y le llevará a la parte del código donde se ha agregado. En este momento, verá:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Los datos enlazados a DetailsView se pueden acceder mediante el `DataItem` propiedad. Recuerde que estamos enlazando los controles a un objeto DataTable fuertemente tipados, que se compone de una colección de instancias de DataRow fuertemente tipado. Cuando se enlaza el DataTable a DetailsView, la primera fila de datos de la DataTable se asigna a la DetailsView `DataItem` propiedad. En concreto, el `DataItem` propiedad se le asigna un `DataRowView` objeto. Podemos usar el `DataRowView`del `Row` propiedad va a obtener acceso al objeto DataRow subyacente, que es realmente un `ProductsRow` instancia. Una vez que tenemos esto `ProductsRow` podemos nuestra decisión simplemente inspeccionando los valores de propiedad del objeto de instancia.

El código siguiente muestra cómo determinar si el `UnitPrice` valor enlazado al control DetailsView es mayor que $75,00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Puesto que `UnitPrice` puede tener un `NULL` valor en la base de datos, se comprueba en primer lugar para asegurarse de que no estamos trabajando con un `NULL` valor antes de acceder a la `ProductsRow`del `UnitPrice` propiedad. Esta comprobación es importante porque si se intenta tener acceso a la `UnitPrice` propiedad cuando tiene un `NULL` valor la `ProductsRow` objeto producirá una [StrongTypingException excepción](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Paso 3: Dar formato al valor de UnitPrice en DetailsView

En este punto podemos determinar si la `UnitPrice` valor enlazado a DetailsView tiene un valor que supera 75,00 $, pero hemos aún ver cómo ajustar mediante programación DetailsView del formato en consecuencia. Para modificar el formato de una fila completa en DetailsView, acceso mediante programación a la fila mediante `DetailsViewID.Rows(index)`; para modificar una celda determinada, use el acceso `DetailsViewID.Rows(index).Cells(index)`. Una vez que tenemos una referencia a la fila o celda, a continuación, podemos ajustar su apariencia estableciendo sus propiedades relacionadas con el estilo.

Acceso mediante programación a una fila requiere que conozca el índice de la fila, que comienza en 0. El `UnitPrice` fila es la quinta en DetailsView, dándole un índice de 4 y hacer que sean accesibles mediante programación utilizando `ExpensiveProductsPriceInBoldItalic.Rows(4)`. En este momento tenemos podríamos contenido de la fila completa que se muestra en una fuente en negrita, cursiva mediante el código siguiente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Sin embargo, esto hará que *ambos* la etiqueta (precio) y el valor de negrita y cursiva. Si queremos realizar solo el valor de negrita y cursiva que se deba aplicar este formato a la segunda celda de la fila, que puede realizarse mediante los siguientes:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Puesto que nuestros tutoriales hasta ahora han usado las hojas de estilo para mantener una separación limpia entre el marcado representado y la información relacionada con el estilo, en lugar de establecer las propiedades de estilo específica, como se indicó anteriormente vamos en su lugar, use una clase CSS. Abra el `Styles.css` hoja de estilos y agregue una nueva clase CSS denominada `ExpensivePriceEmphasis` con la siguiente definición:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

A continuación, en el `DataBound` controlador de eventos, establecer la celda `CssClass` propiedad `ExpensivePriceEmphasis`. El código siguiente muestra el `DataBound` controlador de eventos en su totalidad:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Cuando se visualizan Chai, lo cual cuesta menos de $75,00, el precio se muestra en una fuente normal (consulte la figura 4). Sin embargo, cuando se visualizan buey Mishi Kobe Niku, que tiene un precio de $97.00, el precio se muestra en una fuente en negrita, cursiva (consulte la figura 5).


[![Precios de menos de $75,00 se muestran en una fuente Normal](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Figura 4**: los precios de menos de $75,00 se muestran en una fuente Normal ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image10.png))


[![Se muestran los precios de los productos costoso en un negrita, cursiva fuente](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Figura 5**: se muestran los precios de los productos costoso en un negrita, cursiva fuente ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Usar el Control FormView`DataBound`controlador de eventos

Los pasos para determinar los datos subyacentes que se enlaza a un FormView son idénticos a las para crear un DetailsView un `DataBound` controlador de eventos, puede convertir el `DataItem` enlazado al control de propiedad al tipo de objeto adecuado y determine cómo proceder. FormView y DetailsView difieren, sin embargo, en cómo se actualiza la apariencia de la interfaz de usuario.

FormView no contiene ningún BoundFields y, por tanto, no tiene el `Rows` colección. En su lugar, un FormView se compone de plantillas, que pueden contener una combinación de HTML estático, los controles Web y la sintaxis de enlace de datos. Ajustar el estilo de un FormView normalmente implica ajustar el estilo de uno o varios de los controles Web dentro de las plantillas de FormView.

Para ilustrar esto, vamos a usar un FormView para productos de la lista, como en el ejemplo anterior, pero esta vez vamos a mostrar simplemente el nombre de producto y las unidades en existencias con las unidades disponibles que se muestra en una fuente de color rojo si es menor o igual que 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Paso 4: Mostrar la información del producto en un FormView

Agregar un FormView para el `CustomColors.aspx` página situada bajo la DetailsView y establezca su `ID` propiedad `LowStockedProductsInRed`. Enlazar FormView para el control ObjectDataSource creado en el paso anterior. Esto creará un `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate` de FormView. Quitar el `EditItemTemplate` y `InsertItemTemplate` y simplificar la `ItemTemplate` para incluir solamente el `ProductName` y `UnitsInStock` valores, cada uno de sus propios controles de etiqueta con un nombre adecuado. Al igual que con DetailsView del ejemplo anterior, compruebe también la casilla de verificación Habilitar paginación en la etiqueta inteligente de FormView.

Después de estas modificaciones marcado de su FormView debe ser similar al siguiente:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Tenga en cuenta que el `ItemTemplate` contiene:

- **HTML estático** el texto "producto:" y "unidades en existencias:" junto con el `<br />` y `<b>` elementos.
- **Controles Web** los dos controles de etiqueta, `ProductNameLabel` y `UnitsInStockLabel`.
- **Sintaxis de enlace de datos** el `<%# Bind("ProductName") %>` y `<%# Bind("UnitsInStock") %>` sintaxis, que asigna los valores de estos campos a los controles de etiqueta `Text` propiedades.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Paso 5: Determinar mediante programación el valor de los datos en el controlador de eventos de enlace de datos

Con el marcado de FormView completando, el siguiente paso es determinar mediante programación si la `UnitsInStock` valor es menor o igual a 10. Esto se realiza en la misma manera con FormView, como era con DetailsView. Empiece por crear un controlador de eventos para el FormView `DataBound` eventos.


![Crear el controlador de eventos de enlace de datos](custom-formatting-based-upon-data-vb/_static/image14.png)

**Figura 6**: crear el `DataBound` controlador de eventos


Controlador de eventos puede convertir de FormView `DataItem` propiedad a un `ProductsRow` de instancia y determinar si el `UnitsInPrice` valor es tal que necesitamos para que aparezca en una fuente de color rojo.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Paso 6: Dar formato al Control de etiqueta UnitsInStockLabel ItemTemplate de FormView

El último paso consiste en dar formato a la mostrada `UnitsInStock` valor en una fuente de color rojo si el valor es 10 o menos. Para lograr esto es necesario tener acceso mediante programación el `UnitsInStockLabel` en controlar la `ItemTemplate` y establezca sus propiedades de estilo para que su texto se muestre en rojo. Para obtener acceso a un control Web en una plantilla, use el `FindControl("controlID")` método similar al siguiente:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

En nuestro ejemplo queremos tener acceso a una etiqueta de control cuya `ID` valor es `UnitsInStockLabel`, por lo que deberíamos utilizar:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Una vez que tenemos una referencia al control Web mediante programación, podemos modificar sus propiedades relacionadas con el estilo según sea necesario. Como con el ejemplo anterior, he creado una clase CSS en `Styles.css` denominado `LowUnitsInStockEmphasis`. Para aplicar este estilo al control Web Label, establezca su `CssClass` propiedad según corresponda.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> La sintaxis para dar formato a una plantilla de acceso mediante programación el control Web mediante `FindControl("controlID")` y, a continuación, establecer sus propiedades relacionadas con el estilo también se puede usar cuando se usa [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) del DetailsView o GridView controles. Examinaremos TemplateFields en nuestro tutorial siguiente.


Las figuras 7 muestra FormView al ver un producto cuyo `UnitsInStock` valor es mayor que 10, mientras que el producto en la figura 8 tiene un valor menor que 10.


[![Para los productos con unas lo suficientemente grandes Units In Stock, se aplica sin formato personalizado](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Figura 7**: se aplica para productos con unas lo suficientemente grandes Units In Stock, sin formato personalizado ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Las unidades en existencias número se muestra en rojo para los productos con los valores de 10 o menos](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Figura 8**: The unidades en existencias número se muestra en rojo para los productos con los valores de 10 o menos ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formato con la GridView`RowDataBound`eventos

Anteriormente hemos visto la secuencia de pasos DetailsView y FormView controla el progreso a través de durante el enlace de datos. Echemos un vistazo a través de estos pasos una vez más como un recordatorio.

1. Del control Web de datos `DataBinding` desencadena el evento.
2. Los datos se enlazan a los datos de control Web.
3. Del control Web de datos `DataBound` desencadena el evento.

Estos tres sencillos pasos son suficientes para la DetailsView y FormView porque muestran solo un único registro. Para el control GridView que muestra *todas* registros enlazados a él (no solo el primero), el paso 2 es un poco más complicado.

En el paso 2 GridView enumera el origen de datos y, para cada registro, crea un `GridViewRow` de instancia y el registro actual se enlaza a ella. Para cada `GridViewRow` agregado en el control GridView, se generan dos eventos:

- **`RowCreated`** se activa tras la `GridViewRow` creada
- **`RowDataBound`** se desencadena después de que el registro actual se ha enlazado a la `GridViewRow`.

Del control GridView, a continuación, enlace de datos se describe con mayor precisión mediante la siguiente secuencia de pasos:

1. La GridView `DataBinding` desencadena el evento.
2. Los datos se enlazan a la GridView.   
  
   Para cada registro en el origen de datos 

    1. Crear un `GridViewRow` objeto
    2. Incendio el `RowCreated` eventos
    3. Enlazar el registro para el `GridViewRow`
    4. Incendio el `RowDataBound` eventos
    5. Agregar el `GridViewRow` a la `Rows` colección
3. La GridView `DataBound` desencadena el evento.

Para personalizar el formato de los registros individuales de GridView, a continuación, necesitamos crear un controlador de eventos para el `RowDataBound` eventos. Para ilustrar esto, vamos a agregar un control GridView a la `CustomColors.aspx` página que muestra el nombre, la categoría y el precio de cada producto, resaltado de aquellos productos cuyo precio es inferior a 10,00 USD con un color de fondo amarillo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Paso 7: Mostrar información de producto en un control GridView

Agregue un control GridView debajo de FormView del ejemplo anterior y establezca su `ID` propiedad `HighlightCheapProducts`. Puesto que ya tenemos un origen ObjectDataSource que devuelve todos los productos en la página, enlazar el control GridView a la. Por último, edite BoundFields de GridView para incluir solo los nombres de los productos, categorías y los precios. Después de estas modificaciones marcado de GridView aspecto:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Figura 9 muestra nuestro progreso en este punto, cuando se ve mediante un explorador.


[![El control GridView muestra el nombre, categoría y el precio de cada producto](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Figura 9**: GridView muestra el nombre, categoría y precio de cada producto ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Paso 8: Determinar mediante programación el valor de los datos en el controlador de evento RowDataBound

Cuando el `ProductsDataTable` está enlazado a la GridView su `ProductsRow` instancias están enumerados y para cada `ProductsRow` un `GridViewRow` se crea. El `GridViewRow`del `DataItem` propiedad se asigna a la instancia concreta `ProductRow`, tras el cual la GridView `RowDataBound` se genera el controlador de eventos. Para determinar el `UnitPrice` enlaza el valor de cada producto en el control GridView, a continuación, necesitamos crear un controlador de eventos para el control de GridView `RowDataBound` eventos. En este controlador de eventos podemos inspeccionar el `UnitPrice` valor actual `GridViewRow` y tomar una decisión de formato para esa fila.

Este controlador de eventos se puede crear con la misma serie de pasos como FormView y DetailsView.


![Crear un controlador de eventos para el evento RowDataBound de prvku GridView.](custom-formatting-based-upon-data-vb/_static/image24.png)

**Figura 10**: crear un controlador de eventos para el control de GridView `RowDataBound` eventos


Crear el controlador de eventos de esta forma hará que el código siguiente se agregan automáticamente a la parte del código de la página ASP.NET:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Cuando el `RowDataBound` desencadena el evento, el controlador de eventos se pasa como su segundo parámetro un objeto de tipo `GridViewRowEventArgs`, que tiene una propiedad denominada `Row`. Esta propiedad devuelve una referencia a la `GridViewRow` que era simplemente enlazado a datos. Para tener acceso a la `ProductsRow` instancia enlazada a la `GridViewRow` usamos el `DataItem` propiedad así:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Cuando se trabaja con el `RowDataBound` es importante tener en cuenta que el control GridView se compone de diferentes tipos de filas y de que este evento se desencadena para el controlador de eventos *todas* tipos de fila. Un `GridViewRow`del tipo puede determinarse mediante su `RowType` propiedad y puede tener uno de los valores posibles:

- `DataRow` una fila que está enlazada a un registro desde el control de GridView `DataSource`
- `EmptyDataRow` la fila que muestra si la GridView `DataSource` está vacío
- `Footer` la fila de pie de página; mostrado si la GridView `ShowFooter` propiedad está establecida en `True`
- `Header` la fila de encabezado; se muestra si se establece la propiedad de ShowHeader de GridView en `True` (valor predeterminado)
- `Pager` para de GridView que implementan la paginación, la fila que muestra la interfaz de paginación
- `Separator` no se usa para el control GridView, pero usa el `RowType` controles de propiedades para los controles DataList y Repeater, dos controles Web que trataremos en el futuro tutoriales de datos

Puesto que la `EmptyDataRow`, `Header`, `Footer`, y `Pager` filas no están asociadas con un `DataSource` registros, siempre tendrán un valor de `Nothing` para sus `DataItem` propiedad. Por esta razón, antes de intentar trabajar con el actual `GridViewRow`del `DataItem` propiedad, primero debemos asegurarnos de que estamos trabajando con un `DataRow`. Esto puede realizarse mediante la comprobación de la `GridViewRow`del `RowType` propiedad así:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Paso 9: Resaltado de la fila amarillo cuando el valor UnitPrice es inferior a 10,00 USD

El último paso es resaltar mediante programación toda la `GridViewRow` si la `UnitPrice` valor de esa fila es inferior a 10,00 USD. La sintaxis para tener acceso a un GridView filas o celdas es el mismo que con DetailsView `GridViewID.Rows(index)` para tener acceso a toda la fila `GridViewID.Rows(index).Cells(index)` para tener acceso a una celda determinada. Sin embargo, cuando el `RowDataBound` controlador de eventos activa el enlace a datos `GridViewRow` aún tiene que agregarse a la GridView `Rows` colección. Por lo tanto, no se puede obtener acceso a actual `GridViewRow` instancia desde el `RowDataBound` controlador de eventos mediante la colección de filas.

En lugar de `GridViewID.Rows(index)`, podemos hacer referencia a la actual `GridViewRow` de instancia en el `RowDataBound` controlador de eventos mediante `e.Row`. Es decir, la fecha en orden para resaltar actual `GridViewRow` instancia desde el `RowDataBound` se utilizaría el controlador de eventos:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

En lugar de establecer el `GridViewRow`del `BackColor` propiedad directamente, sigamos con el uso de las clases CSS. He creado una clase CSS denominada `AffordablePriceEmphasis` que establece el color de fondo en amarillo. Completado `RowDataBound` sigue el controlador de eventos:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![Los productos más asequible son resaltado amarillo](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Figura 11**: productos asequible la mayoría son resaltado amarillo ([haga clic aquí para ver imagen en tamaño completo](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo dar formato a la GridView, DetailsView y FormView basado en los datos enlazados al control. Para ello hemos creado un controlador de eventos para el `DataBound` o `RowDataBound` eventos, donde los datos subyacentes se examinan junto con un cambio de formato, si es necesario. Para obtener acceso a los datos enlazados a un DetailsView o FormView, usamos el `DataItem` propiedad en el `DataBound` controlador de eventos; para un control GridView, cada `GridViewRow` la instancia `DataItem` propiedad contiene los datos enlazados a esa fila, que está disponible en el `RowDataBound` controlador de eventos.

La sintaxis para ajustar mediante programación el formato del control de datos Web depende el control Web y cómo se muestran los datos que se va a dar formato. Para DetailsView y GridView controles, las filas y celdas pueden tener acceso mediante un índice ordinal. Para FormView, que usa las plantillas, el `FindControl("controlID")` método se suele utilizar para buscar un control Web desde dentro de la plantilla.

En el siguiente tutorial daremos un vistazo a cómo utilizar plantillas con GridView y DetailsView. Además, veremos otra técnica para personalizar el formato basado en los datos subyacentes.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron E.R. Gil, Dennis Patterson y Dan Jagers. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Siguiente](using-templatefields-in-the-gridview-control-vb.md)
