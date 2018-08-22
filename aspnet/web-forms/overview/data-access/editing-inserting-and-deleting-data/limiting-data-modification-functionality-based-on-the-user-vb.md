---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitar la funcionalidad de modificación de datos en función del usuario (VB) | Microsoft Docs
author: rick-anderson
description: En una aplicación web que permite a los usuarios editar datos, distintas cuentas de usuario que tenga privilegios de edición de datos diferentes. En este tutorial examinaremos cómo t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: 8633d6ba5d82dfa2ebbea6858eb89433573a775f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829902"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitar la funcionalidad de modificación de datos en función del usuario (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) o [descargar PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> En una aplicación web que permite a los usuarios editar datos, distintas cuentas de usuario que tenga privilegios de edición de datos diferentes. En este tutorial, analizaremos cómo ajustar de forma dinámica las capacidades de modificación de datos según el usuario visitante.


## <a name="introduction"></a>Introducción

Un número de aplicaciones web admite cuentas de usuario y proporciona diferentes opciones, informes y funcionalidad basada en el usuario que inició sesión. Por ejemplo, con nuestros tutoriales que podría desear permitir que a los usuarios de las empresas de proveedor para iniciar sesión en el sitio y actualizar la información general acerca de sus productos - su nombre y la cantidad por unidad, quizás: junto con información de proveedor, como su nombre de la empresa dirección, la información de contacto s y así sucesivamente. Además, podría desear incluir algunas cuentas de usuario para las personas de nuestra compañía para que pueden iniciar sesión y actualizar la información del producto, como cambiar el orden de nivel de unidades en existencias y así sucesivamente. Nuestra aplicación web también podría permitir a los usuarios anónimos visitar (personas que no han iniciado sesión), pero se limitaría que solo puedan ver los datos. Con este usuario cuenta sistema en su lugar, queremos que los controles Web de datos en nuestras páginas ASP.NET para ofrecer Insertar, editar y eliminar capacidades adecuadas para el usuario ha iniciado sesión actualmente.

En este tutorial, analizaremos cómo ajustar de forma dinámica las capacidades de modificación de datos según el usuario visitante. En concreto, vamos a crear una página que muestra la información de proveedores en un DetailsView editable junto con un control GridView que muestra los productos suministrados por el proveedor. Si el usuario visita la página es de nuestra empresa, puede: ver cualquier información de proveedor s; editar su dirección; y editar la información de cualquier producto proporcionado por el proveedor. Si, sin embargo, el usuario es de una empresa determinada, solo pueden ver y editar la información de su propia dirección y solo se puede editar sus productos que no se han marcado como discontinuo.


