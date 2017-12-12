---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Lote Insertar (VB) | Documentos de Microsoft
author: rick-anderson
description: "Obtenga información acerca de cómo insertar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se amplía GridView para permitir al usuario que escriba n varios..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: e1e77dde4602350b18508bf5d71dbcd953f8961c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="batch-inserting-vb"></a>Lote Insertar (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) o [descarga de PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Obtenga información acerca de cómo insertar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario se amplía el control GridView para permitir al usuario que escriba varios registros nuevos. En la capa de acceso a datos se incluya las operaciones de inserción varias dentro de una transacción para asegurarse de que todas las inserciones correctamente o se revierten todas las inserciones.


## <a name="introduction"></a>Introducción

En el [actualización por lotes](batch-updating-vb.md) tutorial analizamos personalizar el control GridView para presentar una interfaz donde estaban puede editables varios registros. El usuario visita la página podría realizar una serie de cambios y, a continuación, con un solo clic el botón, llevar a cabo una actualización por lotes. En situaciones donde los usuarios normalmente actualizar número de registros en una sola operación, dicha interfaz puede guardar un sinfín de clics y cambios de contexto de teclado y mouse cuando se compara con el valor predeterminado por fila características de edición que se exploran en primer lugar en el [un Información general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial.

Este concepto también puede aplicarse cuando se agreguen registros. Imagine que aquí en Northwind Traders normalmente recibimos los envíos de proveedores que contienen un número de productos para una categoría determinada. Por ejemplo, tengamos que recibimos un envío de seis productos de café y té diferentes de Comercial Tasmania. Si un usuario escribe los seis productos de uno en uno mediante un control DetailsView, tendrá que elegir muchos de los mismos valores y otra vez: necesitan elegir la misma categoría (bebidas), el mismo proveedor (Comercial Tasmania), la misma ya no incluye el valor ( False) y las mismas unidades en el valor de orden (0). Esta entrada de datos repetitivos solo no es un proceso lento, pero es propensa a errores.

Con un poco de trabajo podemos crear un lote de inserción de interfaz que permite al usuario elegir el proveedor y la categoría una vez, escribir una serie de nombres de producto y los precios de unidad y, a continuación, haga clic en un botón para agregar los nuevos productos a la base de datos (consulte la figura 1). Al agregar cada producto, su `ProductName` y `UnitPrice` campos de datos se asignan los valores especificados en los cuadros de texto, mientras que sus `CategoryID` y `SupplierID` valores se asignan los valores de las listas desplegables en el fo superior del formulario. El `Discontinued` y `UnitsOnOrder` valores se establecen en los valores codificados de forma rígida de `False` y 0, respectivamente.


[![La interfaz de inserción por lotes](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figura 1**: la interfaz de inserción por lotes ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image3.png))


En este tutorial crearemos una página que implementa el lote de inserción de interfaz que se muestra en la figura 1. Como con los dos tutoriales anteriores, se ajustará las inserciones dentro del ámbito de una transacción para garantizar la atomicidad. Permiten s empiece a trabajar.

## <a name="step-1-creating-the-display-interface"></a>Paso 1: Crear la interfaz de presentación

Este tutorial constará de una sola página que se divide en dos regiones: una región de presentación y un área de inserción. La interfaz de presentación, vamos a crear en este paso, muestra los productos en un control GridView e incluye un botón denominado proceso de envío del producto. Cuando se hace clic en este botón, la interfaz de presentación se sustituye por la interfaz de inserción, que se muestra en la figura 1. Devuelve la interfaz de presentación después de agregar productos de envío o se haga clic en los botones Cancelar. Vamos a crear la interfaz de inserción en el paso 2.

Al crear una página que tiene dos interfaces, solo uno de los cuales está visible a la vez, cada interfaz normalmente se coloca dentro de un [control Panel Web](http://www.w3schools.com/aspnet/control_panel.asp), que actúa como contenedor para otros controles. Por lo tanto, nuestra página tendrá dos controles de Panel uno para cada interfaz.

Comience abriendo la `BatchInsert.aspx` página en el `BatchData` carpeta y arrastre un Panel desde el cuadro de herramientas hasta el diseñador (consulte la figura 2). Establecer el Panel de s `ID` propiedad `DisplayInterface`. Al agregar el Panel hasta el diseñador, su `Height` y `Width` propiedades se establecen en 50 px y 125px, respectivamente. Desactive out estos valores de propiedad de la ventana Propiedades.


[![Arrastre un Panel desde el cuadro de herramientas hasta el diseñador](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figura 2**: arrastrar un Panel desde el cuadro de herramientas hasta el diseñador ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image6.png))


A continuación, arrastre un control de botón y GridView en el Panel. Establecer el botón s `ID` propiedad `ProcessShipment` y su `Text` propiedad proceso de envío del producto. Establecer la s GridView `ID` propiedad `ProductsGrid` y, en su etiqueta inteligente, enlazarla a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para extraer los datos de la `ProductsBLL` clase s. `GetProducts` método. Puesto que este GridView sólo se utiliza para mostrar los datos, establezca las listas desplegables de la actualización, INSERCIÓN y eliminar tabulaciones en (ninguno). Haga clic en Finalizar para completar al Asistente para configurar orígenes de datos.


[![Mostrar los datos devueltos por el método de la clase ProductsBLL s GetProducts](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figura 3**: mostrar los datos que se devuelven desde el `ProductsBLL` clase s `GetProducts` método ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image9.png))


[![Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las pestañas en (ninguno)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figura 4**: establecer las listas de lista desplegable en la actualización, INSERCIÓN y eliminar pestañas en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image12.png))


Después de completar al Asistente de ObjectDataSource, Visual Studio agregará BoundFields y un CampoCasillaVerificación para los campos de datos de producto. Quitar todos menos el `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, y `Discontinued` campos. No dude en realizar personalizaciones estéticas. Se decidió para dar formato a la `UnitPrice` campo como un valor de moneda, reordenar los campos y cambiar el nombre de algunos de los campos `HeaderText` valores. Configurar el control GridView para incluir la paginación y la ordenación compatibilidad activando las casillas de verificación Habilitar paginación y habilitar la ordenación en la etiqueta inteligente de s de GridView.

Después de agregar los controles de Panel, botón, GridView y ObjectDataSource y personalizar los campos de s GridView, el marcado declarativo de la página s debe ser similar al siguiente:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Tenga en cuenta que el marcado para el botón y GridView aparecen dentro de la apertura y cierre `<asp:Panel>` etiquetas. Puesto que estos controles están dentro de la `DisplayInterface` Panel, poder ocultarlos estableciendo simplemente el Panel s `Visible` propiedad `False`. Paso 3 se examina cambiar mediante programación el Panel s `Visible` haga clic en Propiedades en respuesta a un botón para mostrar una interfaz al tiempo que oculta la otra.

Tómese un momento para ver el progreso a través de un explorador. Como se muestra en la figura 5, debería ver un botón de envío del producto de proceso por encima de un control GridView que muestra los diez productos a la vez.


[![GridView muestra los productos y ofrece la ordenación y las capacidades de paginación](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figura 5**: GridView muestra los productos y ofrece la ordenación y las capacidades de paginación ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Paso 2: Crear la interfaz de inserción

Con la interfaz de pantalla completa, se re listo para crear la inserción de la interfaz. Para este tutorial, permiten crear una interfaz de inserción que solicita un valor único de proveedor y categoría y, a continuación, permite al usuario especificar hasta cinco nombres de producto y valores de precio unitario de s. Con esta interfaz, el usuario puede agregar uno a cinco nuevos productos que comparten la misma categoría y proveedor, pero tienen nombres de producto único y precios.

Inicio arrastrando un Panel desde el cuadro de herramientas hasta el diseñador, colocar bajo existente `DisplayInterface` Panel. Establecer el `ID` recientemente agrega la propiedad de este Panel para `InsertingInterface` y establecer su `Visible` propiedad `False`. Vamos a agregar código que establece la `InsertingInterface` Panel s `Visible` propiedad `True` en el paso 3. Borrar el Panel s también `Height` y `Width` valores de propiedad.

A continuación, necesitamos crear la interfaz de inserción que se muestra en la figura 1. Esta interfaz se puede crear a través de una serie de técnicas HTML, pero utilizaremos una bastante sencilla: una tabla de cuatro columnas y filas de siete.

> [!NOTE]
> Al escribir el marcado HTML `<table>` elementos, prefiera usar la vista del origen. Mientras que Visual Studio dispone de herramientas para agregar `<table>` elementos a través del diseñador, el diseñador parece demasiado aceptable inyectar treta para `style` configuración en el marcado. Una vez que ha creado el `<table>` marcado, volver al diseñador para agregar los controles Web y establecer sus propiedades. Cuando se crean tablas con columnas predeterminadas y filas prefiero mediante HTML estático en lugar de la [control Table Web](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.table.aspx) porque los controles Web situados dentro de un control Web de tabla solo se pueden acceder mediante la `FindControl("controlID")` patrón. Sin embargo,, uso controles Web de tabla para las tablas de tamaño dinámicamente (las cuyas filas o columnas se basan en alguna base de datos o los criterios especificados por el usuario), desde la tabla Web control puede crearse mediante programación.


Escriba el siguiente marcado dentro de la `<asp:Panel>` etiquetas de la `InsertingInterface` Panel:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Esto `<table>` marcado no incluye todos los controles Web aún, vamos a agregarlos momentáneamente. Tenga en cuenta que cada `<tr>` elemento contiene un valor específico de clase CSS: `BatchInsertHeaderRow` para la fila de encabezado que el proveedor y la categoría listas desplegables se abrirá; `BatchInsertFooterRow` para la fila de pie de página que se abrirá el agregar productos de envío y los botones Cancelar; y alternos `BatchInsertRow` y `BatchInsertAlternatingRow` controles de cuadro de texto de precios de valores para las filas que va a contener el producto y la unidad. Se ha creado las clases CSS correspondientes en el `Styles.css` archivo para proporcionar a la interfaz de insertar un aspecto similar a la GridView y DetailsView controla se ve que se utilizan a lo largo de estos tutoriales. Estas clases CSS se muestran a continuación.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Con este marcado especificado, volver a la vista de diseño. Esto `<table>` debería aparecer como una tabla de cuatro columnas y filas de siete en el diseñador, como se muestra en la figura 6.


[![La interfaz de inserción se compone de una tabla de una fila de siete de cuatro columnas](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figura 6**: la interfaz de inserción se compone de una tabla de una fila de siete de cuatro columnas ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image18.png))


Se re ahora está listo para agregar los controles Web a la interfaz de inserción. Arrastre dos listas desplegables del cuadro de herramientas a las celdas de la tabla para el proveedor y otra para la categoría adecuadas.

Establecer el proveedor DropDownList s `ID` propiedad `Suppliers` y enlazarla a un nuevo ObjectDataSource denominado `SuppliersDataSource`. Configurar el nuevo ObjectDataSource para recuperar sus datos de la `SuppliersBLL` clase s. `GetSuppliers` método y establezca la actualización pestaña lista de desplegable s a (ninguno). Haga clic en Finalizar para completar al asistente.


[![Configurar el ObjectDataSource para utilizar el método de GetSuppliers SuppliersBLL clase s.](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figura 7**: configurar el ObjectDataSource que se utilizan los `SuppliersBLL` clase s `GetSuppliers` método ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image21.png))


Tiene la `Suppliers` DropDownList mostrar la `CompanyName` campo de datos y use la `SupplierID` del campo de datos como su `ListItem` s valores de.


[![Mostrar el campo de datos de CompanyName y usen SupplierID como el valor](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figura 8**: mostrar la `CompanyName` campo de datos y uso `SupplierID` como el valor ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image24.png))


Nombre de la segunda DropDownList `Categories` y enlazarla a un nuevo ObjectDataSource denominado `CategoriesDataSource`. Configurar la `CategoriesDataSource` ObjectDataSource para usar el `CategoriesBLL` clase s. `GetCategories` método; se establece la lista desplegable que se enumera en las fichas UPDATE y DELETE en (ninguno) y haga clic en Finalizar para completar el asistente. Por último, tener la presentación de DropDownList el `CategoryName` campo de datos y use el `CategoryID` como el valor.

Después de que se han agregado y enlazado a ObjectDataSources configurado adecuadamente estas listas dos desplegables, la pantalla debe ser similar a la figura 9.


[![La fila de encabezado contiene ahora los proveedores y las listas desplegables de categorías](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figura 9**: el encabezado de fila ahora contiene la `Suppliers` y `Categories` listas desplegables ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image27.png))


Ahora debemos crear los cuadros de texto para recopilar el nombre y el precio de cada producto nuevo. Arrastre un control de cuadro de texto del cuadro de herramientas hasta el diseñador para cada una de las filas de nombre y el precio del cinco producto. Establecer el `ID` propiedades de los cuadros de texto a `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`, y así sucesivamente.

Agregar un control CompareValidator después de cada uno de lo cuadros de texto, configuración de precio unitario el `ControlToValidate` propiedad correspondientes `ID`. Establecer el `Operator` propiedad `GreaterThanEqual`, `ValueToCompare` en 0, y `Type` a `Currency`. Esta configuración indica a CompareValidator para asegurarse de que el precio, si escribe, es un valor de moneda válidos que es mayor o igual a cero. Establecer el `Text` propiedad \*, y `ErrorMessage` al precio debe ser mayor o igual a cero. Además, por favor, omita cualquier símbolo de divisa.

> [!NOTE]
> La interfaz de inserción no incluye todos los controles RequiredFieldValidator, aunque la `ProductName` campo el `Products` tabla de base de datos no permite `NULL` valores. Esto es porque queremos permitir que el usuario introduzca hasta cinco productos. Por ejemplo, si el usuario proporcionar el nombre y la unidad precio del producto para las tres primeras filas, dejando las dos últimas filas en blanco, d solo agregamos tres nuevos productos en el sistema. Puesto que `ProductName` es necesario, sin embargo, necesitamos comprobar mediante programación para garantizar que, si un precio de venta se ha escrito que se proporciona un valor de nombre de producto correspondiente. Abordaremos esta comprobación en el paso 4.


Al validar los datos proporcionados por el usuario s, CompareValidator informa de los datos no válidos si el valor contiene un símbolo de moneda. Agregue un signo $ delante de cada uno de lo cuadros de texto para que actúe como una indicación visual que indique al usuario que pasa por alto el símbolo de moneda al escribir el precio del precio unitario.

Por último, agregue un control dentro de la `InsertingInterface` Panel de configuración de su `ShowMessageBox` propiedad `True` y su `ShowSummary` propiedad `False`. Con esta configuración, si el usuario introduce un valor de precio de unidad no válida, aparecerá un asterisco junto a los controles de cuadro de texto incorrecto y el control ValidationSummary mostrará un cuadro de mensajes de cliente que se muestra el mensaje de error que se especificó anteriormente.

En este momento, la pantalla debe ser similar a la figura 10.


[![La interfaz insertar ahora incluye cuadros de texto para los productos de nombres y los precios](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figura 10**: la inserción de interfaz ahora incluye cuadros de texto para los nombres de productos y los precios ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image30.png))


A continuación, necesitamos agregar agregar productos de botones de envío y Cancelar a la fila de pie de página. Arrastre dos controles de botón en el cuadro de herramientas en el pie de página de la interfaz de inserción, establecer los botones `ID` propiedades `AddProducts` y `CancelButton` y `Text` propiedades para agregar productos de envío y Cancelar, respectivamente. Además, establecer el `CancelButton` control s `CausesValidation` propiedad `false`.

Por último, tenemos que agregar un control Web Label que mostrará mensajes de estado de las dos interfaces. Por ejemplo, cuando un usuario agrega correctamente un nuevo envío de productos, queremos volver a la interfaz de presentación y mostrar un mensaje de confirmación. Si, sin embargo, el usuario proporciona un precio de un nuevo producto, pero deja fuera el nombre del producto, es necesario mostrar un mensaje de advertencia desde la `ProductName` campo es obligatorio. Puesto que necesitamos este mensaje para mostrar para ambas interfaces, lo coloca en la parte superior de la página fuera de los paneles.

Arrastre un control Web Label del cuadro de herramientas a la parte superior de la página en el diseñador. Establecer el `ID` propiedad `StatusLabel`, desactive out el `Text` propiedad y establezca el `Visible` y `EnableViewState` propiedades para `False`. Como hemos visto en tutoriales anteriores, establecer el `EnableViewState` propiedad `False` nos permite cambiar los valores de propiedad de etiqueta s mediante programación y pídales que revertir automáticamente a sus valores predeterminados en la devolución de datos subsiguientes. Esto simplifica el código para mostrar un mensaje de estado en respuesta a alguna acción del usuario que desaparece en la devolución de datos subsiguientes. Por último, establezca el `StatusLabel` control s `CssClass` propiedad a Warning, que es el nombre de una clase CSS definida en `Styles.css` que muestra texto en una fuente grande, cursiva, negrita y roja.

La figura 11 muestra el Diseñador de Visual Studio después de la etiqueta se ha agregado y configurado.


[![Coloque el Control StatusLabel como encima de los dos controles de Panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figura 11**: lugar la `StatusLabel` Control encima de los dos controles de Panel ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Paso 3: Cambiar entre la presentación y la inserción de Interfaces

En este punto hemos completado el marcado para la visualización e insertar interfaces, pero estamos re aún quedaba dos tareas:

- Cambiar entre la visualización e inserción de interfaces
- Agregar los productos en el envío a la base de datos

Actualmente, la interfaz de presentación es visible pero está oculta la interfaz de inserción. Esto es porque el `DisplayInterface` Panel s `Visible` propiedad está establecida en `True` (el valor predeterminado), mientras que la `InsertingInterface` Panel s `Visible` propiedad está establecida en `False`. Para cambiar entre las dos interfaces necesitamos alternar cada control s `Visible` valor de propiedad.

Debe mover de la interfaz de presentación a la interfaz de inserción cuando se hace clic en el botón de envío del producto de proceso. Por lo tanto, crear un controlador de eventos para que este botón s `Click` eventos que contiene el código siguiente:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Este código simplemente oculta el `DisplayInterface` Panel y se muestra en el `InsertingInterface` Panel.

A continuación, crear controladores de eventos para agregar productos desde los controles de envío y el botón Cancelar de la interfaz de inserción. Cuando se hace clic en cualquiera de estos botones, es necesario volver a la interfaz de presentación. Crear `Click` controladores de eventos para los controles de botón para que llame a `ReturnToDisplayInterface`, un método que agregará en breve. Además de ocultar la `InsertingInterface` Panel y mostrar el `DisplayInterface` el Panel, el `ReturnToDisplayInterface` método debe devolver los controles Web a su estado anterior de edición. Esto implica el establecimiento de las listas desplegables `SelectedIndex` propiedades en 0 y borrando la `Text` propiedades de los controles de cuadro de texto.

> [!NOTE]
> Tenga en cuenta qué puede ocurrir si se Professionals t controles vuelvan a su estado anterior edición antes de volver a la interfaz de presentación. Un usuario podría haga clic en el botón de envío del producto de proceso, escriba los productos desde el envío y, a continuación, haga clic en Agregar productos de envío. Esto sería agregar los productos y el usuario regresará a la interfaz de presentación. En este momento el usuario desea agregar otro envío. Al hacer clic en el botón de envío del producto de proceso que devolvería a la interfaz de inserción pero DropDownList selecciones y los valores de cuadro de texto aún se rellenaría con sus valores anteriores.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Ambos `Click` basta con llamar controladores de eventos el `ReturnToDisplayInterface` método, aunque se tendrá que volver a agregar productos de envío `Click` controlador de eventos en el paso 4 y agregue código para guardar los productos. `ReturnToDisplayInterface`inicia devolviendo el `Suppliers` y `Categories` listas desplegables para sus primeras opciones. Las dos constantes `firstControlID` y `lastControlID` marcar iniciales y finales de los valores de índice de control utilizados para designar el nombre y la unidad precio del producto cuadros de texto en la inserción de la interfaz y se usan en los límites de la `For` bucle que establece el `Text`de respaldo de propiedades de los controles de cuadro de texto en una cadena vacía. Por último, los paneles `Visible` se restablecen propiedades para que se oculta la interfaz de inserción y se muestra la interfaz de presentación.

Tómese un momento para probar esta página en un explorador. Al visitar la página de en primer lugar verá la interfaz de presentación tal y como se muestra en la figura 5. Haga clic en el botón de envío del producto de proceso. La página se de devolución de datos y ahora debería ver la interfaz de inserción tal como se muestra en la figura 12. Al hacer clic en cualquier los productos agregar botones de envío o en Cancelar, se devuelve a la interfaz de presentación.

> [!NOTE]
> Durante la visualización de la interfaz de inserción, tómese un momento para probar el CompareValidators en el precio unitario cuadros de texto. Debería ver un cuadro de mensajes de cliente de advertencia cuando haga clic en Agregar productos de botón de envío con valores de moneda no válido o los precios con un valor menor que cero.


[![Se muestra la interfaz de inserción tras hacer clic en el botón de envío del producto de proceso](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figura 12**: se muestra la interfaz de inserción tras hacer clic en el botón de envío del producto de proceso ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Paso 4: Agregar los productos

Todo lo que queda de este tutorial es guardar los productos en la base de datos en los productos agregar de botón de envío s `Click` controlador de eventos. Esto puede realizarse mediante la creación de un `ProductsDataTable` y agregando un `ProductsRow` instancia para cada uno de los nombres de producto proporcionados. Una vez estos `ProductsRow` se han agregado se realizará una llamada a la `ProductsBLL` clase s `UpdateWithTransaction` método pasando la `ProductsDataTable`. Recuerde que el `UpdateWithTransaction` método, que se creó en el [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, pasa el `ProductsDataTable` a la `ProductsTableAdapter` s `UpdateWithTransaction` método. Desde allí, se inicia una transacción de ADO.NET y los problemas de TableAdatper una `INSERT` instrucción a la base de datos para cada agregado `ProductsRow` en la tabla de datos. Suponiendo que se agregan todos los productos sin errores, que se confirma la transacción, en caso contrario se revierte.

El código para agregar productos de botón de envío s `Click` controlador de eventos también se necesita realizar un poco de comprobación de errores. Dado que no hay ningún RequiredFieldValidators utilizado en la interfaz de inserción, un usuario podría especificar un precio de un producto mientras se omite su nombre. Puesto que el nombre de producto s es obligatorio, si se desarrolla una condición de ese tipo, hay que advertir al usuario y no continuar con las inserciones. La sección completa `Click` sigue el código del controlador de eventos:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

El controlador de eventos inicia asegurándose de que el `Page.IsValid` propiedad devuelve un valor de `True`. Si devuelve `False`, a continuación, que significa que uno o varios de los CompareValidators están informando de datos no válidos; en este caso no queremos intentar insertar los productos especificados o se terminará con una excepción al intentar asignar el precio unitario escrito por el usuario valor para el `ProductsRow` s `UnitPrice` propiedad.

Siguiente, un nuevo `ProductsDataTable` se crea la instancia (`products`). A `For` bucle se usa para recorrer en iteración el producto nombre y el precio unitario cuadros de texto y el `Text` propiedades se leen en las variables locales `productName` y `unitPrice`. Si el usuario ha escrito un valor para el precio unitario, pero no para el nombre del producto correspondiente, el `StatusLabel` muestra el mensaje si se proporciona una unidad de precio, también debe incluir el nombre del producto y se sale del controlador de eventos.

Si se ha proporcionado un nombre de producto, un nuevo `ProductsRow` instancia se crea utilizando el `ProductsDataTable` s `NewProductsRow` método. Esta nueva `ProductsRow` instancia s `ProductName` propiedad está establecida en el producto actual cuadro de texto al nombre del `SupplierID` y `CategoryID` propiedades se asignan a la `SelectedValue` propiedades de las listas desplegables en el encabezado de la interfaz s insertar. Si el usuario especificó un valor para el precio del producto s, se asigna a la `ProductsRow` instancia s `UnitPrice` propiedad; en caso contrario, la propiedad es izquierda sin asignar, lo que dará como resultado un `NULL` valor para `UnitPrice` en la base de datos. Por último, el `Discontinued` y `UnitsOnOrder` propiedades se asignan a los valores codificados de forma rígida `False` y 0, respectivamente.

Después de que las propiedades se han asignado a la `ProductsRow` instancia se agrega a la `ProductsDataTable`.

Al finalizar la `For` bucles, comprobamos si se han agregado todos los productos. El usuario puede, después de todo, ha hecho clic en Agregar productos de envío antes de especificar los nombres de producto o de los precios. Si hay al menos uno de los productos en la `ProductsDataTable`, `ProductsBLL` clase s `UpdateWithTransaction` se llama al método. A continuación, se vuelve a enlazar los datos a la `ProductsGrid` GridView para que los productos recién agregados aparecerá en la interfaz de presentación. El `StatusLabel` se actualiza para mostrar un mensaje de confirmación y la `ReturnToDisplayInterface` se invoca, ocultar la interfaz de insertar y que muestra la interfaz de presentación.

Si no se especificó ningún producto, la interfaz de inserción permanece abierta pero el mensaje que no se agregaron ningún producto. Escriba los nombres de producto y los precios de venta en los cuadros de texto se muestra.

Figura 13, 14 y 15 mostrar la inserción y muestren las interfaces en acción. En la figura 13, el usuario ha escrito un valor del precio unitario sin un nombre de producto correspondiente. Figura 14 muestra la interfaz de presentación después de tres nuevos productos se agregaron correctamente, mientras que la figura 15 muestra dos de los productos recién agregados en GridView (el tercero es en la página anterior).


[![Un nombre de producto es necesaria cuando especifica un precio por unidad](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figura 13**: un nombre de producto es necesaria cuando especifica un precio unitario ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image39.png))


[![Se han agregado tres verduras nuevo para el proveedor Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figura 14**: tres nuevos verduras se han agregado para las operaciones de asignación Mayumi de proveedor ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image42.png))


[![Los nuevos productos pueden encontrarse en la última página de GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figura 15**: el nuevo productos se encuentran en la última página de GridView ([haga clic aquí para ver la imagen a tamaño completo](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> El lote insertar lógica que se utiliza en este tutorial ajusta las inserciones dentro del ámbito de transacción. Para comprobarlo, introducir intencionadamente un error de nivel de base de datos. Por ejemplo, en lugar de asignar la nueva `ProductsRow` instancia s `CategoryID` valor para la propiedad seleccionada en el `Categories` DropDownList, asignar a un valor como `i * 5`. Aquí `i` es el indizador de bucle y tiene valores comprendidos entre 1 y 5. Por lo tanto, al agregar dos o más productos en lote inserción el primer producto tendrá válido `CategoryID` valor (5), pero productos posteriores tendrán `CategoryID` valores que no coinciden hasta `CategoryID` valores en el `Categories` tabla. El efecto neto es que mientras la primera `INSERT` se realizará correctamente, las posteriores se producirá un error con una infracción de restricción de clave externa. Puesto que la inserción de lote es atómica, el primer `INSERT` se revertirá, devolver la base de datos al estado que tenía antes de que el proceso de insertar el lote se inició.


## <a name="summary"></a>Resumen

Sobre esto y los dos tutoriales anteriores hemos creado interfaces que permiten actualizar, eliminar, y la inserción de lotes de datos, que utiliza la compatibilidad de transacciones se agrega a la capa de acceso de datos en el [ajuste modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) tutorial. Para determinados escenarios, dichas interfaces de usuario de procesamiento por lotes mejoran considerablemente el usuario final eficacia cortando hacia abajo en el número de clics y devoluciones, cambios de contexto de teclado y mouse, al tiempo que mantiene la integridad de los datos subyacentes.

Este tutorial completa nuestro aspecto al trabajar con datos por lotes. El siguiente conjunto de tutoriales explora una variedad de escenarios avanzados de capa de acceso a datos, incluido el uso de procedimientos almacenados en los métodos de s TableAdapter, configurar los valores de nivel de conexión y comando en la capa DAL, cifrar cadenas de conexión y mucho más!

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Provocar a revisores para este tutorial fueron Hilton Giesenow y S ren Jacob Lauritsen. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](batch-deleting-vb.md)
