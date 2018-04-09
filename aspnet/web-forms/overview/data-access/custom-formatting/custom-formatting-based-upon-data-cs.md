---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Formato personalizado basado en datos (C#) | Documentos de Microsoft
author: rick-anderson
description: El ajuste del formato de GridView, DetailsView o FormView basado en los datos enlazados a él puede realizarse de varias maneras. En este tutorial te enviaremos un mensaje l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 31cf628baf2250c2e7e71ab38cd64b218dc927e7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="custom-formatting-based-upon-data-c"></a>Formato personalizado basado en datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) o [descarga de PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> El ajuste del formato de GridView, DetailsView o FormView basado en los datos enlazados a él puede realizarse de varias maneras. En este tutorial se examinará cómo llevar a cabo el formato mediante el uso de los controladores de eventos de enlace de datos y RowDataBound de enlazar los datos.


## <a name="introduction"></a>Introducción

La apariencia de los controles GridView, DetailsView y FormView puede personalizarse a través de una gran cantidad de propiedades relacionadas con el estilo. Propiedades, como `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, y `Height`, entre otros, dictar la apariencia general del control representado. Propiedades incluida `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, y otras permiten estos mismos valores de estilo que se aplicará a secciones específicas. Del mismo modo, se puede aplicar esta configuración de estilo en el nivel de campo.

En muchos escenarios, los requisitos de formato dependen el valor de los datos mostrados. Por ejemplo, para llamar la atención para fuera de productos de cotizaciones, un informe que incluya información del producto puede establecer el color de fondo amarillo para los productos cuyo `UnitsInStock` y `UnitsOnOrder` campos son iguales a 0. Para resaltar los productos más caros, podemos queremos mostrar los precios de los productos que cuestan más de $75,00 en una fuente en negrita.

El ajuste del formato de GridView, DetailsView o FormView basado en los datos enlazados a él puede realizarse de varias maneras. En este tutorial se examinará cómo llevar formato mediante el uso de los datos enlazados a la `DataBound` y `RowDataBound` controladores de eventos. En el siguiente tutorial se explorará un enfoque alternativo.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Mediante el Control DetailsView`DataBound`controlador de eventos

Cuando los datos se enlazan a DetailsView, desde un control de origen de datos o mediante la asignación mediante programación los datos para el control `DataSource` propiedad y llamar a su `DataBind()` producirse de método, la siguiente secuencia de pasos:

1. De los datos de control Web `DataBinding` desencadena el evento.
2. Los datos se enlazan a los datos de control Web.
3. De los datos de control Web `DataBound` desencadena el evento.

Puede insertar lógica personalizada inmediatamente después de los pasos 1 y 3 a través de un controlador de eventos. Mediante la creación de un controlador de eventos para el `DataBound` evento podemos determinar mediante programación los datos que se ha enlazado al control Web de datos y ajustar el formato según sea necesario. Para ilustrar esto, vamos a crear un DetailsView que mostrará información general acerca de un producto, sino que mostrará el `UnitPrice` valor en un ***fuente negrita, cursiva*** si supera 75,00 $.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Paso 1: Mostrar la información del producto en DetailsView

Abra el `CustomColors.aspx` página en el `CustomFormatting` carpeta, arrastre un control DetailsView desde el cuadro de herramientas hasta el diseñador, establezca su `ID` valor de propiedad para `ExpensiveProductsPriceInBoldItalic`y enlazarla a un nuevo control ObjectDataSource que invoca el `ProductsBLL` la clase `GetProducts()` método. Los pasos detallados para lograrlo se omiten aquí por razones de brevedad puesto que se examinan en detalle en los tutoriales anteriores.

Una vez que se ha enlazado la ObjectDataSource a DetailsView, tómese un momento para modificar la lista de campos. He optado por eliminar la `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, y `Discontinued` BoundFields y cambia el nombre y volver a formatear el BoundFields restantes. También se ha retirado la `Width` y `Height` configuración. Puesto que DetailsView muestra solamente un único registro, es necesario habilitar la paginación para permitir que el usuario final ver todos los productos. Hacerlo activando la casilla de verificación Habilitar paginación en la etiqueta inteligente de DetailsView.


[![Active la casilla de la paginación de habilitar en la etiqueta inteligente de DetailsView](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Figura 1**: Active la casilla de la paginación de habilitar en la etiqueta inteligente de DetailsView ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image3.png))


Después de estos cambios, el marcado de DetailsView será:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Tómese un momento para probar esta página en el explorador.


[![El Control DetailsView muestra uno de los productos a la vez](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Figura 2**: The DetailsView Control muestra uno de los productos a la vez ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Paso 2: Determinar mediante programación el valor de los datos en el controlador de eventos de enlace de datos

Para mostrar el precio de una fuente negrita, cursiva para aquellos productos cuyo `UnitPrice` supera el valor $75,00, necesitamos poder determinar mediante programación el `UnitPrice` valor. Para DetailsView, esto puede realizarse en el `DataBound` controlador de eventos. Para crear el evento controlador haga clic en DetailsView en el diseñador, a continuación, navegue a la ventana Propiedades. Presione F4 para mostrar, si no está visible, o vaya al menú Ver y seleccione la opción de menú de la ventana Propiedades. En la ventana Propiedades, haga clic en el icono de rayo para mostrar eventos de DetailsView. A continuación, o bien haga doble clic en el `DataBound` eventos o escriba el nombre del controlador de eventos que desea crear.


![Crear un controlador de eventos para el evento de enlace de datos](custom-formatting-based-upon-data-cs/_static/image7.png)

**Figura 3**: crear un controlador de eventos para el `DataBound` eventos


Si lo hace, creará automáticamente el controlador de eventos y llevará a la parte de código donde se ha agregado. En este momento se muestra:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Pueden tener acceso a los datos enlazados a DetailsView a través de la `DataItem` propiedad. Recuerde que estamos enlazando los controles a un objeto DataTable fuertemente tipado, que se compone de una colección de instancias de DataRow fuertemente tipada. Cuando la tabla de datos se enlaza a DetailsView, la primera fila de datos en la tabla de datos se asigna a la DetailsView `DataItem` propiedad. En concreto, el `DataItem` propiedad se asigna un `DataRowView` objeto. Podemos usar la `DataRowView`del `Row` propiedad que se va a obtener acceso al objeto DataRow subyacente, que es realmente un `ProductsRow` instancia. Una vez que tenemos esto `ProductsRow` ayudarnos a nuestra decisión simplemente inspeccionando los valores de propiedad del objeto de instancia.

El código siguiente muestra cómo determinar si el `UnitPrice` valor enlazado al control DetailsView es mayor que $75,00:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Puesto que `UnitPrice` puede tener un `NULL` valor en la base de datos, primero comprobamos para asegurarse de que no estamos trabajando con un `NULL` valor antes de acceder a la `ProductsRow`del `UnitPrice` propiedad. Esta comprobación es importante porque si se intenta tener acceso a la `UnitPrice` propiedad cuando tiene un `NULL` valor la `ProductsRow` objeto producirá un [StrongTypingException excepción](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Paso 3: Dar formato al valor de UnitPrice en DetailsView

En este punto podemos determinar si la `UnitPrice` valor enlazado a DetailsView tiene un valor superior a $75,00, pero hemos todavía ver cómo ajustar mediante programación el DetailsView del formato en consecuencia. Para modificar el formato de una fila completa de DetailsView, acceso mediante programación a la fila mediante `DetailsViewID.Rows[index]`; para modificar una celda determinada, el acceso utilizan `DetailsViewID.Rows[index].Cells[index]`. Una vez que tenemos una referencia a la fila o celda, a continuación, podemos ajustar su apariencia estableciendo sus propiedades relacionadas con el estilo.

Acceso mediante programación a una fila requiere que sepa el índice de la fila, que comienza en 0. El `UnitPrice` fila es el quinto de DetailsView, dándole un índice de 4 y lo que puede tener acceso mediante programación utilizando `ExpensiveProductsPriceInBoldItalic.Rows[4]`. En este momento tenemos podríamos contenido de la fila completa que se muestra en una fuente negrita, cursiva mediante el código siguiente:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Sin embargo, esto hará que *ambos* la etiqueta (precio) y el valor de negrita y cursiva. Si queremos realizar solamente el valor de negrita y cursiva que es necesario aplicar este formato a la segunda celda de la fila, que puede realizar mediante los siguientes:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Puesto que nuestros tutoriales hasta ahora han utilizado las hojas de estilos para mantener una separación clara entre el marcado representado e información relacionada con el estilo, en lugar de establecer las propiedades de estilo en particular, como se indicó anteriormente vamos a en su lugar, utilice una clase CSS. Abra la `Styles.css` hoja de estilos y agregue una nueva clase CSS denominada `ExpensivePriceEmphasis` con la siguiente definición:


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

A continuación, en la `DataBound` controlador de eventos, establecer la celda `CssClass` propiedad `ExpensivePriceEmphasis`. El código siguiente muestra el `DataBound` controlador de eventos en su totalidad:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Cuando se visualizan Chai, lo que cuesta menos de 75,00 $, el precio se muestra en una fuente normal (consulte la figura 4). Sin embargo, cuando se visualizan buey Mishi Kobe Niku, que tiene un precio de $97.00, el precio se muestra en una fuente negrita, cursiva (Véase la figura 5).


[![Los precios inferior $75,00 se muestran en una fuente Normal](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Figura 4**: precios inferior $75,00 se muestran en una fuente Normal ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Los precios de los productos de la cara se muestran en un negrita, cursiva fuente](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Figura 5**: se muestran los precios de los productos de la cara en un negrita, cursiva fuente ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Usar el Control FormView`DataBound`controlador de eventos

Los pasos para determinar los datos subyacentes que se enlaza a un FormView son idénticos a las de DetailsView crear un `DataBound` controlador de eventos, convierte el `DataItem` enlazados al control de propiedad para el tipo de objeto adecuado y determinar cómo continuar. El FormView y DetailsView son diferentes, sin embargo, en cómo se actualiza la apariencia de la interfaz de usuario.

FormView no contiene ningún BoundFields y, por tanto, no tiene la `Rows` colección. En su lugar, un FormView se compone de plantillas, que pueden contener una combinación de HTML estático, controles Web y la sintaxis de enlace de datos. Ajustar el estilo de un FormView normalmente implica ajustar el estilo de uno o varios de los controles Web dentro de las plantillas de FormView.

Para ilustrar esto, vamos a usar un FormView para productos de la lista, como en el ejemplo anterior, pero esta vez vamos a mostrar simplemente el nombre del producto y las unidades en existencias con las unidades en existencias muestra en una fuente de color rojo si es menor o igual a 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Paso 4: Mostrar la información del producto en un FormView

Agregar un FormView a la `CustomColors.aspx` página situada bajo la DetailsView y establezca su `ID` propiedad `LowStockedProductsInRed`. Enlazar el FormView al control ObjectDataSource creado en el paso anterior. Esto creará un `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate` de FormView. Quitar el `EditItemTemplate` y `InsertItemTemplate` y simplificar la `ItemTemplate` para incluir solo los `ProductName` y `UnitsInStock` valores, cada uno de sus propios controles de etiqueta con un nombre adecuado. Al igual que con DetailsView del ejemplo anterior, también activa la casilla Habilitar paginación en la etiqueta inteligente de FormView.

Después de estas modificaciones marcado de su FormView debe ser similar al siguiente:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Tenga en cuenta que el `ItemTemplate` contiene:

- **HTML estático** el texto "producto:" y "unidades en existencia:" junto con el `<br />` y `<b>` elementos.
- **Controles Web** los dos controles de etiqueta, `ProductNameLabel` y `UnitsInStockLabel`.
- **Sintaxis de enlace de datos** el `<%# Bind("ProductName") %>` y `<%# Bind("UnitsInStock") %>` sintaxis, que asigna los valores de estos campos a los controles de etiqueta `Text` propiedades.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Paso 5: Determinar mediante programación el valor de los datos en el controlador de eventos de enlace de datos

Con el marcado de FormView completando, el paso siguiente consiste en determinar mediante programación si la `UnitsInStock` valor es menor o igual a 10. Esto se logra de la misma manera exacta con FormView tal como estaba con DetailsView. Empiece por crear un controlador de eventos para el FormView `DataBound` eventos.


![Crear el controlador de eventos de enlace de datos](custom-formatting-based-upon-data-cs/_static/image14.png)

**Figura 6**: crear el `DataBound` controlador de eventos


En el evento controlador convierte lo FormView `DataItem` propiedad a una `ProductsRow` instancia y determinar si la `UnitsInPrice` valor es tal que es necesario para que se muestre en una fuente de color rojo.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Paso 6: Dar formato al Control de etiqueta de UnitsInStockLabel en ItemTemplate de FormView

El paso final consiste en dar formato a la muestra `UnitsInStock` el valor en una fuente de color rojo si el valor es 10 o menos. Para ello es necesario tener acceso mediante programación el `UnitsInStockLabel` controlar en el `ItemTemplate` y establecer sus propiedades de estilo para que el texto se muestre en rojo. Para obtener acceso a un control Web en una plantilla, use el `FindControl("controlID")` método similar al siguiente:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

En nuestro ejemplo desea tener acceso a una etiqueta de control cuya `ID` valor es `UnitsInStockLabel`, por lo que se utilizaría:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Una vez que tenemos una referencia mediante programación al control Web, se podrá modificar sus propiedades relacionadas con el estilo según sea necesario. Como con el ejemplo anterior, he creado una clase CSS en `Styles.css` denominado `LowUnitsInStockEmphasis`. Para aplicar este estilo al control Web Label, establezca su `CssClass` propiedad según corresponda.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> La sintaxis para dar formato a una plantilla de acceso mediante programación el control de Web mediante `FindControl("controlID")` y, a continuación, establecer sus propiedades relacionadas con el estilo también puede utilizarse cuando se usa [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) en el DetailsView o GridView controles. Examinaremos TemplateFields en nuestro tutorial siguiente.


Las figuras 7 muestra FormView al ver un producto cuyo `UnitsInStock` valor es mayor que 10, mientras que el producto en la figura 8 tiene su valor menor que 10.


[![Para productos con unos suficientemente grandes Units In Stock, se aplica sin formato personalizado](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Figura 7**: para productos con unos suficientemente grandes Units In Stock, sin formato personalizado se aplica ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image17.png))


[![Las unidades en existencias número se muestra en rojo para los productos con valores de 10 o menos](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Figura 8**: The unidades en existencias número se muestra en rojo para los productos con valores de 10 o menos ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formato con la GridView`RowDataBound`eventos

Anteriormente se examina la secuencia de pasos DetailsView y FormView controla el progreso a través durante el enlace de datos. Echemos un vistazo a través de estos pasos una vez más como una actualización.

1. De los datos de control Web `DataBinding` desencadena el evento.
2. Los datos se enlazan a los datos de control Web.
3. De los datos de control Web `DataBound` desencadena el evento.

Estos tres pasos simples son suficientes para la DetailsView y FormView porque se muestre solo un único registro. Del control GridView, que muestra *todos los* registros enlazados a él (no solo la primera), el paso 2 es un poco más complicado.

En el paso 2 GridView enumera el origen de datos y, para cada registro, crea un `GridViewRow` de instancia y el registro actual se enlaza a él. Para cada `GridViewRow` agrega a GridView, se generan dos eventos:

- **`RowCreated`** se activa tras la `GridViewRow` se ha creado
- **`RowDataBound`** se desencadena después de que el registro actual se ha enlazado a la `GridViewRow`.

Del control GridView, a continuación, enlace de datos con más precisión describe la secuencia de pasos siguiente:

1. La GridView `DataBinding` desencadena el evento.
2. Los datos se enlazan a la GridView.   
  
   Para cada registro del origen de datos 

    1. Crear un `GridViewRow` objeto
    2. Activar la `RowCreated` eventos
    3. Enlazar el registro de la `GridViewRow`
    4. Activar la `RowDataBound` eventos
    5. Agregar el `GridViewRow` a la `Rows` colección
3. La GridView `DataBound` desencadena el evento.

Para personalizar el formato de los registros individuales de GridView, a continuación, se debe crear un controlador de eventos para el `RowDataBound` eventos. Para ilustrar esto, vamos a agregar un control GridView a la `CustomColors.aspx` página que enumera el nombre, la categoría y el precio de cada producto, resaltado de aquellos productos cuyo precio es inferior a 10,00 USD, con un color de fondo amarillo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Paso 7: Mostrar información del producto en un control GridView

Agregue un control GridView debajo FormView del ejemplo anterior y establezca su `ID` propiedad `HighlightCheapProducts`. Puesto que ya tenemos un ObjectDataSource que devuelve todos los productos en la página, enlazar el control GridView a la. Por último, edite BoundFields de GridView para incluir solo nombres los productos, categorías y los precios. Después de estas modificaciones marcado de GridView debería ser similar:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

La figura 9 muestra nuestro progreso hasta este punto cuando se ven a través de un explorador.


[![GridView muestra el nombre, la categoría y el precio de cada producto](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Figura 9**: GridView muestra el nombre, la categoría y el precio de cada producto ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Paso 8: Determinar mediante programación el valor de los datos en el controlador de eventos de RowDataBound

Cuando el `ProductsDataTable` está enlazado a la GridView su `ProductsRow` instancias son enumerado y for each `ProductsRow` un `GridViewRow` se crea. El `GridViewRow`del `DataItem` propiedad se asigna a ese `ProductRow`, tras el cual la GridView `RowDataBound` se genera el controlador de eventos. Para determinar el `UnitPrice` valor para cada producto que enlaza a GridView, a continuación, necesitamos crear un controlador de eventos del control de GridView `RowDataBound` eventos. En este controlador de eventos podemos inspeccionar la `UnitPrice` valor actuales `GridViewRow` y tomar una decisión de formato para esa fila.

Este controlador de eventos puede crearse con la misma serie de pasos como FormView y DetailsView.


![Crear un controlador de eventos para eventos de RowDataBound de GridView](custom-formatting-based-upon-data-cs/_static/image24.png)

**Figura 10**: crear un controlador de eventos del control de GridView `RowDataBound` eventos


Crear el controlador de eventos de esta manera se producirá el siguiente código al que se agregarán automáticamente a la parte correspondiente al código de la página ASP.NET:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Cuando el `RowDataBound` desencadena el evento, el controlador de eventos se pasa como su segundo parámetro de un objeto de tipo `GridViewRowEventArgs`, que tiene una propiedad denominada `Row`. Esta propiedad devuelve una referencia a la `GridViewRow` que estaba enlazado a datos únicamente. Para tener acceso a la `ProductsRow` instancia enlazada a la `GridViewRow` usamos el `DataItem` propiedad de este modo:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Cuando se trabaja con el `RowDataBound` controlador de eventos es importante tener en cuenta que el control GridView está compuesto de diferentes tipos de filas y que este evento se desencadena para *todos los* tipos de fila. A `GridViewRow`del tipo se puede determinar mediante su `RowType` propiedad y puede tener uno de los valores posibles:

- `DataRow` una fila que está enlazada a un registro de la GridView `DataSource`
- `EmptyDataRow` la fila que se muestra si la GridView `DataSource` está vacío
- `Footer` la fila de pie de página; if se muestra la GridView `ShowFooter` propiedad está establecida en `true`
- `Header` la fila de encabezado; se muestra si se establece la propiedad de ShowHeader de GridView en `true` (valor predeterminado)
- `Pager` para de GridView que implementan la paginación, la fila que muestra la interfaz de paginación
- `Separator` no se utiliza para el control GridView, pero usa el `RowType` propiedades de DataList y repetidor controla, dos controles Web trataremos en el futuro tutoriales de datos

Puesto que la `EmptyDataRow`, `Header`, `Footer`, y `Pager` filas no están asociadas con un `DataSource` registro, siempre tendrán un `null` valor para sus `DataItem` propiedad. Por este motivo, antes de intentar trabajar con actual `GridViewRow`del `DataItem` propiedad, primero debemos asegurarnos de que estamos trabajando con un `DataRow`. Esto puede realizarse mediante la comprobación de la `GridViewRow`del `RowType` propiedad de este modo:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Paso 9: Resaltado de la fila amarillo cuando el valor UnitPrice es menor que los 10,00 USD

El último paso consiste en Resaltar mediante programación toda la `GridViewRow` si la `UnitPrice` valor de esa fila es menor que los 10,00 USD. La sintaxis para tener acceso a un GridView filas o celdas es igual que con las DetailsView `GridViewID.Rows[index]` para tener acceso a toda la fila, `GridViewID.Rows[index].Cells[index]` para tener acceso a una celda determinada. Sin embargo, cuando la `RowDataBound` controlador de eventos activa el enlace a datos `GridViewRow` todavía tiene que ser agregada a la GridView `Rows` colección. Por lo tanto, no puede acceder a la actual `GridViewRow` instancia desde el `RowDataBound` controlador de eventos mediante la colección de filas.

En lugar de `GridViewID.Rows[index]`, podemos hacer referencia a la actual `GridViewRow` de instancia de la `RowDataBound` controlador de eventos mediante `e.Row`. Es decir, en orden para resaltar actual `GridViewRow` instancia desde el `RowDataBound` se utilizaría el controlador de eventos:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

En lugar de establecer la `GridViewRow`del `BackColor` propiedad directamente, vamos a pegar con el uso de clases CSS. He creado una clase CSS denominada `AffordablePriceEmphasis` que establece el color de fondo en amarillo. Completado `RowDataBound` sigue el controlador de eventos:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![Los productos más asequible son amarillo resaltado](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Figura 11**: productos asequibles el máximo son amarillo resaltado ([haga clic aquí para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo dar formato a la GridView, DetailsView y FormView en función de los datos enlazados al control. Para ello creamos un controlador de eventos para el `DataBound` o `RowDataBound` eventos, donde los datos subyacentes se examinan junto con un cambio de formato, si es necesario. Para obtener acceso a los datos enlazados a un DetailsView o FormView, usamos el `DataItem` propiedad en el `DataBound` controlador de eventos; para un control GridView, cada `GridViewRow` la instancia `DataItem` propiedad contiene los datos enlazados a esa fila, que está disponible en la `RowDataBound` controlador de eventos.

La sintaxis para ajustar mediante programación el control de Web de datos de formato depende del control de Web y cómo se muestran los datos que se va a tener el formato. Para DetailsView y GridView controles, las filas y celdas pueden tener acceso mediante un índice ordinal. Para el FormView, que usa las plantillas, el `FindControl("controlID")` método normalmente se utiliza para buscar un control Web desde dentro de la plantilla.

En el siguiente tutorial se examinará cómo utilizar plantillas con el control GridView y DetailsView. Además, veremos otra técnica utilizada para personalizar el formato en función de los datos subyacentes.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Provocar a revisores para este tutorial son E.R. Gil, Dennis Patterson y Dan Jagers. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](using-templatefields-in-the-gridview-control-cs.md)