[![Un usuario de nuestra empresa puede editar cualquier información de s del proveedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: un usuario de nuestra empresa puede editar cualquier proveedor s información ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Un usuario de un determinado proveedor solo pueden ver y editar su información](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: un usuario de un determinado proveedor puede solo ver y editar su información ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Introducción s Let!

> [!NOTE]
> ASP.NET 2.0 sistema de pertenencia s proporciona una plataforma estandarizada y extensible para crear, administrar y validar las cuentas de usuario. Puesto que un examen del sistema de pertenencia está fuera del ámbito de estos tutoriales, este tutorial en su lugar "fakes" pertenencia, ya que permite visitantes anónimos elegir si se trata de un proveedor determinado o de nuestra empresa. Para obtener más información sobre la pertenencia, consulte mi [s pertenencia de examen de ASP.NET 2.0, Roles y perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) serie de artículos.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Paso 1: Lo que permite al usuario especificar sus derechos de acceso

En una aplicación web del mundo real, información de cuenta de usuario s incluiría si han trabajado para nuestra empresa o para un proveedor determinado, y esta información puede resultar accesible mediante programación desde nuestras páginas ASP.NET cuando el usuario ha iniciado sesión en el sitio. Esta información podría capturarse a través del sistema de roles de ASP.NET 2.0 s, como información de la cuenta de nivel de usuario a través del sistema de perfil o a través de algunos medios personalizados.

Dado que el objetivo de este tutorial es mostrar el ajuste de las capacidades de modificación de datos en función del usuario con sesión iniciada y no está diseñado para sistemas de perfil, roles y pertenencia de ASP.NET 2.0 showcase s, vamos a usar un mecanismo muy sencillas para determinar el capacidades del usuario visitar la página - DropDownList desde el que el usuario puede indicar si debe ser capaz de ver y editar cualquiera de la información de proveedores, o bien, ¿qué información del proveedor determinado s pueden ver y editar. Si el usuario indica que ella puede ver y editar toda la información de proveedor (el valor predeterminado), puede desplazarse por todos los proveedores, modificar cualquier información de dirección de proveedor s y edite el nombre y la cantidad por unidad de cualquier producto proporcionado por el proveedor seleccionado. Si el usuario indica que sólo ella puede ver y editar un proveedor determinado, sin embargo, a continuación, que solo puede ver los detalles y los productos para que un proveedor y solo puede actualizar el nombre y la cantidad de información de aquellos productos que son la unidad *no* discontinuo.

A continuación, es el primer paso en este tutorial crear este DropDownList y rellenarlo con los proveedores en el sistema. Abra el `UserLevelAccess.aspx` página en el `EditInsertDelete` carpeta, agregar DropDownList cuyo `ID` propiedad está establecida en `Suppliers`y enlazar este DropDownList a un nuevo origen ObjectDataSource denominado `AllSuppliersDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: crear un nuevo origen ObjectDataSource denominado `AllSuppliersDataSource` ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Puesto que deseamos que este DropDownList para incluir todos los proveedores, configurar el origen ObjectDataSource para invocar el `SuppliersBLL` clase s `GetSuppliers()` método. Asegúrese también de que la s ObjectDataSource `Update()` método se asigna a la `SuppliersBLL` clase s `UpdateSupplierAddress` método, como este origen ObjectDataSource también usará DetailsView que se agregarán en el paso 2.

Después de completar el ObjectDataSource wizard, complete los pasos mediante la configuración de la `Suppliers` DropDownList, que muestre el `CompanyName` campo de datos y se usa el `SupplierID` campo de datos como el valor para cada `ListItem`.


[![Configurar proveedores DropDownList para usar los campos de datos SupplierID y CompanyName](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configurar el `Suppliers` DropDownList para usar el `CompanyName` y `SupplierID` campos de datos ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


En este momento, DropDownList enumera los nombres de compañía de los proveedores en la base de datos. Sin embargo, también debemos incluir una opción "Mostrar o editar todos los proveedores" al control DropDownList. Para ello, establezca el `Suppliers` DropDownList s `AppendDataBoundItems` propiedad `true` y, a continuación, agregue un `ListItem` cuyo `Text` propiedad es "Mostrar o editar todos los proveedores" y cuyo valor es `-1`. Esto se puede agregar directamente a través el marcado declarativo o a través del diseñador, vaya a la ventana Propiedades y haga clic en el botón de puntos suspensivos en la s DropDownList `Items` propiedad.

> [!NOTE]
> Vuelva a consultar el [ *filtrado de maestro y detalles con un DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) tutorial para obtener una explicación más detallada sobre cómo agregar un elemento Select All a un enlace de datos DropDownList.


Después de la `AppendDataBoundItems` se estableció la propiedad y el `ListItem` agregado, el marcado declarativo de DropDownList s debe ser similar:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Figura 5 muestra una captura de pantalla de nuestro progreso actual, cuando se ve mediante un explorador.


[![Proveedores DropDownList contiene una presentación ListItem todas, más uno para cada proveedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: el `Suppliers` DropDownList contiene mostrar todo `ListItem`, además de uno para cada proveedor ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Puesto que desea actualizar la interfaz de usuario inmediatamente después de que el usuario ha cambiado su selección, establezca el `Suppliers` DropDownList s `AutoPostBack` propiedad `true`. En el paso 2, vamos a crear un control DetailsView que mostrará la información de los proveedores en función de la selección de DropDownList. A continuación, en el paso 3, vamos a crear un controlador de eventos para este s DropDownList `SelectedIndexChanged` eventos, en el que vamos a agregar código que enlaza la información del proveedor correspondiente a DetailsView según el proveedor seleccionado.

## <a name="step-2-adding-a-detailsview-control"></a>Paso 2: Agregar un Control DetailsView

Permiten s usar DetailsView para mostrar información del proveedor. Para el usuario que puede ver y editar todos los proveedores, DetailsView será compatible con la paginación, que permite al usuario recorrer el registro de una de información del proveedor a la vez. Sin embargo, si el usuario trabaja para un proveedor determinado, DetailsView mostrará a sólo ese proveedor determinado información de s y no incluye una interfaz de paginación. En cualquier caso, debe permitir al usuario editar los campos de país, ciudad y dirección de proveedor DetailsView.

Agregar un DetailsView en la página situada bajo la `Suppliers` DropDownList, establezca su `ID` propiedad a `SupplierDetails`y enlácelo al `AllSuppliersDataSource` ObjectDataSource creado en el paso anterior. A continuación, active las casillas de verificación Habilitar paginación y habilitar la edición de la etiqueta inteligente de s DetailsView.

> [!NOTE]
> Si don t ve una opción de habilitar la edición en la s DetailsView inteligente etiquetarla s porque no se asignó la s ObjectDataSource `Update()` método a la `SuppliersBLL` clase s `UpdateSupplierAddress` método. Dedique un momento para volver atrás y cambiar esta configuración, después de que la opción Habilitar edición debe aparecer en la etiqueta inteligente de s DetailsView.


Puesto que la `SuppliersBLL` clase s `UpdateSupplierAddress` método solo acepta cuatro parámetros: `supplierID`, `address`, `city`, y `country` -modificar la s DetailsView BoundFields para que el `CompanyName` y `Phone` BoundFields son de solo lectura. Además, quite el `SupplierID` BoundField completamente. Por último, el `AllSuppliersDataSource` ObjectDataSource actualmente tiene su `OldValuesParameterFormatString` propiedad establecida en `original_{0}`. Tómese un momento para quitar esta configuración de la propiedad definitivamente de la sintaxis declarativa o establézcalo como el valor predeterminado, `{0}`.

Después de configurar el `SupplierDetails` DetailsView y `AllSuppliersDataSource` ObjectDataSource, tendrá el siguiente marcado declarativo:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

En este momento DetailsView puede paginar a través de y se puede actualizar la información de dirección del proveedor seleccionado s, independientemente de la selección realizada en el `Suppliers` DropDownList (consulte la figura 6).


[![Cualquier información de proveedores se puede ver y actualiza su dirección](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: Any proveedores información se puede ver y actualizar la dirección de su ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Paso 3: Mostrar solo la información de s del proveedor seleccionado

Actualmente, nuestra página muestra la información de todos los proveedores, independientemente de si se ha seleccionado un proveedor determinado desde el `Suppliers` DropDownList. Para mostrar solo la información del proveedor para el proveedor seleccionado necesitamos agregar otro ObjectDataSource a nuestra página, que recupera información acerca de un proveedor determinado.

Agregar un nuevo origen ObjectDataSource a la página, asígnele el nombre `SingleSupplierDataSource`. En su etiqueta inteligente, haga clic en el vínculo Configurar origen de datos y tiene usar el `SuppliersBLL` clase s `GetSupplierBySupplierID(supplierID)` método. Igual que con el `AllSuppliersDataSource` ObjectDataSource, tiene la `SingleSupplierDataSource` ObjectDataSource s `Update()` método asignado a la `SuppliersBLL` clase s `UpdateSupplierAddress` método.


[![Configurar para usar el método GetSupplierBySupplierID(supplierID) SingleSupplierDataSource ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configurar el `SingleSupplierDataSource` ObjectDataSource para usar el `GetSupplierBySupplierID(supplierID)` método ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


A continuación, nos re pide que especifique el origen de los parámetros para el `GetSupplierBySupplierID(supplierID)` método s `supplierID` parámetro de entrada. Puesto que deseamos mostrar la información para el proveedor seleccionado de la lista desplegable, use el `Suppliers` DropDownList s `SelectedValue` propiedad como origen del parámetro.


[![Usar proveedores DropDownList como el origen del parámetro supplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: Use el `Suppliers` DropDownList como el `supplierID` origen del parámetro ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Incluso con este segundo origen ObjectDataSource agregado, el control DetailsView actualmente está configurado para usar siempre el `AllSuppliersDataSource` ObjectDataSource. Es necesario agregar lógica para ajustar el origen de datos utilizado por DetailsView en función de la `Suppliers` DropDownList elemento seleccionado. Para ello, cree un `SelectedIndexChanged` controlador de eventos para los proveedores de DropDownList. Se puede crear más fácilmente haciendo doble clic en DropDownList en el diseñador. Este controlador de eventos debe determinar qué origen de datos y debe volver a enlazar los datos a DetailsView. Esto se logra con el código siguiente:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

El controlador de eventos comienza mediante la determinación de si se ha seleccionado la opción "Mostrar o editar todos los proveedores". Si es así, Establece la `SupplierDetails` DetailsView s `DataSourceID` a `AllSuppliersDataSource` y devuelve el usuario para el primer registro en el conjunto de proveedores estableciendo el `PageIndex` propiedad en 0. Si, sin embargo, el usuario ha seleccionado un proveedor determinado de la lista desplegable, la s DetailsView `DataSourceID` se asigna a `SingleSuppliersDataSource`. Independientemente de qué datos se utiliza el origen, el `SuppliersDetails` modo se revertirá al modo de solo lectura y los datos se vuelve a enlazar a DetailsView mediante una llamada a la `SuppliersDetails` control s `DataBind()` método.

Con este controlador de eventos en su lugar, el control DetailsView muestra ahora el proveedor seleccionado, a menos que se seleccionó la opción "Mostrar o editar todos los proveedores", en cuyo caso todos los proveedores pueden verse mediante la interfaz de paginación. Figura 9 se muestra la página con la opción "Mostrar o editar todos los proveedores" seleccionada; Tenga en cuenta que la interfaz de paginación está presente, lo que permite al usuario que visite y actualice cualquier proveedor. Figura 10 muestra la página con el proveedor de la casa de agente de administración seleccionado. Sólo la información de s Ma casa es visible y editable en este caso.


[![Toda la información de proveedores se pueden ver y editar](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: todos los proveedores se pueden ver información y editados ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Solo la información del proveedor seleccionado s se puede ver y editar](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: solo puede ser la información de proveedor seleccionado s se ve y editados ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Para este tutorial, la DropDownList y DetailsView controlan s `EnableViewState` debe establecerse en `true` (predeterminado) porque la s DropDownList `SelectedIndex` y la s DetailsView `DataSourceID` se deben recordar los cambios de propiedad s postbacks.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Paso 4: Lista de los productos de proveedores en un control GridView Editable

Con DetailsView completa, el siguiente paso es incluir un control GridView editable que muestra los productos suministrados por el proveedor seleccionado. Este GridView debe permitir ediciones solo para el `ProductName` y `QuantityPerUnit` campos. Además, si el usuario visita la página es de un proveedor determinado, solo debe permitir las actualizaciones de aquellos productos que son *no* discontinuo. Para lograr esto, deberá agregar primero una sobrecarga de la `ProductsBLL` clase s `UpdateProducts` método que toma solo el `ProductID`, `ProductName`, y `QuantityPerUnit` campos como entradas. Se ve avanzado a través de este proceso con antelación en numerosos tutoriales, así que simplemente mirar en este caso, el código que se debe agregar a s `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Con esta sobrecarga creada, que está listo para agregar el control GridView y su origen ObjectDataSource asociado. Agregue un control GridView nuevo a la página, establezca su `ID` propiedad `ProductsBySupplier`y configúrelo para utilizar un nuevo origen ObjectDataSource denominado `ProductsBySupplierDataSource`. Puesto que deseamos que este control GridView a la lista de esos productos por el proveedor seleccionado, utilice el `ProductsBLL` clase s `GetProductsBySupplierID(supplierID)` método. También se asignan los `Update()` método a la nueva `UpdateProduct` sobrecarga que acabamos de crear.


[![Configurar el origen ObjectDataSource para utilizar la sobrecarga de UpdateProduct acaba de crear](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configurar el origen ObjectDataSource que se usarán el `UpdateProduct` sobrecargar acaba de crear ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Nos re le pide que seleccione el origen de los parámetros para el `GetProductsBySupplierID(supplierID)` método s `supplierID` parámetro de entrada. Puesto que deseamos mostrar los productos para el proveedor seleccionado en DetailsView, use el `SuppliersDetails` control DetailsView s `SelectedValue` propiedad como origen del parámetro.


[![Utilice la propiedad SelectedValue de s de SuppliersDetails DetailsView como origen del parámetro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: Use el `SuppliersDetails` DetailsView s `SelectedValue` propiedad como origen del parámetro ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Volver a la GridView, quite todos los campos de GridView, excepto para `ProductName`, `QuantityPerUnit`, y `Discontinued`, marcar la `Discontinued` CampoCasillaVerificación como de solo lectura. Además, active la opción Habilitar edición de la etiqueta inteligente de s GridView. Una vez realizados estos cambios, el marcado declarativo para el control GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Igual que con nuestro ObjectDataSource anterior, este uno s `OldValuesParameterFormatString` propiedad está establecida en `original_{0}`, lo que causará problemas al intentar actualizar un nombre de producto s o la cantidad por unidad. Quitar esta propiedad de la sintaxis declarativa por completo o se establece en su valor predeterminado, `{0}`.

Con esta configuración completa, nuestra página ahora muestra los productos proporcionados por el proveedor seleccionado en el control GridView (consulte la figura 13). Actualmente *cualquier* se puede actualizar el nombre de producto s o la cantidad por unidad. Sin embargo, debemos actualizar nuestra lógica de página para que esta funcionalidad está prohibida para productos discontinuados para los usuarios asociados con un proveedor determinado. Trataremos esta última pieza en el paso 5.


[![Se muestran los productos proporcionados por el proveedor seleccionado](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: se muestran los productos proporcionados por el proveedor seleccionado ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Con la adición de este control GridView editable la `Suppliers` DropDownList s `SelectedIndexChanged` se debe actualizar el controlador de eventos para devolver el control GridView a un estado de solo lectura. En caso contrario, si se selecciona un proveedor diferente en el transcurso de edición de la información de producto, el índice correspondiente en el control GridView para el nuevo proveedor también será editable. Para evitar esto, basta con establecer la s GridView `EditIndex` propiedad `-1` en el `SelectedIndexChanged` controlador de eventos.


Además, recuerde que es importante que el control GridView s estado de vista habilitado (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, corre el riesgo de tener usuarios simultáneos que se eliminen o modifiquen de forma no intencionada de registros. Consulte [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView y DetailsView/FormViews esa compatibilidad de edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Paso 5: No permitir edición para no incluye productos al mostrar o editar todos los proveedores no es seleccionada

Mientras el `ProductsBySupplier` GridView es totalmente funcional, concede actualmente demasiado acceso a aquellos usuarios que proceden de un proveedor determinado. Por nuestras reglas de negocio, dichos usuarios no deben ser capaz de actualizar productos discontinuados. Para exigir esto, podemos ocultar (o deshabilitar) el botón Editar en aquellas filas en GridView con productos discontinuados cuando un usuario que se visita la página desde un proveedor.

Crear un controlador de eventos para el s GridView `RowDataBound` eventos. En este controlador de eventos es necesario determinar si el usuario está asociado con un proveedor determinado y que, para este tutorial, se puede determinar mediante la comprobación de las operaciones de asignación proveedores DropDownList `SelectedValue` propiedad: si lo es s un valor distinto de -1 y, a continuación, el usuario asociado a un proveedor determinado. Para que estos usuarios, a continuación, necesitamos determinar si el producto no está disponible. Podemos obtenemos una referencia a los datos reales `ProductRow` instancia enlazada a la fila del control GridView a través de la `e.Row.DataItem` propiedad, como se describe en el [ *mostrar información de resumen en el pie de página de GridView s* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Este tutorial. Si el producto no está disponible, podemos obtenemos una referencia al mismo mediante programación en el botón Editar en la s GridView CommandField mediante las técnicas descritas en el tutorial anterior, [ *Agregar cliente confirmación al eliminar* ](adding-client-side-confirmation-when-deleting-vb.md). Una vez que tenemos una referencia, a continuación, podemos ocultar o deshabilitar el botón.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Con este evento de controlador en su lugar, al visitar esta página como un usuario de un proveedor concreto aquellos productos que se han suspendido no son editables, como el botón de edición está oculto para estos productos. Por ejemplo, s Chef Antón tártara es un producto no incluido para el proveedor de Nueva Orleans Cajun terrenales. Cuando se visita la página para este proveedor determinado, se oculta el botón Editar para este producto a la vista (consulte la figura 14). Sin embargo, cuando se visita con la "mostrar o editar todos los proveedores", el botón Editar es disponible (consulte la figura 15).


[![Para los usuarios específicos de cada proveedor se oculta el botón Editar para Chef Antón s tártara](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: para usuarios de específicos de cada proveedor se oculta el botón Editar para Chef Antón s tártara ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Para mostrar o editar todos los usuarios de proveedores, se muestra el botón Editar para Chef Antón s tártara](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: para mostrar o editar todos los proveedores a los usuarios, el botón Editar para s Chef Antón tártara se muestra ([haga clic aquí para ver imagen en tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Comprobación de derechos de acceso en la capa de lógica de negocios

En este tutorial ASP.NET página controla toda la lógica con respecto a qué información puede ver el usuario y qué productos puede actualizar. Idealmente, esta lógica también estará presente en la capa de lógica empresarial. Por ejemplo, el `SuppliersBLL` clase s `GetSuppliers()` método (que devuelve todos los proveedores) puede incluir una comprobación para asegurarse de que el usuario ha iniciado sesión actualmente es *no* asociado con un proveedor concreto. Del mismo modo, el `UpdateSupplierAddress` método podría incluir una comprobación para asegurarse de que el usuario ha iniciado sesión actualmente cualquiera trabajó para nuestra empresa (y, por tanto, puede actualizar la información de dirección de todos los proveedores) o está asociado con el proveedor cuyos datos se está actualizando.

No incluí tales comprobaciones de la capa BLL aquí porque en nuestro tutorial, los derechos de usuario s se determinan por DropDownList en la página, que no se pueden obtener acceso las clases BLL. Cuando se usa el sistema de pertenencia o uno de los esquemas de autenticación fuera del cuadro proporcionados por ASP.NET (por ejemplo, la autenticación de Windows), que tiene iniciada en usuario s pueden obtenerse información y la información de roles de BLL, por lo que dicho acceso derechos comprueba posibles en la presentación y capas BLL.

## <a name="summary"></a>Resumen

Mayoría de los sitios que proporcionan las cuentas de usuario tenga que personalizar la interfaz de modificación de datos según el usuario que inició sesión. Los usuarios administrativos pueden ser capaz de eliminar y editar cualquier registro, mientras que usuarios no administrativos pueden estar limitados sólo actualizar o eliminar registros que crearon ellos mismos. Puede ser cualquier escenario, los controles Web, el origen ObjectDataSource, datos y las clases de la capa de lógica empresarial pueden ampliarse para agregar o denegar cierta funcionalidad en función del usuario con sesión iniciada. En este tutorial hemos visto cómo limitar los datos visibles y editables, dependiendo de si el usuario ha asociado con un proveedor determinado o si han trabajado para nuestra empresa.

En este tutorial concluye nuestro examen de insertar, actualizar y eliminar datos mediante los controles GridView, DetailsView y FormView. Empezando por el siguiente tutorial, centraremos nuestra atención a la adición de paginación y ordenación de soporte técnico.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-client-side-confirmation-when-deleting-vb.md)
