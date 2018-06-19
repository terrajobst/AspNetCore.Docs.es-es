---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Limitar la funcionalidad de modificación de datos según el usuario (C#) | Documentos de Microsoft
author: rick-anderson
description: En una aplicación web que permite a los usuarios editar datos, cuentas de usuario diferentes pueden tener privilegios de edición de datos diferentes. En este tutorial examinaremos cómo t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: b056536eeaa86ef2c73debe23dd38861f41b2a69
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888203"
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Limitar la funcionalidad de modificación de datos según el usuario (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) o [descarga de PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> En una aplicación web que permite a los usuarios editar datos, cuentas de usuario diferentes pueden tener privilegios de edición de datos diferentes. En este tutorial, examinaremos cómo ajustar dinámicamente las capacidades de modificación de datos según el usuario visitante.


## <a name="introduction"></a>Introducción

Un número de aplicaciones web compatible con las cuentas de usuario y proporciona diferentes opciones, informes y funcionalidad basada en la sesión del usuario. Por ejemplo, con nuestros tutoriales que queramos permiten a los usuarios de las empresas de proveedor para iniciar sesión en el sitio y actualizar la información general acerca de sus productos - su nombre y la cantidad por unidad, quizás, junto con información de proveedor, como el nombre de su compañía, dirección, la información de contacto s y así sucesivamente. Además, podría desear incluir algunas cuentas de usuario para las personas de la compañía para que pueden iniciar sesión y actualizar la información de producto como unidades en existencias, nivel de nuevo pedido y así sucesivamente. Nuestra aplicación web también podría permitir a los usuarios anónimos visitar (personas que no han iniciado sesión), pero limitaría para que sólo vean datos. Con este usuario cuenta sistema en su lugar, queremos que los controles Web de datos en nuestras páginas ASP.NET para ofrecer Insertar, modificar y eliminar las capacidades adecuadas para el usuario ha iniciado sesión actualmente.

En este tutorial, examinaremos cómo ajustar dinámicamente las capacidades de modificación de datos según el usuario visitante. En concreto, vamos a crear una página que muestra la información de proveedores en un DetailsView editable junto con un control GridView que muestra los productos suministrados por el proveedor. Si el usuario visita la página es de nuestra empresa, puede: ver cualquier información de proveedor s; editar su dirección; y editar la información de cualquier producto proporcionada por el proveedor. Si, sin embargo, el usuario es de una empresa determinada, solo pueden ver y editar su propia información de dirección y sólo se puede editar sus productos que no se han marcado como no incluidas.


[![Un usuario de nuestra empresa puede modificar cualquier información de s de proveedor](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Figura 1**: un usuario de nuestra empresa puede editar cualquier proveedor s información ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Un usuario de un determinado proveedor solo pueden ver y editar su información](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Figura 2**: un usuario de un determinado proveedor puede solo ver y editar su información ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Permiten s empiece a trabajar.

> [!NOTE]
> ASP.NET 2.0 del sistema de pertenencia s proporciona una plataforma extensible estandarizada para crear, administrar y validar las cuentas de usuario. Puesto que un examen del sistema de pertenencia está fuera del ámbito de estos tutoriales, este tutorial en su lugar "fakes" pertenencia al permitir que los visitantes anónimos elegir si son de un proveedor determinado o de nuestra empresa. Para obtener más información sobre la pertenencia, consulte mi [examinar ASP.NET 2.0 s pertenencia, funciones y perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) serie de artículos.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Paso 1: Permitir que el usuario especifique sus derechos de acceso

En una aplicación web del mundo real, información de cuenta de usuario s incluiría si han funcionado para nuestra empresa o para un proveedor determinado, y esta información estaría sean accesible mediante programación de nuestras páginas ASP.NET una vez que el usuario ha iniciado sesión en el sitio. Esta información podría capturarse a través del sistema de roles de ASP.NET 2.0 s, como información de cuenta de nivel de usuario a través del sistema de perfil, o a través de alguna manera personalizada.

Dado que el objetivo de este tutorial es mostrar el ajuste de las capacidades de modificación de datos en función de usuario con sesión iniciada y no está diseñado para sistemas de perfil, roles y pertenencia de ASP.NET 2.0 showcase s, vamos a usar un mecanismo muy simple para determinar la funciones para el usuario visita la página - DropDownList desde el que el usuario puede indicar si debe ser capaz de ver y modificar cualquiera de la información de proveedores, o bien, ¿qué información del proveedor determinado s pueden ver y editar. Si el usuario indica que ella puede ver y editar toda la información de proveedor (el valor predeterminado), puede desplazarse a través de todos los proveedores, modificar cualquier información de dirección de proveedor s y edite el nombre y la cantidad por unidad de cualquier producto proporcionada por el proveedor seleccionado. Si el usuario indica que solo puede ver y editar un proveedor determinado, sin embargo, a continuación, que sólo puede ver los detalles y los productos para que un proveedor y solo puede actualizar el nombre y la cantidad por la información de la unidad para los productos que están *no* suspendido.

A continuación, es el primer paso en este tutorial crear este DropDownList y rellenarlo con los proveedores en el sistema. Abra la `UserLevelAccess.aspx` página en el `EditInsertDelete` carpeta, agregar DropDownList cuyo `ID` propiedad está establecida en `Suppliers`y enlazar este DropDownList a un nuevo ObjectDataSource denominado `AllSuppliersDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Figura 3**: crear un nuevo ObjectDataSource denominado `AllSuppliersDataSource` ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Puesto que deseamos que este DropDownList para incluir todos los proveedores, configurar el ObjectDataSource para invocar la `SuppliersBLL` clase s. `GetSuppliers()` método. Asegúrese también de que las operaciones de asignación ObjectDataSource `Update()` método se asigna a la `SuppliersBLL` clase s. `UpdateSupplierAddress` método, como este ObjectDataSource también usará DetailsView iremos agregando en el paso 2.

Después de completar el Asistente de ObjectDataSource, complete los pasos mediante la configuración de la `Suppliers` DropDownList, que muestra la `CompanyName` campo de datos y se utiliza el `SupplierID` campo de datos como el valor para cada `ListItem`.


[![Configurar lo proveedores de DropDownList para usar los campos de datos de SupplierID y CompanyName](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Figura 4**: configurar la `Suppliers` DropDownList que se utilizan los `CompanyName` y `SupplierID` campos de datos ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


En este momento, DropDownList enumera los nombres de compañía de los proveedores en la base de datos. Sin embargo, también es necesario incluir una opción "Mostrar o editar todos los proveedores" a los controles DropDownList. Para ello, establezca la `Suppliers` DropDownList s `AppendDataBoundItems` propiedad `true` y, a continuación, agregue un `ListItem` cuyo `Text` propiedad es "Mostrar o editar todos los proveedores" y cuyo valor es `-1`. Esto puede agregarse directamente a través de marcado declarativo o a través del diseñador, vaya a la ventana Propiedades y haciendo clic en el botón de puntos suspensivos en la s DropDownList `Items` propiedad.

> [!NOTE]
> Hacen referencia a la [ *filtrado de maestro y detalles con un DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial para obtener una explicación más detallada sobre cómo agregar un elemento Select All a un enlace de datos DropDownList.


Después de la `AppendDataBoundItems` se ha establecido la propiedad y el `ListItem` agregado, el marcado declarativo de DropDownList s debería ser similar:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Figura 5 se muestra una captura de pantalla de nuestro progreso actual, cuando se ven a través de un explorador.


[![DropDownList proveedores contiene una presentación ListItem todos, más uno para cada proveedor](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Figura 5**: el `Suppliers` DropDownList contiene mostrar todo `ListItem`, además de uno para cada proveedor ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Puesto que deseamos actualizar la interfaz de usuario inmediatamente después de que el usuario ha cambiado su selección, establezca la `Suppliers` DropDownList s `AutoPostBack` propiedad `true`. En el paso 2, vamos a crear un control de DetailsView que mostrará la información de los proveedores según la selección de DropDownList. A continuación, en el paso 3, vamos a crear un controlador de eventos para este s DropDownList `SelectedIndexChanged` eventos, en el que vamos a agregar código que enlaza la información de proveedor adecuado a DetailsView según el proveedor seleccionado.

## <a name="step-2-adding-a-detailsview-control"></a>Paso 2: Agregar un Control DetailsView

Permitir que s use DetailsView para mostrar información de proveedor. Para el usuario que puede ver y editar todos los proveedores, DetailsView será compatible con la paginación, que permite al usuario recorrer el registro de una de información de proveedor a la vez. Sin embargo, si el usuario trabaja para un proveedor determinado, DetailsView mostrará a solo ese proveedor determinado información de s y no incluirá una interfaz de paginación. En cualquier caso, debe permitir al usuario editar los campos de país, ciudad y dirección de proveedor s DetailsView.

Agregar un DetailsView a la página situada bajo la `Suppliers` DropDownList, establezca su `ID` propiedad `SupplierDetails`y enlácelo al `AllSuppliersDataSource` ObjectDataSource creado en el paso anterior. A continuación, active las casillas de verificación Habilitar paginación y habilitar la edición de la etiqueta inteligente de DetailsView s.

> [!NOTE]
> Si don t ve una opción de Habilitar edición en la s de DetailsView inteligente etiquetarlo s porque no se asignó la s ObjectDataSource `Update()` método a la `SuppliersBLL` clase s. `UpdateSupplierAddress` método. Tómese un momento para volver atrás y cambiar esta configuración, después de que la opción Habilitar edición debe aparecer en la etiqueta inteligente de DetailsView s.


Puesto que la `SuppliersBLL` clase s `UpdateSupplierAddress` método sólo acepta cuatro parámetros: `supplierID`, `address`, `city`, y `country` -modificar la s de DetailsView BoundFields para que la `CompanyName` y `Phone` BoundFields son de solo lectura. Además, quitar el `SupplierID` BoundField completamente. Por último, el `AllSuppliersDataSource` ObjectDataSource actualmente tiene su `OldValuesParameterFormatString` propiedad establecida en `original_{0}`. Tómese un momento para quitar esta configuración de la propiedad de por completo la sintaxis declarativa o establezca el valor predeterminado, `{0}`.

Después de configurar la `SupplierDetails` DetailsView y `AllSuppliersDataSource` ObjectDataSource, tenemos el siguiente marcado declarativo:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

En este momento se puede enviar mensajes a DetailsView a través de y se puede actualizar la información de dirección de proveedor seleccionado s, independientemente de la selección realizada en el `Suppliers` DropDownList (consulte la figura 6).


[![Cualquier información de proveedores se puede ver y actualiza su dirección](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Figura 6**: Any proveedores podrá ver la información y su dirección actualizado ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Paso 3: Mostrar sólo la información de s del proveedor seleccionado

Nuestra página actualmente muestra la información de todos los proveedores, independientemente de si se ha seleccionado un proveedor determinado desde el `Suppliers` DropDownList. Para mostrar únicamente la información de proveedor para el proveedor seleccionado que necesitamos agregar otro ObjectDataSource a nuestra página, que recupera información sobre un proveedor determinado.

Agregar un nuevo origen ObjectDataSource a la página, asígnele el nombre `SingleSupplierDataSource`. En su etiqueta inteligente, haga clic en el vínculo Configurar origen de datos y para que use la `SuppliersBLL` clase s. `GetSupplierBySupplierID(supplierID)` método. Al igual que con la `AllSuppliersDataSource` ObjectDataSource, tiene la `SingleSupplierDataSource` ObjectDataSource s `Update()` método asigna a la `SuppliersBLL` clase s. `UpdateSupplierAddress` método.


[![Configurar el SingleSupplierDataSource ObjectDataSource para utilizar el método GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Figura 7**: configurar la `SingleSupplierDataSource` ObjectDataSource que se utilizan los `GetSupplierBySupplierID(supplierID)` método ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


A continuación, se vuelve pedirá que especifique el origen del parámetro para el `GetSupplierBySupplierID(supplierID)` método s `supplierID` parámetro de entrada. Puesto que deseamos mostrar la información para el proveedor seleccionado de la lista desplegable, use la `Suppliers` DropDownList s `SelectedValue` propiedad como origen del parámetro.


[![Usar los proveedores de DropDownList como el origen del parámetro supplierID](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Figura 8**: Use la `Suppliers` DropDownList como el `supplierID` origen del parámetro ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


Incluso con esta segunda ObjectDataSource agregado, el control de DetailsView está configurado actualmente para que use siempre el `AllSuppliersDataSource` ObjectDataSource. Tenemos que agregar lógica para ajustar el origen de datos utilizado por el DetailsView según el `Suppliers` DropDownList elemento seleccionado. Para ello, cree un `SelectedIndexChanged` controlador de eventos para los proveedores de DropDownList. Se puede crear más fácilmente haciendo doble clic en DropDownList en el diseñador. Este controlador de eventos necesita determinar el origen de datos debe usar y debe volver a enlazar los datos a DetailsView. Esto se consigue con el código siguiente:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

El controlador de eventos se comienza mediante la determinación de si se ha seleccionado la opción "Mostrar o editar todos los proveedores". Si es así, Establece el `SupplierDetails` DetailsView s `DataSourceID` a `AllSuppliersDataSource` y devuelve al usuario al primer registro en el conjunto de proveedores estableciendo la `PageIndex` propiedad en 0. Si, sin embargo, el usuario ha seleccionado un proveedor determinado de la lista desplegable, las operaciones de asignación DetailsView `DataSourceID` se asigna a `SingleSuppliersDataSource`. Independientemente de qué datos de origen se utiliza, el `SuppliersDetails` modo se revierte al modo de solo lectura y los datos se vuelve a DetailsView a enlazar mediante una llamada a la `SuppliersDetails` control s `DataBind()` método.

Con este controlador de eventos en su lugar, el control DetailsView muestra ahora el proveedor seleccionado, a menos que se ha seleccionado la opción "Mostrar o editar todos los proveedores", en cuyo caso todos los proveedores pueden verse mediante la interfaz de paginación. Ilustración 9 se muestra la página con la opción "Mostrar o editar todos los proveedores" seleccionada; Tenga en cuenta que la interfaz de paginación está presente, lo que permite al usuario que visite y actualizar cualquier proveedor. Figura 10 muestra la página con el proveedor de casa Ma seleccionado. Solo información de casa Ma s es visibles y modificables en este caso.


[![Toda la información de proveedores se pueden ver y editar](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Figura 9**: todos los proveedores se pueden ver información y editada ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Solo la información del proveedor seleccionado s se pueden ver y editar](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Figura 10**: sólo pueden ser la información de proveedor seleccionado s se ve y editada ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> Para este tutorial, DropDownList tanto de DetailsView controlan s `EnableViewState` debe establecerse en `true` (valor predeterminado) porque la s DropDownList `SelectedIndex` y las operaciones de asignación DetailsView `DataSourceID` cambios de propiedad s se deben recordar a través de las devoluciones de datos.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Paso 4: Lista de los productos de proveedores en un control GridView Editable

Con el DetailsView completa, el paso siguiente es incluir un control GridView editable que enumera los productos suministrados por el proveedor seleccionado. Este GridView debe permitir modificaciones para que solo el `ProductName` y `QuantityPerUnit` campos. Además, si el usuario visita la página procede de un proveedor determinado, debe permitir sólo las actualizaciones de los productos que son *no* suspendido. Para ello se deberá agregar primero una sobrecarga de la `ProductsBLL` clase s `UpdateProducts` método que toma en simplemente el `ProductID`, `ProductName`, y `QuantityPerUnit` campos como entradas. Se ve avanza a través de este proceso con antelación de numerosos tutoriales, por lo que permiten s simplemente mirar en este caso, el código que debe agregarse a `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Con esta sobrecarga creada, se re listo para agregar el control GridView y su ObjectDataSource asociado. Agregar un control GridView nuevo a la página, establezca su `ID` propiedad `ProductsBySupplier`y configúrelo para utilizar un nuevo ObjectDataSource denominado `ProductsBySupplierDataSource`. Puesto que deseamos que este GridView para enumerar los productos por el proveedor seleccionado, utilice la `ProductsBLL` clase s. `GetProductsBySupplierID(supplierID)` método. Asignar el `Update()` método a la nueva `UpdateProduct` sobrecarga que acabamos de crear.


[![Configurar el ObjectDataSource para utilizar la sobrecarga de UpdateProduct recién creada](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Figura 11**: configurar el ObjectDataSource que se utilizan los `UpdateProduct` sobrecarga acaba de crear ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


Se re le pide que seleccione el origen del parámetro para el `GetProductsBySupplierID(supplierID)` método s `supplierID` parámetro de entrada. Puesto que deseamos mostrar los productos para el proveedor seleccionado en DetailsView, use la `SuppliersDetails` DetailsView control s `SelectedValue` propiedad como origen del parámetro.


[![Utilice la propiedad de SuppliersDetails DetailsView s SelectedValue como origen del parámetro](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Figura 12**: Use la `SuppliersDetails` DetailsView s `SelectedValue` propiedad como origen del parámetro ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Volver a GridView, quite todos los campos de GridView excepto `ProductName`, `QuantityPerUnit`, y `Discontinued`, marcar la `Discontinued` CampoCasillaVerificación como de solo lectura. Además, active la opción Habilitar edición de la etiqueta inteligente de s de GridView. Una vez realizados estos cambios, el marcado declarativo para el control GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Al igual que con nuestro ObjectDataSources anterior, esta una s `OldValuesParameterFormatString` propiedad está establecida en `original_{0}`, lo que causará problemas al intentar actualizar un nombre de producto s o la cantidad por unidad. Elimine esta propiedad de la sintaxis declarativa por completo o se establece en su valor predeterminado, `{0}`.

Con esta configuración completa, nuestra página enumera ahora los productos suministrados por el proveedor seleccionado en el control GridView (Véase la figura 13). Actualmente *cualquier* se puede actualizar el nombre de producto s o la cantidad por unidad. Sin embargo, es necesario actualizar la lógica de la página para que esta funcionalidad está prohibida para productos no disponibles para los usuarios asociados con un proveedor determinado. Abordaremos esta última pieza en el paso 5.


[![Se muestran los productos suministrados por el proveedor seleccionado](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Figura 13**: se muestran los productos proporcionados por el proveedor seleccionado ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> Con la adición de este control GridView editable la `Suppliers` DropDownList s `SelectedIndexChanged` controlador de eventos debe actualizarse para devolver el control GridView a un estado de solo lectura. En caso contrario, si se selecciona un proveedor diferente mientras se esté realizando la edición de información de producto, el índice correspondiente en el control GridView para el nuevo proveedor también se puede editar. Para evitar esto, basta con establecer la s GridView `EditIndex` propiedad `-1` en el `SelectedIndexChanged` controlador de eventos.


Además, recuerde que es importante que GridView posible estado de vista de s habilitado (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, se corre el riesgo de tener usuarios simultáneos involuntariamente eliminen o modifiquen los registros. Vea [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Paso 5: No permitir edición para no incluye productos al mostrar o editar todos los proveedores no es seleccionada

Mientras el `ProductsBySupplier` GridView es totalmente funcional, concede actualmente demasiado acceso a aquellos usuarios que provienen de un proveedor determinado. Por nuestro reglas de negocios, estos usuarios no deben poder actualizar productos desusados. Para exigir esto, podemos ocultar (o deshabilitar) el botón Editar en las filas de GridView con productos desusados cuando se visita la página por un usuario desde un proveedor.

Crear un controlador de eventos para el s GridView `RowDataBound` eventos. En este controlador de eventos es necesario determinar si el usuario está asociado con un proveedor determinado y que, para este tutorial, se puede determinar mediante la comprobación de las operaciones de asignación de DropDownList proveedores `SelectedValue` propiedad: si lo s un valor distinto de -1 y, a continuación, el usuario es asociado a un proveedor determinado. Para que estos usuarios, a continuación, necesitamos determinar si el producto ya no está disponible. Se puede obtener una referencia a los datos reales `ProductRow` instancia se enlaza a la fila de GridView a través de la `e.Row.DataItem` propiedad, como se describe en el [ *mostrar información de resumen en el pie de página de GridView e* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) tutorial. Si el producto ya no está disponible, podemos obtenemos una referencia al mismo mediante programación en el botón Editar en la s de GridView CommandField mediante las técnicas descritas en el tutorial anterior, [ *Agregar cliente confirmación al eliminar* ](adding-client-side-confirmation-when-deleting-cs.md). Una vez que tenemos una referencia, a continuación, podemos ocultar o deshabilitar el botón.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Con este evento controlador en su lugar, cuando no son editables, visite esta página como un usuario de un proveedor concreto aquellos productos que no se pueden utilizar como el botón de edición se oculta para estos productos. Por ejemplo, s Chef Anton tártara es un producto no incluido para el proveedor de New Orleans Cajun Delights. Al visitar la página para este proveedor determinado, el botón Editar para este producto está oculto de la vista (Véase la figura 14). Sin embargo, cuando se visita con la "mostrar o editar todos los proveedores", el botón Editar es disponible (vea la figura 15).


[![Para los usuarios específicos de cada proveedor se oculta el botón Editar para Chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Figura 14**: para usuarios de específicos de cada proveedor se oculta el botón Editar para Chef Anton s Gumbo Mix ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Para mostrar o editar todos los usuarios de proveedores, se muestra el botón Editar para Chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Figura 15**: para mostrar o editar todos los proveedores a los usuarios, el botón Editar para s Chef Anton tártara se muestra ([haga clic aquí para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Comprobación de los derechos de acceso en la capa de lógica de negocios

En este tutorial ASP.NET página controla toda la lógica con respecto a la información que el usuario puede ver y qué productos puede actualizar. Idealmente, esta lógica podría estar presente en la capa de lógica de negocios. Por ejemplo, el `SuppliersBLL` clase s `GetSuppliers()` método (que devuelve todos los proveedores) podría incluir una comprobación para asegurarse de que el usuario ha iniciado sesión actualmente es *no* asociada a un proveedor concreto. Del mismo modo, el `UpdateSupplierAddress` método podría incluir una comprobación para asegurarse de que el usuario ha iniciado sesión actualmente cualquiera trabajó para nuestra empresa (y, por tanto, puede actualizar la información de dirección de todos los proveedores) o se asocia con el proveedor cuyos datos se está actualizando.

No incluía esas comprobaciones de la capa BLL aquí porque en nuestro tutorial DropDownList en la página, que no se pueden tener acceso las clases BLL determina los derechos de usuario s. Cuando se usa el sistema de pertenencia o uno de los esquemas de autenticación fuera del cuadro proporcionados por ASP.NET (por ejemplo, autenticación de Windows), que tiene iniciada en usuario s información e información de roles pueden tener acceso desde la capa BLL, por lo que dicho acceso derechos comprueba posibles en la presentación y los niveles BLL.

## <a name="summary"></a>Resumen

Mayoría de los sitios que proporcionan las cuentas de usuario necesario personalizar la interfaz de modificación de datos en función de la sesión del usuario. Los usuarios administrativos podrán que pueda eliminar y editar cualquier registro, mientras que los usuarios no administrativos pueden estar limitados sólo actualizar o eliminar registros que crearon ellos mismos. Puede ser cualquier el escenario, los datos de controles Web, ObjectDataSource, y las clases de la capa de lógica empresarial pueden ampliarse para agregar o denegar cierta funcionalidad en función de usuario con sesión iniciada. En este tutorial, hemos visto cómo limitar los datos visibles y modificables dependiendo de si el usuario estaba asociado con un proveedor determinado o si ha trabajado para nuestra empresa.

Este tutorial concluye el examen de insertar, actualizar y eliminar datos mediante los controles GridView, DetailsView y FormView. A partir de la siguiente tutorial, centraremos nuestro atención a la adición de paginación y la ordenación de soporte técnico.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-client-side-confirmation-when-deleting-cs.md)
> [Siguiente](an-overview-of-inserting-updating-and-deleting-data-vb.md)
