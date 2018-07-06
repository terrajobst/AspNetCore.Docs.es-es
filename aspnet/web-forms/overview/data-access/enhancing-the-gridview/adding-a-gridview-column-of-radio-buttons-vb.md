---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Agregar una columna GridView de botones de Radio (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial explica cómo agregar una columna de botones de radio a un control GridView para proporcionar al usuario una manera más intuitiva de la selección de una sola fila de...
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 56aa37392ab51306112f934f8dbff4151f232d35
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835617"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Agregar una columna GridView de botones de Radio (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) o [descargar PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> Este tutorial examina cómo agregar una columna de botones de radio a un control GridView para proporcionar al usuario una manera más intuitiva de la selección de una sola fila del control GridView.


## <a name="introduction"></a>Introducción

El control GridView ofrece un amplio abanico de funcionalidades integradas. Incluye una serie de diferentes campos para mostrar texto, imágenes, hipervínculos y botones. Admite plantillas para una mayor personalización. Con unos pocos clics del mouse, lo posible para realizar un GridView donde cada fila se puede seleccionar a través de un botón, o para habilitar la edición o eliminación de recursos. A pesar de la gran cantidad de características proporcionadas, con frecuencia habrá situaciones en que adicionales, las características no admitidas deberá agregarse. En este tutorial y los dos siguientes examinaremos cómo mejorar la funcionalidad de s GridView para incluir características adicionales.

En este tutorial y el siguiente se centran en mejorar el proceso de selección de fila. Como examinarse en la [Master/Detail mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), podemos agregar un CommandField en el control GridView que incluye un botón de selección. Al hacer clic, que habrá trastornos una devolución de datos y la s GridView `SelectedIndex` propiedad se actualiza con el índice de la fila cuyo seleccione botón se hizo clic. En el [Master/Detail mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial, vimos cómo usar esta característica para mostrar los detalles de la fila seleccionada de GridView.

Aunque el botón de selección funciona en muchas situaciones, puede no funcionar así para otros usuarios. En lugar de usar un botón, normalmente se usan dos otros elementos de interfaz de usuario para la selección: el botón de radio y casilla de verificación. Podemos aumentamos la GridView para que en lugar de un botón de selección, cada fila contiene un botón de radio o casilla. En escenarios donde el usuario solo puede seleccionar uno de los registros de GridView, el botón de radio podría estar preferido sobre el botón de selección. En situaciones donde el usuario potencialmente puede seleccionar varios registros, como en una aplicación de correo electrónico basado en web, donde un usuario podría desear para seleccionar varios mensajes para eliminar la casilla de verificación ofrece una funcionalidad que no está disponible en el botón de selección o un botón de radio interfaces de usuario.

Este tutorial examina cómo agregar una columna de botones de radio en el control GridView. El tutorial continuar analiza el uso de las casillas de verificación.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Paso 1: Creación de la mejora de las páginas Web de GridView

Antes de empezar a mejorar el control GridView para incluir una columna de botones de radio, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial y los dos siguientes. Empiece por agregar una nueva carpeta denominada `EnhancedGridView`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource


Al igual que en las demás carpetas `Default.aspx` en el `EnhancedGridView` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Por último, agregue estas cuatro páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de utilizar el SqlDataSource Control `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para la edición, inserción y eliminación de tutoriales.


![El mapa del sitio incluye ahora entradas para la mejora de los tutoriales de GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Figura 3**: el mapa del sitio incluye ahora entradas para la mejora de los tutoriales de GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Paso 2: Mostrar los proveedores en un control GridView

Para este tutorial permite s crear un control GridView que muestra los proveedores de Estados Unidos, con cada fila GridView que proporciona un botón de radio. Después de seleccionar un proveedor mediante el botón de radio, el usuario puede ver los productos de s del proveedor, haga clic en un botón. Aunque esta tarea puede parecer trivial, hay una serie de sutilezas que facilitan especialmente difícil. Antes de adentrarnos en estos matices, permiten s primero obtener un listado de los proveedores de GridView.

Comience abriendo la `RadioButtonField.aspx` página en el `EnhancedGridView` carpeta arrastrando un control GridView del cuadro de herramientas hasta el diseñador. Establezca la s GridView `ID` a `Suppliers` y, en la etiqueta inteligente, de optar por crear un nuevo origen de datos. En concreto, cree un origen ObjectDataSource denominado `SuppliersDataSource` que extrae sus datos de la `SuppliersBLL` objeto.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Figura 4**: crear un nuevo origen ObjectDataSource denominado `SuppliersDataSource` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Configurar el origen ObjectDataSource para usar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Figura 5**: configurar el origen ObjectDataSource que se usarán el `SuppliersBLL` clase ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Puesto que sólo deseamos enumerar los proveedores de Estados Unidos, elija el `GetSuppliersByCountry(country)` método en la lista desplegable en la ficha Seleccionar.


[![Configurar el origen ObjectDataSource para usar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Figura 6**: configurar el origen ObjectDataSource que se usarán el `SuppliersBLL` clase ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


En la pestaña actualizaciones, seleccione el (ninguno) opción y haga clic en siguiente.


[![Configurar el origen ObjectDataSource para usar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Figura 7**: configurar el origen ObjectDataSource que se usarán el `SuppliersBLL` clase ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Puesto que el `GetSuppliersByCountry(country)` método acepta un parámetro, el Asistente para configurar orígenes de datos nos pide el origen de dicho parámetro. Para especificar un valor codificado (EE. UU., en este ejemplo), deje el parámetro de lista desplegable de origen se establece en None y escriba el valor predeterminado en el cuadro de texto. Haga clic en Finalizar para completar al asistente.


[![Usar Estados Unidos como el valor predeterminado para el parámetro de país](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Figura 8**: Estados Unidos de uso como el valor predeterminado para el `country` parámetro ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Después de completar al asistente, el control GridView incluirá un BoundField para cada uno de los campos de datos del proveedor. Quite todos menos a los `CompanyName`, `City`, y `Country` BoundFields y cambiar el nombre de la `CompanyName` BoundFields `HeaderText` propiedad al proveedor. Una vez hecho esto, la sintaxis declarativa del control GridView y ObjectDataSource debe ser similar al siguiente.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Para este tutorial, permiten que los productos de s al usuario ver el proveedor seleccionado en la misma página que la lista de proveedores, o en una página distinta de s. Para dar cabida a esto, agregue dos controles de botón Web a la página. Puedo guardar conjunto el `ID` s de estos dos botones a `ListProducts` y `SendToProducts`, con la idea de que, cuando `ListProducts` se hace clic en se producirá una devolución de datos y los productos del proveedor seleccionado s se mostrarán en la misma página, pero, cuando `SendToProducts` se hace clic en, el usuario se obtener acceso a una página de otra que enumera los productos.

Figura 9 se muestra el `Suppliers` GridView y la Web botón dos controles cuando se ve mediante un explorador.


[![Los proveedores de Estados Unidos tienen su nombre, ciudad y muestran información del país](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Figura 9**: los proveedores de Estados Unidos tienen su nombre, ciudad y país en la lista información ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Paso 3: Agregar una columna de botones de Radio

En este momento el `Suppliers` GridView tiene tres BoundFields mostrando el nombre de la empresa, ciudad y país de cada proveedor de Estados Unidos. Todavía le falta una columna de botones de radio, sin embargo. Por desgracia, t GridView incluyen un integrados RadioButtonField, en caso contrario, se podría agregar simplemente que a la cuadrícula y realizarse. En su lugar, podemos agregar TemplateField y configurar su `ItemTemplate` para representar un botón de radio, lo que resulta en un botón de radio para cada fila de GridView.

Inicialmente, podríamos suponemos que la interfaz de usuario deseado puede implementarse mediante la adición de un control RadioButton Web para el `ItemTemplate` de TemplateField. Aunque esto realmente agregará un botón de radio único a cada fila del control GridView, los botones de radio no se pueden agrupar y, por tanto, no son mutuamente excluyentes. Es decir, un usuario final puede seleccionar varios botones de radio simultáneamente desde el control GridView.

Aunque usar TemplateField de controles RadioButton Web no ofrece la funcionalidad que necesitamos, s permiten implementar este enfoque, tal como se merece la pena examinar por qué no se agrupan los botones de radio resultantes s. Empiece agregando un TemplateField en GridView proveedores, lo que el campo de la izquierda. A continuación, en la etiqueta inteligente de GridView s, haga clic en el vínculo Editar plantillas y arrastre un control RadioButton Web desde el cuadro de herramientas a la s TemplateField `ItemTemplate` (consulte la figura 10). Establecer la s RadioButton `ID` propiedad `RowSelector` y el `GroupName` propiedad `SuppliersGroup`.


[![Agregar un Control RadioButton Web a la plantilla ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Figura 10**: agregar un Control RadioButton Web a la `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Después de realizar estas adiciones a través del diseñador, el código de marcado de GridView s debe ser similar al siguiente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

Las operaciones de asignación RadioButton [ `GroupName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) es lo que se usa para agrupar una serie de botones de radio. Todos los controles RadioButton con el mismo `GroupName` valor se consideran agrupar; se puede seleccionar sólo un botón de opción de un grupo a la vez. El `GroupName` propiedad especifica el valor para el botón de radio representado s `name` atributo. El explorador examina los botones de radio `name` agrupaciones de botón de atributos para determinar la radio.

Con el control RadioButton Web agregado a la `ItemTemplate`, visite esta página a través de un explorador y haga clic en los botones de radio en las filas de cuadrícula s. Observe cómo los botones de radio no están agrupados, lo que permite seleccionar todas las filas, como la figura 11 se muestra.


[![Los botones de Radio de GridView s no son agrupados](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Figura 11**: botones de Radio de GridView s no son agrupados ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


La razón no se agrupan los botones de opción es que sus representado `name` atributos son diferentes, a pesar de tener el mismo `GroupName` configuración de la propiedad. Para ver estas diferencias, realice un origen de la vista del explorador y examinar el marcado del botón de radio:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Tenga en cuenta tanto la `name` y `id` atributos no son los valores exactos, tal como se especifica en la ventana Propiedades, pero van precedidas de un número de otros `ID` valores. El adicionales `ID` valores agregados a la parte delantera de la representado `id` y `name` atributos son la `ID` s de la radio botones controles primarios el `GridViewRow` s `ID` s, la s de GridView `ID`, el S de control de contenido `ID`y la s de formulario Web Forms `ID`. Estos `ID` s se agregan para que cada uno representado tiene un único control Web en el control GridView `id` y `name` valores.

Representan una diferentes necesidades de control `name` y `id` porque se trata cómo el explorador identifica cada control en el lado cliente y cómo identifica en el servidor web qué acción o se ha producido el cambio en el postback. Por ejemplo, imagine que deseamos ejecutar algún código del lado servidor cada vez que una s RadioButton activado se cambió el estado. Podríamos lograr esto estableciendo la s RadioButton `AutoPostBack` propiedad `True` y la creación de un controlador de eventos para el `CheckChanged` eventos. Sin embargo, si el texto representado `name` y `id` los valores de todos los botones de radio eran iguales, en de devolución de datos no se pudo determinar qué específico se hizo clic en el botón de opción.

El resumen, es que no podemos crear una columna de botones de radio en un control GridView que usa el control RadioButton Web. En su lugar, debemos usar bastante arcaicos técnicas para asegurarse de que el formato adecuado se inserta en cada fila GridView.

> [!NOTE]
> Como el control RadioButton Web, el botón de radio HTML control, cuando se agrega a una plantilla, incluirá el único `name` atributo, hacer que los botones de radio en la cuadrícula no agrupada. Si no está familiarizado con los controles HTML, puede pasar por alto esta nota, ya que rara vez se usan controles HTML, especialmente en ASP.NET 2.0. Pero si está interesado en obtener más información, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) entrada de blog de s [controles Web y controles HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Uso de un Control Literal para insertar el marcado del botón de Radio

Con el fin de agrupar correctamente todos los botones de radio dentro de GridView, necesitamos inyectar manualmente el marcado de los botones de radio en el `ItemTemplate`. Cada botón de radio tiene el mismo `name` de atributo, pero debe tener un único `id` atributo (en caso de que desea tener acceso a un botón de radio a través de script de cliente). Después de que un usuario selecciona un botón de radio y publicaciones de realizar una copia de la página, el explorador enviará de vuelta el valor del botón de radio seleccionado s `value` atributo. Por lo tanto, habrá un único cada botón de radio `value` atributo. Por último, en el postback se debe asegurarse de agregar el `checked` atributo para el botón de radio que está seleccionado, en caso contrario, una vez que el usuario realiza una selección y entradas de vuelta, los botones de radio volverá a su estado predeterminado (sin seleccionar todo).

Existen dos enfoques que se pueden realizar con el fin de insertar el marcado de bajo nivel en una plantilla. Uno es realizar una combinación de marcado y las llamadas a métodos definidos en la clase de código subyacente de formato. Esta técnica en primer lugar se ha explicado en la [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial. En nuestro caso podría ser similar:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

En este caso, `GetUniqueRadioButton` y `GetRadioButtonValue` sería métodos definidos en la clase de código subyacente que devuelve el texto adecuado `id` y `value` valores para cada botón de radio del atributo. Este enfoque funciona bien para asignar el `id` y `value` atributos, pero no es suficiente cuando es necesario especificar el `checked` valor de atributo porque la sintaxis de enlace de datos solo se ejecuta cuando primero se enlazan datos a la GridView. Por lo tanto, si el control GridView tiene habilitado el estado de vista, los métodos de formato activarán únicamente cuando se carga la página por primera vez (o cuando el control GridView se explícitamente vuelve a enlazar al origen de datos) y, por tanto, la función que establece la `checked` atributo ganado t se llama en devolución de datos. Lo s un problema bastante sutil y un poco más allá del ámbito de este artículo, así que dejaré en esto. Hacer, sin embargo, recomendamos que intente usar el enfoque anterior y trabajar a través en el punto donde se le bloquea. Aunque este tipo t de ejercicio ganó un obtener acercarme más a una versión, le ayudará a fomentar una comprensión más profunda de GridView y el ciclo de vida de enlace de datos.

Otro enfoque para insertar personalizado, marcado de bajo nivel en una plantilla y el enfoque que vamos a usar para este tutorial consiste en Agregar un [control Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) a la plantilla. A continuación, en la s GridView `RowCreated` o `RowDataBound` controlador de eventos, el control Literal puede tener acceso mediante programación y su `Text` propiedad establecida en el marcado para emitir.

Empiece quitando el botón de opción de s TemplateField `ItemTemplate`, reemplazándolo por un control Literal. Establecer el control Literal s `ID` a `RadioButtonMarkup`.


[![Agregar un Control Literal a la plantilla ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Figura 12**: agregar un Control Literal para el `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


A continuación, cree un controlador de eventos para el s GridView `RowCreated` eventos. El `RowCreated` evento se desencadena una vez por cada fila agregada, si los datos se que se vuelve a enlazar a GridView. Esto significa que incluso en una devolución de datos cuando se vuelve a cargar los datos del estado de vista, el `RowCreated` aún se activa el evento y esta es la razón que usamos en lugar de `RowDataBound` (que activa únicamente cuando los datos explícitamente se enlazan a los datos de control Web).

En este controlador de eventos, sólo vamos a continuar si se re tratar con una fila de datos. Para cada fila de datos que queremos hacer referencia mediante programación el `RadioButtonMarkup` control Literal y establezca su `Text` propiedad en el marcado para emitir. Como se muestra en el código siguiente, el marcado que genera crea una radio de botón cuya propiedad `name` atributo está establecido en `SuppliersGroup`, cuya `id` atributo está establecido en `RowSelectorX`, donde *X* es el índice de la fila GridView, y cuyo `value` atributo está establecido en el índice de la fila GridView.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Cuando se selecciona una fila GridView y se produce un postback, estamos interesados en la `SupplierID` del proveedor seleccionado. Por lo tanto, se podría pensar que el valor de cada botón de radio debe ser real `SupplierID` (en lugar de índice de la fila GridView). Aunque esto podría funcionar en determinadas circunstancias, sería un riesgo de seguridad a ciegas aceptará y procesará un `SupplierID`. Nuestro GridView, por ejemplo, enumera solo esos proveedores en Estados Unidos. ¿Sin embargo, si la `SupplierID` se pasa directamente en el botón de radio, novedades para impedir que un usuario travieso manipular el `SupplierID` valor enviado en el postback? Con el índice de fila como el `value`y, a continuación, obtener el `SupplierID` en el postback de la `DataKeys` colección, es posible garantizar que el usuario está usando solo uno de la `SupplierID` valores asociados a una de las filas de GridView.

Después de agregar este código de controlador de eventos, tómese un minuto para probar la página en un explorador. En primer lugar, tenga en cuenta esa opción solo se puede seleccionar el botón en la cuadrícula a la vez. Sin embargo, al seleccionar un botón de radio y haga clic en uno de los botones, se produce un postback y todos los botones de radio se vuelve a su estado inicial (es decir, en el postback, botón de radio seleccionado está ya no seleccionado). Para solucionar este problema, es necesario aumentar la `RowCreated` controlador de eventos para que inspecciona el índice del botón de radio seleccionado enviado desde la devolución de datos y agrega el `checked="checked"` atributos al marcado emitido el índice de coincidencias de filas.

Cuando se produce un postback, el explorador envía de vuelta el `name` y `value` del botón de radio seleccionado. El valor puede recuperarse mediante programación utilizando `Request.Form("name")`. El [ `Request.Form` propiedad](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) proporciona un [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) que representa las variables de formulario. Las variables de formulario son los nombres y valores de los campos de formulario en la página web y se envían por el explorador web, siempre que sea una devolución de datos que habrá trastornos. Dado que el representado `name` es el atributo de los botones de radio en el control GridView `SuppliersGroup`, cuando la página web se envíe de nuevo el explorador enviará `SuppliersGroup=valueOfSelectedRadioButton` al servidor web (junto con los demás campos de formulario). Esta información, a continuación, se puede acceder desde el `Request.Form` utilizando la propiedad: `Request.Form("SuppliersGroup")`.

Desde que creamos deberá necesita para determinar el botón de radio seleccionado indexar no solo en el `RowCreated` controlador de eventos, pero en el `Click` permiten s de controladores de eventos para los controles de botón Web, agregue un `SuppliersSelectedIndex` propiedad a la clase de código subyacente que devuelve `-1`si se ha seleccionado ningún botón de opción y el índice seleccionado si uno de los botones de radio está seleccionado.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Con esta propiedad se agrega, sabemos que para agregar el `checked="checked"` marcado en el `RowCreated` controlador de eventos cuando `SuppliersSelectedIndex` es igual a `e.Row.RowIndex`. Actualice el controlador de eventos para incluir esta lógica:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Con este cambio, el botón de radio seleccionado permanece seleccionado después de un postback. Ahora que tenemos la capacidad de especificar qué botón de radio está seleccionado, podríamos cambiar el comportamiento para que cuando primero se ha visitado la página, se ha seleccionado el primer botón de radio de la fila s GridView (en lugar de no tener ningún botón de radio seleccionado de forma predeterminada, que es actual comportamiento). Para que el primer botón de radio seleccionado de forma predeterminada, basta con cambiar la `If SuppliersSelectedIndex = e.Row.RowIndex Then` instrucción a la siguiente: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

En este punto hemos agregado una columna de botones de radio agrupados en el control GridView que permite que una sola fila GridView se seleccionen y recuerdan entre devoluciones de datos. Nuestros próximos pasos son para mostrar los productos proporcionados por el proveedor seleccionado. En el paso 4 veremos cómo redirigir al usuario a otra página, enviar a lo largo de los seleccionados `SupplierID`. En el paso 5, veremos cómo se muestran los productos de s del proveedor seleccionado en un control GridView en la misma página.

> [!NOTE]
> En lugar de usar TemplateField (el enfoque de este paso 3 largas), podríamos crear una personalizada `DataControlField` clase que representa la interfaz de usuario adecuada y funcionalidad. El [ `DataControlField` clase](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) es la clase base desde la que derivan el BoundField, CampoCasillaVerificación, TemplateField y otros campos de GridView y DetailsView integrados. Crear un personalizado `DataControlField` clase significaría que la columna de botones de radio podría agregarse simplemente mediante la sintaxis declarativa y también haría que replican la funcionalidad en otras páginas web y otras aplicaciones web mucho más fáciles.


Si ha creado jamás personalizado, compilado los controles de ASP.NET, sin embargo, sabrá que al hacerlo así que requiere una gran cantidad de tareas y conlleva un sinfín de matices y casos extremos que deben controlarse cuidadosamente. Por lo tanto, se renunciará al implementar una columna de botones de radio a un personalizado `DataControlField` clase por ahora y seguir con la opción TemplateField. Quizás se tendrá la oportunidad de explorar la creación, uso e implementación de custom `DataControlField` clases en un futuro tutorial!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Paso 4: Mostrar los productos de s del proveedor seleccionado en una página independiente

Después de que el usuario ha seleccionado una fila GridView, tenemos que mostrar productos s de proveedor seleccionado. En algunas circunstancias, podemos queremos mostrar estos productos en una página independiente, en otros que preferimos podríamos hacerlo en la misma página. Permiten s examinar primero cómo mostrar los productos en una página independiente; en el paso 5, echaremos un vistazo a la adición de un control GridView a `RadioButtonField.aspx` para mostrar los productos s del proveedor seleccionado.

Actualmente hay dos controles Web de botón en la página `ListProducts` y `SendToProducts`. Cuando el `SendToProducts` se hace clic en el botón, queremos enviar al usuario `~/Filtering/ProductsForSupplierDetails.aspx`. Esta página se ha creado en el [filtrado de maestro y detalles en dos páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial y muestra los productos para el proveedor cuyo `SupplierID` se pasa a través del campo de cadena de consulta denominado `SupplierID`.

Para proporcionar esta funcionalidad, cree un controlador de eventos para el `SendToProducts` botón s `Click` eventos. En el paso 3 se ha agregado el `SuppliersSelectedIndex` propiedad, que devuelve el índice de la fila cuyo botón de radio está seleccionado. El correspondiente `SupplierID` pueden obtenerse de la s GridView `DataKeys` recopilación y el usuario, a continuación, se pueden enviar a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` mediante `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Este código funciona maravillosamente siempre y cuando uno de los botones de radio está seleccionado en el control GridView. Si, inicialmente, el control GridView no tiene ningún botón de radio seleccionado y el usuario hace clic en el `SendToProducts` botón, `SuppliersSelectedIndex` será `-1`, lo que provocará una excepción que se produzca desde `-1` está fuera del intervalo del índice de la `DataKeys`colección. Esto es no es un problema, sin embargo, si decide actualizar el `RowCreated` controlador de eventos, como se describe en el paso 3 con el fin de tener el primer botón de radio en el control GridView seleccionado inicialmente.

Para dar cabida a un `SuppliersSelectedIndex` valor `-1`, agregue un control Web de la etiqueta a la página por encima del control GridView. Establecer su `ID` propiedad `ChooseSupplierMsg`, sus `CssClass` propiedad `Warning`, sus `EnableViewState` y `Visible` propiedades para `False`y su `Text` propiedad por favor, elija un proveedor de la cuadrícula. La clase CSS `Warning` muestra texto en una fuente roja, cursiva, negrita, grande y se define en `Styles.css`. Estableciendo el `EnableViewState` y `Visible` propiedades a `False`, no se procesa la etiqueta excepto para solo las devoluciones de datos donde el control s `Visible` propiedad se establece mediante programación en `True`.


[![Agregar un Control Web de etiqueta por encima del control GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Figura 13**: agregar una etiqueta Web Control anteriormente el control GridView ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


A continuación, aumentar la `Click` controlador de eventos para mostrar el `ChooseSupplierMsg` etiqueta si `SuppliersSelectedIndex` es menor que cero y redirigir al usuario al `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` en caso contrario.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Visite la página en un explorador y haga clic en el `SendToProducts` botón antes de seleccionar un proveedor en el control GridView. Como se muestra en la figura 14, se muestra la `ChooseSupplierMsg` etiqueta. A continuación, seleccione un proveedor y haga clic en el `SendToProducts` botón. Esto captar a una página que se enumera los productos suministrados por el proveedor seleccionado. Figura 15 muestra la `ProductsForSupplierDetails.aspx` página cuando se selecciona el proveedor con Piesgrandes fábricas.


[![La etiqueta ChooseSupplierMsg se muestra si se selecciona un proveedor de n](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Figura 14**: el `ChooseSupplierMsg` etiqueta se muestra si se selecciona un proveedor de n ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Los productos de s del proveedor seleccionado se muestran en ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Figura 15**: el proveedor seleccionado s productos se muestran en `ProductsForSupplierDetails.aspx` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Paso 5: Mostrar los productos de s del proveedor seleccionado en la misma página

En el paso 4, hemos visto cómo enviar al usuario a otra página web para mostrar el proveedor seleccionado productos s. Como alternativa, se pueden mostrar los productos de s del proveedor seleccionado en la misma página. Para ilustrar esto, vamos a agregar otro control GridView a `RadioButtonField.aspx` para mostrar los productos s del proveedor seleccionado.

Puesto que sólo deseamos que este control GridView de productos para mostrar una vez que se ha seleccionado un proveedor, agregar un control de Panel Web bajo el `Suppliers` GridView, establecer su `ID` a `ProductsBySupplierPanel` y su `Visible` propiedad `False`. Dentro del Panel, agregue el texto productos para el proveedor seleccionado, seguido por un control GridView denominado `ProductsBySupplier`. En la etiqueta inteligente s GridView, elija para enlazarla a un nuevo origen ObjectDataSource denominado `ProductsBySupplierDataSource`.


[![Enlazar el control ProductsBySupplier GridView a un nuevo origen ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Figura 16**: enlazar el `ProductsBySupplier` GridView para un nuevo origen ObjectDataSource ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


A continuación, configure el origen ObjectDataSource para usar el `ProductsBLL` clase. Puesto que sólo deseamos recuperar esos productos suministrados por el proveedor seleccionado, especifique que el ObjectDataSource debería invocar el `GetProductsBySupplierID(supplierID)` método para recuperar sus datos. Seleccione (ninguno) en las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas.


[![Configurar el origen ObjectDataSource para usar el método GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Figura 17**: configurar el origen ObjectDataSource que se usarán el `GetProductsBySupplierID(supplierID)` método ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Establecer las listas desplegables en (None) en la actualización, INSERCIÓN y eliminar las fichas](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Figura 18**: establecer la lista desplegable se enumeran en (None) en la actualización, INSERCIÓN y eliminar pestañas ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Después de configurar el seleccionar, actualizar, insertar y eliminar las fichas, haga clic en siguiente. Puesto que el `GetProductsBySupplierID(supplierID)` método espera un parámetro de entrada, el Asistente para crear orígenes de datos nos pedirá que especifique el origen para el valor del parámetro.

Tenemos un par de opciones que aquí se especifican en el origen del que el valor del parámetro. Podríamos usar el objeto de parámetro predeterminado y asigne mediante programación el valor de la `SuppliersSelectedIndex` propiedad para el parámetro s `DefaultValue` propiedad en la s ObjectDataSource `Selecting` controlador de eventos. Vuelva a consultar el [configurar valores de parámetro de ObjectDataSource mediante programación](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) tutorial para hacer un repaso sobre la asignación mediante programación los valores para los parámetros de ObjectDataSource s.

Como alternativa, podemos usar un ControlParameter y hacer referencia a la `Suppliers` GridView s [ `SelectedValue` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (consulte la figura 19). Las operaciones de asignación GridView `SelectedValue` propiedad devuelve el `DataKey` valor que corresponde a la [ `SelectedIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). En orden para esta opción funcione, es necesario establecer mediante programación la s GridView `SelectedIndex` propiedad a la fila cuando la `ListProducts` se hace clic en el botón. Como ventaja adicional, estableciendo el `SelectedIndex`, vaya a realizar el registro seleccionado en el `SelectedRowStyle` definido en el `DataWebControls` tema (un fondo amarillo).


[![Use un ControlParameter para especificar el elemento SelectedValue s de GridView como origen del parámetro](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Figura 19**: Use un ControlParameter para especificar la s GridView SelectedValue como origen del parámetro ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Al finalizar al asistente, Visual Studio agregará automáticamente los campos para los campos de datos de producto s. Quite todos menos a los `ProductName`, `CategoryName`, y `UnitPrice` BoundFields y cambie el `HeaderText` propiedades precio, categoría y producto. Configurar la `UnitPrice` BoundField para que su valor tiene el formato como una moneda. Después de realizar estos cambios, el marcado declarativo de s Panel GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Para completar este ejercicio, es necesario establecer la s GridView `SelectedIndex` propiedad a la `SelectedSuppliersIndex` y el `ProductsBySupplierPanel` Panel s `Visible` propiedad `True` cuando el `ListProducts` se hace clic en el botón. Para ello, cree un controlador de eventos para el `ListProducts` control de botón Web s `Click` eventos y agregue el código siguiente:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Si no se ha seleccionado un proveedor de GridView, el `ChooseSupplierMsg` se muestra la etiqueta y el `ProductsBySupplierPanel` ocultado el Panel. En caso contrario, si se ha seleccionado un proveedor, el `ProductsBySupplierPanel` se muestra y la s GridView `SelectedIndex` se actualiza la propiedad.

Figura 20 muestra los resultados cuando se ha seleccionado el proveedor con Piesgrandes fábricas y los productos que se muestra en el botón de página se ha hecho clic.


[![Los productos proporcionados por fábricas con Piesgrandes aparecen en la misma página](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Figura 20**: el productos suministrados por fábricas con Piesgrandes aparecen en la misma página ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Resumen

Como se describe en el [Master/Detail mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial, se pueden seleccionar los registros de un GridView que usa un CommandField cuyo `ShowSelectButton` propiedad está establecida en `True`. Pero la CommandField muestra los botones como botones normales de inserción, vínculos o imágenes. Una interfaz de usuario de selección de fila alternativa es proporcionar un botón de radio o casilla de verificación de cada fila GridView. En este tutorial hemos visto cómo agregar una columna de botones de radio.

Por desgracia, agregar una columna de radio botones t como directa ni sencilla como cabría esperar. No hay ningún RadioButtonField integrado que se puede agregar con el clic de un botón y uso del control RadioButton Web en TemplateField presenta su propio conjunto de problemas. Al final, para proporcionar esta interfaz tenemos que crear un personalizado `DataControlField` clase ni recurrir a insertar el código HTML adecuado en TemplateField durante el `RowCreated` eventos.

Tener exploramos cómo agregar una columna de botones de radio, nos permiten prestar atención a la adición de una columna de casillas de verificación. Con una columna de casillas de verificación, un usuario puede seleccionar una o varias filas de GridView y, a continuación, realizar alguna operación en todas las filas seleccionadas (por ejemplo, seleccionar un conjunto de mensajes de correo electrónico desde un cliente de correo electrónico basado en web y, a continuación, seleccionando eliminar todos los correos electrónicos seleccionados). En el siguiente tutorial veremos cómo agregar este tipo de columna.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era David Suru. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Siguiente](adding-a-gridview-column-of-checkboxes-vb.md)
