---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Agregar y responder a los botones a un control GridView (VB) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial se examinará cómo agregar botones personalizados, a una plantilla y a los campos de un control GridView o DetailsView. En concreto, te enviaremos un mensaje GE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 58b570c897810eeaa182a201616a182c02e9d92c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878105"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Agregar y responder a los botones a un control GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) o [descarga de PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> En este tutorial se examinará cómo agregar botones personalizados, a una plantilla y a los campos de un control GridView o DetailsView. En concreto, vamos a crear una interfaz que tiene un FormView que permite al usuario desplazarse a través de los proveedores.


## <a name="introduction"></a>Introducción

Mientras muchos escenarios de informes implican el acceso de solo lectura a los datos del informe, s no infrecuente que los informes incluyan la capacidad para realizar acciones según los datos mostrados. Normalmente esto implica la adición de un control Button, LinkButton o ImageButton Web con cada registro que se muestra en el informe que, al hacer clic, provoca una devolución de datos y algún código de servidor, se invoca. Editar y eliminar los datos en una base de registro por registro son el ejemplo más común. De hecho, como hemos visto a partir de la [información general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) es muy común que los controles GridView, DetailsView y FormView pueden admitir esta funcionalidad sin tutorial, edición y eliminación de la necesario para escribir una sola línea de código.

Además para modificar y eliminar botones, el control GridView, DetailsView y FormView controles pueden incluir también botones, LinkButton o ImageButtons que, al hacer clic, realizar alguna lógica personalizada de servidor. En este tutorial se examinará cómo agregar botones personalizados, a una plantilla y a los campos de un control GridView o DetailsView. En concreto, vamos a crear una interfaz que tiene un FormView que permite al usuario desplazarse a través de los proveedores. Para un proveedor determinado, FormView mostrará información acerca del proveedor junto con un control de botón Web que, si hace clic en, marcará todos sus productos asociados como discontinuo. Además, un control GridView muestra los productos proporcionados por el proveedor seleccionado, con cada fila que contenga aumentar precio y botones de precio de descuento que, si hace clic en, aumentar o reducir el producto s `UnitPrice` en un 10% (consulte la figura 1).


