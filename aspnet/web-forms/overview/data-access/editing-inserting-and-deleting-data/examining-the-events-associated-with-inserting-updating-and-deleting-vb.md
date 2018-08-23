---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Examinar los eventos asociados con la inserción, actualización y eliminación (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial que se examinarán con los eventos que se producen antes, durante y después de una inserción, actualizar o eliminar la operación de un control Web de datos ASP.NET. SITIO WEB
ms.author: riande
ms.date: 07/17/2006
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 71ae661ade23d18ebd302e2902f1094d61ce968f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836299"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Examinar los eventos relacionados con la inserción, actualización y eliminación (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) o [descargar PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> En este tutorial que se examinarán con los eventos que se producen antes, durante y después de una inserción, actualizar o eliminar la operación de un control Web de datos ASP.NET. También verá cómo personalizar la interfaz de edición para actualizar solo un subconjunto de los campos de producto.


## <a name="introduction"></a>Introducción

Cuando se usa el integradas de inserción, editar o eliminar las características de los controles GridView, DetailsView o FormView, una variedad de pasos tienen lugar cuando el usuario final complete el proceso de agregar un nuevo registro o actualizar o eliminar un registro existente. Como se explicó en la [tutorial anterior](an-overview-of-inserting-updating-and-deleting-data-vb.md), cuando se edita una fila en el control GridView el botón de edición se reemplaza por botones Actualizar y Cancelar y activar BoundFields en cuadros de texto. Después de que el usuario final actualiza los datos y hace clic en la actualización, los pasos siguientes se realizan en el postback:

1. El control GridView rellena su ObjectDataSource `UpdateParameters` con campos de identificación único del registro modificada (a través de la `DataKeyNames` propiedad) junto con los valores especificados por el usuario
2. El control GridView invoca su ObjectDataSource `Update()` método, que a su vez invoca el método adecuado en el objeto subyacente (`ProductsDAL.UpdateProduct`, en nuestro tutorial anterior)
3. Se vuelve a enlazar los datos subyacentes, que ahora incluyen los cambios actualizados, en el control GridView

Durante esta secuencia de pasos, un número de eventos se activan, lo que nos permite crear controladores de eventos para agregar lógica personalizada cuando sea necesario. Por ejemplo, antes de paso 1, el control GridView `RowUpdating` desencadena el evento. En este punto, nos podemos, cancelar la solicitud de actualización si se produce algún error de validación. Cuando el `Update()` se invoca al método, el ObjectDataSource `Updating` desencadena el evento, dando la oportunidad de agregar o personalizar los valores de cualquiera de los `UpdateParameters`. Después de que el origen ObjectDataSource subyacente del método del objeto ha terminado de ejecutarse, el ObjectDataSource `Updated` provoca el evento. Un controlador de eventos para el `Updated` eventos pueden inspeccionar los detalles sobre la operación de actualización, como el número de filas afectado y si se produjo una excepción. Por último, después del paso 2, el control GridView `RowUpdated` desencadena el evento; un controlador de eventos para este evento se puede examinar la información adicional acerca de la operación de actualización solo realiza.

Figura 1 se muestra esta serie de eventos y pasos al actualizar un control GridView. El patrón de eventos en la figura 1 no es único a la actualización de un control GridView. Insertar, actualizar o eliminar datos de GridView, DetailsView o FormView precipita la misma secuencia de eventos previos y posteriores al niveles para el control de datos Web y el origen ObjectDataSource.


[![Una serie de previo y Fire de eventos posteriores al actualizar datos en un control GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Figura 1**: una serie de previas y eventos posteriores Fire al actualizar los datos de un control GridView ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


En este tutorial que examinaremos usa estos eventos para extender las integradas de inserción, actualización y eliminación de las capacidades de los datos ASP.NET Web controla. También verá cómo personalizar la interfaz de edición para actualizar solo un subconjunto de los campos de producto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Paso 1: Actualizar un producto`ProductName`y`UnitPrice`campos

En las interfaces de edición desde el tutorial anterior *todas* campos de producto que no eran de solo lectura tenía que se incluirán. Si tuviéramos que quitar un campo de GridView - diga `QuantityPerUnit` : al actualizar los datos el control Web de datos no establecería el ObjectDataSource `QuantityPerUnit` `UpdateParameters` valor. ObjectDataSource, a continuación, pasar un valor de `Nothing` en el `UpdateProduct` método de la capa de lógica empresarial (BLL), que cambiaría el registro de base de datos editado `QuantityPerUnit` columna a una `NULL` valor. De forma similar, si un campo obligatorio, como `ProductName`, se quita de la interfaz de edición, se producirá un error de la actualización con un "*columna 'ProductName' no permite valores NULL*" excepción. El motivo de este comportamiento era porque el origen ObjectDataSource se configuró para llamar a la `ProductsBLL` la clase `UpdateProduct` método, que se esperaba un parámetro de entrada para cada uno de los campos de producto. Por lo tanto, el origen ObjectDataSource `UpdateParameters` colección contiene un parámetro para cada uno de lo método de parámetros de entrada.

Si desea proporcionar un control Web que permite al usuario final actualizar solo un subconjunto de campos de datos, es necesario o bien establecer mediante programación la falta `UpdateParameters` valores en el ObjectDataSource `Updating` controlador de eventos o crear e invocar un método BLL que espera a que solo un subconjunto de los campos. Veamos este último enfoque.

En concreto, vamos a crear una página que muestra solamente el `ProductName` y `UnitPrice` campos en un control GridView editable. Interfaz de edición de esta GridView sólo permitirá al usuario actualizar los campos que se muestran dos, `ProductName` y `UnitPrice`. Puesto que esta interfaz de edición proporciona solo un subconjunto de campos de un producto, o bien es necesario crear un origen ObjectDataSource que usa la capa BLL existente `UpdateProduct` método y que los valores de campo que faltan de producto establecido mediante programación en su `Updating` eventos controlador, o bien deberá crear un nuevo método BLL que espera que sólo el subconjunto de campos definidos en el control GridView. Para este tutorial, vamos a usar la última opción y crear una sobrecarga de la `UpdateProduct` método, que tarda en sólo tres parámetros de entrada: `productName`, `unitPrice`, y `productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Al igual que el original `UpdateProduct` esta sobrecarga de método, comienza comprobando para ver si hay un producto en la base de datos con los valores especificados `ProductID`. Si no, devuelve `False`, que indica que el error en la solicitud para actualizar la información del producto. En caso contrario, actualiza el registro de producto existente `ProductName` y `UnitPrice` campos según corresponda y confirma la actualización mediante una llamada a la TableAdpater `Update()` método, pasando el `ProductsRow` instancia.

Con esta adición a nuestro `ProductsBLL` (clase), estamos listos para crear la interfaz de GridView simplificada. Abra el `DataModificationEvents.aspx` en el `EditInsertDelete` carpeta y agregue un control GridView a la página. Crear un nuevo origen ObjectDataSource y configúrelo para utilizar el `ProductsBLL` clase con su `Select()` asignación del método a `GetProducts` y su `Update()` asignación del método a la `UpdateProduct` sobrecarga que toma solo en el `productName`, `unitPrice`, y `productID` parámetros de entrada. La figura 2 muestra el Asistente para crear orígenes de datos al asignar el ObjectDataSource `Update()` método a la `ProductsBLL` la nueva clase `UpdateProduct` sobrecarga del método.


[![Map (método) Update() de ObjectDataSource a la nueva sobrecarga UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Figura 2**: asignación de ObjectDataSource `Update()` método el nuevo `UpdateProduct` sobrecargar ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


Puesto que nuestro ejemplo inicialmente solo necesitarán la capacidad de modificar datos, pero no permite insertar o eliminar registros, dedique un momento para indicar explícitamente que el ObjectDataSource `Insert()` y `Delete()` métodos no deben asignarse a cualquiera de los `ProductsBLL` métodos de la clase, vaya a las pestañas INSERT y DELETE y elegir (ninguno) en la lista desplegable.


[![Elija (ninguno) en la lista desplegable para las fichas DELETE y INSERT](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Figura 3**: elija (ninguno) de la lista desplegable para la INSERCIÓN y eliminar pestañas ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


Después de completar este asistente Active la casilla Habilitar edición de etiquetas inteligentes de GridView.

Con la finalización del Asistente Crear origen de datos y enlazar a GridView, Visual Studio ha creado la sintaxis declarativa para ambos controles. Vaya a la vista de origen para inspeccionar el marcado declarativo de ObjectDataSource, que se indican a continuación:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Puesto que existen asignaciones para el origen de ObjectDataSource `Insert()` y `Delete()` métodos, hay ninguna `InsertParameters` o `DeleteParameters` secciones. Además, desde el `Update()` método se asigna a la `UpdateProduct` sobrecarga del método que solo acepta tres parámetros de entrada, el `UpdateParameters` sección tiene solo tres `Parameter` instancias.

Tenga en cuenta que el ObjectDataSource `OldValuesParameterFormatString` propiedad está establecida en `original_{0}`. Esta propiedad se establece automáticamente por Visual Studio cuando se usa al Asistente para configurar orígenes de datos. Sin embargo, puesto que nuestros métodos BLL no espera que el original `ProductID` valor que se pasa, quite esta asignación de propiedad por completo de la sintaxis declarativa de ObjectDataSource.

> [!NOTE]
> Si simplemente borre el `OldValuesParameterFormatString` seguirá existiendo en la sintaxis declarativa del valor de propiedad de la ventana de propiedades en la vista Diseño, la propiedad, pero se establecerá en una cadena vacía. Quite la propiedad completamente de la sintaxis declarativa o, en la ventana Propiedades, establezca el valor en el valor predeterminado, `{0}`.


Mientras que solo tiene el origen ObjectDataSource `UpdateParameters` para el producto nombre, precio y el identificador, Visual Studio ha agregado un BoundField o CampoCasillaVerificación en GridView para cada uno de los campos del producto.


[![GridView contiene un BoundField o CampoCasillaVerificación para cada uno de los campos del producto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Figura 4**: GridView contiene un BoundField o CampoCasillaVerificación para cada uno de los campos del producto ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


Cuando el usuario final se edita un producto y hace clic en el botón de actualización, el control GridView enumera los campos que no eran de solo lectura. A continuación, Establece el valor del parámetro correspondiente en el ObjectDataSource `UpdateParameters` colección en el valor especificado por el usuario. Si no es un parámetro correspondiente, el control GridView suma uno a la colección. Por lo tanto, si nuestra GridView contiene BoundFields y CheckBoxFields para todos los campos del producto, el origen ObjectDataSource terminará invocar el `UpdateProduct` sobrecarga que toma en todos estos parámetros, a pesar de que el ObjectDataSource marcado declarativo especifica sólo tres parámetros de entrada (consulte la figura 5). De forma similar, si hay alguna combinación de los que no son de solo lectura producto campos en el control GridView que no se corresponde con los parámetros de entrada para un `UpdateProduct` sobrecarga, se producirá una excepción al intentar actualizar.


[![La GridView se realizará agregar parámetros a la colección de ObjectDataSource UpdateParameters](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Figura 5**: The GridView le agregar parámetros de ObjectDataSource `UpdateParameters` colección ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


Para asegurarse de que el origen ObjectDataSource invoca el `UpdateProduct` sobrecarga que toma solo nombre del producto, precio y ID, se necesita restringir el control GridView a tener campos modificables para simplemente el `ProductName` y `UnitPrice`. Esto puede realizarse mediante la eliminación de los demás BoundFields y CheckBoxFields, estableciendo los otros campos `ReadOnly` propiedad `True`, o alguna combinación de ambos. En este tutorial vamos a basta con quitar todos los campos de GridView, excepto el `ProductName` y `UnitPrice` BoundFields, tras el cual marcado declarativo de GridView tendrá el siguiente aspecto:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Aunque el `UpdateProduct` sobrecarga espera tres parámetros de entrada, sólo tenemos dos BoundFields en nuestra GridView. Esto es porque el `productID` parámetro de entrada es un valor de clave principal y pasa a través del valor de la `DataKeyNames` propiedad para la fila editada.

Nuestro GridView, junto con el `UpdateProduct` sobrecarga, permite al usuario editar únicamente el nombre y el precio de un producto sin perder ninguno de los demás campos de producto.


[![La interfaz permite simplemente el producto nombre y el precio de edición](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Figura 6**: la interfaz permite que simplemente el producto nombre y el precio de edición ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> Como se describe en el tutorial anterior, es sumamente importante que el control GridView s estado de vista habilitado (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, corre el riesgo de tener usuarios simultáneos que se eliminen o modifiquen de forma no intencionada de registros. Consulte [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView y DetailsView/FormViews esa compatibilidad de edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.


## <a name="improving-theunitpriceformatting"></a>Mejorar la`UnitPrice`formato

Aunque el ejemplo de GridView que se muestra en la figura 6 funciona, el `UnitPrice` campo no tiene el formato en absoluto, lo que resulta en una pantalla de precio que carece de una moneda símbolos y tiene cuatro decimales. Para aplicar una formato para las filas que no son editables de moneda, basta con establecer la `UnitPrice` del BoundField `DataFormatString` propiedad `{0:c}` y su `HtmlEncode` propiedad `False`.


[![Establezca el UnitPrice DataFormatString y HtmlEncode propiedades según corresponda](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Figura 7**: establecer el `UnitPrice`del `DataFormatString` y `HtmlEncode` propiedades según corresponda ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


Con este cambio, las filas que no son editables formatear el precio como una moneda; Sin embargo, la fila editada, aún muestra el valor con cuatro decimales y sin el símbolo de moneda.


[![Las filas que no son editables son ahora con formato como valores de moneda](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Figura 8**: las filas que no son editables son ahora con formato de los valores de moneda ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


Las instrucciones de formato especificadas en el `DataFormatString` propiedad se puede aplicar a la interfaz de edición estableciendo el BoundField `ApplyFormatInEditMode` propiedad `True` (el valor predeterminado es `False`). Dedique un momento para establecer esta propiedad en `True`.


[![Establecer propiedad de ApplyFormatInEditMode de UnitPrice BoundField en True](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Figura 9**: establecer el `UnitPrice` del BoundField `ApplyFormatInEditMode` propiedad `True` ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


Con este cambio, el valor de la `UnitPrice` aparece en el texto editado también tiene formato de fila como una moneda.


[![Valor de la fila editada UnitPrice es ahora el formato como una moneda](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Figura 10**: The editar la fila `UnitPrice` valor es ahora el formato como una moneda ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


Sin embargo, actualizar un producto con el símbolo de moneda en el cuadro de texto, como $19.00 produce un `FormatException`. Cuando el control GridView intenta asignar los valores proporcionados por el usuario a de ObjectDataSource `UpdateParameters` colección no puede convertir el `UnitPrice` cadena "19.00$" en el `Decimal` requerido por el parámetro (consulte la figura 11). Para solucionar este problema podemos crear un controlador de eventos para el control de GridView `RowUpdating` evento y hacer que analizar el proporcionado por el usuario `UnitPrice` como un formato de moneda `Decimal`.

La GridView `RowUpdating` eventos acepta como su segundo parámetro de un objeto de tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), que incluye un `NewValues` diccionario como uno de sus propiedades que contiene los valores proporcionados por el usuario listos para ser asignado a de ObjectDataSource `UpdateParameters` colección. Nos podemos sobrescribir la existente `UnitPrice` valor en el `NewValues` colección con un valor decimal analiza con el formato de moneda con las siguientes líneas de código en el `RowUpdating` controlador de eventos:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Si el usuario especificó un `UnitPrice` valor (por ejemplo, "$ las 19: 00"), este valor lo sobrescribe con el valor decimal calculado por [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analizar el valor como una moneda. Esto analizará correctamente la coma decimal en el caso de los símbolos de moneda, comas, puntos decimales y así sucesivamente y usa el [enumeración NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) en el [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) espacio de nombres.

Figura 11 muestra tanto el problema causado por símbolos de moneda de la proporcionada por el usuario `UnitPrice`, junto con cómo la GridView `RowUpdating` controlador de eventos puede utilizarse para analizar correctamente dicha entrada.


[![Valor de la fila editada UnitPrice es ahora el formato como una moneda](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Figura 11**: The editar la fila `UnitPrice` valor es ahora el formato como una moneda ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Paso 2: prohibición`NULL UnitPrices`

Mientras la base de datos está configurado para permitir `NULL` valores en el `Products` la tabla `UnitPrice` columna, nos conviene evitar que los usuarios visitar esta página concreta de especificar un `NULL` `UnitPrice` valor. Es decir, si un usuario no especifica un `UnitPrice` valor cuando se edita una fila del producto, en lugar de guardar los resultados en la base de datos que queremos mostrar un mensaje que informa al usuario que, a través de esta página, los productos editados deben tener un precio especificado.

El `GridViewUpdateEventArgs` objeto pasa a la GridView `RowUpdating` controlador de eventos contiene un `Cancel` propiedad que, si establece en `True`, finaliza el proceso de actualización. Vamos a ampliar el `RowUpdating` controlador de eventos para establecer `e.Cancel` a `True` y mostrar un mensaje que explique el motivo si el `UnitPrice` valor en el `NewValues` colección tiene un valor de `Nothing`.

Empiece agregando un control Web de la etiqueta a la página llamada `MustProvideUnitPriceMessage`. Este control de etiqueta se mostrará si el usuario no puede especificar un `UnitPrice` valor al actualizar un producto. Establezca la etiqueta `Text` propiedad "Debe proporcionar un precio para el producto". También he creado una nueva clase CSS en `Styles.css` denominado `Warning` con la siguiente definición:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Por último, establezca la etiqueta `CssClass` propiedad `Warning`. En este momento el diseñador debe mostrar el mensaje de advertencia en un color rojo, negrita, cursiva, el tamaño de fuente extra grande por encima del control GridView, tal como se muestra en la figura 12.


[![Se ha agregado una etiqueta por encima del control GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Figura 12**: una etiqueta tiene se ha agregado anteriormente GridView ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


De forma predeterminada, esta etiqueta debe ocultarse, así que establezca su `Visible` propiedad `False` en el `Page_Load` controlador de eventos:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Si el usuario intenta actualizar un producto sin especificar el `UnitPrice`, desea cancelar la actualización y mostrar la etiqueta de advertencia. Aumentar la GridView `RowUpdating` controlador de eventos como sigue:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Si un usuario intenta guardar un producto sin especificar un precio, se cancela la actualización y se muestra un mensaje útil. Mientras la base de datos (y la lógica de negocios) permite `NULL` `UnitPrice` s, esta página ASP.NET determinada no lo hace.


[![Un usuario no puede dejar en blanco UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Figura 13**: un usuario no puede abandonar `UnitPrice` en blanco ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


Hasta ahora hemos visto cómo usar la GridView `RowUpdating` evento para modificar mediante programación los valores de parámetro asignados a de ObjectDataSource `UpdateParameters` colección así como la forma para cancelar la actualización de procesar por completo. Estos conceptos se transmitirán a los controles DetailsView y FormView y también se aplican a la inserción y eliminación.

Estas tareas también pueden realizarse en el nivel de ObjectDataSource mediante controladores de eventos para su `Inserting`, `Updating`, y `Deleting` eventos. Estos eventos se activan antes de invoca el método asociado del objeto subyacente y proporcionan una última oportunidad para modificar la colección de parámetros de entrada o cancelar la operación completa. Los controladores de eventos para estos tres eventos se le pasa un objeto de tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) que tiene dos propiedades de interés:

- [Cancelar](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), que, si establece en `True`, cancela la operación se realiza
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), que es la colección de `InsertParameters`, `UpdateParameters`, o `DeleteParameters`, dependiendo de si es el controlador de eventos para el `Inserting`, `Updating`, o `Deleting` eventos

Para ilustrar cómo trabajar con los valores de parámetro en el nivel de origen ObjectDataSource, vamos a incluir un DetailsView en nuestra página que permite a los usuarios agregar un nuevo producto. Este DetailsView se usará para proporcionar una interfaz para agregar rápidamente un nuevo producto a la base de datos. Debe tener una interfaz de usuario coherente al agregar un nuevo producto vamos a permitir que el usuario sólo escriba valores para el `ProductName` y `UnitPrice` campos. De forma predeterminada, los valores que no se proporcionan en la interfaz de inserción de DetailsView se establecerá en un `NULL` valor de la base de datos. Sin embargo, podemos usar el ObjectDataSource `Inserting` eventos para insertar valores predeterminados diferentes, como veremos en breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Paso 3: Proporcionar una interfaz para agregar nuevos productos

Arrastre un DetailsView desde el cuadro de herramientas hasta el diseñador por encima del control GridView, desactive su `Height` y `Width` propiedades y lo enlazará a ObjectDataSource ya está presente en la página. Esta acción agregará un BoundField o CampoCasillaVerificación para cada uno de los campos del producto. Puesto que deseamos utilizar este DetailsView para agregar nuevos productos, debemos comprobar la opción de habilitar la inserción de la etiqueta inteligente; Sin embargo, hay ninguna opción de este tipo porque el ObjectDataSource `Insert()` método no está asignado a un método en el `ProductsBLL` clase (Recuerde que se debe establecer esta asignación (ninguno) al configurar el origen de datos Véase la figura 3).

Para configurar el origen ObjectDataSource, seleccione el vínculo Configurar origen de datos en su etiqueta inteligente, iniciar al asistente. La primera pantalla le permite cambiar el objeto subyacente que ObjectDataSource está enlazado Déjelo establecido en `ProductsBLL`. La siguiente pantalla muestra las asignaciones de métodos de ObjectDataSource para el objeto subyacente. Aunque se indica explícitamente que el `Insert()` y `Delete()` no deberían asignarse a los métodos a los métodos, si se dirige a las pestañas INSERT y DELETE, verá que una asignación está allí. Esto es porque el `ProductsBLL`del `AddProduct` y `DeleteProduct` métodos usan el `DataObjectMethodAttribute` atributo para indicar que son los métodos predeterminados para `Insert()` y `Delete()`, respectivamente. Por lo tanto, el Asistente de ObjectDataSource selecciona ambos identificadores cada vez que se ejecuta al asistente, a menos que haya algún otro valor especificado explícitamente.

Deje el `Insert()` método al que apunta a la `AddProduct` método, pero volver a establecer la lista desplegable de la pestaña de la eliminación en (None).


[![Establece la lista desplegable de la pestaña de INSERCIÓN en el método AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Figura 14**: establece la lista desplegable de la pestaña Insertar en el `AddProduct` método ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![Establecer la lista desplegable de la pestaña de la eliminación en (None)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Figura 15**: establecer la lista desplegable de la pestaña de eliminar en (None) ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


Después de realizar estos cambios, la sintaxis declarativa de ObjectDataSource se ampliará para incluir un `InsertParameters` colección, como se muestra a continuación:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Volver a ejecutar el Asistente para agregar el `OldValuesParameterFormatString` propiedad. Dedique un momento para borrar esta propiedad si se establece en el valor predeterminado (`{0}`) o quítelo por completo de la sintaxis declarativa.

Con el origen ObjectDataSource que proporciona funciones de inserción, etiquetas inteligentes de DetailsView ahora incluyen la casilla de verificación Habilitar inserción; volver al diseñador y seleccione esta opción. A continuación, reducir DetailsView para que solo tiene dos BoundFields - `ProductName` y `UnitPrice` - y el CommandField. En este momento, la sintaxis declarativa de DetailsView debería ser similar:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

Figura 16 se muestra esta página cuando se ve mediante un explorador en este momento. Como puede ver, DetailsView muestra el nombre y el precio del producto primera (Chai). Sin embargo, se lo que queremos es una interfaz de inserción que proporciona un medio para el usuario agregar rápidamente un nuevo producto a la base de datos.


[![DetailsView está representando actualmente en modo de solo lectura](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Figura 16**: The DetailsView está representando actualmente en modo de solo lectura ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


Para poder mostrar DetailsView en su modo de inserción es necesario establecer la `DefaultMode` propiedad `Inserting`. Esto representa DetailsView en modo de inserción cuando visita en primer lugar y mantiene allí después de insertar un nuevo registro. Como se muestra en la figura 17, tal DetailsView proporciona una interfaz rápida para agregar un nuevo registro.


[![DetailsView proporciona una interfaz para agregar rápidamente un nuevo producto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Figura 17**: DetailsView proporciona una interfaz para agregar rápidamente un nuevo producto ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


Cuando el usuario introduce un nombre de producto y el precio (por ejemplo, "Water Acme" y 1.99, como se muestra en la figura 17) y hace clic en Insertar, una devolución de datos que habrá trastornos y comienza el flujo de trabajo de inserción, y se culmina en un nuevo registro de producto que se va a agregar a la base de datos. DetailsView mantiene su interfaz de inserción y el control GridView se automáticamente vuelve a enlazar al origen de datos con el fin de incluir el nuevo producto, tal como se muestra en la figura 18.


![El producto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Figura 18**: el producto "Agua Acme" se ha agregado a la base de datos


Mientras el control GridView en la figura 18, no muestra los campos de producto que carecen de la interfaz DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, y así sucesivamente se asignan `NULL` los valores de la base de datos. Puede ver esto mediante la realización de los pasos siguientes:

1. Vaya al explorador de servidores en Visual Studio
2. Expandir el `NORTHWND.MDF` nodo base de datos
3. Haga doble clic en el `Products` nodo de la tabla de base de datos
4. Seleccione Mostrar datos de tabla

Esto mostrará todos los registros en el `Products` tabla. Como la figura 19 se muestra, todas las columnas de nuestro nuevo producto de distinto `ProductID`, `ProductName`, y `UnitPrice` tiene `NULL` valores.


[![Los campos de producto no proporcionado en DetailsView están asignados los valores NULL](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Figura 19**: el producto campos no se proporcionaron en DetailsView se asignan `NULL` valores ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


Nos conviene proporcionar un valor predeterminado distinto `NULL` para uno o varios de estos valores de columna, ya sea porque `NULL` no sea la mejor opción predeterminada o porque no permite la misma columna de base de datos `NULL` s. Para lograr esto podemos establecer mediante programación los valores de los parámetros de la DetailsView `InputParameters` colección. Esta asignación puede hacerse ya sea en el evento controlador para el DetailsView `ItemInserting` eventos o el ObjectDataSource `Inserting` eventos. Puesto que ya hemos analizado en el uso de los eventos anteriores y posteriores al niveles en los datos Web controlar el nivel, vamos a examinar mediante eventos de ObjectDataSource en este momento.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Paso 4: Asignar valores a la`CategoryID`y`SupplierID`parámetros

En este tutorial vamos a imaginar que para nuestra aplicación al agregar un nuevo producto a través de esta interfaz se debe asignar un `CategoryID` y `SupplierID` valor 1. Como se mencionó anteriormente, el origen ObjectDataSource tiene un par de eventos previos y posteriores al niveles que desencadenen durante el proceso de modificación de datos. Cuando su `Insert()` se invoca el método, el origen ObjectDataSource primero genera su `Inserting` eventos, a continuación, llama al método que su `Insert()` método se ha asignado a y, por último, se genera el `Inserted` eventos. El `Inserting` controlador de eventos nos ofrece una última oportunidad para ajustar los parámetros de entrada o cancelar la operación completa.

> [!NOTE]
> En una aplicación real, probablemente querría para permitir que el usuario especifica la categoría y el proveedor o podría seleccionar este valor para ellos según algunos criterios o business logic (en lugar de ciegamente seleccionando un identificador 1). No obstante, en el ejemplo se muestra cómo establecer mediante programación el valor de un parámetro de entrada de eventos en el nivel previamente de ObjectDataSource.


Dedique un momento para crear un controlador de eventos para el origen de ObjectDataSource `Inserting` eventos. Tenga en cuenta que el segundo parámetro de entrada del controlador de eventos es un objeto de tipo `ObjectDataSourceMethodEventArgs`, que tiene una propiedad para tener acceso a la colección de parámetros (`InputParameters`) y una propiedad para cancelar la operación (`Cancel`).


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

En este momento, el `InputParameters` propiedad contiene de ObjectDataSource `InsertParameters` colección con los valores asignados de DetailsView. Para cambiar el valor de uno de estos parámetros, basta con usar: `e.InputParameters("paramName") = value`. Por lo tanto, para establecer el `CategoryID` y `SupplierID` a valores de 1, ajustar el `Inserting` controlador de eventos para el siguiente aspecto:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Esta vez cuando se agrega un nuevo producto (por ejemplo, Acme Soda), el `CategoryID` y `SupplierID` columnas del nuevo producto que se establecen en 1 (Véase la figura 20).


[![Nuevos productos ahora tienen sus CategoryID y SupplierID valores establecidos en 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Figura 20**: Their de productos nuevos ahora tienen `CategoryID` y `SupplierID` valores se establecen en 1 ([haga clic aquí para ver imagen en tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>Resumen

Durante la edición, insertar y eliminar el proceso, el control de datos Web y el origen ObjectDataSource continúe con un número de eventos previos y posteriores al niveles. En este tutorial se examinan los eventos de niveles previamente y ha visto cómo utilizarlos para personalizar los parámetros de entrada o cancelar la operación de modificación de datos por completo, tanto desde el control de datos Web y eventos de ObjectDataSource. En el siguiente tutorial, buscaremos en la creación y uso de controladores de eventos para los eventos posteriores al niveles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Jackie Goor y Liz Shulok. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [Siguiente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
