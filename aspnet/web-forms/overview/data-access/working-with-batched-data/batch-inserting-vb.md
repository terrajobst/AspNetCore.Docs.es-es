---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Lote de inserción (VB) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo insertar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se amplía el control GridView para permitir al usuario que escriba n varios...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 58338d8bfdd782167aafaa440f2d549d6eeb838e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836591"
---
<a name="batch-inserting-vb"></a>Lote de inserción (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) o [descargar PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Obtenga información sobre cómo insertar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se amplía el control GridView para permitir al usuario escribir varios registros nuevos. En la capa de acceso a datos se encapsulan las operaciones de inserción varias dentro de una transacción para garantizar que todas las inserciones se realizan correctamente o se revierten todas las inserciones.


## <a name="introduction"></a>Introducción

En el [actualización por lotes](batch-updating-vb.md) tutorial analizamos personalizar el control GridView para presentar una interfaz donde estaban puede editables varios registros. El usuario visita la página podría realizar una serie de cambios y, a continuación, con un solo clic el botón, realizar una actualización por lotes. Para situaciones donde los usuarios normalmente actualizan varios registros en una sola operación, esta interfaz puede ahorrar innumerables clics y cambios de contexto de teclado para el mouse cuando se compara con el valor predeterminado por fila características de edición que se han explorado en primer lugar en el [un Información general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial.

Este concepto también se puede aplicar cuando se agreguen registros. Imagine que aquí de Northwind Traders se suele recepción envíos de proveedores que contienen un número de productos para una categoría determinada. Por ejemplo, podríamos recibimos un envío de seis productos de café y té diferentes de Tokio Traders. Si un usuario escribe los seis productos de uno a la vez a través de un control DetailsView, deberá elegir muchos de los mismos valores y otra vez: deberá elegir la misma categoría (bebidas), el mismo proveedor (Tokio Traders), el mismo valor (de suspendido False) y las mismas unidades en el valor de orden (0). Esta entrada de datos repetitivos no sólo es lenta, pero es propensa a errores.

Podemos crear un lote de inserción de interfaz que permite al usuario elegir el proveedor y la categoría una vez, escriba una serie de nombres de producto y los precios de venta y, a continuación, haga clic en un botón para agregar los nuevos productos a la base de datos con un poco de trabajo (consulte la figura 1). Al agregar cada producto, su `ProductName` y `UnitPrice` campos de datos se asignan los valores especificados en los cuadros de texto, mientras que su `CategoryID` y `SupplierID` valores se asignan los valores de las listas desplegables en el fo superior del formulario. El `Discontinued` y `UnitsOnOrder` valores se establecen en los valores codificados de forma rígida de `False` y 0, respectivamente.


[![La interfaz de inserción por lotes](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figura 1**: la interfaz de inserción por lotes ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image3.png))


En este tutorial crearemos una página que implementa el lote de inserción de interfaz que se muestra en la figura 1. Como con los dos tutoriales anteriores, se ajustarán las inserciones dentro del ámbito de una transacción para garantizar la atomicidad. Introducción s Let!

## <a name="step-1-creating-the-display-interface"></a>Paso 1: Creación de la interfaz de visualización

En este tutorial se compone de una sola página que se divide en dos regiones: una región de presentación y una región de inserción. La interfaz de presentación, vamos a crear en este paso, muestra los productos en un control GridView e incluye un botón llamado el proceso de envío del producto. Cuando se hace clic en este botón, la interfaz de presentación se reemplaza con la interfaz de inserción, que se muestra en la figura 1. Devuelve la interfaz de la pantalla después de agregar productos de envío o se hace clic en los botones Cancelar. Vamos a crear la interfaz de inserción en el paso 2.

Al crear una página que tiene dos interfaces, sólo uno de los cuales está visible en un momento, cada interfaz normalmente se coloca dentro de un [control Panel Web](http://www.w3schools.com/aspnet/control_panel.asp), que actúa como contenedor para otros controles. Por lo tanto, nuestra página tendrá dos controles de Panel uno para cada interfaz.

Comience abriendo la `BatchInsert.aspx` página en el `BatchData` arrastrar un Panel desde el cuadro de herramientas hasta el diseñador y carpeta (consulte la figura 2). Establecer el Panel de s `ID` propiedad `DisplayInterface`. Cuando se agrega el Panel hasta el diseñador, su `Height` y `Width` propiedades se establecen en 50 px y 125px, respectivamente. Limpiar estos valores de propiedad de la ventana Propiedades.


[![Arrastre un Panel desde el cuadro de herramientas hasta el diseñador](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figura 2**: arrastrar un Panel desde el cuadro de herramientas hasta el diseñador ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image6.png))


A continuación, arrastre un control de botón y GridView en el Panel. Establezca el botón s `ID` propiedad `ProcessShipment` y su `Text` propiedad proceso de envío del producto. Establezca la s GridView `ID` propiedad `ProductsGrid` y, en la etiqueta inteligente, de enlazarla a un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para extraer sus datos de la `ProductsBLL` clase s `GetProducts` método. Puesto que este GridView sólo se utiliza para mostrar los datos, establezca las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None). Haga clic en Finalizar para completar al Asistente para configurar orígenes de datos.


[![Mostrar los datos devueltos por el método de clase ProductsBLL s GetProducts](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figura 3**: mostrar los datos devueltos desde el `ProductsBLL` clase s `GetProducts` método ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image9.png))


[![Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figura 4**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image12.png))


Después de completar al ObjectDataSource wizard, Visual Studio agregará BoundFields y un CampoCasillaVerificación para los campos de datos del producto. Quite todos menos a los `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, y `Discontinued` campos. No dude en realizar personalizaciones estéticas. He decidido dar formato a la `UnitPrice` campo como un valor de moneda, reordenar los campos y cambiar el nombre de algunos de los campos `HeaderText` valores. También configurar el control GridView para incluir la paginación y ordenación soporte activando las casillas de verificación Habilitar paginación y habilitar la ordenación en la etiqueta inteligente de s GridView.

Después de agregar los controles de Panel, botón, GridView y ObjectDataSource y personalizar los campos de s GridView, el marcado declarativo de la página s debe ser similar al siguiente:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Tenga en cuenta que el marcado para el botón y GridView aparecen dentro de la apertura y cierre `<asp:Panel>` etiquetas. Puesto que estos controles están en el `DisplayInterface` Panel, nos podemos ocultarlos simplemente estableciendo el Panel s `Visible` propiedad `False`. Paso 3 examina cambiar mediante programación el Panel s `Visible` haga clic en Propiedades en respuesta a un botón para mostrar una interfaz ocultando la otra.

Dedique un momento para ver nuestro progreso a través de un explorador. Como se muestra en la figura 5, debería ver un botón de envío del producto de proceso por encima de un control GridView que muestra los diez productos a la vez.


[![El control GridView enumera los productos y ofrece la ordenación y paginación capacidades](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figura 5**: el control GridView enumera los productos y ofrece la ordenación y paginación capacidades ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Paso 2: Creación de la interfaz de inserción

Con la interfaz de pantalla completa, que está listo para crear la inserción de la interfaz. Para este tutorial, permiten crear una interfaz de inserción que se le pide un valor único de proveedor y categoría y, a continuación, permite al usuario escribir hasta cinco nombres de producto y los valores de precio unitario de s. Con esta interfaz, el usuario puede agregar uno a cinco nuevos productos que todos compartan la misma categoría y proveedor, pero tienen precios y los nombres de producto único.

Comience por arrastrar un Panel desde el cuadro de herramientas hasta el diseñador, colocarlo debajo existente `DisplayInterface` Panel. Establecer el `ID` recién agrega la propiedad de este Panel para `InsertingInterface` y establezca su `Visible` propiedad `False`. Vamos a agregar código que establece la `InsertingInterface` Panel s `Visible` propiedad `True` en el paso 3. Borrar el Panel s también `Height` y `Width` los valores de propiedad.

A continuación, necesitamos crear la interfaz de inserción que se mostró en la figura 1. Esta interfaz se puede crear a través de una variedad de técnicas HTML, pero vamos a utilizar uno bastante sencillo: una tabla de cuatro columnas y filas siete.

> [!NOTE]
> Al escribir el marcado HTML `<table>` elementos, prefiero usar la vista del origen. Mientras que Visual Studio dispone de herramientas para agregar `<table>` elementos a través del diseñador, el diseñador parece todo demasiado dispuesto inyectar treta para `style` configuración en el marcado. Una vez que he creado la `<table>` marcado, normalmente volver al diseñador para agregar los controles Web y establecer sus propiedades. Al crear tablas con filas y columnas predeterminadas prefieren usar HTML estático en lugar de [control de tabla Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) porque los controles de Web situados dentro de un control de tabla Web solo se pueden acceder mediante el `FindControl("controlID")` patrón. Sin embargo,, uso los controles Web de la tabla para las tablas de tamaño dinámicamente (los cuyas filas o columnas se basan en alguna base de datos o los criterios especificados por el usuario), desde el control puede crearse mediante programación de tabla Web.


Escriba el siguiente marcado dentro de la `<asp:Panel>` etiquetas de la `InsertingInterface` Panel:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Esto `<table>` marcado no incluye todos los controles Web aún, vamos a agregarlos momentáneamente. Tenga en cuenta que cada `<tr>` elemento contiene un valor específico de clase CSS: `BatchInsertHeaderRow` para la fila de encabezado donde el proveedor y la categoría DropDownLists irá; `BatchInsertFooterRow` para la fila de pie de página donde se van agregar productos de envío y los botones Cancelar; y alternos `BatchInsertRow` y `BatchInsertAlternatingRow` controles TextBox de precios de los valores de las filas que se va a contener el producto y la unidad. Se ve crea clases CSS correspondientes en el `Styles.css` archivo para que la interfaz de insertar un aspecto similar a la GridView y DetailsView controla se ve que se usan en estos tutoriales. Estas clases CSS se muestran a continuación.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Con este marcado, vuelva a la vista de diseño. Esto `<table>` debe aparecer como una tabla de cuatro columnas y siete filas en el diseñador, como se muestra en la figura 6.


[![La interfaz de inserción se compone de una tabla de una fila de siete de cuatro columnas](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figura 6**: la interfaz de la inserción se compone de una tabla de una fila de siete de cuatro columnas ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image18.png))


Nos re ahora está listo para agregar los controles Web a la interfaz de inserción. Arrastre dos DropDownLists desde el cuadro de herramientas a las celdas de la tabla para el proveedor y otra para la categoría adecuadas.

Establecer proveedor DropDownList s `ID` propiedad `Suppliers` y enlazarlo a un nuevo origen ObjectDataSource denominado `SuppliersDataSource`. Configurar el nuevo origen ObjectDataSource para recuperar sus datos desde el `SuppliersBLL` clase s `GetSuppliers` método y la actualización del conjunto pestaña lista desplegable de s en (None). Haga clic en Finalizar para completar al asistente.


[![Configurar el origen ObjectDataSource para usar el método de clase SuppliersBLL s GetSuppliers](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figura 7**: configurar el origen ObjectDataSource que se usarán el `SuppliersBLL` clase s `GetSuppliers` método ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image21.png))


Tiene la `Suppliers` DropDownList mostrar el `CompanyName` campo de datos y el uso la `SupplierID` del campo de datos como su `ListItem` s valores.


[![Mostrar el campo de datos CompanyName y usar SupplierID como valor](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figura 8**: mostrar la `CompanyName` campo de datos y Use `SupplierID` como el valor ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image24.png))


Nombre de la segunda DropDownList `Categories` y enlazarlo a un nuevo origen ObjectDataSource denominado `CategoriesDataSource`. Configurar la `CategoriesDataSource` ObjectDataSource para usar el `CategoriesBLL` clase s `GetCategories` método; el conjunto de la lista desplegable se enumera en las fichas UPDATE y DELETE en (None) y haga clic en Finalizar para completar el asistente. Por último, tiene la presentación de DropDownList el `CategoryName` campo de datos y use el `CategoryID` como valor.

Después de que estos dos DropDownLists han sido agregados y enlazado a ObjectDataSource configurado adecuadamente, la pantalla debe ser similar a la figura 9.


[![La fila de encabezado contiene ahora los proveedores y categorías DropDownLists](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figura 9**: el encabezado de fila contiene ahora el `Suppliers` y `Categories` DropDownLists ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image27.png))


Ahora debemos crear los cuadros de texto para recopilar el nombre y el precio de cada nuevo producto. Arrastre un control de cuadro de texto del cuadro de herramientas hasta el diseñador para cada una de las filas de nombre y el precio del cinco producto. Establecer el `ID` las propiedades de los cuadros de texto a `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`, y así sucesivamente.

Agregue un control CompareValidator después de cada uno de los cuadros de texto, establecer el precio por unidad el `ControlToValidate` propiedad correspondientes `ID`. Establezca también la `Operator` propiedad `GreaterThanEqual`, `ValueToCompare` en 0, y `Type` a `Currency`. Estas configuraciones indicar CompareValidator para asegurarse de que el precio, si se especifica, es un valor de moneda válidos que es mayor o igual que cero. Establecer el `Text` propiedad \*, y `ErrorMessage` al precio debe ser mayor o igual que cero. Además, por favor, omita cualquier símbolo de divisa.

> [!NOTE]
> La interfaz de inserción no incluye los controles RequiredFieldValidator, aunque el `ProductName` campo el `Products` tabla de base de datos no admite `NULL` valores. Esto es porque queremos permitir que el usuario escriba hasta cinco productos. Por ejemplo, si el usuario proporcionar el nombre y la unidad de precio del producto para las tres primeras filas, dejando en blanco, las dos últimas filas d solo agregamos tres nuevos productos en el sistema. Puesto que `ProductName` es necesario, sin embargo, se deberá comprobar mediante programación garantizar que si un precio de venta se especifica que se proporciona un valor de nombre de producto correspondiente. Trataremos esta comprobación en el paso 4.


Al validar la entrada del usuario s, CompareValidator notifica datos no válidos si el valor contiene un símbolo de moneda. Agregue un carácter $ delante de cada uno de los cuadros de texto para que actúe como una indicación visual que indica al usuario que omite el símbolo de moneda al entrar en el precio del precio unitario.

Por último, agregue un control ValidationSummary dentro de la `InsertingInterface` Panel de configuración de su `ShowMessageBox` propiedad `True` y su `ShowSummary` propiedad `False`. Con esta configuración, si el usuario escribe un valor del precio de unidad no válida, aparecerá un asterisco junto a los controles de cuadro de texto incorrectos y el control ValidationSummary mostrará un cuadro de mensajes del lado cliente que se muestra el mensaje de error que se especificó anteriormente.

En este momento, la pantalla debe ser similar a la figura 10.


[![La interfaz de inserción incluye ahora los cuadros de texto para los productos de nombres y precios](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figura 10**: la inserción de interfaz ahora incluye cuadros de texto para los nombres de productos y los precios ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image30.png))


A continuación, necesitamos agregar agregar productos de botones de envío y de cancelación a la fila de pie de página. Arrastre dos controles de botón en el cuadro de herramientas en el pie de página de la interfaz de inserción, la configuración de los botones `ID` propiedades a `AddProducts` y `CancelButton` y `Text` propiedades para agregar productos de envío y Cancelar, respectivamente. Además, establezca el `CancelButton` control s `CausesValidation` propiedad `false`.

Por último, necesitamos agregar un control Web de la etiqueta que se mostrará mensajes de estado de las dos interfaces. Por ejemplo, cuando un usuario agrega correctamente un nuevo envío de productos, queremos volver a la interfaz de presentación y mostrar un mensaje de confirmación. Si, sin embargo, el usuario proporciona un precio para un producto nuevo, pero deja el nombre del producto, es necesario mostrar un mensaje de advertencia desde la `ProductName` campo es obligatorio. Debido a que necesitamos este mensaje para mostrar de ambas interfaces, colóquelo en la parte superior de la página fuera de los paneles.

Arrastre un control Web de la etiqueta del cuadro de herramientas a la parte superior de la página en el diseñador. Establecer el `ID` propiedad a `StatusLabel`, desactive out el `Text` propiedad y establezca el `Visible` y `EnableViewState` propiedades a `False`. Como hemos visto en los tutoriales anteriores, establecer el `EnableViewState` propiedad `False` nos permite cambiar los valores de propiedad de etiqueta s mediante programación y tenerlos revertir automáticamente a sus valores predeterminados en el postback subsiguientes. Esto simplifica el código para mostrar un mensaje de estado en respuesta a alguna acción del usuario que desaparece en el postback subsiguientes. Por último, establezca el `StatusLabel` control s `CssClass` propiedad a la advertencia, que es el nombre de una clase CSS definida en `Styles.css` que muestra texto en una fuente grande, cursiva, negrita y roja.

Figura 11 muestra el Diseñador de Visual Studio después de la etiqueta se ha agregado y configurado.


[![Colocar el Control StatusLabel como encima de los dos controles de Panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figura 11**: colocar el `StatusLabel` Control encima de los dos controles de Panel ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Paso 3: Cambiar entre la presentación Interfaces e inserción

En este punto hemos completado el marcado de nuestra presentación y la inserción de interfaces, pero nos re aún quedan dos tareas:

- Cambiar entre la presentación y la inserción de interfaces
- Agregar los productos en el envío a la base de datos

Actualmente, la interfaz de presentación está visible, pero la interfaz de inserción está oculta. Esto es porque el `DisplayInterface` Panel s `Visible` propiedad está establecida en `True` (el valor predeterminado), mientras que el `InsertingInterface` Panel s `Visible` propiedad está establecida en `False`. Para cambiar entre las dos interfaces simplemente tenemos que activar o desactivar cada control s `Visible` valor de propiedad.

Deseamos mover desde la interfaz de presentación a la interfaz de inserción cuando se hace clic en el botón de envío del producto de proceso. Por lo tanto, crear un controlador de eventos para este botón s `Click` eventos que contiene el código siguiente:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Este código simplemente oculta el `DisplayInterface` Panel y se muestra en el `InsertingInterface` Panel.

A continuación, crear controladores de eventos para agregar productos de envío y el botón Cancelar los controles en la interfaz de inserción. Cuando se hace clic en cualquiera de estos botones, es necesario volver a la interfaz de presentación. Crear `Click` controladores de eventos para los controles de botón para que llame a `ReturnToDisplayInterface`, un método agregará momentáneamente. Además de ocultar la `InsertingInterface` Panel y mostrar el `DisplayInterface` Panel, el `ReturnToDisplayInterface` método debe devolver los controles Web a su estado de edición previamente. Esto implica el establecimiento de las listas desplegables `SelectedIndex` propiedades en 0 y borrarlos el `Text` propiedades de los controles de cuadro de texto.

> [!NOTE]
> Tenga en cuenta qué puede ocurrir si se t devolver los controles a su estado previo edición antes de volver a la interfaz de presentación. Un usuario podría haga clic en el botón de envío del producto de proceso, escriba los productos desde el envío y, a continuación, haga clic en Agregar productos de envío. Esto sería agregar los productos y el usuario regresará a la interfaz de presentación. En este momento, el usuario podría desear agregar otro envío. Al hacer clic en el botón de envío del producto de proceso que devolvería la interfaz de inserción, pero la DropDownList selecciones y los valores del cuadro de texto todavía se rellenaría con los valores anteriores.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Ambos `Click` basta con llamar controladores de eventos el `ReturnToDisplayInterface` método, aunque se tendrá que volver a agregar productos de envío `Click` controlador de eventos en el paso 4 y agregue código para guardar los productos. `ReturnToDisplayInterface` se inicia al devolver el `Suppliers` y `Categories` DropDownLists en sus primeras opciones. Las dos constantes `firstControlID` y `lastControlID` marcar iniciales y finales de los valores de índice de control usados en la nomenclatura el precio de nombre y la unidad de producto en la inserción de cuadros de texto de la interfaz y se usan en los límites de la `For` bucle que establece el `Text`hacer una copia de las propiedades de los controles de cuadro de texto en una cadena vacía. Por último, los paneles `Visible` se restablecen las propiedades para que se oculte la interfaz de inserción y la interfaz de la pantalla mostrada.

Dedique un momento para probar esta página en un explorador. Cuando se visita primero la página debería ver la interfaz de presentación como se mostró en la figura 5. Haga clic en el botón de envío del producto de proceso. La página se postback y ahora debería ver la interfaz de inserción tal como se muestra en la figura 12. Al hacer clic en cualquier el agregar productos de botones de envío o en Cancelar, se devuelve a la interfaz de presentación.

> [!NOTE]
> Durante la visualización de la interfaz de inserción, dedique un momento para probar la CompareValidators en el precio unitario de cuadros de texto. Debería ver un cuadro de mensajes del lado cliente de advertencia al hacer clic en Agregar productos de botón de envío con los valores de moneda no válido o los precios con un valor menor que cero.


[![Se muestra la interfaz de inserción tras hacer clic en el botón de envío del producto de proceso](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figura 12**: la interfaz de la inserción se muestra al hacer clic en el botón de envío del producto de proceso ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Paso 4: Agregar los productos

Todo lo que queda de este tutorial es guardar los productos en la base de datos en los productos de agregar de botón de envío s `Click` controlador de eventos. Esto puede realizarse mediante la creación de un `ProductsDataTable` y agregando un `ProductsRow` instancia para cada uno de los nombres de producto proporcionados. Una vez que estos `ProductsRow` se han agregado se hará una llamada a la `ProductsBLL` clase s `UpdateWithTransaction` método pasando la `ProductsDataTable`. Recuerde que el `UpdateWithTransaction` método, que se creó en el [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, pasa el `ProductsDataTable` a la `ProductsTableAdapter` s `UpdateWithTransaction` método. Desde allí, se inicia una transacción de ADO.NET y los problemas de TableAdatper un `INSERT` instrucción a la base de datos para cada agregado `ProductsRow` en la tabla de datos. Suponiendo que se agregan todos los productos sin errores, que se confirma la transacción, en caso contrario se revierte.

El código para agregar productos de botón de envío s `Click` controlador de eventos también se necesita realizar un poco de comprobación de errores. Puesto que no hay ningún RequiredFieldValidators utilizado en la interfaz de inserción, un usuario podría escribir un precio para un producto mientras omite su nombre. Puesto que el nombre de producto s es obligatorio, si se desarrolla una condición de ese tipo se debe avisar al usuario y no continuar con las inserciones. La completa `Click` sigue el código del controlador de eventos:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

El controlador de eventos se inicia si se asegura que el `Page.IsValid` propiedad devuelve un valor de `True`. Si devuelve `False`, a continuación, eso significa que uno o varios de los CompareValidators notifican datos no válidos; en ese caso no queremos intenta insertar los productos especificados o se terminará con una excepción al intentar asignar el precio unitario escrito por el usuario valor para el `ProductsRow` s `UnitPrice` propiedad.

A continuación, un nuevo `ProductsDataTable` se crea la instancia (`products`). Un `For` bucle se usa para iterar por el nombre y la unidad de precio del producto cuadros de texto y el `Text` se leen las propiedades en las variables locales `productName` y `unitPrice`. Si el usuario ha escrito un valor para el precio unitario, pero no para el nombre del producto correspondiente, el `StatusLabel` muestra el mensaje si se proporciona una unidad de precio, también debe incluir el nombre del producto y se sale del controlador de eventos.

Si se ha proporcionado un nombre de producto, un nuevo `ProductsRow` instancia se crea utilizando el `ProductsDataTable` s `NewProductsRow` método. Esta nueva `ProductsRow` instancia s `ProductName` propiedad está establecida en el producto actual mientras el cuadro de texto Nombre el `SupplierID` y `CategoryID` propiedades se asignan a la `SelectedValue` propiedades de las listas desplegables en el encabezado de la interfaz s insertar. Si el usuario especificó un valor para el precio del producto s, se asigna a la `ProductsRow` instancia s `UnitPrice` propiedad; en caso contrario, es la propiedad izquierda no está asignado, lo que dará como resultado un `NULL` valor para `UnitPrice` en la base de datos. Por último, el `Discontinued` y `UnitsOnOrder` propiedades se asignan a los valores codificados de forma rígida `False` y 0, respectivamente.

Después de las propiedades se han asignado a la `ProductsRow` instancia se agrega a la `ProductsDataTable`.

Al finalizar el `For` bucle, comprobamos si se han agregado todos los productos. El usuario puede, después de todo, ha hecho clic en Agregar productos de envío antes de entrar en los nombres de producto o los precios. Si hay al menos un producto en el `ProductsDataTable`, `ProductsBLL` clase s `UpdateWithTransaction` se llama al método. A continuación, se vuelve a enlazar los datos a la `ProductsGrid` GridView para que los productos recién agregados aparecerá en la interfaz de presentación. El `StatusLabel` se actualiza para mostrar un mensaje de confirmación y la `ReturnToDisplayInterface` se invoca, ocultar la inserción de interfaz y que muestra la interfaz de presentación.

Si no hay productos se han especificado, la interfaz de inserción permanece en la pantalla, pero el mensaje que no hay productos se han agregado. Escriba los nombres de producto y los precios de venta en los cuadros de texto se muestra.

Figura 13, 14 y 15 muestran la inserción de y mostrar interfaces en acción. En la figura 13, el usuario ha escrito un valor del precio unitario sin un nombre de producto correspondiente. Figura 14 se muestra la interfaz de la pantalla después de tres nuevos productos se han agregado correctamente, mientras que la figura 15 muestra dos de los productos recién agregados en el control GridView (el tercero es en la página anterior).


[![Un nombre de producto es necesaria al escribir un precio por unidad](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figura 13**: un nombre de producto es necesaria al escribir un precio por unidad ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image39.png))


[![Se han agregado tres verduras nuevo para el proveedor Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figura 14**: tres nuevos verduras se han agregado para el s proveedor Mayumi ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image42.png))


[![Los nuevos productos pueden encontrarse en la última página del control GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figura 15**: los nuevos productos se encuentran en la última página del control GridView ([haga clic aquí para ver imagen en tamaño completo](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> El lote de inserción de lógica que se usa en este tutorial incluye las inserciones dentro del ámbito de transacción. Para comprobarlo, intencionadamente introducir un error de nivel de base de datos. Por ejemplo, en lugar de asignar la nueva `ProductsRow` instancia s `CategoryID` propiedad al valor seleccionado en el `Categories` DropDownList, asigne un valor como `i * 5`. Aquí `i` es el indizador de bucle y tiene valores comprendidos entre 1 y 5. Por consiguiente, al agregar dos o más productos en lote inserción el primer producto tendrá válido `CategoryID` valor (5), pero con productos posteriores tendrán `CategoryID` valores que no coinciden con hasta `CategoryID` valores en el `Categories` tabla. El efecto neto es que mientras la primera `INSERT` se realizará correctamente, se producirá un error de las posteriores con una infracción de restricción de clave externa. Puesto que la inserción de lote es atómica, la primera `INSERT` se revertirá, devolver la base de datos a su estado antes de que el proceso de insertar el lote se inició.


## <a name="summary"></a>Resumen

Sobre esto y los dos tutoriales anteriores que hemos creado interfaces que permiten actualizar, eliminar, y la inserción de lotes de datos, que usa la compatibilidad de transacciones que agregamos a la capa de acceso a datos en el [modificaciones de base de datos de ajuste dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) tutorial. Para determinados escenarios, estas interfaces de usuario de procesamiento por lotes mejoran considerablemente la eficacia del usuario final mediante la reducción del número de clics, las devoluciones de datos y cambios de contexto de teclado para el mouse, al tiempo que mantiene la integridad de los datos subyacentes.

En este tutorial se complete nuestra cómo trabajar con datos por lotes. El siguiente conjunto de tutoriales explora una variedad de escenarios avanzados de capa de acceso a datos, incluido el uso de procedimientos almacenados en los métodos TableAdapter s, configuración de nivel de conexión y comando en la capa DAL, cifrar cadenas de conexión y mucho más!

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Provocar a revisores para este tutorial fueron Hilton Giesenow y S ren Jacob Lauritsen. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-deleting-vb.md)