[![FormView y GridView contienen botones que realizan acciones personalizadas](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: ambos FormView y GridView contienen botones que realizan acciones personalizadas ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Paso 1: Agregar las páginas Web Tutorial de botón

Antes de adentrarnos en cómo agregar un botones personalizados, permiten s primero Tómese un momento para crear las páginas ASP.NET en el proyecto de sitio Web que se necesitará para este tutorial. Empiece por agregar una nueva carpeta denominada `CustomButtons`. A continuación, agregue las siguientes dos páginas ASP.NET a la carpeta y asegúrese de que asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `CustomButtons.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con botones personalizados](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: agregar las páginas ASP.NET para los tutoriales relacionados con botones personalizados


Al igual que en las otras carpetas, `Default.aspx` en el `CustomButtons` carpeta enumerará los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página s la vista Diseño.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de la paginación y la ordenación `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para modificar, insertar y eliminar los tutoriales.


![El mapa del sitio incluye ahora la entrada para el Tutorial de botones personalizados](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: la asignación de sitio incluye ahora la entrada para el Tutorial de botones personalizados


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Paso 2: Agregar un FormView que enumera los proveedores

Permiten s empezar a trabajar con este tutorial agregando FormView que enumera los proveedores. Según lo descrito en la introducción, esta FormView permitirá al usuario para paginar a través de los proveedores, que muestra los productos suministrados por el proveedor en un control GridView. Además, este FormView incluirá un botón que, al hacer clic, marcará todos los productos de proveedor s dentro de esta categoría. Antes de que se hacen referencia a nosotros mismos con la adición del botón personalizado a FormView, permiten s primero basta con crear FormView para que se muestre la información del proveedor.

Comience abriendo la `CustomButtons.aspx` página en el `CustomButtons` carpeta. Agregar un FormView a la página arrastrándolo desde el cuadro de herramientas en el diseñador y el conjunto de sus `ID` propiedad `Suppliers`. Desde la etiqueta inteligente de FormView s, optar por crear un nuevo origen ObjectDataSource denominado `SuppliersDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figura 5**: crear un nuevo ObjectDataSource denominado `SuppliersDataSource` ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Configure este nuevo ObjectDataSource de tal modo que realiza una consulta desde el `SuppliersBLL` clase s. `GetSuppliers()` (método) (consulte la figura 6). Puesto que este FormView no proporciona una interfaz para actualizar la información del proveedor, seleccione opción el (ninguno) en la lista desplegable en la pestaña de la actualización.


[![Configurar el origen de datos para utilizar la clase SuppliersBLL s GetSuppliers() (método)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configurar el origen de datos para utilizar el `SuppliersBLL` clase s `GetSuppliers()` método ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Después de configurar el ObjectDataSource, Visual Studio generará una `InsertItemTemplate`, `EditItemTemplate`, y `ItemTemplate` de FormView. Quitar el `InsertItemTemplate` y `EditItemTemplate` y modificar el `ItemTemplate` para que muestre solo el proveedor s empresa nombre y número de teléfono. Por último, activar compatibilidad con la paginación para FormView activando la casilla de verificación Habilitar paginación de su etiqueta inteligente (o estableciendo su `AllowPaging` propiedad `True`). Después de estos cambios el marcado declarativo de s de página debe ser similar al siguiente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

La figura 7 muestra la página CustomButtons.aspx cuando se ven a través de un explorador.


[![FormView enumera los campos de teléfono del proveedor seleccionado actualmente y CompanyName](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: las listas de FormView el `CompanyName` y `Phone` campos desde el proveedor seleccionado actualmente ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Paso 3: Agregar un control GridView que muestra los productos de s del proveedor seleccionado

Antes de agregar el botón de productos todo suspender a la plantilla de s FormView, dejamos s Agregar un control GridView debajo FormView que contiene los productos suministrados por el proveedor seleccionado. Para lograr esto, agregue un control GridView a la página, establezca su `ID` propiedad `SuppliersProducts`, y agregar un nuevo origen ObjectDataSource denominado `SuppliersProductsDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: crear un nuevo ObjectDataSource denominado `SuppliersProductsDataSource` ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Configurar este ObjectDataSource para utilizar la clase ProductsBLL s `GetProductsBySupplierID(supplierID)` (método) (consulte la figura 9). Mientras esta GridView permitirá a un precio del producto s se ajusten, ganó t usar integrado en la edición o eliminación de características de GridView. Por lo tanto, podemos establecer la lista desplegable en (ninguno) para las operaciones de asignación ObjectDataSource pestañas UPDATE, INSERT y DELETE.


[![Configurar el origen de datos para utilizar la clase ProductsBLL s GetProductsBySupplierID(supplierID) (método)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configurar el origen de datos para utilizar el `ProductsBLL` clase s `GetProductsBySupplierID(supplierID)` método ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Puesto que la `GetProductsBySupplierID(supplierID)` método acepta un parámetro de entrada, el Asistente ObjectDataSource nos solicita el origen de este valor de parámetro. Para pasar el `SupplierID` valor de FormView, establezca la lista desplegable de origen de parámetro en el Control y la lista desplegable de ControlID para `Suppliers` (el Id. de FormView creado en el paso 2).


[![Indicar que la columna supplierID parámetro debe provenir del control FormView de proveedores](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicar que el *`supplierID`* parámetro debe provenir de la `Suppliers` FormView Control ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Después de completar al Asistente de ObjectDataSource GridView contendrá un BoundField o CampoCasillaVerificación para cada uno de los campos de datos de producto s. Permiten s recortar esto para mostrar simplemente el `ProductName` y `UnitPrice` BoundFields junto con el `Discontinued` CampoCasillaVerificación; Además, permiten el formato s el `UnitPrice` BoundField tal que el texto tiene el formato como una moneda. El control GridView y `SuppliersProductsDataSource` marcado declarativo de ObjectDataSource s debería ser similar al código siguiente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

En este momento nuestro tutorial muestra un informe maestro/detalles, lo que permite al usuario seleccionar un proveedor de FormView en la parte superior y ver los productos suministrados por ese proveedor a través de GridView en la parte inferior. La figura 11 muestra una captura de pantalla de esta página al seleccionar el proveedor Comercial Tasmania de FormView.


[![Los productos de s del proveedor seleccionado se muestran en el control GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: el proveedor seleccionado s productos se muestran en el control GridView ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Paso 4: Crear DAL y BLL métodos para suspender todos los productos para un proveedor

Antes de que podemos agregar un botón a FormView que, al hacer clic, se interrumpe todos los productos de proveedor s, primero tenemos que agregar un método a la capa DAL y BLL que lleva a cabo esta acción. En concreto, este método se denominará `DiscontinueAllProductsForSupplier(supplierID)`. Cuando se hace clic en el botón de FormView s, se podrá invocar este método en la capa de lógica empresarial, pasar en el proveedor seleccionado s `SupplierID`; BLL, a continuación, llamará al método correspondiente capa de acceso a datos, que se van a emitir un `UPDATE` instrucción la base de datos que los productos de proveedor especificado s se interrumpe.

Como hemos trabajado en nuestros tutoriales anteriores, vamos a usar un enfoque de abajo arriba, a partir de crear el método DAL, a continuación, el método BLL y por último, implementar la funcionalidad en la página ASP.NET. Abra la `Northwind.xsd` DataSet con tipo en el `App_Code/DAL` carpeta y agregue un nuevo método para el `ProductsTableAdapter` (haga doble clic en el `ProductsTableAdapter` y elija Agregar consulta). Si lo hace, se abrirá el Asistente para configuración de consultas de TableAdapter, que nos recorre el proceso de agregar el nuevo método. Empiece por que indica que el método de la capa DAL utilizará una instrucción de SQL ad hoc.


[![Cree el método DAL con una instrucción de SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: crear el método de la capa DAL mediante una instrucción de SQL Ad Hoc ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


A continuación, el asistente le preguntará nos en cuanto a qué tipo de consulta para crear. Puesto que la `DiscontinueAllProductsForSupplier(supplierID)` método será necesario actualizar la `Products` tabla de base de datos, establecer el `Discontinued` campo en 1 para todos los productos suministrados por especificado *`supplierID`*, necesitamos crear una consulta que actualiza los datos.


[![Elija el tipo de consulta de actualización](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: elija el tipo de consulta de actualización ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


La siguiente pantalla del asistente proporciona s TableAdapter existente `UPDATE` instrucción, que actualiza cada uno de los campos definidos en el `Products` DataTable. Reemplace el texto de consulta con la siguiente instrucción:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Después de escribir esta consulta y haga clic en siguiente, la última pantalla del asistente pide el nombre del nuevo método s usar `DiscontinueAllProductsForSupplier`. Complete el asistente, haga clic en el botón Finalizar. Al volver al diseñador de DataSet verá un nuevo método en el `ProductsTableAdapter` denominado `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nombre de la nueva DiscontinueAllProductsForSupplier DAL (método)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: nombre del nuevo método de DAL `DiscontinueAllProductsForSupplier` ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Con el `DiscontinueAllProductsForSupplier(supplierID)` método creado en la capa de acceso a datos, la siguiente tarea consiste en crear el `DiscontinueAllProductsForSupplier(supplierID)` método en la capa de lógica de negocios. Para ello, abra el `ProductsBLL` archivo de clase y agregue lo siguiente:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Este método se llama simplemente hacia abajo hasta la `DiscontinueAllProductsForSupplier(supplierID)` método en la capa DAL, pasando a través de proporcionado *`supplierID`* el valor del parámetro. Si se produjeron las reglas de negocio que solo permiten a un proveedor de productos de s se interrumpa en determinadas circunstancias, deben implementar esas reglas en este caso, en la capa BLL.

> [!NOTE]
> A diferencia de la `UpdateProduct` sobrecargas en el `ProductsBLL` (clase), el `DiscontinueAllProductsForSupplier(supplierID)` firma del método no incluye el `DataObjectMethodAttribute` atributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Esto impide la `DiscontinueAllProductsForSupplier(supplierID)` método en la lista de desplegable ObjectDataSource s s del Asistente para configurar origen de datos en la pestaña de la actualización. Se ha omitido este atributo porque se llamará a la `DiscontinueAllProductsForSupplier(supplierID)` (método) directamente desde un controlador de eventos en nuestra página de ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Paso 5: Agregar un botón de todos los productos para FormView de prestar

Con el `DiscontinueAllProductsForSupplier(supplierID)` método BLL y DAL que se ejecuta, el último paso para agregar la capacidad de suspender todos los productos para el proveedor seleccionado es agregar un control de botón Web a las operaciones de asignación FormView `ItemTemplate`. S permiten agregar un botón de este tipo por debajo del número de teléfono de proveedor s con el texto del botón, interrumpir todos los productos y una `ID` el valor de propiedad `DiscontinueAllProductsForSupplier`. Puede agregar este control de botón Web a través del diseñador haciendo clic en el vínculo Editar plantillas en la etiqueta inteligente de FormView s (consulte la figura 15), o directamente a través de la sintaxis declarativa.


[![Agregar un interrumpir todos los productos botón Web Control FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: agregar un interrumpir todos los productos Web Control de botón a las operaciones de asignación FormView `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Cuando se hace clic en el botón una visita de usuario que tiene lugar la página, una devolución de datos y las operaciones de asignación FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) se activa. Para ejecutar código personalizado en respuesta a este botón se hizo clic, podemos crear un controlador de eventos para este evento. Comprender, sin embargo, que la `ItemCommand` cada vez que se activa el evento *cualquier* control Button, LinkButton o ImageButton Web se hace clic en FormView. Esto significa que cuando el usuario se mueve de una página a otra en FormView, la `ItemCommand` desencadena el evento; de lo mismo cuando el usuario hace clic en nuevo, editar o eliminar en un FormView que admite la inserción, actualización o eliminación.

Puesto que el `ItemCommand` se activa sin tener en cuenta se hace clic en el botón, en caso de controlador necesitamos una manera de determinar si se ha hecho clic en el botón de productos todo suspender o si es algún otro botón. Para ello, podemos establecer el control de botón Web s `CommandName` propiedad con un valor de identificación. Cuando se presiona el botón, esto `CommandName` valor se pasa a la `ItemCommand` controlador de eventos, lo que nos permite determinar si el botón de productos todo suspender fue el botón ha hecho clic. Establecer la s interrumpir todos los productos botón `CommandName` propiedad DiscontinueProducts.

Por último, permiten s usar un cuadro de diálogo de confirmación del lado cliente para asegurarse de que el usuario realmente quiere dejar de los productos del proveedor seleccionado s. Como vimos en el [Agregar cliente confirmación cuando se elimina](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) tutorial, esto puede realizarse con una pequeña parte de JavaScript. En particular, establezca la propiedad de OnClientClick de s de control de botón Web en `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Después de realizar estos cambios, la sintaxis declarativa de FormView s debería ser similar al siguiente:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

A continuación, cree un controlador de eventos para el s FormView `ItemCommand` eventos. En este controlador de eventos, es necesario determinar primero si se ha hecho clic en el botón de productos todo suspender. Si por lo tanto, desea crear una instancia de la `ProductsBLL` clase e invocar su `DiscontinueAllProductsForSupplier(supplierID)` método, pasando el `SupplierID` de FormView seleccionado:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Tenga en cuenta que la `SupplierID` del proveedor seleccionado actual en FormView puede tener acceso mediante las operaciones de asignación FormView [ `SelectedValue` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). El `SelectedValue` propiedad devuelve el valor del registro que se muestra en el FormView de clave de los primeros datos. Las operaciones de asignación FormView [ `DataKeyNames` propiedad](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), lo que indica que los datos se extraen los campos desde el que los datos de los valores de clave, se establece automáticamente en `SupplierID` por Visual Studio cuando se enlaza el ObjectDataSource a la parte posterior de FormView en el paso 2.

Con el `ItemCommand` controlador de eventos creado, tómese un momento para probar la página. Vaya a la Quesos de Cooperativa 'Venta Cabras' proveedor (se s del proveedor quinto en FormView para mí). Este proveedor proporciona dos productos, Queso Cabrales y Queso Manchego La Pastora, ambos de los cuales son *no* suspendido.

Imagine que Alemania Cooperativa Quesos 'Venta Cabras' ha salido del negocio y, por tanto, son los productos se interrumpa. Haga clic en el botón de todos los productos de prestar. Aparecerá el cuadro de diálogo de confirmación del lado cliente cuadro (Véase la figura 16).


[![Cooperativa de Quesos venta Cabras proporciona dos productos activos](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: Cooperativa de Quesos venta Cabras proporciona dos productos activos ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Si hace clic en Aceptar en el cuadro de diálogo de confirmación de cliente, el envío del formulario continuará, provocando una devolución de datos en el que las operaciones de asignación FormView `ItemCommand` desencadenará el evento. El controlador de eventos se crea, a continuación, se ejecutará, invocar el `DiscontinueAllProductsForSupplier(supplierID)` método y dejar el Queso Cabrales y Queso Manchego La Pastora productos.

Si ha deshabilitado el estado de vista de GridView s, GridView es que se va a enlazar al almacén de datos subyacente en cada devolución de datos y, por tanto, inmediatamente se actualizará para reflejar que estos dos productos ahora se suspenden (Véase la figura 17). Si, sin embargo, no ha deshabilitado el estado de vista en el control GridView, debe enlazar manualmente los datos en GridView después de realizar este cambio. Para ello, basta con realizar una llamada a las operaciones de asignación GridView `DataBind()` método inmediatamente después de invocar el `DiscontinueAllProductsForSupplier(supplierID)` método.


[![Después el botón interrumpir todos los productos, los productos de proveedor s son actualiza en consecuencia](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: después el botón interrumpir todos los productos, los productos de proveedor s son actualiza en consecuencia ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Paso 6: Crear una sobrecarga UpdateProduct en la capa de lógica de negocios para ajustar un precio del producto s

Al igual que con interrumpir todos los productos en el botón FormView, con el fin de agregar los botones para aumentar y disminuir el precio de un producto de GridView necesitamos agregar primero los métodos adecuados de la capa de acceso a datos y capa de lógica empresarial. Puesto que ya tenemos un método que actualiza una fila única de producto en la capa DAL, podemos proporcionar esta funcionalidad mediante la creación de una nueva sobrecarga para el `UpdateProduct` método en la capa BLL.

Nuestro pasado `UpdateProduct` sobrecargas realizadas en una combinación de campos de producto como valores escalares de entrada y, a continuación, se ha actualizado el solo aquellos campos para el producto especificado. Esta sobrecarga se pasa en su lugar en el producto s y varían ligeramente respecto a este estándar `ProductID` y el porcentaje por el que se va a ajustar la `UnitPrice` (en lugar de pasar el nuevo, ajustar `UnitPrice` propio). Este enfoque simplificará el código que se necesita para escribir en la clase de código subyacente de la página ASP.NET, ya que no queremos t tiene que preocuparse por determinar el producto actual s `UnitPrice`.

El `UpdateProduct` sobrecarga para este tutorial se muestra a continuación:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Esta sobrecarga recupera información sobre el producto especificado a través de la capa DAL `GetProductByProductID(productID)` método. A continuación, se comprueba si el producto s `UnitPrice` se asigna a una base de datos `NULL` valor. Si es así, el precio se deja sin modificar. Si no, sin embargo, hay no`NULL` `UnitPrice` valor, el método actualiza el producto s `UnitPrice` por el porcentaje especificado (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Paso 7: Agregar los botones de disminución y aumento a GridView

El control GridView (y DetailsView) son ambos formada por una colección de campos. Además de BoundFields, CheckBoxFields y TemplateFields, ASP.NET incluye el ButtonField, que, como su nombre implica, se representa como una columna con un botón, LinkButton o ImageButton para cada fila. Similar a la FormView, haga clic en *cualquier* botón dentro de la GridView botones de paginación, editar o eliminar botones, botones de ordenación y así sucesivamente provoca una devolución de datos y genera las operaciones de asignación GridView [ `RowCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

El ButtonField tiene un `CommandName` propiedad que asigna el valor especificado a cada uno de sus botones `CommandName` propiedades. Al igual que con la FormView el `CommandName` valor es utilizado por el `RowCommand` controlador de eventos para determinar qué botón se hizo clic.

S permiten agregar dos ButtonFields nuevo a GridView, uno con un texto del botón precio + 10% y el otro con el texto precio -10%. Para agregar estos ButtonFields, haga clic en el vínculo Editar columnas de la etiqueta inteligente de GridView s, seleccione el tipo de campo ButtonField en la lista en la parte superior izquierda y haga clic en el botón Agregar.


![Agregar dos ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: agregar dos ButtonFields a GridView


Mover el dos ButtonFields para que aparezcan como los dos primeros campos de GridView. A continuación, establezca el `Text` propiedades de estos dos ButtonFields para fijar los precios + 10% y el precio -10% y el `CommandName` propiedades IncreasePrice y DecreasePrice, respectivamente. De forma predeterminada, un ButtonField representa su columna de botones como LinkButton. Esto se puede cambiar, sin embargo, a través de las operaciones de asignación ButtonField [ `ButtonType` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Permiten s tienen estos dos ButtonFields representados como botones de comando regulares; Por consiguiente, establecer el `ButtonType` propiedad `Button`. Figura 19 muestra los campos de cuadro de diálogo una vez realizados estos cambios; Después de que es el marcado declarativo de s de GridView.


![Configurar el texto de ButtonFields, CommandName y ButtonType propiedades](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configurar la ButtonFields `Text`, `CommandName`, y `ButtonType` propiedades


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Con estos ButtonFields creados, el último paso es crear un controlador de eventos para el s GridView `RowCommand` eventos. Este controlador de eventos, si se desencadenó porque el precio + 10% o % de precio -10 se hizo clic, debe determinar la `ProductID` para la fila cuyo botón se hizo clic y, a continuación, invocar el `ProductsBLL` clase s. `UpdateProduct` método, pasando el apropiado `UnitPrice` ajuste porcentaje junto con la `ProductID`. El código siguiente realiza estas tareas:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Para determinar el `ProductID` para la fila cuyo precio + 10 se hizo clic en el botón de precio -10% o %, necesitamos que puede consultar la s GridView `DataKeys` colección. Esta colección contiene los valores de los campos especificados en la `DataKeyNames` propiedad para cada fila de GridView. Desde el s GridView `DataKeyNames` propiedad se estableció en ProductID por Visual Studio cuando se enlaza el ObjectDataSource a GridView, `DataKeys(rowIndex).Value` proporciona el `ProductID` para el elemento especificado *rowIndex*.

El ButtonField pasa automáticamente en el *rowIndex* de la fila cuyo botón se hizo clic a través de la `e.CommandArgument` parámetro. Por lo tanto, para determinar el `ProductID` para la fila cuyo precio + 10 se hizo clic en el botón de precio -10% o %, usaremos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Como con el botón interrumpir todos los productos, si se ha deshabilitado el estado de vista de GridView s, GridView es que se va a enlazar al almacén de datos subyacente en cada devolución de datos y por lo tanto, inmediatamente se actualizará para reflejar un cambio de precio que podría producirse al hacer clic en cualquiera de los botones. Si, sin embargo, no ha deshabilitado el estado de vista en el control GridView, debe enlazar manualmente los datos en GridView después de realizar este cambio. Para ello, basta con realizar una llamada a las operaciones de asignación GridView `DataBind()` método inmediatamente después de invocar el `UpdateProduct` método.

Figura 20 muestra la página cuando se visualizan los productos suministrados por Homestead de su abuela Kelly. Figura 21 muestra los resultados después el precio + 10% botón se ha hecho clic dos veces en el botón de precio -10% y Boysenberry Spread de su abuela una vez para salsa de arándanos Northwoods.


[![GridView incluye precio + 10% y botones de precio -10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: el precio de inclusión de GridView + 10% y el precio -10% botones ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Se han actualizado los precios para el producto primero y tercero mediante el precio + 10% y botones de precio -10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: los precios para el primer y tercer producto se han actualizado mediante el precio + 10% y el precio -10% botones ([haga clic aquí para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> El control GridView (y DetailsView) también pueden tener botones, LinkButton o ImageButtons agrega a sus TemplateFields. Como con BoundField, estos botones, al hacer clic, inducirían un postback, cuando se genera la s GridView `RowCommand` eventos. Al agregar botones TemplateField, sin embargo, el botón s `CommandArgument` no se establece automáticamente en el índice de la fila tal cual al usar ButtonFields. Si necesita determinar el índice de fila del botón que se ha hecho clic dentro de la `RowCommand` controlador de eventos, deberá establecer manualmente el botón s `CommandArgument` propiedad en su sintaxis declarativa en TemplateField, con un código como:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Resumen

Pueden incluir los controles GridView, DetailsView y FormView todos los botones, LinkButton o ImageButtons. Estos botones, al hacer clic, hacer una devolución de datos y generar el `ItemCommand` eventos en los controles FormView y DetailsView y `RowCommand` evento en GridView. Estos controles Web de datos tienen una funcionalidad integrada para controlar las acciones comunes relacionadas con el comando, como eliminar o modificar registros. Sin embargo, se puede usar los botones que, al hacer clic en, responder a la ejecución de nuestro propio código personalizado.

Para lograr esto, debemos crear un controlador de eventos para el `ItemCommand` o `RowCommand` eventos. En este controlador de eventos primero comprobamos entrante `CommandName` valor para determinar qué botón se hizo clic y, a continuación, tome las medidas adecuadas de personalizado. En este tutorial, hemos visto cómo usar los botones y ButtonFields de suspender todos los productos para un proveedor especificado o para aumentar o disminuir el precio de un producto determinado en un 10%.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-and-responding-to-buttons-to-a-gridview-cs.md)
