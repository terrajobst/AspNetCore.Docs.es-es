---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: Insertar un nuevo registro de pie de página de GridView (C#) | Documentos de Microsoft
author: rick-anderson
description: Mientras el control GridView no proporciona compatibilidad integrada para insertar un nuevo registro de datos, este tutorial muestra cómo aumentar GridView para incluir un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: f131280f4769507d169f8ada7568184233591446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Insertar un nuevo registro de pie de página de GridView (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) o [descarga de PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Mientras el control GridView no proporciona compatibilidad integrada para insertar un nuevo registro de datos, este tutorial muestra cómo aumentar GridView para incluir una interfaz de inserción.


## <a name="introduction"></a>Introducción

Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, los controles GridView, DetailsView y FormView Web incluyen funciones de modificación de datos integrados. Cuando se usa con controles de origen de datos declarativo, el estos tres controles Web pueden configurarse de forma rápida y sencilla para modificar datos y en escenarios sin necesidad de escribir una sola línea de código. Por desgracia, solo los controles DetailsView y FormView proporcionan integrados Insertar, modificar y eliminar las capacidades. GridView sólo ofrece edición y eliminación de soporte técnico. Sin embargo, con un poco de codo de grasa, podemos aumentar GridView para incluir una interfaz de inserción.

Para agregar funciones de inserción a la GridView, somos responsables de decidir en cómo los registros nuevos se agregarán, creación de la interfaz de inserción y escribir el código para insertar el nuevo registro. En este tutorial, veremos agregar la interfaz de inserción en el pie de página de GridView s fila (consulte la figura 1). La celda de pie de página para cada columna incluye el elemento de interfaz de usuario de recopilación de datos correspondiente (un cuadro de texto para el nombre del producto s, DropDownList para el proveedor y así sucesivamente). También se necesita una columna para un complemento botón que, al hacer clic en, provocará una devolución de datos e insertar un nuevo registro en el `Products` tabla usando los valores proporcionados en la fila de pie de página.


[![La fila de pie de página proporciona una interfaz para agregar nuevos productos](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: la fila de pie de página proporciona una interfaz para agregar nuevos productos ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Paso 1: Mostrar información del producto en un control GridView

Antes de que se hacen referencia a nosotros mismos con la creación de la interfaz de inserción en el pie de página de s GridView, permiten foco primera s sobre cómo agregar un control GridView a la página que muestra los productos en la base de datos. Comience abriendo la `InsertThroughFooter.aspx` página en el `EnhancedGridView` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, configurando la s GridView `ID` propiedad `Products`. A continuación, utilice la etiqueta inteligente de GridView s para enlazar a un nuevo ObjectDataSource denominado `ProductsDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Figura 2**: crear un nuevo ObjectDataSource denominado `ProductsDataSource` ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))


Configurar el ObjectDataSource para usar el `ProductsBLL` clase s. `GetProducts()` método para recuperar información del producto. Para este tutorial, permiten foco s estrictamente sobre cómo agregar funciones de inserción y no se preocupe acerca de la edición y eliminación. Por lo tanto, asegúrese de que se establece la lista desplegable en la pestaña Insertar en `AddProduct()` y que las listas desplegables en las pestañas UPDATE y DELETE se establecen en (ninguno).


[![El método AddProduct se asignan al método ObjectDataSource s Insert()](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Figura 3**: mapa la `AddProduct` método para las operaciones de asignación ObjectDataSource `Insert()` método ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))


[![Establece la actualización y eliminación tabulaciones listas desplegables en (ninguno)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Figura 4**: establezca la actualización y eliminar listas de lista desplegable de pestañas (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))


Después de completar al Asistente para configurar origen de datos de ObjectDataSource s, Visual Studio agregará automáticamente campos a GridView para cada uno de los campos de datos correspondiente. Por ahora, deje todos los campos agregados por Visual Studio. Más adelante en este tutorial se podrá regresar y quitar algunos de los campos cuyos valores don t deben especificarse cuando se agrega un nuevo registro.

Dado que hay cerca de 80 productos de la base de datos, un usuario tenga que desplazarse hasta la parte inferior de la página web con el fin de agregar un nuevo registro. Por lo tanto, permiten s habilitar la paginación hacer la interfaz insertar más visible y accesible. Para activar la paginación, solo tiene que activar la casilla de verificación Habilitar paginación de la etiqueta inteligente de s de GridView.

En este momento, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]


[![Todos los campos de datos de producto se muestran en un control GridView paginado](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Figura 5**: todos los campos de datos de producto se muestran en un control GridView paginado ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Paso 2: Agregar una fila de pie de página

Junto con su encabezado y filas de datos, el control GridView incluye una fila de pie de página. Las filas de encabezado y pie de página se muestran según los valores de las operaciones de asignación GridView [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) y [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) propiedades. Para mostrar la fila de pie de página, basta con establecer la `ShowFooter` propiedad `true`. Como se muestra en la figura 6, establecer el `ShowFooter` propiedad `true` agrega una fila de pie de página a la cuadrícula.


[![Para mostrar la fila de pie de página, establezca ShowFooter en True](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Figura 6**: para mostrar la fila de pie de página, establezca `ShowFooter` a `True` ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))


Tenga en cuenta que la fila de pie de página tiene un color de fondo rojo oscuro. Esto es porque el tema DataWebControls se crean y se aplican a todas las páginas en el [mostrar datos con el ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial. En concreto, el `GridView.skin` archivo configura el `FooterStyle` propiedad de este tipo que utiliza la `FooterStyle` clase CSS. El `FooterStyle` clase se define en `Styles.css` como se indica a continuación:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Se ha explorado con la fila de pie de página de GridView s en tutoriales anteriores. Si es necesario, hacen referencia a la [mostrar información de resumen en el pie de página de GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) tutorial para repasar.


Después de establecer el `ShowFooter` propiedad `true`, tómese un momento para ver el resultado en un explorador. Actualmente el t de fila de pie de página contiene texto ni controles Web. En el paso 3 se modificará el pie de página para cada campo de GridView para que incluya la interfaz adecuada de inserción.


[![La fila de pie de página vacía es aparece encima de la paginación controles de interfaz](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Figura 7**: fila el pie de página vacía es aparece encima de la paginación controles de interfaz ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Paso 3: Personalizar la fila de pie de página

En el [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial hemos visto cómo personalizar en gran medida la presentación de una determinada columna de GridView con TemplateFields (en lugar de BoundFields o CheckBoxFields); en [ Personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) analizamos uso TemplateFields para personalizar la interfaz de edición en un control GridView. Recuerde que TemplateField se compone de una serie de plantillas que define la combinación de marcado, los controles Web y la sintaxis de enlace de datos había usada para ciertos tipos de filas. El `ItemTemplate`, por ejemplo, especifica la plantilla utilizada para las filas de solo lectura, mientras que la `EditItemTemplate` define la plantilla para la fila editable.

Junto con el `ItemTemplate` y `EditItemTemplate`, TemplateField también incluye un `FooterTemplate` que especifica el contenido de la fila de pie de página. Por lo tanto, podemos agregar los controles Web necesarios para cada campo s insertar interfaz en el `FooterTemplate`. Para iniciar, convertir todos los campos en el control GridView en TemplateFields. Esto puede hacerse el vínculo Editar columnas de la GridView s etiqueta inteligente, seleccione cada campo en la esquina inferior izquierda y haga clic en la función Convert este campo en un vínculo de TemplateField.


![Convierte cada campo en TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Figura 8**: convierte cada campo en TemplateField


Haga clic en la función Convert este campo en TemplateField activa el tipo de campo actual a un TemplateField equivalente. Por ejemplo, se reemplaza cada BoundField por TemplateField con un `ItemTemplate` que contiene una etiqueta que muestra el campo de datos correspondiente y un `EditItemTemplate` que muestra el campo de datos en un cuadro de texto. El `ProductName` BoundField se ha convertido en el siguiente marcado TemplateField:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Del mismo modo, el `Discontinued` CampoCasillaVerificación se ha convertido en TemplateField cuya `ItemTemplate` y `EditItemTemplate` contienen un control de casilla de verificación Web (con el `ItemTemplate` s casilla deshabilitada). Sólo lectura `ProductID` BoundField se ha convertido en TemplateField con un control de etiqueta tanto en el `ItemTemplate` y `EditItemTemplate`. En resumen, convertir un GridView existente en TemplateField es una manera rápida y sencilla para cambiar a la más personalizable TemplateField sin perder ninguna de las funciones de campo s existente.

Desde el control GridView se está trabajando t admiten la edición, no dude en quitar el `EditItemTemplate` desde cada TemplateField, dejando sólo los `ItemTemplate`. Una vez hecho esto, el código de marcado declarativo de GridView s debería ser similar al siguiente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Ahora que se ha convertido cada campo de GridView en TemplateField, podemos escribir la interfaz de inserción adecuada en cada campo s `FooterTemplate`. Algunos de los campos no tendrán una interfaz Insertar (`ProductID`, por ejemplo); otros usuarios varían en los controles Web sirve para recopilar la información de producto s nueva.

Para crear la interfaz de edición, elija el vínculo Editar plantillas de la etiqueta inteligente de s de GridView. A continuación, en la lista desplegable, seleccione el campo correspondiente s `FooterTemplate` y arrastre el control adecuado en el cuadro de herramientas hasta el diseñador.


[![Agregar la interfaz de inserción adecuada a cada FooterTemplate s de campo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Figura 9**: agregar la interfaz adecuada de inserción para cada campo s `FooterTemplate` ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))


En la lista con viñetas siguiente enumera los campos de GridView, especificar la interfaz de inserción para agregar:

- `ProductID` Ninguno.
- `ProductName` Agregue un cuadro de texto y establezca su `ID` a `NewProductName`. Agregue un control RequiredFieldValidator para asegurarse de que el usuario escribe un valor para el nuevo nombre de producto s.
- `SupplierID` Ninguno.
- `CategoryID` Ninguno.
- `QuantityPerUnit` Agregar un cuadro de texto, establecer su `ID` a `NewQuantityPerUnit`.
- `UnitPrice` Agregar un cuadro de texto denominado `NewUnitPrice` y un control CompareValidator que garantiza que el valor especificado es un valor de moneda mayor que o igual a cero.
- `UnitsInStock` usar un cuadro de texto cuyo `ID` está establecido en `NewUnitsInStock`. Incluir un control CompareValidator que garantiza que el valor especificado es un valor entero mayor o igual a cero.
- `UnitsOnOrder` usar un cuadro de texto cuyo `ID` está establecido en `NewUnitsOnOrder`. Incluir un control CompareValidator que garantiza que el valor especificado es un valor entero mayor o igual a cero.
- `ReorderLevel` usar un cuadro de texto cuyo `ID` está establecido en `NewReorderLevel`. Incluir un control CompareValidator que garantiza que el valor especificado es un valor entero mayor o igual a cero.
- `Discontinued` Agregar un control CheckBox, establecer su `ID` a `NewDiscontinued`.
- `CategoryName` Agregue un DropDownList y establezca su `ID` a `NewCategoryID`. Enlazar a un nuevo ObjectDataSource denominado `CategoriesDataSource` y configúrelo para utilizar el `CategoriesBLL` clase s. `GetCategories()` método. Tiene la s DropDownList `ListItem` mostrar s el `CategoryName` datos campo, con el `CategoryID` campo de datos como sus valores.
- `SupplierName` Agregue un DropDownList y establezca su `ID` a `NewSupplierID`. Enlazar a un nuevo ObjectDataSource denominado `SuppliersDataSource` y configúrelo para utilizar el `SuppliersBLL` clase s. `GetSuppliers()` método. Tiene la s DropDownList `ListItem` mostrar s el `CompanyName` datos campo, con el `SupplierID` campo de datos como sus valores.

Para cada uno de los controles de validación, borrar el `ForeColor` propiedad para que la `FooterStyle` se usará el color de primer plano blanco de la clase s CSS en lugar del predeterminado rojo. Usar el `ErrorMessage` propiedad para obtener una descripción detallada, pero establece el `Text` propiedad con un asterisco. Para evitar que el texto del control s validación haciendo que la interfaz de inserción ajustar en dos líneas, establezca el `FooterStyle` s `Wrap` propiedad en false para cada uno de los `FooterTemplate` que utilizan un control de validación. Por último, agregue un control debajo la GridView y establezca su `ShowMessageBox` propiedad `true` y su `ShowSummary` propiedad `false`.

Al agregar un nuevo producto, es necesario proporcionar el `CategoryID` y `SupplierID`. Esta información se captura a través de las listas desplegables de las celdas de pie de página para la `CategoryName` y `SupplierName` campos. Eligió usar estos campos en contraposición a la `CategoryID` y `SupplierID` TemplateFields dado en la cuadrícula s filas de datos del usuario es probable que más le interesa ver los nombres de categoría y proveedor en lugar de los valores del identificador. Puesto que la `CategoryID` y `SupplierID` valores ahora se están capturando en el `CategoryName` y `SupplierName` interfaces Insertar s de campo, podemos quitar el `CategoryID` y `SupplierID` TemplateFields de GridView.

De forma similar, el `ProductID` no se utiliza cuando se agrega un nuevo producto, por lo que el `ProductID` TemplateField también se puede quitar. No obstante, permiten s deje el `ProductID` campo en la cuadrícula. Además de los cuadros de texto, listas desplegables, casillas y controles de validación que conforman la interfaz de inserción, también será necesario agregar botón que, al hacer clic, ejecuta la lógica para agregar el nuevo producto a la base de datos. En el paso 4 incluiremos un botón Add en la interfaz de inserción en el `ProductID` TemplateField s `FooterTemplate`.

No dude en mejorar el aspecto de los distintos campos de GridView. Por ejemplo, puede dar formato a la `UnitPrice` valores como una moneda, Alinear a la derecha el `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` campos y actualizar el `HeaderText` valores para el TemplateFields.

Después de crear la infinidad de inserción de interfaces en el `FooterTemplate` s, quitar la `SupplierID`, y `CategoryID` TemplateFields y se mejora la estética de la cuadrícula a través de formato y alinear el TemplateFields, la s de GridView declarativa marcado debe ser similar al siguiente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Cuando se visualizan a través de un explorador, la fila de pie de página de GridView s ahora incluye la haya finalizado de inserción de la interfaz (consulte la figura 10). En este momento, la inserción t de interfaz son un medio para el usuario indicar que escribió los datos para el nuevo producto y desea insertar un nuevo registro en la base de datos. Además, se ve todavía para abordar cómo los datos introducidos en el pie de página se traducirán en un nuevo registro en el `Products` base de datos. En el paso 4, analizaremos cómo incluir un botón de agregar a la interfaz de inserción y cómo ejecutar código de devolución de datos cuando lo s hizo clic. Paso 5 muestra cómo insertar un nuevo registro con los datos del pie de página.


[![El pie de página de GridView proporciona una interfaz para agregar un nuevo registro](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Figura 10**: el pie de página de GridView proporciona una interfaz para agregar un nuevo registro ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Paso 4: Incluir un botón Add en la interfaz de inserción

Es necesario incluir un botón Add en alguna parte en la interfaz de inserción porque la s de la fila de pie de página Insertar interfaz actualmente no tiene medios para el usuario indicar que ha completado escribir la nueva información de producto s. Esto podría colocarse en una de las existentes `FooterTemplate` s o se puede agregar una nueva columna a la cuadrícula para este propósito. Para este tutorial, s permiten colocar en el botón Agregar el `ProductID` TemplateField s `FooterTemplate`.

En el diseñador, haga clic en el vínculo Editar plantillas en la etiqueta inteligente de GridView s y, a continuación, elija la `ProductID` campo s `FooterTemplate` en la lista desplegable. Agregar un control Button Web (o LinkButton o ImageButton, si lo prefiere) a la plantilla, estableciendo su ID `AddProduct`, sus `CommandName` para Insert y su `Text` propiedad Agregar tal como se muestra en la figura 11.


[![Coloque el botón Agregar en la plantilla FooterTemplate s de ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Figura 11**: coloque el botón Agregar en el `ProductID` TemplateField s `FooterTemplate` ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))


Una vez que se ha había incluido el botón Agregar, pruebe la página en un explorador. Tenga en cuenta que al hacer clic en el botón Agregar con datos no válidos en la interfaz de inserción, la devolución de datos es de circuito corto y el control indica que los datos no válidos (vea la figura 12). Con los datos pertinentes especificados, al hacer clic en el botón Agregar provoca una devolución de datos. No hay ningún registro se agrega a la base de datos, sin embargo. Necesitamos un poco de código para llevar a cabo la inserción de escritura.


[![El botón Agregar devolución de datos es de circuito corto si no hay datos no válidos en la interfaz de inserción](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Figura 12**: el botón Agregar devolución de datos es de circuito corto si no hay datos no válidos en la interfaz de inserción ([haga clic aquí para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))


> [!NOTE]
> Los controles de validación en la interfaz de inserción no se asignaron a un grupo de validación. Esto funciona bien siempre y cuando la interfaz de inserción es el conjunto único de controles de validación en la página. Si, sin embargo, hay otros controles de validación en la página (como los controles de validación de la interfaz de edición de cuadrícula s), los controles de validación en la inserción de la interfaz y agregar botón s `ValidationGroup` propiedades deben asignarse el mismo valor de fin asociar estos controles a un grupo de validación en particular. Vea [Diseccionando los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) para obtener más información acerca de las particiones de los botones en una página y los controles de validación en los grupos de validación.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Paso 5: Insertar un nuevo registro en el`Products`tabla

Al utilizar las características de edición integradas del control GridView, GridView controla automáticamente todo el trabajo necesario para realizar la actualización. En concreto, cuando se hace clic en el botón de actualización copia los valores especificados de la interfaz de edición para los parámetros en las operaciones de asignación ObjectDataSource `UpdateParameters` recopilación y aparece desactivar la actualización mediante la invocación de las operaciones de asignación ObjectDataSource `Update()` método. Puesto que el control GridView no proporciona esta funcionalidad integrada para insertar, debemos implementar código que llama a las operaciones de asignación ObjectDataSource `Insert()` método y copia los valores de inserción de la interfaz para las operaciones de asignación ObjectDataSource `InsertParameters` colección .

Esta lógica de inserción se debería ejecutar después de que se ha hecho clic el botón Agregar. Como se describe en el [agregar y responder a los botones en un control GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) tutorial, cada vez que un botón, LinkButton o ImageButton en un control GridView se hace clic, la s GridView `RowCommand` desencadena el evento de postback. Este evento se desencadena si el botón, LinkButton o ImageButton se agregó explícitamente como el botón Agregar en la fila de pie de página o si se agrega automáticamente mediante el control GridView (como el LinkButton en la parte superior de cada columna cuando se selecciona la opción Habilitar ordenación, o LinkButton en la interfaz de paginación cuando se selecciona Habilitar paginación).

Por lo tanto, para responder al usuario hacer clic en el botón Agregar, es necesario crear un controlador de eventos para el s GridView `RowCommand` eventos. Puesto que este evento se desencadena cada vez que *cualquier* se hace clic en Button, LinkButton o ImageButton en GridView, lo s vital que se continúe solo con la lógica de inserción si la `CommandName` propiedad pasará las asignaciones de controlador de eventos para el `CommandName` valor del botón Agregar (Insert). Además, nos debemos continúe también solo si los controles de validación de datos válidos de informes. Para dar cabida a esto, cree un controlador de eventos para el `RowCommand` evento con el código siguiente:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Quizás se pregunte por qué el controlador de eventos molesta comprobando el `Page.IsValid` propiedad. ¿Después de todo, se pueden suprimir ganado t la devolución de datos si no se proporcionan datos no válidos en la interfaz de inserción? Esta suposición es correcta, siempre y cuando el usuario no ha deshabilitado el JavaScript o ha tomado medidas para evitar la lógica de validación del lado cliente. En resumen, uno nunca debería basarse estrictamente en la validación del lado cliente; siempre debe realizarse una comprobación de servidor para la validez antes de trabajar con los datos.


En el paso 1 se creó el `ProductsDataSource` ObjectDataSource que su `Insert()` método se asigna a la `ProductsBLL` clase s. `AddProduct` método. Para insertar el nuevo registro en el `Products` tabla, basta con que podemos llamar a las operaciones de asignación ObjectDataSource `Insert()` método:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Ahora que el `Insert()` se ha invocado el método, todo lo que queda es copiar los valores de la interfaz de inserción a los parámetros pasados a la `ProductsBLL` clase s. `AddProduct` método. Como hemos visto en la [examinar los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial, esto puede realizarse a través de las operaciones de asignación ObjectDataSource `Inserting` eventos. En el `Inserting` evento tenemos que hacer referencia mediante programación a los controles de la `Products` pie de página de GridView s filas y asignar los valores que se la `e.InputParameters` colección. Si el usuario la omita un valor como dejando la `ReorderLevel` en blanco del cuadro de texto es necesario para especificar que el valor insertado en la base de datos debe ser `NULL`. Puesto que la `AddProducts` método acepta los tipos que aceptan valores NULL para los campos de la base de datos que aceptan valores NULL, simplemente, use un tipo que acepta valores NULL y establezca su valor en `null` en el caso donde se omiten los proporcionados por el usuario.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Con el `Inserting` completado un controlador de eventos, se pueden agregar nuevos registros a la `Products` tabla de base de datos a través de la fila de pie de página de s de GridView. Continúe y pruebe a agregar varios productos nuevo.

## <a name="enhancing-and-customizing-the-add-operation"></a>Mejorar y personalizar la operación de agregar

Actualmente, al hacer clic en el botón Agregar agrega un nuevo registro a la tabla de base de datos pero no proporciona a ningún tipo de información visual que el registro se ha agregado correctamente. Idealmente, un cuadro de alerta de control o de cliente de Web Label podría informar al usuario que su insertar ha finalizado con éxito. Dejar este como un ejercicio para el lector.

GridView usado en este tutorial no aplica ningún criterio de ordenación para los productos enumerados, ni tampoco permite al usuario final ordenar los datos. Por lo tanto, los registros se ordenan como aparecen en la base de datos por su campo de clave principal. Puesto que cada nuevo registro tiene una `ProductID` valor mayor que la última de ellas, cada vez que se agrega un nuevo producto se ha agregado al final de la cuadrícula. Por lo tanto, puede que desee enviar automáticamente al usuario a la última página de GridView después de agregar un nuevo registro. Esto puede realizarse mediante la adición de la siguiente línea de código después de llamar a `ProductsDataSource.Insert()` en el `RowCommand` controlador de eventos para indicar que el usuario debe enviarse a la última página después de enlazar los datos en GridView:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage` se asigna una variable booleana de nivel de página que al principio, es un valor de `false`. En las operaciones de asignación GridView `DataBound` controlador de eventos, si `SendUserToLastPage` es false, el `PageIndex` propiedad se actualiza para enviar al usuario a la última página.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

La razón la `PageIndex` propiedad se establece en el `DataBound` controlador de eventos (en contraposición a la `RowCommand` controlador de eventos) es porque cuando el `RowCommand` controlador de eventos se desencadena tegorías todavía para agregar el nuevo registro a la `Products` tabla de base de datos. Por lo tanto, en la `RowCommand` el último índice de la página del controlador de eventos (`PageCount - 1`) representa el último índice de la página *antes de* se ha agregado el nuevo producto. Para la mayoría de los productos que se agrega, el último índice de la página es el mismo después de agregar el nuevo producto. Pero cuando el producto agregado como resultado en un nuevo índice de página último, si se actualiza correctamente el `PageIndex` en el `RowCommand` controlador de eventos, a continuación, se le dirigirá a la segunda a la última página (el último índice de la página antes de agregar el nuevo producto) en lugar de la última página nueva i Ajustar valores. Puesto que la `DataBound` controlador de eventos se desencadena después de que se ha agregado el nuevo producto y vuelve a enlazar los datos a la cuadrícula, estableciendo el `PageIndex` propiedad existe sabemos se re obtener el último índice de página correctos.

Por último, GridView usado en este tutorial es bastante amplio debido al número de campos que deben recopilarse para agregar un nuevo producto. A causa de este ancho, un diseño vertical de DetailsView s puede ser preferido. Las operaciones de asignación GridView ancho global podría reducirse mediante la recopilación de entradas menos. Quizás no queremos t necesario recopilar la `UnitsOnOrder`, `UnitsInStock`, y `ReorderLevel` campos al agregar un nuevo producto, en cuyo caso se pudieron quitar estos campos de GridView.

Para ajustar los datos recopilados, podemos usar uno de estos dos enfoques:

- Seguir usando el `AddProduct` método que espera valores para la `UnitsOnOrder`, `UnitsInStock`, y `ReorderLevel` campos. En el `Inserting` controlador de eventos, proporcione codificado de forma rígida, que se usará para estas entradas que se han quitado de la interfaz de insertar los valores predeterminados.
- Crear una nueva sobrecarga de la `AddProduct` método en el `ProductsBLL` clase que no acepta entradas para el `UnitsOnOrder`, `UnitsInStock`, y `ReorderLevel` campos. A continuación, en la página ASP.NET, configure el ObjectDataSource para utilizar esta sobrecarga nuevo.

Cualquiera de las opciones funcionará como igualmente. En más allá de los tutoriales se utiliza la última opción, crear varias sobrecargas para el `ProductsBLL` clase s. `UpdateProduct` método.

## <a name="summary"></a>Resumen

GridView no tiene las capacidades integradas de inserción que se encuentra en el DetailsView y FormView, pero con un poco de trabajo se puede agregar una interfaz de inserción a la fila de pie de página. Para mostrar la fila de pie de página en un control GridView simplemente establece su `ShowFooter` propiedad `true`. El contenido de la fila de pie de página se puede personalizar para cada campo convertir el campo en TemplateField y agregar la inserción de la interfaz para el `FooterTemplate`. Como hemos visto en este tutorial, el `FooterTemplate` puede contener botones, cuadros de texto, listas desplegables, casillas de verificación, los controles de origen de datos para rellenar los controles de Web controlada por datos (por ejemplo, listas desplegables) y los controles de validación. Junto con los controles para recopilar datos proporcionados por el usuario s, se necesita un botón Agregar, LinkButton o ImageButton.

Cuando se presiona el botón Agregar, la s ObjectDataSource `Insert()` se invoca el método para iniciar el flujo de trabajo de inserción. ObjectDataSource, a continuación, llamará al método de inserción configurado (el `ProductsBLL` clase s. `AddProduct` método, en este tutorial). Debemos copiaremos los valores de la s de GridView insertar interfaz para las operaciones de asignación ObjectDataSource `InsertParameters` colección antes de que se invoca el método de inserción. Esto puede realizarse mediante programación que hacen referencia a los controles Web de interfaz insertar en las operaciones de asignación ObjectDataSource `Inserting` controlador de eventos.

Este tutorial completa nuestro aspecto en las técnicas para mejorar la apariencia de s GridView. El siguiente conjunto de tutoriales examinará los datos de controles Web y de cómo trabajar con datos binarios como imágenes, archivos PDF, documentos de Word y así sucesivamente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Bernadette Leigh. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-checkboxes-cs.md)
> [Siguiente](adding-a-gridview-column-of-radio-buttons-vb.md)
