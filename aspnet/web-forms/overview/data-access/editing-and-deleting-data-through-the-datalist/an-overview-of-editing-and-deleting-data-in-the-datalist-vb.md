---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Información general de editar y eliminar datos en el control DataList (VB) | Microsoft Docs
author: rick-anderson
description: Mientras el control DataList carece de edición integrada y eliminación de funciones, en este tutorial veremos cómo crear a un control DataList que admite Editar y eliminar o...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: aa1e07be5b634a0e66f7c6f03f80a1aa2c741189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804503"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Información general de editar y eliminar datos en el control DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) o [descargar PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Mientras el control DataList carece de edición integrada y eliminación de funciones, en este tutorial veremos cómo crear a un control DataList que admite la edición y eliminación de los datos subyacentes.


## <a name="introduction"></a>Introducción

En el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial hemos visto cómo insertar, actualizar y eliminar datos mediante la arquitectura de aplicación, un origen ObjectDataSource y GridView, DetailsView y FormView controles. Con el origen ObjectDataSource y estos controles Web de tres datos, implementar las interfaces de modificación de datos simple era un complemento e implica simplemente funciona a la perfección una casilla de verificación de una etiqueta inteligente. No se necesita se escriban código.

Lamentablemente, el control DataList no tiene integrado en la edición y eliminación de funciones inherentes en el control GridView. Esta funcionalidad que falta es debe en parte al hecho de que el control DataList es un relic desde la versión anterior de ASP.NET, cuando los controles de origen de datos declarativos y páginas de modificación de datos sin código no estaban disponibles. Si bien el control DataList de ASP.NET 2.0 no ofrecen la misma fábrica capacidades de modificación de datos como GridView, podemos usar ASP.NET 1.x técnicas para incluir esta funcionalidad. Este enfoque requiere un poco de código, pero, como veremos en este tutorial, el control DataList tiene algunas propiedades y eventos en su lugar para facilitar este proceso.

En este tutorial, veremos cómo crear a un control DataList que admite la edición y eliminación de los datos subyacentes. Tutoriales futuros examinará la edición más avanzada y escenarios de eliminación, incluida la validación de campo de entrada, controla correctamente las excepciones producidas desde el acceso a datos o las capas de lógica de negocio y así sucesivamente.

> [!NOTE]
> Al igual que el control DataList, el control Repeater carece de fuera de la funcionalidad del cuadro para insertar, actualizar o eliminar. Aunque se puede agregar esta funcionalidad, el control DataList incluye propiedades y eventos no se encuentra en el control Repeater que simplifican la adición de estas capacidades. Por lo tanto, este tutorial y los próximos que mirar la edición y eliminación se centrarán estrictamente en el control DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Paso 1: Creación de las páginas Web de tutoriales de edición y eliminación

