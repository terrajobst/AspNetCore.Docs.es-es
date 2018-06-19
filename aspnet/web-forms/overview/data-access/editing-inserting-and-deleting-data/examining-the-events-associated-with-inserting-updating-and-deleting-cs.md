---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Examinar los eventos asociados a insertar, actualizar y eliminar (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial que examinaremos con los eventos que se producen antes, durante y después de insertar, actualizar o eliminar el funcionamiento de un control Web de datos ASP.NET. W....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 61542f37c9fa3e6c5893aabb0a3116571bd6be5a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889402"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Examinar los eventos asociados a insertar, actualizar y eliminar (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) o [descarga de PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> En este tutorial que examinaremos con los eventos que se producen antes, durante y después de insertar, actualizar o eliminar el funcionamiento de un control Web de datos ASP.NET. También veremos cómo personalizar la interfaz de edición para actualizar solo un subconjunto de los campos de producto.


## <a name="introduction"></a>Introducción

Cuando se usa integradas de inserción, editar o eliminar funciones de los controles GridView, DetailsView o FormView, una serie de pasos transcurrir cuando el usuario final complete el proceso de agregar un nuevo registro o actualizar o eliminar un registro existente. Como se explicó en la [tutorial anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md), cuando se edita una fila en GridView el botón de edición se reemplaza con botones Actualizar y Cancelar y activar BoundFields en cuadros de texto. Después de que el usuario final actualiza los datos y hace clic en actualizar, se realizan los pasos siguientes en la devolución de datos:

1. GridView rellena su ObjectDataSource `UpdateParameters` con campos de identificación único del registro editado (a través de la `DataKeyNames` propiedad) junto con los valores especificados por el usuario
2. GridView invoca su ObjectDataSource `Update()` método, que a su vez, se invoca el método adecuado en el objeto subyacente (`ProductsDAL.UpdateProduct`, en nuestro tutorial anterior)
3. Los datos subyacentes, que ahora incluyen los cambios actualizados, se vuelve a enlazar a GridView

Durante esta secuencia de pasos, se activan un número de eventos, lo que nos permite crear controladores de eventos para agregar lógica personalizada cuando sea necesario. Por ejemplo, antes del paso 1, GridView `RowUpdating` desencadena el evento. En este momento, cancelar la solicitud de actualización si se produce algún error de validación. Cuando el `Update()` se invoca el método, el ObjectDataSource `Updating` desencadena el evento, lo que proporciona una oportunidad para agregar o personalizar los valores de cualquiera de los `UpdateParameters`. Después de que el ObjectDataSource subyacente del método del objeto ha terminado de ejecutarse, el ObjectDataSource `Updated` evento se desencadena. Un controlador de eventos para el `Updated` evento puede inspeccionar los detalles sobre la operación de actualización, como el número de filas afectado y si no se ha producido una excepción. Por último, después del paso 2, GridView `RowUpdated` desencadena el evento; un controlador de eventos para este evento puede examinar la información adicional sobre la operación de actualización sólo realiza.

Figura 1 muestra esta serie de eventos y pasos al actualizar un control GridView. El patrón de eventos en la figura 1 no es único para la actualización con un control GridView. Insertar, actualizar o eliminar datos de GridView, DetailsView o FormView precipita la misma secuencia de eventos previas y posteriores al niveles para el control de datos Web y ObjectDataSource.


[![Una serie de previo e incendio de eventos posteriores a la hora de actualizar datos en un control GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: una serie de previas y eventos posteriores a la activan al actualizar los datos de un control GridView ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


En este tutorial que examinaremos usa estos eventos para extender el integradas de inserción, actualización y eliminación de las capacidades de los datos ASP.NET controla la Web. También veremos cómo personalizar la interfaz de edición para actualizar solo un subconjunto de los campos de producto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Paso 1: Actualizar un producto`ProductName`y`UnitPrice`campos

En las interfaces de edición en el tutorial anterior *todos los* campos de producto que no son de solo lectura tenía que debe incluirse. Si deseáramos quitar un campo de GridView - diga `QuantityPerUnit` : al actualizar los datos del control Web de datos no debe establecer el ObjectDataSource `QuantityPerUnit` `UpdateParameters` valor. ObjectDataSource pasaría, a continuación, en un `null` valor en el `UpdateProduct` método de capa de lógica empresarial (BLL), que cambiaría el registro de base de datos modificada `QuantityPerUnit` columna a una `NULL` valor. De forma similar, si un campo obligatorio, como `ProductName`, se quita de la interfaz de edición, se producirá un error de la actualización con un "*columna 'ProductName' no permite valores NULL*" excepción. El motivo de este comportamiento era porque se configuró el ObjectDataSource para llamar a la `ProductsBLL` la clase `UpdateProduct` método, que espera un parámetro de entrada para cada uno de los campos de producto. Por lo tanto, el ObjectDataSource `UpdateParameters` colección contiene un parámetro para cada uno de lo método de parámetros de entrada.

Si desea proporcionar un control Web que permite al usuario final actualizar solo un subconjunto de los campos de datos, es necesario o bien establecer mediante programación la falta `UpdateParameters` valores en el ObjectDataSource `Updating` controlador de eventos o crear e invocar un método BLL que espera a que solo un subconjunto de los campos. Vamos a examinar este último enfoque.

En concreto, vamos a crear una página que muestre solo los `ProductName` y `UnitPrice` campos de un control GridView editable. Interfaz de edición de esta GridView solo permitirá al usuario que actualice los campos que se muestran dos, `ProductName` y `UnitPrice`. Puesto que esta interfaz edición proporciona solo un subconjunto de los campos de un producto, o es necesario crear un ObjectDataSource que usa la capa BLL existente `UpdateProduct` método y los valores de campo de producto que faltan estableció mediante programación en su `Updating` eventos controlador, o se necesita crear un nuevo método BLL que espera que sólo el subconjunto de los campos definidos en GridView. Para este tutorial, vamos a usar esta última opción y crear una sobrecarga de la `UpdateProduct` /método siguiente, que toma solo tres parámetros de entrada: `productName`, `unitPrice`, y `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Al igual que el original `UpdateProduct` esta sobrecarga de método, inicia y comprueba si hay un producto en la base de datos con los valores especificados `ProductID`. Si no, devuelve `false`, que indica que el error en la solicitud para actualizar la información de producto. En caso contrario, actualiza el registro de producto existente `ProductName` y `UnitPrice` campos en consecuencia y confirma la actualización mediante una llamada a la TableAdpater `Update()` método, pasando el `ProductsRow` instancia.

Con esta adición a nuestro `ProductsBLL` (clase), estamos listos crear la interfaz de GridView simplificada. Abra la `DataModificationEvents.aspx` en el `EditInsertDelete` carpeta y agregar un control GridView a la página. Crear un nuevo origen ObjectDataSource y configúrelo para utilizar el `ProductsBLL` clase con su `Select()` asignación del método a `GetProducts` y su `Update()` asignación del método para el `UpdateProduct` sobrecarga que toma solo en el `productName`, `unitPrice`, y `productID` parámetros de entrada. La figura 2 muestra el Asistente para crear orígenes de datos al asignar el ObjectDataSource `Update()` método a la `ProductsBLL` de la nueva clase `UpdateProduct` sobrecarga del método.


[![Asignar Update() método de ObjectDataSource a la nueva sobrecarga UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: asignar el ObjectDataSource `Update()` método el nuevo `UpdateProduct` de sobrecarga ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Puesto que nuestro ejemplo inicialmente solo necesitarán la capacidad de modificar datos, pero no permite insertar o eliminar registros, tómese un momento para indicar explícitamente que el ObjectDataSource `Insert()` y `Delete()` métodos no deberían asignarse a cualquiera de los `ProductsBLL` métodos de la clase, vaya a las pestañas INSERT y DELETE y elija (ninguno) en la lista desplegable.


[![Elija (ninguno) en la lista desplegable de las fichas DELETE y INSERT](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: elija (ninguno) de la lista desplegable para la INSERCIÓN y eliminar pestañas ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Después de completar este asistente Active la casilla Habilitar edición de etiquetas inteligentes de GridView.

Con la finalización del Asistente Crear origen de datos y enlazar a GridView, Visual Studio ha creado la sintaxis declarativa para ambos controles. Vaya a la vista de origen para inspeccionar marcado declarativo de ObjectDataSource, que se muestra a continuación:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Puesto que no hay ninguna asignación para el ObjectDataSource `Insert()` y `Delete()` métodos, no hay ningún `InsertParameters` o `DeleteParameters` secciones. Además, desde el `Update()` método se asigna a la `UpdateProduct` sobrecarga del método que solo acepta tres parámetros de entrada, el `UpdateParameters` sección tiene solo tres `Parameter` instancias.

Tenga en cuenta que el ObjectDataSource `OldValuesParameterFormatString` propiedad está establecida en `original_{0}`. Esta propiedad se establece automáticamente en Visual Studio cuando se usa al Asistente para configurar orígenes de datos. Sin embargo, puesto que nuestros métodos BLL no espera que el original `ProductID` valor que se pasa, quite por completo esta asignación de propiedad de la sintaxis declarativa de ObjectDataSource.

> [!NOTE]
> Si simplemente limpiar el `OldValuesParameterFormatString` valor de la propiedad de la ventana de propiedades en la vista de diseño, la propiedad seguirá existiendo en la sintaxis declarativa, pero se establecerá en una cadena vacía. Quite la propiedad por completo de la sintaxis declarativa, o bien, en la ventana Propiedades, establezca el valor en el valor predeterminado, `{0}`.


Mientras el ObjectDataSource solo tiene `UpdateParameters` para el producto name, precio y ID, Visual Studio ha agregado un BoundField o CampoCasillaVerificación en GridView para cada uno de los campos del producto.


[![GridView contiene un BoundField o CampoCasillaVerificación para cada uno de los campos del producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: GridView contiene un BoundField o CampoCasillaVerificación para cada uno de los campos del producto ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Cuando el usuario final edita un producto y haga clic en el botón de actualización, GridView enumera los campos que no fueran de solo lectura. A continuación, Establece el valor del parámetro correspondiente en el ObjectDataSource `UpdateParameters` colección para el valor especificado por el usuario. Si no es un parámetro correspondiente, GridView suma uno a la colección. Por lo tanto, si nuestro GridView contiene BoundFields y CheckBoxFields para todos los campos del producto, ObjectDataSource terminará invocar el `UpdateProduct` sobrecarga que toma en todos estos parámetros, a pesar del hecho de que el ObjectDataSource marcado declarativo especifica solo tres parámetros de entrada (Véase la figura 5). De forma similar, si hay alguna combinación de no son de solo lectura producto campos en el control GridView que no se corresponde con los parámetros de entrada para un `UpdateProduct` sobrecarga, se producirá una excepción al intentar actualizar.


[![Usará GridView agregar parámetros a la colección de UpdateParameters de ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: GridView el le y agregar los parámetros a la ObjectDataSource `UpdateParameters` colección ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Para asegurarse de que el ObjectDataSource invoca el `UpdateProduct` sobrecarga que toma solo nombre del producto, precio y de Id., es necesario restringir GridView a tener campos editables para simplemente el `ProductName` y `UnitPrice`. Esto puede realizarse mediante la eliminación de los demás BoundFields y CheckBoxFields, estableciendo los demás campos `ReadOnly` propiedad `true`, o alguna combinación de los dos. Para este tutorial vamos a basta con quitar todos los campos de GridView excepto la `ProductName` y `UnitPrice` BoundFields, tras el cual marcado declarativo del GridView tendrá un aspecto similar:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Aunque el `UpdateProduct` sobrecarga espera tres parámetros de entrada, sólo tenemos dos BoundFields en nuestra GridView. Esto es porque el `productID` parámetro de entrada es un valor de clave principal y pasan a través del valor de la `DataKeyNames` propiedad para la fila editada.

Nuestro GridView, junto con el `UpdateProduct` sobrecarga, permite al usuario editar únicamente el nombre y el precio de un producto sin perder algunos de los otros campos de producto.


[![La interfaz permite la edición únicamente el producto nombre y precio](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: el permite interfaz edición únicamente el producto nombre y precio ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Como se describe en el tutorial anterior, es sumamente importante que GridView posible estado de vista de s habilitado (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, se corre el riesgo de tener usuarios simultáneos involuntariamente eliminen o modifiquen los registros. Vea [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) para obtener más información.


## <a name="improving-theunitpriceformatting"></a>Mejorar la`UnitPrice`formato

Mientras que en el ejemplo de GridView se muestra en la figura 6 funciona, el `UnitPrice` campo no tiene el formato en absoluto, lo que resulta en una pantalla de precios que no tiene ninguna divisa símbolos y tiene cuatro decimales. Para aplicar una formato para las filas no editables de moneda, basta con establecer la `UnitPrice` del BoundField `DataFormatString` propiedad `{0:c}` y su `HtmlEncode` propiedad `false`.


[![Establecer el UnitPrice DataFormatString y HtmlEncode propiedades según corresponda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: establecer el `UnitPrice`del `DataFormatString` y `HtmlEncode` propiedades según corresponda ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Con este cambio, las filas no editables formatee el precio como una moneda; Sin embargo, la fila modificada, aún muestra el valor sin el símbolo de moneda y con cuatro decimales.


[![Filas no editables son ahora un formato como valores de moneda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: no se puede modificar filas son ahora formato de valores de moneda ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Las instrucciones de formato especificadas en el `DataFormatString` propiedad se puede aplicar a la interfaz de edición estableciendo la BoundField `ApplyFormatInEditMode` propiedad `true` (el valor predeterminado es `false`). Tómese un momento para establecer esta propiedad en `true`.


[![Establecer propiedad de ApplyFormatInEditMode de UnitPrice BoundField en true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: establecer el `UnitPrice` del BoundField `ApplyFormatInEditMode` propiedad `true` ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Con este cambio, el valor de la `UnitPrice` aparece en el texto editado fila también se da formato como una moneda.


[![Valor de la fila editada UnitPrice es ahora un formato como una moneda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: The editar la fila `UnitPrice` valor es ahora el formato como una moneda ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Sin embargo, actualizar un producto con el símbolo de moneda en el cuadro de texto, como $ las 19: 00 produce un `FormatException`. Cuando intente asignar los valores proporcionados por el usuario a la ObjectDataSource GridView `UpdateParameters` colección no es capaz de convertir la `UnitPrice` cadena "$ las 19: 00" en el `decimal` requerido por el parámetro (consulte la figura 11). Para remediar este problema podemos crear un controlador de eventos del control de GridView `RowUpdating` eventos y para que analizar proporcionada por el usuario `UnitPrice` como un formato de moneda `decimal`.

La GridView `RowUpdating` eventos acepta como su segundo parámetro de un objeto de tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), que incluye un `NewValues` diccionario como uno de sus propiedades que contiene los valores proporcionados por el usuario están preparados para asignado a la ObjectDataSource `UpdateParameters` colección. Se puede sobrescribir la existente `UnitPrice` valor en el `NewValues` colección con un valor decimal se analiza mediante el formato de moneda con las siguientes líneas de código en el `RowUpdating` controlador de eventos:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Si el usuario ha especificado un `UnitPrice` valor (por ejemplo, "$ las 19: 00"), este valor se sobrescribe con el valor decimal calculado por [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analizar el valor como una moneda. Esto correctamente analizará la coma decimal en el caso de los símbolos de moneda, comas, decimales y así sucesivamente y utiliza el [enumeración NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) en el [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) espacio de nombres.

La figura 11 muestra tanto el problema causado por los símbolos de divisa en proporcionada por el usuario `UnitPrice`, así como la manera de GridView `RowUpdating` controlador de eventos se puede utilizar para analizar correctamente dicha entrada.


[![Valor de la fila editada UnitPrice es ahora un formato como una moneda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: The editar la fila `UnitPrice` valor es ahora el formato como una moneda ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Paso 2: prohibición`NULL UnitPrices`

Mientras la base de datos está configurado para permitir `NULL` valores en el `Products` la tabla `UnitPrice` columna, que podríamos desear evitar que los usuarios visitar esta página concreta de especificar un `NULL` `UnitPrice` valor. Es decir, si un usuario no se puede escribir un `UnitPrice` valor cuando se edita una fila de producto, en lugar de guardar los resultados en la base de datos que desea mostrar un mensaje que informa al usuario que, a través de esta página, los productos editados deben tener un precio especificado.

El `GridViewUpdateEventArgs` objeto pasa a la GridView `RowUpdating` contiene el controlador de eventos un `Cancel` propiedad que, si establece en `true`, finaliza el proceso de actualización. Vamos a ampliar el `RowUpdating` controlador de eventos para establecer `e.Cancel` a `true` y mostrar un mensaje que explica por qué si la `UnitPrice` valor en el `NewValues` colección es `null`.

Empiece agregando un control Web Label a la página con el nombre `MustProvideUnitPriceMessage`. Este control de etiqueta se mostrará si el usuario no se puede especificar un `UnitPrice` valor al actualizar un producto. Establezca la etiqueta `Text` propiedad "Debe proporcionar un precio para el producto". También he creado una nueva clase CSS en `Styles.css` denominado `Warning` con la siguiente definición:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Por último, establezca la etiqueta `CssClass` propiedad `Warning`. En este momento el diseñador debe mostrar el mensaje de advertencia en un color rojo, negrita, cursiva, tamaño de fuente extra grande por encima del control GridView, tal como se muestra en la figura 12.


[![Se ha agregado una etiqueta por encima de GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: una etiqueta tiene ha agregado anteriormente el GridView ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


De forma predeterminada, esta etiqueta debe ocultarse, por tanto, establecer su `Visible` propiedad `false` en el `Page_Load` controlador de eventos:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Si el usuario intenta actualizar un producto sin especificar el `UnitPrice`, desea cancelar la actualización y mostrar la etiqueta de advertencia. Aumentar la GridView `RowUpdating` controlador de eventos como se indica a continuación:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Si un usuario intenta guardar un producto sin especificar un precio, se cancela la actualización y se muestra un mensaje útil. Mientras que la base de datos (y la lógica de negocios) permite `NULL` `UnitPrice` s, esta página ASP.NET determinada no lo hace.


[![Un usuario no puede dejar en blanco UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: un usuario no puede dejar `UnitPrice` en blanco ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Hasta ahora hemos visto cómo utilizar la GridView `RowUpdating` evento para modificar mediante programación los valores de parámetro asignados a la ObjectDataSource `UpdateParameters` colección así como el modo para cancelar la actualización procesar por completo. Estos conceptos se mantienen en los controles DetailsView y FormView y también se aplican a la inserción y eliminación.

También pueden realizar estas tareas en el nivel de ObjectDataSource mediante controladores de eventos para su `Inserting`, `Updating`, y `Deleting` eventos. Estos eventos se desencadenan antes de que se invoca el método asociado del objeto subyacente y proporcionan una oportunidad de última oportunidad para modificar la colección de parámetros de entrada o cancelar la operación directamente. Los controladores de eventos para estos tres eventos se pasan un objeto de tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) que tiene dos propiedades interesantes:

- [Cancelar](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), que, si establece en `true`, cancela la operación que se está realiza
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), que es la colección de `InsertParameters`, `UpdateParameters`, o `DeleteParameters`, dependiendo de si es el controlador de eventos para el `Inserting`, `Updating`, o `Deleting` eventos

Para ilustrar trabajar con los valores de parámetro en el nivel de ObjectDataSource, vamos a incluir DetailsView en nuestra página que permite a los usuarios agregar un nuevo producto. Se usará este DetailsView para proporcionar una interfaz para agregar rápidamente un nuevo producto en la base de datos. Debe tener una interfaz de usuario coherente al agregar un nuevo producto vamos a permitir que el usuario especifique solo los valores de la `ProductName` y `UnitPrice` campos. De forma predeterminada, los valores que no se proporcionan en la interfaz de inserción de DetailsView se establecerá en un `NULL` valor de la base de datos. Sin embargo, podemos usar la ObjectDataSource `Inserting` eventos para insertar valores predeterminados diferentes, como veremos en breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Paso 3: Proporcionar una interfaz para agregar nuevos productos

Arrastre un DetailsView desde el cuadro de herramientas en el diseñador, encima del control GridView, borrar sus `Height` y `Width` propiedades y se enlaza a ObjectDataSource ya presente en la página. Esto agregará una BoundField o CampoCasillaVerificación para cada uno de los campos del producto. Puesto que deseamos utilizar esta DetailsView para agregar nuevos productos, es necesario activar la opción de habilitar la inserción de la etiqueta inteligente; Sin embargo, no es posible este tipo porque la ObjectDataSource `Insert()` método no está asignado a un método en el `ProductsBLL` clase (Recuerde que se debe establecer esta asignación (ninguno) al configurar el origen de datos consulte la figura 3).

Para configurar el origen ObjectDataSource, seleccione el vínculo Configurar origen de datos en su etiqueta inteligente, iniciar al asistente. La primera pantalla le permite cambiar el objeto subyacente que ObjectDataSource está enlazado Deje `ProductsBLL`. La siguiente pantalla muestra las asignaciones de métodos de ObjectDataSource para el objeto subyacente. Aunque se indica explícitamente que el `Insert()` y `Delete()` métodos no deben asignarse a cualquier método, si va a las pestañas INSERT y DELETE verá que una asignación está allí. Esto es porque el `ProductsBLL`del `AddProduct` y `DeleteProduct` métodos utilizan la `DataObjectMethodAttribute` atributo para indicar que son los métodos predeterminados para `Insert()` y `Delete()`, respectivamente. Por lo tanto, el Asistente de ObjectDataSource selecciona estos cada vez que ejecute al asistente a menos que haya algún otro valor especificado explícitamente.

Deje el `Insert()` método que apunta a la `AddProduct` método, pero volver a establecer la lista desplegable de la ficha eliminación en (ninguno).


[![Establece la lista de desplegable de la pestaña Insertar en el método de AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figura 14**: establece la lista de desplegable de la pestaña Insertar en el `AddProduct` método ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Establecer la lista desplegable de la ficha eliminación en (ninguno)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: establecer lista de desplegable de la ficha eliminar a (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Después de realizar estos cambios, la sintaxis declarativa de ObjectDataSource se ampliará para incluir una `InsertParameters` colección, tal y como se muestra a continuación:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Volver a ejecutar el Asistente para agregar de nuevo el `OldValuesParameterFormatString` propiedad. Tómese un momento para borrar esta propiedad si se establece en el valor predeterminado (`{0}`) o se quita por completo de la sintaxis declarativa.

Con ObjectDataSource ofreciendo funcionalidad de inserción, etiquetas inteligentes de DetailsView ahora incluirá la casilla de verificación Habilitar inserción; Vuelva al diseñador y seleccione esta opción. A continuación, reducir el DetailsView para que solo tiene dos BoundFields - `ProductName` y `UnitPrice` - y el CommandField. En este momento la sintaxis declarativa de DetailsView debería ser similar:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

La figura 16 muestra esta página cuando se ven a través de un explorador en este momento. Como puede ver, DetailsView muestra el nombre y el precio del producto primer (Chai). Sin embargo, se lo que queremos es una interfaz de inserción que proporciona un medio para el usuario agregar rápidamente un nuevo producto a la base de datos.


[![DetailsView es representada actualmente en modo de solo lectura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: The DetailsView está representada actualmente en modo de solo lectura ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Para poder mostrar DetailsView en su modo de inserción es necesario establecer la `DefaultMode` propiedad `Inserting`. Esto representa la DetailsView en modo de inserción cuando visita por primera vez y mantiene allí después de insertar un nuevo registro. Como se muestra en la figura 17, tal DetailsView proporciona una interfaz rápida para agregar un nuevo registro.


[![DetailsView proporciona una interfaz para agregar rápidamente un nuevo producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: DetailsView proporciona una interfaz para agregar rápidamente un nuevo producto ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Cuando el usuario escribe un nombre de producto y el precio (por ejemplo, "Acme yate" y 1.99, como se muestra en la figura 17) y hace clic en Insertar, seguidamente, tiene lugar una devolución de datos y comenzará el flujo de trabajo de inserción, hasta la preinstalación en un nuevo registro de producto que se va a agregar a la base de datos. DetailsView mantiene su interfaz de inserción y GridView es automáticamente vuelve a enlazar a su origen de datos para incluir el nuevo producto, tal como se muestra en la figura 18.


![El producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: el producto "Agua Acme" se ha agregado a la base de datos


Mientras el control GridView en la figura 18, no muestra los campos de producto que carecen de la interfaz de DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, y así sucesivamente se asignan `NULL` valores de la base de datos. Puede ver esto mediante la realización de los pasos siguientes:

1. Vaya al explorador de servidores en Visual Studio
2. Al expandir el `NORTHWND.MDF` nodo base de datos
3. Haga doble clic en el `Products` nodo de la tabla de base de datos
4. Seleccione Mostrar datos de tabla

Esto mostrará una lista de todos los registros en la `Products` tabla. Tal y como figura 19 muestra, todas las columnas de nuestro nuevo producto distinto de `ProductID`, `ProductName`, y `UnitPrice` tienen `NULL` valores.


[![Los campos de producto no proporciona en DetailsView son asignados valores NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: el producto campos no proporciona en DetailsView se asignan `NULL` valores ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Queremos podemos proporcionar un valor predeterminado distinto de `NULL` para uno o varios de estos valores de columna, ya sea porque `NULL` no es la mejor opción de manera predeterminada o porque no admite la columna de base de datos propia `NULL` s. Para lograr esto podemos establecer mediante programación los valores de los parámetros de la DetailsView `InputParameters` colección. Esta asignación puede hacerse ya sea en el evento controlador para el DetailsView `ItemInserting` evento o el ObjectDataSource `Inserting` eventos. Puesto que ya hemos observado en el uso de los eventos anteriores y posteriores al niveles en los datos Web controlar el nivel, vamos a explorar con eventos de ObjectDataSource en este momento.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Paso 4: Asignar valores a la`CategoryID`y`SupplierID`parámetros

Para este tutorial imaginemos que para nuestra aplicación, al agregar un nuevo producto a través de esta interfaz se debe asignar un `CategoryID` y `SupplierID` valor 1. Como se mencionó anteriormente, ObjectDataSource tiene un par de eventos previos y posteriores al niveles que desencadenen durante el proceso de modificación de datos. Cuando su `Insert()` se invoca el método, ObjectDataSource genera en primer lugar su `Inserting` evento, a continuación, llama al método que su `Insert()` método se ha asignado a y, por último, se genera el `Inserted` eventos. El `Inserting` controlador de eventos, obtienen nos una última oportunidad para ajustar los parámetros de entrada o cancelar la operación directamente.

> [!NOTE]
> En una aplicación del mundo real puede que quieras para permitir que el usuario especifique la categoría y el proveedor o seleccionará este valor para ellos en función de algunos criterios o business lógica (en lugar de a ciegas seleccionando un Id. de 1). En cualquier caso, el ejemplo muestra cómo establecer mediante programación el valor de un parámetro de entrada de eventos de nivel previamente de ObjectDataSource.


Tómese un momento para crear un controlador de eventos para el ObjectDataSource `Inserting` eventos. Observe que el segundo parámetro de entrada del controlador de eventos es un objeto de tipo `ObjectDataSourceMethodEventArgs`, que tiene una propiedad para tener acceso a la colección de parámetros (`InputParameters`) y una propiedad que se va a cancelar la operación (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

En este momento, el `InputParameters` propiedad contiene el ObjectDataSource `InsertParameters` colección con los valores asignados de DetailsView. Para cambiar el valor de uno de estos parámetros, basta con usar: `e.InputParameters["paramName"] = value`. Por lo tanto, para establecer el `CategoryID` y `SupplierID` a valores de 1, ajustar el `Inserting` controlador de eventos debe ser similar a lo siguiente:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Esta vez cuando se agrega un nuevo producto (por ejemplo, Acme Soda), el `CategoryID` y `SupplierID` columnas del nuevo producto se establecen en 1 (Véase la figura 20).


[![Nuevos productos ahora tienen sus CategoryID y SupplierID valores establecidos en 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: Their de productos nueva ahora tienen `CategoryID` y `SupplierID` valores establecido en 1 ([haga clic aquí para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Resumen

Durante la edición, insertar y eliminar proceso, el control de datos Web y ObjectDataSource continuar a través de un número de eventos previos y posteriores al niveles. En este tutorial se examinan los eventos de niveles previamente y hemos visto cómo usarlos para personalizar los parámetros de entrada o cancelar la operación de modificación de datos totalmente tanto para el control Web de datos y eventos de ObjectDataSource. En el siguiente tutorial se examinará crear y usar controladores de eventos para los eventos posteriores al niveles.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Jackie Goor y Liz Shulok. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Siguiente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
