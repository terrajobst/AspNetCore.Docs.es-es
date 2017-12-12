---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Agregar una columna de GridView de botones de Radio (C#) | Documentos de Microsoft
author: rick-anderson
description: "Este tutorial examina cómo agregar una columna de botones de radio a un control GridView para ofrecer al usuario una manera más intuitiva de la selección de una sola fila de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: ba6b163078bbab1bda302676e7e4c8a1d07f3c98
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Agregar una columna de GridView de botones de Radio (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) o [descarga de PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> Este tutorial examina cómo agregar una columna de botones de radio a un control GridView para ofrecer al usuario una manera más intuitiva de la selección de una sola fila de GridView.


## <a name="introduction"></a>Introducción

El control GridView ofrece una gran cantidad de funciones integradas. Incluye un número de campos diferentes para mostrar el texto, imágenes, hipervínculos y botones. Admite plantillas para aumentar la personalización. Con unos pocos clics del mouse, lo s posible para realizar un GridView donde cada fila se puede seleccionar a través de un botón, o para habilitar la edición o eliminación de recursos. A pesar de la gran cantidad de características proporcionados, con frecuencia habrá situaciones en la que adicionales, características no admitidas deberá agregarse. En este tutorial y los dos siguientes examinaremos cómo mejorar la funcionalidad de s GridView para incluir características adicionales.

Este tutorial y siguiente se centran en mejorar el proceso de selección de fila. Tal y como se examina en el [principal/detalle mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), podemos agregar una CommandField a GridView que incluye un botón de selección. Al hacer clic, tiene lugar una devolución de datos y las operaciones de asignación GridView `SelectedIndex` propiedad se actualiza con el índice de la fila cuyo botón Seleccionar se hizo clic. En el [principal/detalle mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial, hemos visto cómo utilizar esta característica para ver detalles sobre la fila seleccionada de GridView.

Mientras que el botón de selección funciona en muchas situaciones, puede que no funcione también para otras personas. En lugar de usar un botón, normalmente se usan dos otros elementos de la interfaz de usuario para la selección: el botón de radio y casilla de verificación. Podemos aumentamos GridView para que en lugar de un botón de selección, cada fila contiene un botón de radio o una casilla de verificación. En escenarios donde el usuario solo puede seleccionar uno de los registros de GridView, el botón de radio podría estar preferido sobre el botón de selección. En situaciones donde el usuario potencialmente puede seleccionar varios registros, como en una aplicación de correo electrónico basado en web, donde un usuario podría desear selecciona varios mensajes para eliminar la casilla de verificación ofrece una funcionalidad que no está disponible en el botón de selección o el botón de radio interfaces de usuario.

Este tutorial examina cómo agregar una columna de botones de radio a la GridView. El tutorial continuar explora el uso de las casillas de verificación.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Paso 1: Crear la mejora de las páginas Web de GridView

Antes de empezar a mejorar el control GridView para incluir una columna de botones de radio, permiten s primero Tómese un momento para crear las páginas ASP.NET en el proyecto de sitio Web que se necesitará para este tutorial y los dos siguientes. Empiece por agregar una nueva carpeta denominada `EnhancedGridView`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de que asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource


Al igual que en las otras carpetas, `Default.aspx` en el `EnhancedGridView` carpeta enumerará los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página s la vista Diseño.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


Por último, agregue estos cuatro páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de utilizar el SqlDataSource Control `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para modificar, insertar y eliminar los tutoriales.


![El mapa del sitio ahora incluye las entradas para la mejora de los tutoriales de GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Figura 3**: la asignación de sitio ahora incluye las entradas para la mejora de los tutoriales de GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Paso 2: Mostrar los proveedores en un control GridView

Para este tutorial permite s crear un control GridView que muestra los proveedores de Estados Unidos, con cada fila de GridView proporcionar un botón de radio. Después de seleccionar un proveedor mediante el botón de radio, el usuario puede ver los productos de s proveedor haciendo clic en un botón. Aunque esta tarea puede parecer trivial, hay un número de matices que hacen que sea especialmente difícil. Antes de adentrarnos en estos matices, permiten s primero hay que obtener una lista de los proveedores de GridView.

Comience abriendo la `RadioButtonField.aspx` página en el `EnhancedGridView` carpeta, arrastre un control GridView en el cuadro de herramientas hasta el diseñador. Establecer la s GridView `ID` a `Suppliers` y, en su etiqueta inteligente, decide crear un nuevo origen de datos. En concreto, cree un ObjectDataSource denominado `SuppliersDataSource` que extrae los datos de la `SuppliersBLL` objeto.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Figura 4**: crear un nuevo ObjectDataSource denominado `SuppliersDataSource` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Configurar el ObjectDataSource para utilizar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Figura 5**: configurar el ObjectDataSource que se utilizan los `SuppliersBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Puesto que deseamos solo enumerar los proveedores de Estados Unidos, elija la `GetSuppliersByCountry(country)` método en la lista desplegable de la ficha Seleccionar.


[![Configurar el ObjectDataSource para utilizar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Figura 6**: configurar el ObjectDataSource que se utilizan los `SuppliersBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


En la ficha actualización, seleccione el (ninguno) opción y haga clic en siguiente.


[![Configurar el ObjectDataSource para utilizar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Figura 7**: configurar el ObjectDataSource que se utilizan los `SuppliersBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Puesto que la `GetSuppliersByCountry(country)` método acepta un parámetro, el Asistente para configurar orígenes de datos le pide a nosotros para el origen de ese parámetro. Para especificar un valor codificado (Estados Unidos, en este ejemplo), deje el parámetro de lista desplegable de origen establecida en None y escriba el valor predeterminado en el cuadro de texto. Haga clic en Finalizar para completar al asistente.


[![Usar Estados Unidos como el valor predeterminado para el parámetro de país](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Figura 8**: Estados Unidos de uso como el valor predeterminado para la `country` parámetro ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


Después de completar al asistente, GridView incluirá un BoundField para cada uno de los campos de datos de proveedor. Quitar todos menos el `CompanyName`, `City`, y `Country` BoundFields y cambiar el nombre de la `CompanyName` BoundFields `HeaderText` propiedad de proveedor. Una vez hecho esto, la sintaxis declarativa GridView y ObjectDataSource debe ser similar al siguiente.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Para este tutorial, permiten s permiten al usuario ver el proveedor seleccionado productos s en la misma página que la lista de proveedores, o en una página diferente. Para ello, agregue dos controles de botón Web a la página. ¿Ve conjunto el `ID` s de estos dos botones a `ListProducts` y `SendToProducts`, con la idea que, cuando `ListProducts` se hace clic en se producirá una devolución de datos y los productos de proveedor seleccionado s se mostrarán en la misma página, pero, cuando `SendToProducts` se hace clic en, el usuario se obtener acceso a otra página que enumera los productos.

Ilustración 9 se muestra la `Suppliers` GridView y la Web de botón dos controles cuando se ven a través de un explorador.


[![Los proveedores de Estados Unidos tienen su nombre, la ciudad y la información de país que se muestra](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Figura 9**: los proveedores de Estados Unidos tienen su nombre, ciudad y país en la lista información ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Paso 3: Agregar una columna de botones de Radio

En este momento la `Suppliers` GridView tiene tres BoundFields mostrar el nombre de la compañía, la ciudad y el país de cada proveedor de Estados Unidos. Todavía le falta una columna de botones de radio, sin embargo. Desafortunadamente, t GridView incluyen un integrados RadioButtonField, en caso contrario se podría agregar que a la cuadrícula y realizarse. En su lugar, podemos agregar TemplateField y configurar su `ItemTemplate` para presentar un botón de radio, lo que da lugar a un botón de radio para cada fila de GridView.

Inicialmente, tengamos que suponemos que la interfaz de usuario deseado se puede implementar mediante la adición de un control RadioButton Web para el `ItemTemplate` de TemplateField. Mientras que esto realmente agregará un botón de radio único a cada fila del control GridView, los botones de opción no se pueden agrupar y, por tanto, no son mutuamente excluyentes. Es decir, un usuario final puedo seleccionar simultáneamente varios botones de radio de GridView.

Aunque utilizando TemplateField de controles RadioButton Web no ofrece la funcionalidad que necesitamos, permiten s implementar este enfoque, tal y como s merece la pena examinar por qué no se agrupan los botones de radio resultantes. Empiece agregando un TemplateField a GridView proveedores, lo que el campo más a la izquierda. A continuación, desde la etiqueta inteligente de GridView s, haga clic en el vínculo Editar plantillas y arrastre un control RadioButton Web desde el cuadro de herramientas en las operaciones de asignación TemplateField `ItemTemplate` (consulte la figura 10). Establecer la s RadioButton `ID` propiedad `RowSelector` y `GroupName` propiedad a `SuppliersGroup`.


[![Agregar un Control RadioButton Web a la plantilla ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Figura 10**: agregar un Control RadioButton Web a la `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


Después de realizar estas adiciones a través del diseñador, el código de marcado de GridView s debe ser similar al siguiente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

Las operaciones de asignación RadioButton [ `GroupName` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) es lo que se usa para agrupar una serie de botones de radio. Todos los controles RadioButton con el mismo `GroupName` se consideran agrupar; botón de solo radio puede seleccionarse de un grupo a la vez. El `GroupName` propiedad especifica el valor para el botón de radio representado s `name` atributo. El explorador examina los botones de opción `name` agrupaciones de botón de atributos para determinar la radio.

Con el control RadioButton Web agregado a la `ItemTemplate`, visite esta página a través de un explorador y haga clic en los botones de radio en filas de la cuadrícula s. Observe cómo los botones de radio no están agrupados, lo que permite seleccionar todas las filas, como la figura 11 muestra.


[![Los botones de opción de GridView s no son agrupados](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Figura 11**: botones de Radio de GridView la s no son agrupados ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


La razón no se agrupan los botones de opción es porque su representado `name` atributos son diferentes, a pesar de tener el mismo `GroupName` configuración de la propiedad. Para ver estas diferencias, lo hace con un origen de la vista del explorador y examine el marcado del botón de radio:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Observe cómo la `name` y `id` atributos no son los valores exactos tal como se especifica en la ventana Propiedades, pero van precedidos de un número de otros `ID` valores. La adición `ID` valores agregados a la parte delantera de la representado `id` y `name` atributos son la `ID` s de la radio botones controles primarios el `GridViewRow` s `ID` s, las operaciones de asignación GridView `ID`, el Control s de contenido `ID`y el formulario Web Forms `ID`. Estos `ID` s se agregan para que represente cada uno tiene un único control Web en el control GridView `id` y `name` valores.

Representan otro control necesidades `name` y `id` porque se trata cómo el explorador identifica de forma única cada control en el lado del cliente y cómo identifica al servidor web ¿qué acción o se ha producido el cambio en el postback. Por ejemplo, imagine que desea ejecutar el código del servidor cada vez que una s RadioButton activa se cambió el estado. Podremos hacerlo estableciendo la s RadioButton `AutoPostBack` propiedad `true` y la creación de un controlador de eventos para el `CheckChanged` eventos. Sin embargo, si el representado `name` y `id` los valores de todos los botones de radio fueran los mismos en devolución de datos no se pudo determinar qué específica RadioButton se hizo clic.

La derivación de ella es que no podemos crear una columna de botones de radio en un control GridView que usa el control RadioButton Web. En su lugar, debemos usar en su lugar arcaicos técnicas para asegurarse de que se aplica el formato adecuado en cada fila de GridView.

> [!NOTE]
> Al igual que el control RadioButton Web, el botón de radio HTML control, cuando se agrega a una plantilla, incluirá el único `name` atributo, hacer que los botones de radio en la cuadrícula desagrupada. Si no está familiarizado con los controles HTML, puede pasar por alto esta nota, como controles HTML se usan con poca frecuencia, especialmente en ASP.NET 2.0. Pero si está interesado en aprender más, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) entrada de blog de s [controles Web y controles HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Usar un Control Literal para insertar marcado del botón de Radio

Para agrupar correctamente todos los botones de radio dentro de GridView, debe insertar manualmente el marcado de botones de radio en el `ItemTemplate`. Cada botón de radio tiene el mismo `name` de atributo, pero debe tener un único `id` atributo (en caso de que desea tener acceso a un botón de radio a través de script de cliente). Después de que un usuario selecciona un botón de radio y entradas de copia de la página, el explorador le enviará el valor del botón de radio seleccionado s `value` atributo. Por lo tanto, habrá un único cada botón de radio `value` atributo. Por último, en la devolución de datos es necesario para asegurarse de que al agregar el `checked` atributo para el botón de opción está seleccionado, en caso contrario, una vez que el usuario realiza una selección y publicaciones de nuevo, los botones de opción volverá a su estado predeterminado (todos los no seleccionados).

Existen dos enfoques que se pueden realizar con el fin de insertar marcas de bajo nivel en una plantilla. Uno consiste en una combinación de marcado y las llamadas a métodos definidos en la clase de código subyacente de formato. Esta técnica se describe en primer lugar en la [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial. En nuestro caso podría ser similar:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

En este caso, `GetUniqueRadioButton` y `GetRadioButtonValue` sería métodos definidos en la clase de código subyacente que devuelve la correspondiente `id` y `value` valores para cada botón de radio del atributo. Este enfoque funciona bien para asignar el `id` y `value` atributos, pero no cumple las expectativas cuando es necesario especificar el `checked` valor del atributo porque la sintaxis de enlace de datos solo se ejecuta cuando primero se enlazan datos en GridView. Por lo tanto, si el control GridView tiene habilitado el estado de vista, los métodos de formato sólo activarán cuando se carga la página por primera vez (o cuando el control GridView se explícitamente vuelve a enlazar al origen de datos) y, por tanto, la función que establece la `checked` atributo won t llamará en devolución de datos. Un problema en su lugar sutil s y un poco más allá del ámbito de este artículo, por lo que deberá dejarlo en este. Hacer, sin embargo, recomendamos que intente usar el enfoque anterior y a través de trabajo hasta el punto donde podrá atascar. Aunque este tipo t de un ejercicio won obtener cualquier cuanto más se acerque a una versión de trabajo, le ayudará a fomentar una comprensión más profunda de GridView y el ciclo de vida de enlace de datos.

Otro enfoque para insertar personalizado, marcado de bajo nivel en una plantilla y el enfoque que usaremos para este tutorial consiste en Agregar un [control del Literal](https://msdn.microsoft.com/en-us/library/sz4949ks(VS.80).aspx) a la plantilla. A continuación, en las operaciones de asignación GridView `RowCreated` o `RowDataBound` controlador de eventos, el control del Literal puede tener acceso mediante programación y su `Text` propiedad establecida en el marcado para emitir.

Iniciar quitando RadioButton de las operaciones de asignación TemplateField `ItemTemplate`, reemplazarlo con un control del Literal. Establecer el control del Literal s `ID` a `RadioButtonMarkup`.


[![Agregar un Control Literal a ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Figura 12**: agregar un Control Literal a la `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


A continuación, cree un controlador de eventos para el s GridView `RowCreated` eventos. El `RowCreated` evento se desencadena una vez para cada fila agregada, o no los datos es que se va a enlazar a GridView. Esto significa que incluso en una devolución de datos cuando se vuelve a cargar los datos del estado de vista, la `RowCreated` aún se activa el evento y esta es la razón que usamos en lugar de `RowDataBound` (que activa únicamente cuando los datos explícitamente se enlazan a los datos de control de Web).

En este controlador de eventos, solamente queremos continúe si se vuelve a tratar con una fila de datos. Para cada fila de datos desea hacer referencia mediante programación el `RadioButtonMarkup` control del Literal y establezca su `Text` propiedad en el marcado para emitir. Como se muestra en el código siguiente, el marcado que se emiten crea una radio botón cuya `name` atributo está establecido en `SuppliersGroup`, cuyo `id` atributo está establecido en `RowSelectorX`, donde *X* es el índice de la fila de GridView y cuyo `value` atributo se establece en el índice de la fila de GridView.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Cuando se selecciona una fila de GridView y se produce un postback, nos interesa la `SupplierID` del proveedor seleccionado. Por lo tanto, uno puede pensar que el valor de cada botón de radio debe ser real `SupplierID` (en lugar del índice de la fila de GridView). Aunque esto puede funcionar en determinadas circunstancias, sería un riesgo de seguridad a ciegas acepte y procese un `SupplierID`. Nuestro GridView, por ejemplo, muestra solo los proveedores de Estados Unidos. ¿Sin embargo, si la `SupplierID` se pasa directamente en el botón de radio, novedades para impedir que un usuario perjudiciales manipular el `SupplierID` valor enviado en el postback? Mediante el índice de fila como la `value`y, a continuación, obtener el `SupplierID` en devolución de datos desde el `DataKeys` colección, es posible asegurarse de que el usuario sólo está utilizando uno de la `SupplierID` valores asociados a una de las filas de GridView.

Después de agregar este código de controlador de eventos, tómese un minuto para probar la página en un explorador. En primer lugar, tenga en cuenta que solo un radio puede seleccionarse un botón en la cuadrícula a la vez. Sin embargo, cuando selecciona un botón de radio y haga clic en uno de los botones, se produce un postback y todos los botones de radio vuelve a su estado inicial (es decir, en la devolución de datos, el botón de radio seleccionado es ya no está seleccionado). Para solucionar este problema, es necesario aumentar la `RowCreated` controlador de eventos para que inspecciona el índice del botón de radio seleccionado enviado desde la devolución de datos y agrega los `checked="checked"` de atributo para el marcado emitido el índice de coincidencias de filas.

Cuando se produce un postback, el explorador vuelve a enviar el `name` y `value` del botón de radio seleccionado. El valor puede recuperarse mediante programación utilizando `Request.Form["name"]`. El [ `Request.Form` propiedad](https://msdn.microsoft.com/en-us/library/system.web.httprequest.form.aspx) proporciona un [ `NameValueCollection` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.namevaluecollection.aspx) que representa las variables de formulario. Las variables de formulario son los nombres y valores de los campos de formulario en la página web y se envían por el explorador web cada vez que tiene lugar una devolución de datos. Dado que el representado `name` atributo de los botones de radio en el control GridView es `SuppliersGroup`, cuando la página web se envíe de vuelta el explorador enviará `SuppliersGroup=valueOfSelectedRadioButton` al servidor web (junto con los demás campos de formulario). Esta información puede tener acceso desde el `Request.Form` propiedad mediante: `Request.Form["SuppliersGroup"]`.

Desde que se le necesita para determinar el botón de radio seleccionado de índice no solo en el `RowCreated` controlador de eventos, pero en el `Click` permiten s de controladores de eventos para los controles de botón Web, agregue un `SuppliersSelectedIndex` propiedad a la clase de código subyacente que devuelve `-1`si se ha seleccionado ningún botón de radio y el índice seleccionado si uno de los botones de radio está seleccionado.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Con esta propiedad se agrega, se conoce para agregar el `checked="checked"` marcado en el `RowCreated` controlador de eventos cuando `SuppliersSelectedIndex` es igual a `e.Row.RowIndex`. Actualice el controlador de eventos para incluir esta lógica:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Con este cambio, el botón de radio seleccionado permanece seleccionado después de un postback. Ahora que tenemos la capacidad de especificar qué botón de radio está seleccionado, puede cambiar el comportamiento para que cuando primero se visita la página, se selecciona el primer botón de radio de fila s de GridView (en lugar de no tener ningún botones de opción seleccionada de forma predeterminada, que es el actual comportamiento). Para que el primer botón de radio seleccionado de forma predeterminada, simplemente cambie el `if (SuppliersSelectedIndex == e.Row.RowIndex)` instrucción al siguiente: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

En este punto hemos agregado una columna de botones de radio agrupados en GridView que permite que una sola fila de GridView seleccionado y recuerda a través de las devoluciones de datos. Los próximos pasos son mostrar los productos suministrados por el proveedor seleccionado. En el paso 4 veremos cómo redirigir al usuario a otra página, enviar a lo largo de los seleccionados `SupplierID`. En el paso 5, veremos cómo mostrar los productos de s del proveedor seleccionado en un control GridView en la misma página.

> [!NOTE]
> En lugar de usar TemplateField (el enfoque de este paso 3 largas), podríamos crear una personalizada `DataControlField` clase que representa la funcionalidad y la interfaz de usuario adecuada. El [ `DataControlField` clase](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datacontrolfield.aspx) es la clase base de la que derivan el BoundField, CampoCasillaVerificación, TemplateField y otros campos de GridView y DetailsView integrados. Crear un personalizado `DataControlField` clase significaría que la columna de botones de radio se podría agregar simplemente utilizando la sintaxis declarativa y también haría que replican la funcionalidad en otras páginas web y otras aplicaciones web mucho más fácil.


Si ve alguna vez ha creado personalizado, compila controles en ASP.NET, sin embargo, sabe que al hacerlo por lo que requiere una cantidad considerable de trabajo duro y conlleva una gran cantidad de detalles y casos avanzados que deben controlarse cuidadosamente. Por lo tanto, se renunciará al implementar una columna de botones de radio como un personalizado `DataControlField` clase por ahora y opte por la opción TemplateField. Quizás se tendrá la oportunidad de explorar la creación, uso e implementación personalizada `DataControlField` clases en un futuro tutorial!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Paso 4: Mostrar los productos de s del proveedor seleccionado en una página independiente

Después de que el usuario ha seleccionado una fila de GridView, es necesario mostrar los productos de s de proveedor seleccionado. En algunas circunstancias, podemos queremos mostrar estos productos en una página independiente, en otras que tengamos que prefiere hacer en la misma página. Permiten s primero examinar cómo mostrar los productos en una página independiente; en el paso 5 se examinará agregar un control GridView a `RadioButtonField.aspx` para mostrar los productos del proveedor seleccionado s.

Actualmente, hay dos controles de botón Web en la página `ListProducts` y `SendToProducts`. Cuando el `SendToProducts` se hace clic en el botón, queremos enviar al usuario `~/Filtering/ProductsForSupplierDetails.aspx`. Esta página se ha creado en el [filtrado de maestro y detalles a través de dos páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) tutorial y muestra los productos para el proveedor cuyo `SupplierID` se pasa a través del campo de cadena de consulta denominado `SupplierID`.

Para proporcionar esta funcionalidad, crear un controlador de eventos para el `SendToProducts` botón s `Click` eventos. En el paso 3 se agregó la `SuppliersSelectedIndex` propiedad, que devuelve el índice de la fila cuyo botón de radio está seleccionado. El correspondiente `SupplierID` se pueden recuperar de las operaciones de asignación GridView `DataKeys` colección y el usuario, a continuación, se pueden enviar a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` con `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Este código funciona muy siempre y cuando se selecciona uno de los botones de opción de GridView. Si, en principio, GridView no tiene ningún botón de radio seleccionado, y el usuario hace clic en el `SendToProducts` botón, `SuppliersSelectedIndex` será `-1`, que producirá una excepción que se produzca desde `-1` está fuera del intervalo del índice de la `DataKeys`colección. Esto es no es un problema, sin embargo, si decide actualizar el `RowCreated` controlador de eventos tal como se describe en el paso 3 con el fin de tener el primer botón de radio en GridView seleccionado inicialmente.

Para dar cabida a un `SuppliersSelectedIndex` valo `-1`, agregar un control Web Label a la página situada encima de GridView. Establecer su `ID` propiedad `ChooseSupplierMsg`, sus `CssClass` propiedad `Warning`, sus `EnableViewState` y `Visible` propiedades para `false`y su `Text` propiedad por favor, elija un proveedor de la cuadrícula. La clase CSS `Warning` muestra texto en una fuente de color rojo, cursiva, negrita, grande y se define en `Styles.css`. Estableciendo la `EnableViewState` y `Visible` propiedades `false`, no se representa la etiqueta excepto para solo las devoluciones de datos donde el control s `Visible` propiedad se establece mediante programación en `true`.


[![Agregar un Control Web de etiqueta por encima de GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Figura 13**: agregar un Control de Web a etiqueta anteriormente el GridView ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


A continuación, aumentar la `Click` controlador de eventos para mostrar el `ChooseSupplierMsg` etiqueta si `SuppliersSelectedIndex` es menor que cero y redirigir al usuario a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` en caso contrario.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Visite la página en un explorador y haga clic en el `SendToProducts` botón antes de seleccionar un proveedor de GridView. Como se muestra en la figura 14, esto muestra el `ChooseSupplierMsg` etiqueta. A continuación, seleccione un proveedor y haga clic en el `SendToProducts` botón. Esto captar a una página que contiene los productos suministrados por el proveedor seleccionado. La figura 15 muestra la `ProductsForSupplierDetails.aspx` página cuando se selecciona el proveedor Bigfoot fábricas.


[![La etiqueta de ChooseSupplierMsg se muestra si se selecciona un proveedor de n](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Figura 14**: el `ChooseSupplierMsg` etiqueta se muestra si se selecciona un proveedor de n ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![Los productos de s del proveedor seleccionado se muestran en ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Figura 15**: el proveedor seleccionado s productos se muestran en `ProductsForSupplierDetails.aspx` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Paso 5: Mostrar los productos de s del proveedor seleccionado en la misma página.

En el paso 4, hemos visto cómo enviar al usuario a otra página web para mostrar el proveedor seleccionado productos s. Como alternativa, se pueden mostrar los productos de s del proveedor seleccionado en la misma página. Para ilustrar esto, vamos a agregar otro GridView a `RadioButtonField.aspx` para mostrar los productos del proveedor seleccionado s.

Puesto que deseamos solo este GridView de productos para mostrar una vez que se ha seleccionado un proveedor, agregar un control de Panel Web bajo el `Suppliers` GridView, establecer su `ID` a `ProductsBySupplierPanel` y su `Visible` propiedad `false`. Dentro del Panel, agregue el texto productos para el proveedor seleccionado, seguido por un control GridView denominado `ProductsBySupplier`. En la etiqueta inteligente de GridView s, elija para enlazar a un nuevo ObjectDataSource denominado `ProductsBySupplierDataSource`.


[![Enlazar ProductsBySupplier GridView a un nuevo origen ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Figura 16**: enlazar el `ProductsBySupplier` GridView a un nuevo ObjectDataSource ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


A continuación, configure el ObjectDataSource para usar la `ProductsBLL` clase. Puesto que deseamos solo recuperar los productos suministrados por el proveedor seleccionado, especifique que se debe invocar el ObjectDataSource el `GetProductsBySupplierID(supplierID)` método para recuperar sus datos. Seleccionar (ninguno) en las listas de lista desplegable de la actualización, INSERCIÓN y eliminar las fichas.


[![Configurar el ObjectDataSource para utilizar el método GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Figura 17**: configurar el ObjectDataSource que se utilizan los `GetProductsBySupplierID(supplierID)` método ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Establecer las listas desplegables para (ninguno) en la actualización, INSERCIÓN y eliminar pestañas](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Figura 18**: establecer las listas desplegables para (ninguno) en la actualización, INSERCIÓN y eliminar pestañas ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


Después de configurar la instrucción SELECT, UPDATE, INSERT y eliminar pestañas, haga clic en siguiente. Puesto que la `GetProductsBySupplierID(supplierID)` método espera un parámetro de entrada, el Asistente para crear orígenes de datos le pide que especifique el origen para el valor del parámetro.

Tenemos un par de opciones que aquí se especifican en el origen del valor del parámetro. Podríamos usar el objeto de parámetro predeterminado y asignar mediante programación el valor de la `SuppliersSelectedIndex` propiedad para el parámetro s `DefaultValue` propiedad en las operaciones de asignación ObjectDataSource `Selecting` controlador de eventos. Hacen referencia a la [establecer mediante programación los valores de parámetro de ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) tutorial para repasar los conocimientos sobre mediante programación asignar valores a los parámetros de s ObjectDataSource.

Como alternativa, podemos usar un ControlParameter y hacer referencia a la `Suppliers` GridView s [ `SelectedValue` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (Véase la figura 19). Las operaciones de asignación GridView `SelectedValue` propiedad devuelve el `DataKey` valor correspondiente a la [ `SelectedIndex` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). En orden para esta opción funcione, es necesario establecer mediante programación la s GridView `SelectedIndex` propiedad seleccionada fila cuando la `ListProducts` se hace clic en el botón. Como ventaja adicional, estableciendo el `SelectedIndex`, vaya a realizar el registro seleccionado en el `SelectedRowStyle` definido en el `DataWebControls` tema (un fondo amarillo).


[![Use un ControlParameter para especificar el valor SelectedValue s de GridView como origen del parámetro](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Figura 19**: Use un ControlParameter para especificar la s de GridView SelectedValue como origen del parámetro ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


Al finalizar al asistente, Visual Studio agregará automáticamente campos para los campos de datos de producto s. Quitar todos menos el `ProductName`, `CategoryName`, y `UnitPrice` BoundFields y cambie el `HeaderText` propiedades para el producto, la categoría y el precio. Configurar la `UnitPrice` BoundField para que su valor se le aplica formato como una moneda. Después de realizar estos cambios, el marcado declarativo de s Panel y GridView, ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Para completar este ejercicio, es necesario establecer la s GridView `SelectedIndex` propiedad a la `SelectedSuppliersIndex` y `ProductsBySupplierPanel` Panel s `Visible` propiedad a `true` cuando el `ListProducts` se hace clic en el botón. Para ello, cree un controlador de eventos para el `ListProducts` control Button Web s `Click` eventos y agregue el código siguiente:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Si no se ha seleccionado un proveedor de GridView, el `ChooseSupplierMsg` se mostrarán las etiquetas y los `ProductsBySupplierPanel` ocultado el Panel. En caso contrario, si se ha seleccionado un proveedor, el `ProductsBySupplierPanel` se muestra y las operaciones de asignación GridView `SelectedIndex` se actualiza la propiedad.

Figura 20 muestra los resultados cuando se ha seleccionado el proveedor Bigfoot fábricas y los productos de mostrar en el botón de página se ha hecho clic.


[![Los productos proporcionados por fábricas Bigfoot se muestran en la misma página.](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Figura 20**: los productos proporcionados por fábricas Bigfoot aparecen en la misma página ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Resumen

Como se describe en el [principal/detalle mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial, los registros se pueden seleccionar de una GridView que usa un CommandField cuyo `ShowSelectButton` propiedad está establecida en `true`. Pero la CommandField muestra sus botones en forma de botones de comando regulares, vínculos o imágenes. Es una interfaz de usuario de selección de filas alternativas proporcionar un botón de radio o una casilla de verificación en cada fila de GridView. En este tutorial se examina cómo agregar una columna de botones de radio.

Por desgracia, agregar una columna de radio botones ejecutando t como directa ni sencilla tal y como se podría esperar que. No hay ningún RadioButtonField integrado que se pueden agregar al hacer clic en un botón y usando el control RadioButton Web dentro de un TemplateField presenta su propio conjunto de problemas. Al final, para proporcionar una interfaz de este tipo ya tenemos que crear una personalizada `DataControlField` clase ni recurrir a insertar el código HTML adecuado en TemplateField durante la `RowCreated` eventos.

Tener explorar cómo agregar una columna de botones de radio, hacernos nos centraremos en Agregar una columna de casillas de verificación. Con una columna de casillas de verificación, un usuario puede seleccionar una o varias filas de GridView y, a continuación, realizar alguna operación en todas las filas seleccionadas (por ejemplo, al seleccionar un conjunto de mensajes de correo electrónico desde un cliente de correo electrónico basado en web y, a continuación, elija Eliminar todos los correos electrónicos seleccionados). En el siguiente tutorial veremos cómo agregar este tipo de columna.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era David Suru. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Siguiente](adding-a-gridview-column-of-checkboxes-cs.md)
