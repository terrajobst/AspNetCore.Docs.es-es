---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Agregar y responder a los botones en un control GridView (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, buscaremos en cómo agregar botones personalizados tanto a los campos de un control GridView o DetailsView a una plantilla. En concreto, vamos a GE...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 92743d252e6d8895ac1bff104456ec5874202ec9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804751"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Agregar y responder a los botones en un control GridView (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) o [descargar PDF](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> En este tutorial, buscaremos en cómo agregar botones personalizados tanto a los campos de un control GridView o DetailsView a una plantilla. En concreto, vamos a crear una interfaz que tiene un FormView que permite al usuario desplazarse por los proveedores.


## <a name="introduction"></a>Introducción

Aunque muchos escenarios de informes implican el acceso de solo lectura a los datos del informe, no es raro que los informes incluyan la capacidad para realizar acciones en función de los datos mostrados. Normalmente esto implica la adición de un control Button, LinkButton o ImageButton Web con cada registro que se muestra en el informe que, al hacer clic, produce un postback e invoca el código del lado servidor. Edición y eliminación de los datos de registro por registro son el ejemplo más común. De hecho, como hemos visto a partir de la [información general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, edición y eliminación es tan común que los controles GridView, DetailsView y FormView pueden admitir dicha funcionalidad sin el se necesita para escribir una sola línea de código.

Además para editar y eliminar botones, el control GridView, DetailsView y FormView controles pueden incluir también los botones, LinkButtons o ImageButtons que, al hacer clic, realizar alguna lógica personalizada de servidor. En este tutorial, buscaremos en cómo agregar botones personalizados tanto a los campos de un control GridView o DetailsView a una plantilla. En concreto, vamos a crear una interfaz que tiene un FormView que permite al usuario desplazarse por los proveedores. Para un proveedor determinado y FormView mostrará información sobre el proveedor junto con un control de botón Web que, si hace clic en, marcará todos sus productos asociados como discontinuo. Además, un control GridView enumera los productos proporcionados por el proveedor seleccionado, con cada fila que contenga aumentar de precio y botones de precio de descuento que, si hace clic en, aumentar o reducir el producto `UnitPrice` en un 10% (consulte la figura 1).


[![FormView y GridView contienen botones que realizan acciones personalizadas](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Figura 1**: ambos FormView y GridView contienen botones que realice acciones personalizadas ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Paso 1: Agregar las páginas Web Tutorial de botón

Antes de adentrarnos en cómo agregar un botones personalizados, en primer lugar dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial. Empiece por agregar una nueva carpeta denominada `CustomButtons`. A continuación, agregue las siguientes dos páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `CustomButtons.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con los botones personalizados](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Figura 2**: agregar las páginas ASP.NET para los tutoriales relacionados con los botones personalizados


Al igual que en las demás carpetas `Default.aspx` en el `CustomButtons` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la vista de diseño de la página.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Figura 3**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de la paginación y ordenación `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para la edición, inserción y eliminación de tutoriales.


![El mapa del sitio incluye ahora la entrada para el Tutorial de botones personalizados](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Figura 4**: el mapa del sitio incluye ahora la entrada para el Tutorial de botones personalizados


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Paso 2: Agregar un FormView que enumera los proveedores

Vamos a empezar a trabajar con este tutorial agregando FormView que enumera los proveedores. Como se describe en la introducción, este FormView permitirá al usuario a la página a través de los proveedores, que muestra los productos proporcionados por el proveedor en un control GridView. Además, este FormView incluirá un botón que, al hacer clic, marcará todos los productos del proveedor como discontinuo. Antes de que se refieren a nosotros mismos con la adición del botón personalizado a FormView, primero creemos FormView para que se muestre la información del proveedor.

Comience abriendo la `CustomButtons.aspx` página en el `CustomButtons` carpeta. Agregar un FormView a la página arrastrándolo desde el cuadro de herramientas hasta el diseñador y el conjunto de sus `ID` propiedad `Suppliers`. Desde las etiquetas inteligentes de FormView, optar por crear un nuevo origen ObjectDataSource denominado `SuppliersDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Figura 5**: crear un nuevo origen ObjectDataSource denominado `SuppliersDataSource` ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Configurar este nuevo origen ObjectDataSource que realiza una consulta desde el `SuppliersBLL` la clase `GetSuppliers()` (método) (consulte la figura 6). Puesto que este FormView no proporciona una interfaz para actualizar la información del proveedor, seleccione el (ninguno) opción de la lista desplegable en la pestaña de la actualización.


[![Configurar el origen de datos para utilizar la clase SuppliersBLL s GetSuppliers() (método)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Figura 6**: configurar el origen de datos para utilizar el `SuppliersBLL` la clase `GetSuppliers()` método ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


Después de configurar el origen ObjectDataSource, Visual Studio generará un `InsertItemTemplate`, `EditItemTemplate`, y `ItemTemplate` de FormView. Quitar el `InsertItemTemplate` y `EditItemTemplate` y modificar el `ItemTemplate` para que muestre solo del proveedor empresa nombre y número de teléfono. Finalmente, activar la compatibilidad con la paginación para FormView activando la casilla Habilitar paginación en la etiqueta inteligente de (o estableciendo su `AllowPaging` propiedad `True`). Después de estos cambios marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Figura 7 muestra la página CustomButtons.aspx cuando se ve mediante un explorador.


[![FormView enumera los campos de teléfono del proveedor seleccionado actualmente y CompanyName](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Figura 7**: las listas de FormView el `CompanyName` y `Phone` campos desde el proveedor seleccionado actualmente ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Paso 3: Agregar un control GridView que se enumera los productos del proveedor seleccionado

Antes de agregar el botón de productos interrumpir todo a plantillas de FormView, vamos a agregar primero una clase GridView debajo de FormView que enumera los productos proporcionados por el proveedor seleccionado. Para lograr esto, agregue un control GridView a la página, establezca su `ID` propiedad `SuppliersProducts`, y agregue un nuevo origen ObjectDataSource denominado `SuppliersProductsDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Figura 8**: crear un nuevo origen ObjectDataSource denominado `SuppliersProductsDataSource` ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Configurar este origen ObjectDataSource para usar la clase ProductsBLL `GetProductsBySupplierID(supplierID)` (método) (consulte la figura 9). Mientras esta GridView permitirá por precio de un producto que se ajusten, no usará la integrada, editar o eliminar características desde el control GridView. Por lo tanto, podemos establecer la lista desplegable en (None) de ObjectDataSource UPDATE, INSERT y DELETE pestañas.


[![Configurar el origen de datos para utilizar la clase ProductsBLL s GetProductsBySupplierID(supplierID) (método)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Figura 9**: configurar el origen de datos para utilizar el `ProductsBLL` la clase `GetProductsBySupplierID(supplierID)` método ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Puesto que el `GetProductsBySupplierID(supplierID)` método acepta un parámetro de entrada, el ObjectDataSource wizard nos pide el origen de este valor de parámetro. Para pasar el `SupplierID` valor de FormView, establezca la lista desplegable de origen de parámetro para el Control y la lista desplegable de ControlID a `Suppliers` (el Id. de FormView creado en el paso 2).


[![Indicar que la columna supplierID parámetro debe provenir del FormView Control de proveedores](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Figura 10**: indicar que el *`supplierID`* parámetro debe provenir de la `Suppliers` FormView Control ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


Después de completar al Asistente de ObjectDataSource en GridView se contendrá un BoundField o CampoCasillaVerificación para cada uno de los campos de datos del producto. Vamos a reducir esto para mostrar solamente el `ProductName` y `UnitPrice` BoundFields junto con el `Discontinued` CampoCasillaVerificación; Además, vamos a dar formato el `UnitPrice` BoundField tal que el texto tiene formato de moneda. El control GridView y `SuppliersProductsDataSource` marcado declarativo de ObjectDataSource debe ser similar al código de marcado siguiente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

En este momento en este tutorial muestra un informe de detalles y maestras, lo que permite al usuario a elegir un proveedor de FormView en la parte superior y ver los productos proporcionados por ese proveedor a través del control GridView en la parte inferior. Figura 11 muestra una captura de pantalla de esta página al seleccionar el proveedor Comercial Tasmania de FormView.


[![Los productos de s del proveedor seleccionado se muestran en el control GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Figura 11**: productos del proveedor seleccionado The se muestran en el control GridView ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Paso 4: Crear métodos BLL y DAL para suspender todos los productos para un proveedor

Antes de que podemos agregar un botón a FormView que, al hacer clic, interrumpe todos los productos del proveedor, primero es necesario agregar un método a DAL y BLL que lleva a cabo esta acción. En concreto, este método se denominará `DiscontinueAllProductsForSupplier(supplierID)`. Cuando se hace clic en el botón de FormView, deberá invocar a este método en la capa de lógica empresarial, pasando en el proveedor seleccionado `SupplierID`; BLL, a continuación, llamará al método de capa de acceso a datos correspondiente, que emitirá un `UPDATE` instrucción la base de datos interrumpe de productos del proveedor especificado.

Como se ha hecho en los tutoriales anteriores, vamos a usar un enfoque de abajo a arriba, a partir de crear el método DAL, a continuación, el método BLL y, por último, implementar la funcionalidad en la página ASP.NET. Abra el `Northwind.xsd` DataSet con tipo en el `App_Code/DAL` carpeta y agregue un nuevo método a la `ProductsTableAdapter` (haga doble clic en el `ProductsTableAdapter` y elija Agregar consulta). Si lo hace, se abrirá el Asistente de configuración de la consulta de TableAdapter, que nos guía a través del proceso de agregar el nuevo método. Para empezar, que indica que el método DAL usará una instrucción de SQL ad hoc.


[![Crear el método de DAL mediante una instrucción de SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Figura 12**: crear el método DAL mediante una instrucción de SQL Ad Hoc ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


A continuación, el asistente pide nos sobre qué tipo de consulta para crear. Puesto que la `DiscontinueAllProductsForSupplier(supplierID)` método deberá actualizar el `Products` tabla de base de datos, establecer el `Discontinued` en 1 para todos los productos proporcionados por el campo *`supplierID`*, necesitamos crear una consulta que actualiza los datos.


[![Elija el tipo de consulta de actualización](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Figura 13**: elija el tipo de consulta de actualización ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


La siguiente pantalla del asistente proporciona el TableAdapter existente del `UPDATE` instrucción, que se actualiza cada uno de los campos definidos en el `Products` DataTable. Reemplace el texto de consulta con la siguiente instrucción:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Después de escribir esta consulta y haga clic en siguiente, se solicita la última pantalla del Asistente para utilizar el nombre del nuevo método `DiscontinueAllProductsForSupplier`. Complete el asistente, haga clic en el botón Finalizar. Al volver al diseñador de DataSet debería ver un nuevo método en el `ProductsTableAdapter` denominado `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nombre de la nueva DiscontinueAllProductsForSupplier DAL (método)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Figura 14**: nombre del nuevo método DAL `DiscontinueAllProductsForSupplier` ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


Con el `DiscontinueAllProductsForSupplier(supplierID)` método creado en la capa de acceso a datos, la siguiente tarea consiste en crear el `DiscontinueAllProductsForSupplier(supplierID)` método en la capa de lógica empresarial. Para ello, abra el `ProductsBLL` archivo de clase y agregue lo siguiente:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Este método simplemente llama a la `DiscontinueAllProductsForSupplier(supplierID)` método en la capa DAL, pasando a través de proporcionado *`supplierID`* el valor del parámetro. Si se produjeron las reglas de negocios que solo permiten los productos del proveedor se interrumpa en determinadas circunstancias, esas reglas deben implementarse en este caso, en la capa BLL.

> [!NOTE]
> A diferencia de la `UpdateProduct` sobrecargas en la `ProductsBLL` (clase), el `DiscontinueAllProductsForSupplier(supplierID)` firma del método no incluye el `DataObjectMethodAttribute` atributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Esto impide la `DiscontinueAllProductsForSupplier(supplierID)` método de la lista desplegable del Asistente de ObjectDataSource Configurar origen de datos en la pestaña de la actualización. Se ve omite este atributo porque se llamará a la `DiscontinueAllProductsForSupplier(supplierID)` método directamente desde un controlador de eventos en nuestra página ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Paso 5: Agregar un interrumpir el botón de todos los productos a FormView

Con el `DiscontinueAllProductsForSupplier(supplierID)` método en el BLL y DAL completa, el último paso para agregar la capacidad de suspender todos los productos para el proveedor seleccionado es agregar un control de botón Web a la FormView `ItemTemplate`. Vamos a agregar un botón a continuación del número de teléfono del proveedor con el texto del botón, interrumpir todos los productos y una `ID` el valor de propiedad `DiscontinueAllProductsForSupplier`. Puede agregar este control de botón Web a través del diseñador, haga clic en el vínculo Editar plantillas en la etiqueta inteligente de FormView (consulte la figura 15), o directamente a través de la sintaxis declarativa.


[![Agregar un interrumpir todos los productos botón Web Control FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Figura 15**: agregar un interrumpir todos los productos Web Control de botón a la FormView `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


Cuando se hace clic en el botón una visita del usuario que habrá trastornos la página, una devolución de datos y de FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) se activa. Para ejecutar código personalizado en respuesta a este botón se hizo clic, podemos crear un controlador de eventos para este evento. Comprender, sin embargo, que el `ItemCommand` cada vez que se activa el evento *cualquier* se hace clic en control de botón, LinkButton o ImageButton Web dentro de FormView. Esto significa que cuando el usuario se mueve de una página a otra en FormView, el `ItemCommand` desencadena el evento; de lo mismo cuando el usuario hace clic en nuevo, editar o eliminar en un FormView que admite la inserción, actualización o eliminación.

Puesto que el `ItemCommand` se activa independientemente hace clic en qué botón, en caso de controlador, necesitamos una manera para determinar si se hizo clic el botón de productos interrumpir todo o si fuese un botón otro. Para lograr esto, podemos establecer el control de botón Web `CommandName` propiedad a algún valor de identificación. Cuando se presiona el botón, esto `CommandName` valor se pasa a la `ItemCommand` controlador de eventos, lo que nos permite determinar si el botón de productos interrumpir todo fue el botón ha hecho clic. Establecer interrumpir todos los productos del botón `CommandName` propiedad DiscontinueProducts.

Por último, vamos a usar un cuadro de diálogo de confirmación del lado cliente para asegurarse de que el usuario realmente desea dejar de productos del proveedor seleccionado. Como hemos visto en el [adición del lado cliente confirmación cuando se elimina](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) tutorial, esto puede realizarse con un poco de JavaScript. Establézcalo en particular, propiedad OnClientClick del control de botón Web `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Después de realizar estos cambios, la sintaxis declarativa de FormView debe ser similar al siguiente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

A continuación, cree un controlador de eventos para el FormView `ItemCommand` eventos. En este controlador de eventos, necesitamos determinar primero si se hizo clic el botón de productos interrumpir todo. Si por lo tanto, deseamos crear una instancia de la `ProductsBLL` clase e invocar sus `DiscontinueAllProductsForSupplier(supplierID)` método, pasando el `SupplierID` de FormView seleccionado:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Tenga en cuenta que el `SupplierID` del proveedor seleccionado en FormView actual se puede acceder mediante el FormView [ `SelectedValue` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). El `SelectedValue` propiedad devuelve el valor del registro que se muestran en FormView de clave de los primeros datos. El FormView [ `DataKeyNames` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), lo que indica que los datos se extraen los campos desde el que los datos de los valores de clave, se establece automáticamente en `SupplierID` por Visual Studio cuando se enlaza el origen ObjectDataSource a FormView en el paso 2.

Con el `ItemCommand` controlador de eventos creado, dedique un momento para probar la página. Vaya a la Quesos de Cooperativa supplier 'Las Cabras' (es el proveedor quinto en FormView para mí). Este proveedor ofrece dos productos, Queso Cabrales y Queso Manchego La Pastora, ambos de los cuales son *no* discontinuo.

Imagine que Cooperativa de Quesos 'Las Cabras' ha salido del negocio y, por tanto, sus productos se interrumpa. Haga clic en el botón de todos los productos de prestar. Esto mostrará el cuadro de diálogo de confirmación del lado cliente cuadro (consulte la figura 16).


[![Cooperativa de Quesos Las Cabras ofrece dos productos activos](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras ofrece dos productos activos ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Si hace clic en Aceptar en el cuadro de diálogo de confirmación del lado cliente, el envío del formulario continuará, lo que provoca una devolución de datos en el que el FormView `ItemCommand` evento se activará. El controlador de eventos se crea, a continuación, se ejecutará, invocar el `DiscontinueAllProductsForSupplier(supplierID)` método y dejar el Queso Cabrales y Queso Manchego La Pastora productos.

Si ha deshabilitado el estado de vista de GridView, el control GridView es que se va a enlazar en el almacén de datos subyacente en cada devolución de datos y, por tanto, inmediatamente se actualizará para reflejar que estos dos productos ahora están suspendidos (consulte la figura 17). Si, sin embargo, no ha deshabilitado el estado de vista en el control GridView, deberá enlazar manualmente los datos en GridView después de realizar este cambio. Para ello, basta con realizar una llamada a la GridView `DataBind()` método inmediatamente después de invocar el `DiscontinueAllProductsForSupplier(supplierID)` método.


[![Después el botón interrumpir todos los productos, los productos de proveedor s son actualiza en consecuencia](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Figura 17**: después el botón interrumpir todos los productos, los productos del proveedor son actualiza en consecuencia ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Paso 6: Crear una sobrecarga UpdateProduct por separado en la capa de lógica empresarial para ajustar el precio de un producto

Al igual que con interrumpir todos los productos en el botón FormView, con el fin de agregar los botones para aumentar y disminuir el precio de un producto en el control GridView se deberá agregar los métodos adecuados de la capa de acceso a datos y capa de lógica empresarial. Puesto que ya tenemos un método que actualiza una fila única de producto en la capa DAL, podemos proporcionar esta funcionalidad mediante la creación de una nueva sobrecarga para el `UpdateProduct` método en la capa BLL.

Nuestro pasado `UpdateProduct` sobrecargas realizadas en una combinación de campos de producto como valores escalares de entrada y, a continuación, se han actualizado los campos para el producto especificado. Para esta sobrecarga a variar ligeramente de este estándar y pasar en su lugar en el producto `ProductID` y el porcentaje en que se va a ajustar el `UnitPrice` (en lugar de pasar de nuevo, el ajusta `UnitPrice` propio). Este enfoque simplificará el código que necesita para escribir en la clase de código subyacente de página ASP.NET, ya que no es necesario preocuparse por determinar el producto actual `UnitPrice`.

El `UpdateProduct` sobrecarga para este tutorial se muestra a continuación:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Esta sobrecarga recupera información sobre el producto especificado a través de la capa DAL `GetProductByProductID(productID)` método. A continuación, se comprueba si el producto `UnitPrice` se asigna a una base de datos `NULL` valor. Si es así, el precio se deja sin modificar. Si no, sin embargo, hay que no es`NULL` `UnitPrice` valor, el método actualiza el producto `UnitPrice` por el porcentaje especificado (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Paso 7: Agregar los botones de disminución y un aumento en el control GridView

El control GridView (y DetailsView) son ambos formado por una colección de campos. Además de BoundFields, CheckBoxFields y TemplateFields, ASP.NET incluye el ButtonField, que, como su nombre implica, se representa como una columna con un botón, LinkButton o ImageButton para cada fila. Similar a FormView, al hacer clic en *cualquier* botón dentro de la GridView botones de paginación, editar o eliminar botones, botones de ordenación y así sucesivamente provoca una devolución de datos y genera la GridView [ `RowCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

El ButtonField tiene un `CommandName` propiedad que se asigna el valor especificado a cada uno de sus botones `CommandName` propiedades. Al igual que con FormView, el `CommandName` valor lo utilizan los `RowCommand` controlador de eventos para determinar qué botón se hizo clic.

Vamos a agregar dos ButtonFields nuevo en el control GridView, uno con un texto del botón precio + 10% y el otro con el texto de precios -10%. Para agregar estos ButtonFields, haga clic en el vínculo Editar columnas de etiqueta inteligente de GridView, seleccione el tipo de campo ButtonField en la lista en la parte superior izquierda y haga clic en el botón Agregar.


![Agregue dos ButtonFields en GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Figura 18**: agregar dos ButtonFields en GridView


Mueva el dos ButtonFields para que aparezcan como los dos primeros campos de GridView. A continuación, establezca el `Text` las propiedades de estos dos ButtonFields a + 10% de precio y precio -10% y el `CommandName` propiedades IncreasePrice y DecreasePrice, respectivamente. De forma predeterminada, un ButtonField representa su columna de botones como LinkButtons. Esto se puede cambiar, sin embargo, mediante el ButtonField [ `ButtonType` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Vamos a tener estos dos ButtonFields representan como botones normales de inserción; Por consiguiente, establecer el `ButtonType` propiedad `Button`. Figura 19 muestra los campos de cuadro de diálogo una vez realizados estos cambios; Después de que es el marcado declarativo de GridView.


![Configurar las propiedades de ButtonType, CommandName y ButtonFields texto](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Figura 19**: configurar el ButtonFields `Text`, `CommandName`, y `ButtonType` propiedades


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Con estos ButtonFields creados, el último paso es crear un controlador de eventos para la GridView `RowCommand` eventos. Este controlador de eventos, si se desencadenó porque el precio + 10 botones de precios -10% o % se hizo clic, debe determinar la `ProductID` para la fila cuyo botón se hizo clic en y, a continuación, invocar el `ProductsBLL` la clase `UpdateProduct` método, pasando el adecuado `UnitPrice` ajuste porcentaje junto con la `ProductID`. El código siguiente realiza estas tareas:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Con el fin de determinar la `ProductID` para la fila cuyo precio + 10 se hizo clic en el botón de precios -10% o %, necesitamos consultar la GridView `DataKeys` colección. Esta colección contiene los valores de los campos especificados en el `DataKeyNames` propiedad para cada fila de GridView. Desde la GridView `DataKeyNames` propiedad se estableció en ProductID por Visual Studio al enlazar el origen ObjectDataSource en GridView, `DataKeys(rowIndex).Value` proporciona el `ProductID` especificado *rowIndex*.

El ButtonField pasa automáticamente en el *rowIndex* de la fila cuyo botón se hizo clic a través de la `e.CommandArgument` parámetro. Por lo tanto, para determinar el `ProductID` para la fila cuyo precio + 10 se hizo clic en el botón de precios -10% o %, usamos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Como con el botón interrumpir todos los productos, si ha deshabilitado el estado de vista de GridView, el control GridView se que se vuelve a enlazar al almacén de datos subyacente en cada devolución de datos y, por tanto, inmediatamente se actualizará para reflejar un cambio de precio que tiene lugar de hacer clic en cualquiera de los botones. Si, sin embargo, no ha deshabilitado el estado de vista en el control GridView, deberá enlazar manualmente los datos en GridView después de realizar este cambio. Para ello, basta con realizar una llamada a la GridView `DataBind()` método inmediatamente después de invocar el `UpdateProduct` método.

Figura 20 se muestra la página al ver los productos proporcionados por el Homestead de su abuela Kelly. Figura 21 muestra los resultados después el precio + 10% botón se ha hecho clic dos veces en el botón de precios -10% y Boysenberry Spread de su abuela una vez para arándanos Northwoods.


[![El control GridView incluye precio + 10% y botones de precios -10%](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Figura 20**: el precio de GridView incluye + 10% y botones de precios -10% ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Se han actualizado los precios para el producto primero y tercero mediante el precio + 10% y botones de precios -10%](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Figura 21**: los precios para el primer y tercer producto se han actualizado a través del precio + 10% y botones de precios -10% ([haga clic aquí para ver imagen en tamaño completo](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> El control GridView (y DetailsView) también pueden tener botones, LinkButtons o ImageButtons agregado a su TemplateFields. Como con el BoundField, estos botones, al hacer clic, inducen una devolución de datos, provocar la GridView `RowCommand` eventos. Al agregar los botones en TemplateField, sin embargo, el botón `CommandArgument` no se establece automáticamente en el índice de la fila ya que es cuando se usa ButtonFields. Si necesita determinar el índice de fila del botón que se hizo clic en el `RowCommand` controlador de eventos, deberá establecer manualmente el botón `CommandArgument` propiedad en su sintaxis declarativa en TemplateField, mediante código similar al siguiente:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.


## <a name="summary"></a>Resumen

Pueden incluir los controles GridView, DetailsView y FormView todos los botones, LinkButtons o ImageButtons. Estos botones, al hacer clic, hacer que una devolución de datos y generar el `ItemCommand` eventos en los controles FormView y DetailsView y `RowCommand` eventos en el control GridView. Estos controles Web de datos tienen funcionalidad integrada para controlar las acciones relacionadas con comandos comunes, como eliminar o modificar registros. Sin embargo, se puede usar los botones que, al hacer clic en, responda con la ejecución de nuestro propio código personalizado.

Para lograr esto, debemos crear un controlador de eventos para el `ItemCommand` o `RowCommand` eventos. En este controlador de eventos entrante primero comprobamos `CommandName` valor para determinar qué botón se hizo clic y, a continuación, tomar las medidas adecuadas de personalizado. En este tutorial, vimos cómo usar los botones y ButtonFields interrumpir todos los productos para un proveedor especificado o para aumentar o disminuir el precio de un producto determinado en un 10%.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Siguiente](adding-and-responding-to-buttons-to-a-gridview-vb.md)