Antes de empezar a explorar cómo actualizar y eliminar datos de un control DataList, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial y las diversas siguientes. Empiece por agregar una nueva carpeta denominada `EditDeleteDataList`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Agregar las páginas ASP.NET para los tutoriales](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales


Al igual que en las demás carpetas `Default.aspx` en el `EditDeleteDataList` carpeta enumeran los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de los informes de maestro y detalles con los controles DataList y Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para el control DataList, edición y eliminación de tutoriales.


![El mapa del sitio incluye ahora entradas para el control DataList, edición y eliminación de tutoriales](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figura 3**: el mapa del sitio incluye ahora entradas para el control DataList, edición y eliminación de tutoriales


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Paso 2: Análisis de las técnicas para actualizar y eliminar datos

Editar y eliminar datos con el control GridView es muy sencillo porque interiormente, el control GridView y ObjectDataSource trabajan en conjunto. Como se describe en el [examinando los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial, cuando se hace clic en un botón de actualización de fila s, el control GridView asigna automáticamente sus campos que usa el enlace de datos bidireccional a la `UpdateParameters` colección de su origen ObjectDataSource y, a continuación, llama a ObjectDataSource s `Update()` método.

Por desgracia, el control DataList no proporciona esta funcionalidad integrada. Es nuestra responsabilidad para asegurarse de que el usuario s los valores se asignan a los parámetros de ObjectDataSource s y que su `Update()` se llama al método. Para ayudar a nosotros en este esfuerzo, el control DataList proporciona las siguientes propiedades y eventos:

- **El [ `DataKeyField` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  al actualizar o eliminar, necesitamos poder identificar de forma exclusiva cada elemento en el control DataList. Establezca esta propiedad en el campo de clave principal de los datos mostrados. Si lo hace, se rellenará el control DataList s [ `DataKeys` colección](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) con los valores especificados `DataKeyField` valor para cada elemento DataList.
- **El [ `EditCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en Editar.
- **El [ `CancelCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en Cancelar.
- **El [ `UpdateCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en actualizar.
- **El [ `DeleteCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en eliminar.

Utilizando estas propiedades y eventos, existen cuatro enfoques que podemos usar para actualizar y eliminar datos desde el control DataList:

1. **Uso de técnicas ASP.NET 1.x** DataList existían antes de ASP.NET 2.0 y ObjectDataSource y fue capaz de actualizar y eliminar datos completamente a través de medios de programación. Esta técnica se deshace de ObjectDataSource por completo y requiere que se enlazar los datos para el control DataList directamente desde la capa de lógica empresarial, tanto al recuperar los datos que se va a mostrar y al actualizar o eliminar un registro.
2. **Con un solo ObjectDataSource Control en la página de selección, actualización y eliminación** mientras el control DataList no tiene la edición inherente de GridView s y la eliminación de recursos, s hay ninguna razón que podemos t agregarlos en nosotros mismos. Con este enfoque, se usa un origen ObjectDataSource igual que en los ejemplos de GridView, pero debe crear un controlador de eventos para el control DataList s `UpdateCommand` donde configuramos los parámetros de ObjectDataSource s y una llamada de evento su `Update()` método.
3. **Uso de un ObjectDataSource Control para cuando se selecciona, pero actualizar y eliminar directamente contra el BLL** cuando se usa la opción 2, debemos escribir algo de código en el `UpdateCommand` eventos, asignación de valores de parámetro y así sucesivamente. En su lugar, nos podemos ciña a través de ObjectDataSource para seleccionar, pero realizan las llamadas de eliminación y actualización directamente en el nivel de lógica empresarial (por ejemplo, con la opción 1). En mi opinión, la actualización de datos mediante una interfaz directamente con la capa BLL da lugar a código más legible que asignar la s ObjectDataSource `UpdateParameters` y llamar a su `Update()` método.
4. **Un medio declarativo a través de ObjectDataSource varios** los tres enfoques anteriores todas requieren un poco de código. Si en su lugar, d seguir usando como sintaxis declarativa mucho como sea posible, última opción es incluir varios ObjectDataSource en la página. El primer ObjectDataSource recupera los datos de la capa BLL y lo enlaza al control DataList. Para actualizar, ObjectDataSource otro agregado, pero agregar directamente en el control DataList s `EditItemTemplate`. Para incluir la eliminación de soporte técnico, todavía otra ObjectDataSource sería necesarios en el `ItemTemplate`. Con este enfoque, estos incrustado ObjectDataSource s emplean `ControlParameters` enlazar mediante declaración los parámetros de ObjectDataSource s a los controles de entrada de usuario (en lugar de tener que especificar mediante programación en el control DataList s `UpdateCommand` controlador de eventos). Este enfoque requiere un poco de código es necesario para llamar a la s ObjectDataSource incrustado `Update()` o `Delete()` comando, pero requiere mucho menor que con las otras tres enfoques. La desventaja aquí es que el ObjectDataSource varios provocar un desorden en la página, apartarse de la legibilidad global.

Si se ven obligados a usar solo uno de estos enfoques, d elijo la opción 1 porque proporciona la máxima flexibilidad y porque el control DataList se diseñó originalmente para dar cabida a este patrón. Mientras el control DataList se ha ampliado para trabajar con los controles de origen de datos de ASP.NET 2.0, no tiene todos los puntos de extensibilidad o las características de los datos oficiales de ASP.NET 2.0 los controles Web (GridView, DetailsView y FormView). Opciones de 2 a 4 no son fundamentada, sin embargo.

Esto y el futuro edición y eliminación de tutoriales utilizará un origen ObjectDataSource para recuperar los datos para mostrar y dirigir las llamadas a la capa BLL para actualizar y eliminar datos (opción 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Paso 3: Agregar al control DataList y configurar su origen ObjectDataSource

En este tutorial crearemos a un control DataList que muestra información de producto y, para cada producto, proporciona al usuario la capacidad de editar el nombre y el precio y para eliminar completamente el producto. En concreto, se recuperará los registros que se muestran con un origen ObjectDataSource, pero realizar la actualización y acciones de eliminación mediante una interfaz directamente con la capa BLL. Antes de que se preocupe sobre la implementación de las capacidades de edición y eliminación para el control DataList, permiten s obtenga primero la página para mostrar los productos en una interfaz de solo lectura. Desde que creamos ve examina estos pasos en los tutoriales anteriores, voy a través de ellos rápidamente.

Comience abriendo la `Basics.aspx` página en el `EditDeleteDataList` carpeta y, en la vista Diseño, agregue un control DataList a la página. A continuación, en la etiqueta inteligente s DataList, cree un nuevo origen ObjectDataSource. Puesto que estamos trabajando con datos de productos, configúrelo para utilizar el `ProductsBLL` clase. Para recuperar *todas* productos, elija el `GetProducts()` método en la ficha Seleccionar.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figura 4**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Devolver la información de producto mediante el método GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figura 5**: devolver la información de producto mediante la `GetProducts()` método ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


El control DataList, como GridView, no está diseñado para insertar nuevos datos; por lo tanto, Seleccionar opción de la lista desplegable en la pestaña Insertar de la (ninguno). También elegir (ninguno) para las pestañas UPDATE y DELETE desde las actualizaciones y eliminaciones se realizarán mediante programación a través de la capa BLL.


[![Confirmar que la lista desplegable se enumeran en la INSERCIÓN de ObjectDataSource s, actualización y eliminar pestañas se establecen en (None)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figura 6**: confirme que la lista desplegable se enumeran en ObjectDataSource s INSERT, UPDATE y eliminar las fichas se establecen en (None) ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Después de configurar el origen ObjectDataSource, haga clic en Finalizar, devolver al diseñador. Como se ve visto en ejemplos anteriores, cuando se completa automáticamente la configuración del origen ObjectDataSource, Visual Studio crea un `ItemTemplate` DropDownList, mostrando cada uno de los campos de datos. Reemplácelo `ItemTemplate` por uno que muestra el nombre de producto s y el precio. Además, establezca el `RepeatColumns` propiedad en 2.

> [!NOTE]
> Como se describe en el *información general de insertar, actualizar y eliminar datos* tutorial, al modificar los datos a través de ObjectDataSource nuestra arquitectura requiere que se quite el `OldValuesParameterFormatString` propiedad de ObjectDataSource s marcado declarativo (o lo restablece a su valor predeterminado, `{0}`). Sin embargo, en este tutorial, estamos usando el origen ObjectDataSource sólo para recuperar datos. Por lo tanto, no necesitamos modificar la s ObjectDataSource `OldValuesParameterFormatString` valor de propiedad (aunque lo t es malo para hacerlo).


Después de reemplazar el valor predeterminado de DataList `ItemTemplate` con una personalizada, el marcado declarativo en la página debería ser similar al siguiente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Dedique un momento para ver nuestro progreso a través de un explorador. Como se muestra en la figura 7, el control DataList muestra el precio del producto, nombre y la unidad para cada producto de dos columnas.


[![Los nombres de productos y los precios se muestran en un control DataList de dos columnas](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figura 7**: los nombres de productos y los precios se muestran en un control DataList de dos columnas ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> El control DataList tiene un número de propiedades que son necesarias para el proceso de actualización y eliminación, y estos valores se almacenan en estado de vista. Por lo tanto, cuando es compatible con la creación de un control DataList que editen o eliminen datos, es esencial que el estado de vista de DataList s esté habilitada.  
>   
>  El lector astuto puede recordar que pudimos deshabilitar el estado de vista al crear GridView editable, DetailsViews y FormViews. Esto es porque los controles Web de ASP.NET 2.0 pueden incluir *controlar el estado de*, que se conserva el estado entre las devoluciones de datos, como el estado de vista, pero lo considere esenciales.


Deshabilitar la vista de estado en el control GridView simplemente omite la información de estado trivial, pero mantiene el estado de control (que incluye el estado necesario para modificar y eliminar). El control DataList, si se hubiera creado en el marco ASP.NET 1.x, no utiliza el estado de control y, por tanto, debe tener habilitado el estado de vista. Consulte [estado del Control. Estado de vista](https://msdn.microsoft.com/library/1whwt1k7.aspx) para obtener más información sobre el propósito del estado de control y cómo se diferencia del estado de vista.

## <a name="step-4-adding-an-editing-user-interface"></a>Paso 4: Agregar una interfaz de usuario de edición

El control GridView se compone de una colección de campos (BoundFields, CheckBoxFields, TemplateFields y así sucesivamente). Estos campos pueden ajustar su marcado representado según su modo. Por ejemplo, cuando está en modo de solo lectura, BoundField muestra su valor de campo de datos como texto; Cuando está en modo de edición, representa un cuadro de texto de Web control cuya `Text` propiedad se asigna el valor del campo de datos.

El control DataList, por otro lado, representa sus elementos mediante plantillas. Elementos de solo lectura se representan mediante el `ItemTemplate` , mientras que los elementos en modo de edición se representan mediante el `EditItemTemplate`. En este momento, nuestra DataList tiene sólo un `ItemTemplate`. Para admitir la funcionalidad de edición de nivel de elemento que necesitamos agregar un `EditItemTemplate` que contiene el marcado que se mostrará para el elemento editable. Para este tutorial, vamos a usar los controles Web de cuadro de texto para editar el precio del producto s, nombre y la unidad.

El `EditItemTemplate` se pueden crear mediante declaración o a través del diseñador (seleccionando la opción Editar plantillas de la etiqueta inteligente de DataList s). Para usar la opción de editar plantillas, primero haga clic en el vínculo Editar plantillas en la etiqueta inteligente y, a continuación, seleccione el `EditItemTemplate` elemento de la lista desplegable.


[![Trabajo con DataList s EditItemTemplate participar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figura 8**: participar para trabajar con el control DataList s `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


A continuación, escriba el nombre de producto: y precio: y, a continuación, arrastre dos controles TextBox en el cuadro de herramientas en el `EditItemTemplate` interfaz en el diseñador. Establecer los cuadros de texto `ID` propiedades a `ProductName` y `UnitPrice`.


[![Agregar un cuadro de texto para el nombre del producto y el precio](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figura 9**: agregar un cuadro de texto para el nombre de producto y el precio ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


Es necesario enlazar los valores de campo de datos de producto correspondiente a la `Text` las propiedades de los dos cuadros de texto. En las etiquetas inteligentes de cuadros de texto, haga clic en el vínculo Editar DataBindings y, a continuación, asocie el campo de datos adecuado con la `Text` propiedad, como se muestra en la figura 10.

> [!NOTE]
> Al enlazar la `UnitPrice` campo de datos para el precio de cuadro de texto s `Text` campo, puede formatearlo según un valor de divisa (`{0:C}`), un número general (`{0:N}`), o dejarla sin formato.


![Enlazar los campos de datos de UnitPrice y ProductName a las propiedades de texto de los cuadros de texto](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figura 10**: enlazar el `ProductName` y `UnitPrice` campos de datos a la `Text` las propiedades de los cuadros de texto


Tenga en cuenta cómo se realiza el cuadro de diálogo Editar DataBindings en la figura 10 *no* incluyen la casilla de verificación de enlace de datos bidireccional está presente cuando se edita un TemplateField en el control GridView o DetailsView o una plantilla de FormView. La característica de enlace de datos bidireccional permite el valor especificado en el control Web de entrada que se asignará automáticamente a la s ObjectDataSource correspondiente `InsertParameters` o `UpdateParameters` al insertar o actualizar datos. El control DataList no admite el enlace bidireccional como veremos más adelante en este tutorial, después el usuario realiza su cambia y está listo para actualizar los datos, es necesario obtener acceso mediante programación a estos cuadros de texto `Text` propiedades y los valores que se pase el adecuado `UpdateProduct` método en el `ProductsBLL` clase.

Por último, tenemos que agregar la actualización de botones y cancelar el `EditItemTemplate`. Como hemos visto en el [maestro/detalle con una lista con viñetas de registros maestros con un control DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial, cuando un botón, LinkButton o ImageButton cuyo `CommandName` se establece la propiedad se hace clic en desde dentro de un control Repeater o DataList, el S Repeater o DataList `ItemCommand` provoca el evento. Para el control DataList, si la `CommandName` propiedad está establecida en un determinado valor, es posible que se genera un evento adicional también. Especial `CommandName` valores de propiedad incluyen, entre otros:

- Cancelar genera el `CancelCommand` eventos
- Editar genera el `EditCommand` eventos
- Actualizar genera el `UpdateCommand` eventos

Tenga en cuenta que estos eventos se generan *además* el `ItemCommand` eventos.

Agregar a la `EditItemTemplate` dos controles de botón Web, uno cuyo `CommandName` se establece en la actualización y a los otros elementos establecido en Cancelar. Después de agregar estos dos controles de botón Web, el diseñador debe ser similar al siguiente:


[![Agregar actualización de los botones para EditItemTemplate y cancelar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figura 11**: agregar actualizar y cancelar los botones a la `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Con el `EditItemTemplate` completa el marcado declarativo DataList s debe ser similar al siguiente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Paso 5: Adición de la mecánica para entrar en modo de edición

En este momento nuestro DataList tiene una interfaz de edición definida a través de su `EditItemTemplate`; sin embargo, hay s actualmente ninguna manera para un usuario que visita nuestra página para indicar que desea editar la información de un producto s. Necesitamos agregar un botón Editar para cada producto que, al hacer clic, presenta esa DataList de elemento en modo de edición. Empiece agregando un botón de edición para el `ItemTemplate`, ya sea a través del diseñador o mediante declaración. Asegúrese de establecer el botón Editar s `CommandName` propiedad a editar.

Después de haber agregado este botón de edición, dedique un momento para ver la página mediante un explorador. Con esta adición, cada descripción de producto debe incluir un botón de edición.


[![Agregar actualización de los botones para EditItemTemplate y cancelar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figura 12**: agregar actualizar y cancelar los botones a la `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Al hacer clic en el botón produce un postback, pero *no* ponga el producto listado en modo de edición. Para que pueda modificar el producto, es necesario:

1. Establecer el control DataList s [ `EditItemIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) al índice de la `DataListItem` cuyo botón Editar simplemente se hizo clic.
2. Volver a enlazar los datos para el control DataList. Cuando se vuelven a representar el control DataList el `DataListItem` cuyo `ItemIndex` se corresponde con el control DataList s `EditItemIndex` se representarán mediante su `EditItemTemplate`.

Desde el control DataList s `EditCommand` evento se desencadena cuando se hace clic en el botón Editar, crear un `EditCommand` controlador de eventos con el código siguiente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

El `EditCommand` controlador de eventos se pasa un objeto de tipo `DataListCommandEventArgs` como su segundo parámetro de entrada, que incluye una referencia a la `DataListItem` cuyo botón Edición se hizo clic (`e.Item`). El controlador de eventos establece primero el control DataList s `EditItemIndex` a la `ItemIndex` del editable `DataListItem` y, a continuación, vuelve a enlazar los datos para el control DataList mediante una llamada a DataList s `DataBind()` método.

Después de agregar este controlador de eventos, volver a visitar la página en un explorador. Haga clic en el botón Editar ahora hace que el producto hace clic en él editable (consulte la figura 13).


[![Al hacer clic en la edición botón facilita el producto Editable](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figura 13**: al hacer clic en el botón Editar hace que el producto Editable ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Paso 6: Guardar los cambios de usuario s

Al hacer clic en los botones de cancelación o actualización del producto editado s no hace nada en este momento; Para agregar esta funcionalidad es necesario crear controladores de eventos para el control DataList s `UpdateCommand` y `CancelCommand` eventos. Comience creando el `CancelCommand` controlador de eventos, que se ejecutará cuando se hace clic en el botón Cancelar de producto modificada s y encargan devolviendo el control DataList a su estado de edición previamente.

Para que el control DataList represente todos sus elementos en el modo de solo lectura, es necesario:

1. Establecer el control DataList s [ `EditItemIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) al índice de un inexistente `DataListItem` índice. `-1` es una opción segura, ya que el `DataListItem` índices a partir de `0`.
2. Volver a enlazar los datos para el control DataList. Desde la n `DataListItem` `ItemIndex` es que se corresponden con el control DataList s `EditItemIndex`, se representará el control DataList todo en un modo de solo lectura.

Estos pasos pueden realizarse con el siguiente código de controlador de eventos:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Con esta versión, al hacer clic en el botón Cancelar, devuelve el control DataList a su estado de edición previamente.

Es el último controlador de eventos es necesario para completar la `UpdateCommand` controlador de eventos. Este controlador de eventos debe:

1. Obtener acceso mediante programación el nombre de producto escrito por el usuario y precios, así como el producto editado s `ProductID`.
2. Iniciar el proceso de actualización mediante una llamada a la correspondiente `UpdateProduct` sobrecarga en el `ProductsBLL` clase.
3. Establecer el control DataList s [ `EditItemIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) al índice de un inexistente `DataListItem` índice. `-1` es una opción segura, ya que el `DataListItem` índices a partir de `0`.
4. Volver a enlazar los datos para el control DataList. Desde la n `DataListItem` `ItemIndex` es que se corresponden con el control DataList s `EditItemIndex`, se representará el control DataList todo en un modo de solo lectura.

Los pasos 1 y 2 son responsables de guardar los cambios s; el usuario los pasos 3 y 4 devuelven el control DataList a su estado previo edición después de que los cambios se guardaron y son idénticos a los pasos realizados en el `CancelCommand` controlador de eventos.

Para obtener el nombre de producto actualizado y el precio, debemos usar la `FindControl` método mediante programación la referencia Web cuadro de texto se controla dentro de la `EditItemTemplate`. También es necesario obtener el producto editado s `ProductID` valor. Cuando inicialmente se enlaza el origen ObjectDataSource para el control DataList, Visual Studio asigna el control DataList s `DataKeyField` propiedad en el valor de clave principal del origen de datos (`ProductID`). Este valor, a continuación, se puede recuperar desde el control DataList s `DataKeys` colección. Dedique un momento para asegurarse de que el `DataKeyField` propiedad realmente está establecida en `ProductID`.

El código siguiente implementa los cuatro pasos:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

El controlador de eventos se inicia mediante la lectura en el producto editado s `ProductID` desde el `DataKeys` colección. A continuación, los dos cuadros de texto en el `EditItemTemplate` se hace referencia y sus `Text` propiedades almacenadas en variables locales, `productNameValue` y `unitPriceValue`. Usamos el `Decimal.Parse()` método para leer el valor de la `UnitPrice` para ese if el valor especificado del cuadro de texto tiene un símbolo de moneda, todavía se convierten correctamente en un `Decimal` valor.

> [!NOTE]
> Los valores de la `ProductName` y `UnitPrice` cuadros de texto solo se asignan a las variables productNameValue y unitPriceValue si las propiedades de texto de los cuadros de texto tienen un valor especificado. En caso contrario, un valor de `Nothing` se usa para las variables, que tiene el efecto de actualizar los datos con una base de datos `NULL` valor. Es decir, nuestro código trata convierte las cadenas a la base de datos vacías `NULL` valores, que es el comportamiento predeterminado de la interfaz de edición en los controles GridView, DetailsView y FormView.


Después de leer los valores, el `ProductsBLL` clase s `UpdateProduct` se llama al método, pasando el nombre de producto s, precio, y `ProductID`. El controlador de eventos finaliza al devolver el control DataList a su estado de edición previamente con la misma lógica exacta como en el `CancelCommand` controlador de eventos.

Con el `EditCommand`, `CancelCommand`, y `UpdateCommand` completar los controladores de eventos, un visitante puede editar el nombre y el precio de un producto. Las figuras 14-16 se muestra este flujo de trabajo de edición en acción.


[![Cuando son la primera visita la página, todos los productos en modo de solo lectura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figura 14**: en primer lugar, visite la página, todos los productos están en modo de solo lectura ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Para actualizar un producto s nombre o precio, haga clic en el botón Editar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figura 15**: para actualizar un producto s nombre o precio, haga clic en el botón Editar ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Después de cambiar el valor, haga clic en Actualizar para volver al modo de solo lectura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figura 16**: después de cambiar el valor, haga clic en Actualizar para volver al modo de solo lectura ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Paso 7: Agregar capacidades de eliminación

Los pasos para agregar capacidades de eliminación a un control DataList son similares a las de agregar capacidades de edición. En resumen, tenemos que agregar un botón Eliminar para el `ItemTemplate` que, al hacer clic en:

1. Lee en el producto correspondiente s `ProductID` a través de la `DataKeys` colección.
2. Realiza la eliminación mediante una llamada a la `ProductsBLL` clase s `DeleteProduct` método.
3. Vuelve a enlazar los datos para el control DataList.

Permiten s empiece agregando un botón de eliminación para el `ItemTemplate`.

Cuando hace clic en un botón cuyo `CommandName` es editar, actualizar, o cancelar provoca el control DataList s `ItemCommand` evento junto con un evento adicional (por ejemplo, cuando se usa la edición del `EditCommand` evento se desencadena también). De forma similar, cualquier botón, LinkButton o ImageButton en el control DataList cuyo `CommandName` propiedad está establecida para eliminar las causas del `DeleteCommand` activación del evento (junto con `ItemCommand`).

Agregar un botón Eliminar junto al botón de edición en el `ItemTemplate`, estableciendo su `CommandName` propiedad para su eliminación. Después de agregar este botón control DataList s `ItemTemplate` debe ser la sintaxis declarativa similar:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el control DataList s `DeleteCommand` evento, mediante el código siguiente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Al hacer clic en el botón Eliminar provoca una devolución de datos y se activa el control DataList s `DeleteCommand` eventos. En el controlador de eventos, el producto hace clic en él s `ProductID` valor se obtiene acceso desde el `DataKeys` colección. A continuación, se elimina el producto mediante una llamada a la `ProductsBLL` clase s `DeleteProduct` método.

Después de eliminar el producto, lo importante que se vuelva a enlazar los datos para el control DataList (`DataList1.DataBind()`), en caso contrario, el control DataList seguirá mostrando el producto que solo se ha eliminado.

## <a name="summary"></a>Resumen

Aunque el control DataList no tiene el punto y haga clic en la edición y eliminación de soporte técnico que haya disfrutado de GridView, con un poco corto de código puede mejorarse para incluir estas características. En este tutorial hemos visto cómo crear una lista de dos columnas de los productos que se puede eliminar y cuyo nombre y el precio pueden editarse. Adición, edición y eliminación de soporte técnico es una cuestión de incluidos los controles Web adecuados en el `ItemTemplate` y `EditItemTemplate`, crear los controladores de eventos correspondiente, leer los valores de clave principales y escrito por el usuario e interactuar con la empresa Capa de lógica.

Aunque hemos agregado la edición básica y eliminación de funcionalidades para el control DataList, carece de las características más avanzadas. Por ejemplo, no hay ninguna validación de campo de entrada - si un usuario escribe un precio de demasiadas costoso, una excepción se producirá por `Decimal.Parse` al intentar convertir demasiado costosas en un `Decimal`. De forma similar, si hay un problema en la actualización de los datos en la lógica de negocios o los niveles de acceso a datos al usuario le mostrará la pantalla de error estándar. Sin ningún tipo de confirmación en el botón Eliminar, la eliminación accidental de un producto es probable todo demasiado.

Experimentan de tutoriales, veremos cómo mejorar el usuario de edición en el futuro.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Zack Jones, Ken Pespisa y Randy Schmidt. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](customizing-the-datalist-s-editing-interface-cs.md)
> [Siguiente](performing-batch-updates-vb.md)
