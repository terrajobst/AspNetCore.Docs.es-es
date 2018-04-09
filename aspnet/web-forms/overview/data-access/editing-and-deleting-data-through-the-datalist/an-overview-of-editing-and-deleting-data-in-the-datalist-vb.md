---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Información general de editar y eliminar datos en el control DataList (VB) | Documentos de Microsoft
author: rick-anderson
description: Mientras el control DataList no tiene integrada editar y eliminar funciones, en este tutorial veremos cómo crear a un control DataList que admite Editar y eliminar o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 6956777e91184a92e189db7aa716a4bd7dbbfccd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Información general de editar y eliminar datos en el control DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) o [descarga de PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Mientras el control DataList no tiene integrada editar y eliminar funciones, en este tutorial veremos cómo crear a un control DataList que admite la edición y eliminación de los datos subyacentes.


## <a name="introduction"></a>Introducción

En el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial explicamos cómo insertar, actualizar y eliminar datos mediante la arquitectura de la aplicación, un ObjectDataSource y GridView, DetailsView y FormView controles. Con ObjectDataSource y estos controles Web de datos de tres, implementar interfaces de modificación de datos simple era un complemento e implicados simplemente la cuenta atrás una casilla de verificación de una etiqueta inteligente. No se necesita para escribir código.

Desafortunadamente, el control DataList no tiene integrado en la edición y eliminación de las capacidades inherentes en el control GridView. Esta funcionalidad que falta es debe en parte al hecho de que el control DataList resulta un relic desde la versión anterior de ASP.NET, controles de origen de datos declarativo y páginas de modificación de datos sin código no estaban disponibles. Mientras el control DataList de ASP.NET 2.0 no ofrece la misma lista para usar capacidades de modificación de datos como GridView, podemos usar ASP.NET 1.x técnicas para incluir esta funcionalidad. Este enfoque requiere un poco de código pero, como veremos más adelante en este tutorial, el control DataList tiene algunas propiedades y eventos en su lugar para facilitar este proceso.

En este tutorial veremos cómo crear a un control DataList que admite la edición y eliminación de los datos subyacentes. Tutoriales futuros examinará edición más avanzada y escenarios de eliminación, incluida la validación de campo de entrada, correctamente controlar las excepciones producidas en el acceso a datos o capas de lógica de negocios y así sucesivamente.

> [!NOTE]
> Al igual que el control DataList, el control de repetidor carece de fuera de la funcionalidad del cuadro para insertar, actualizar o eliminar. Mientras se puede agregar esta funcionalidad, el control DataList incluye propiedades y eventos que no se encuentra en el repetidor que simplifican la adición de dichas funciones. Por lo tanto, este tutorial y los próximos que examinan la edición y eliminación se centrarán estrictamente en el control DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Paso 1: Crear las páginas Web de tutoriales de edición y eliminación

Antes de empezar a explorar cómo actualizar y eliminar datos de un control DataList, permiten s primero Tómese un momento para crear las páginas ASP.NET en el proyecto de sitio Web que se necesitará para este tutorial y los varios siguientes. Empiece por agregar una nueva carpeta denominada `EditDeleteDataList`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de que asociar cada página con el `Site.master` página maestra:

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


Al igual que en las otras carpetas, `Default.aspx` en el `EditDeleteDataList` carpeta enumeran los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página s la vista Diseño.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de los informes de maestro y detalles con la DataList y repetidor `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. Ahora, el menú de la izquierda incluye elementos para el control DataList edición y eliminación de tutoriales.


![El mapa del sitio ahora incluye las entradas para el control DataList edición y eliminación de tutoriales](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figura 3**: la asignación de sitio ahora incluye las entradas para el control DataList edición y eliminación de tutoriales


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Paso 2: Examinar técnicas para actualizar y eliminar datos

Editar y eliminar datos con GridView es tan fácil porque interiormente, el control GridView y ObjectDataSource trabajan en conjunto. Como se describe en el [examinar los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial, cuando se hace clic en un botón de actualización de la fila s, GridView asigna automáticamente sus campos que utilizan el enlace de datos bidireccional con el `UpdateParameters` colección de su ObjectDataSource y, a continuación, se invoca ese s ObjectDataSource `Update()` método.

Desafortunadamente, DataList no se proporciona ninguna de estas funcionalidades integradas. Es nuestra responsabilidad para asegurarse de que los valores de usuario s se asignan a los parámetros de s ObjectDataSource y que su `Update()` se llama al método. Para ayudar a nosotros en este esfuerzo, el control DataList proporciona las propiedades y los eventos siguientes:

- **El [ `DataKeyField` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  al actualizar o eliminar, necesitamos poder identificar de forma exclusiva cada elemento en el control DataList. Establezca esta propiedad en el campo de clave principal de los datos mostrados. Si lo hace, se rellenará el control DataList s [ `DataKeys` colección](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) con los valores especificados `DataKeyField` valor para cada elemento DataList.
- **El [ `EditCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en Editar.
- **El [ `CancelCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en Cancelar.
- **El [ `UpdateCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en actualizar.
- **El [ `DeleteCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  se desencadena cuando un botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en se hace clic en eliminar.

Utilizando estas propiedades y eventos, existen cuatro enfoques que podemos usar para actualizar y eliminar datos desde el control DataList:

1. **Uso de técnicas ASP.NET 1.x** DataList existía antes de ASP.NET 2.0 y ObjectDataSources y se puede actualizar y eliminar datos completamente a través de medios mediante programación. Esta técnica se deshace el ObjectDataSource por completo y requiere que enlazar los datos al control DataList directamente desde la capa de lógica empresarial, tanto en recuperar los datos que se va a mostrar y al actualizar o eliminar un registro.
2. **Usar un ObjectDataSource Control único en la página de selección, actualización y eliminación** mientras el control DataList no tiene la edición inherente de GridView s y la eliminación de recursos, s hay ningún motivo podemos t agregarlos en nosotros mismos. Con este enfoque, se usa un ObjectDataSource igual que en los ejemplos de GridView, pero se debe crear un controlador de eventos para el control DataList s `UpdateCommand` eventos donde establecemos el parámetros de ObjectDataSource s y llame a su `Update()` método.
3. **Usar un ObjectDataSource Control de selección, pero actualizar y eliminar directamente en el BLL** cuando se usa la opción 2, es necesario escribir un poco de código en el `UpdateCommand` eventos, asignación de valores de parámetro y así sucesivamente. En su lugar, le podemos opte por a través de ObjectDataSource para seleccionar pero realizar las llamadas de actualización y eliminación directamente en la capa BLL (al igual que con la opción 1). En mi opinión, actualizar datos interactuar directamente con la capa BLL conduce a un código más legible que asignar la s ObjectDataSource `UpdateParameters` y llamar a su `Update()` método.
4. **Un medio declarativo a través de varios ObjectDataSources** los tres enfoques anteriores todos requieren un poco de código. Si en su lugar, d seguir usando como la sintaxis declarativa mucho como sea posible, una última opción es incluir varios ObjectDataSources en la página. El primer ObjectDataSource recupera los datos de la capa BLL y la enlaza con el control DataList. Para actualizar, otro ObjectDataSource es agregado, pero agregan directamente dentro de DataList s `EditItemTemplate`. Para incluir la eliminación de compatibilidad, aún ObjectDataSource otro se necesitan en el `ItemTemplate`. Con este enfoque, estos incrustan ObjectDataSource s utilizarán `ControlParameters` para enlazar mediante declaración a los parámetros de ObjectDataSource s a los controles de entrada de usuario (en lugar de tener que especificar las credenciales mediante programación en el control DataList s `UpdateCommand` controlador de eventos). Este enfoque requiere un poco de código es necesario para llamar a las operaciones de asignación ObjectDataSource incrustado `Update()` o `Delete()` comando sino que requiere mucho menor que con las demás tres enfoques. La desventaja aquí es que la ObjectDataSources varios desordenar la página desvirtuar la legibilidad global.

Si se fuerza siempre usar uno de estos enfoques, puedo d elija la opción 1 porque proporciona la máxima flexibilidad y porque el control DataList se diseñó originalmente para admitir este patrón. Mientras el control DataList se ha ampliado para trabajar con los controles de origen de datos de ASP.NET 2.0, no tiene todos los puntos de extensibilidad o las características de los datos de ASP.NET 2.0 oficiales los controles Web (GridView, DetailsView y FormView). Opciones de 2 a 4 no son sin méritos, aunque.

Esto y la futura edición y eliminación de tutoriales usará un ObjectDataSource para recuperar los datos para mostrar y dirigir las llamadas a la capa BLL para actualizar y eliminar datos (opción 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Paso 3: Agregar al control DataList y configurar su ObjectDataSource

En este tutorial se creará a un control DataList que muestra información de producto y, para cada producto, proporciona al usuario la capacidad para editar el nombre y el precio y eliminar por completo el producto. En concreto, se recuperará los registros que se va a mostrar utilizando un ObjectDataSource, pero realizar la actualización y eliminar acciones interactuar directamente con la capa BLL. Antes de que nos preocupamos sobre la implementación de las capacidades de edición y eliminación para el control DataList, permiten s primero hay que obtener la página para mostrar los productos en una interfaz de solo lectura. Desde que se ha examinado estos pasos en los tutoriales anteriores, podrá continuar a través de ellos rápidamente.

Comience abriendo la `Basics.aspx` página en el `EditDeleteDataList` carpeta y, en la vista Diseño, agregue un control DataList a la página. A continuación, desde la etiqueta inteligente de DataList s, cree un nuevo origen ObjectDataSource. Puesto que estamos trabajando con datos de productos, configurarlo de modo que use la `ProductsBLL` clase. Para recuperar *todos los* productos, elija la `GetProducts()` método en la ficha Seleccionar.


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figura 4**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Devolver la información de producto mediante el método GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figura 5**: devolver la información de producto mediante el `GetProducts()` método ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


El control DataList, como GridView, no está diseñado para insertar nuevos datos; por lo tanto, Seleccionar opción de la lista desplegable en la pestaña Insertar de la (ninguno). Elegir (ninguno) para las pestañas UPDATE y DELETE desde las actualizaciones y eliminaciones se realizará mediante programación a través de la capa BLL.


[![Confirme que las listas desplegables en las operaciones de asignación ObjectDataSource INSERT, UPDATE y eliminar pestañas se establecen en (ninguno)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figura 6**: confirme que las listas desplegables en ObjectDataSource s INSERT, UPDATE y eliminar pestañas se establecen en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Después de configurar el ObjectDataSource, haga clic en Finalizar, devolver al diseñador. Como se ha visto en ejemplos anteriores, al completar la configuración de ObjectDataSource, Visual Studio automáticamente crea un `ItemTemplate` DropDownList, mostrando cada uno de los campos de datos. Reemplace este `ItemTemplate` por uno que muestra el nombre de producto s y el precio. Además, establezca el `RepeatColumns` propiedad en 2.

> [!NOTE]
> Como se describe en el *información general de insertar, actualizar y eliminar datos* tutorial, al modificar los datos a través de ObjectDataSource nuestra arquitectura requiere que se quite el `OldValuesParameterFormatString` propiedad de las operaciones de asignación ObjectDataSource marcado declarativo (o lo restablece a su valor predeterminado, `{0}`). Sin embargo, en este tutorial, estamos utilizando ObjectDataSource sólo para recuperar datos. Por lo tanto, no es necesario modificar las operaciones de asignación ObjectDataSource `OldValuesParameterFormatString` valor de propiedad (aunque lo t afectar al hacerlo).


Después de reemplazar el valor predeterminado de DataList `ItemTemplate` con una cadena personalizada, el marcado declarativo en la página debe ser similar al siguiente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Tómese un momento para ver el progreso a través de un explorador. Como se muestra en la figura 7, el control DataList muestra el precio de unidad y el nombre del producto para cada producto de dos columnas.


[![Los nombres de productos y los precios se muestran en un control DataList de dos columnas](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figura 7**: los nombres de productos y los precios se muestran en un control DataList de dos columnas ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> El control DataList tiene un número de propiedades que son necesarias para el proceso de actualización y eliminación, y estos valores se almacenan en estado de vista. Por lo tanto, cuando cree a un control DataList que admite editen o eliminen datos, es fundamental que el estado de vista de DataList s esté habilitada.  
>   
>  Es posible que el lector astuto Recuerde que pudimos deshabilitar el estado de vista al crear editable GridView, DetailsViews y FormViews. Esto es porque los controles Web de ASP.NET 2.0 pueden incluir *el estado de control*, que se conserva de estado entre las devoluciones de datos como el estado de vista, pero lo considere esenciales.


Deshabilitar la vista de estado en el control GridView simplemente omite la información de estado trivial, pero mantiene el estado del control (que incluye el estado necesario para modificar y eliminar). El control DataList, si hubiese sido creado en el período de tiempo ASP.NET 1.x, no utiliza el estado del control y, por tanto, debe tener habilitado el estado de vista. Vea [estado del Control. Estado de vista](https://msdn.microsoft.com/library/1whwt1k7.aspx) para obtener más información sobre el propósito del estado de control y cómo se diferencia del estado de vista.

## <a name="step-4-adding-an-editing-user-interface"></a>Paso 4: Agregar una interfaz de usuario de edición

El control GridView se compone de una colección de campos (BoundFields, CheckBoxFields, TemplateFields y así sucesivamente). Estos campos pueden ajustar su marcado representado según su modo. Por ejemplo, cuando se encuentra en modo de solo lectura, BoundField muestra su valor de campo de datos como texto; Cuando se encuentra en modo de edición, representa un cuadro de texto Web control cuya `Text` propiedad se asigna el valor del campo de datos.

El control DataList, por otro lado, representa sus elementos mediante plantillas. Elementos de solo lectura se representan con el `ItemTemplate` , mientras que los elementos en modo de edición se representan mediante el `EditItemTemplate`. En este momento, nuestra DataList tiene solo un `ItemTemplate`. Para admitir la funcionalidad de edición de nivel de elemento que necesitamos agregar una `EditItemTemplate` que contiene el marcado que se mostrará para el elemento editable. Para este tutorial, vamos a usar controles de cuadro de texto Web para su edición del producto s nombre y el precio unitario.

El `EditItemTemplate` pueden crearse mediante declaración o a través del diseñador (seleccionando la opción de editar plantillas de la etiqueta inteligente de DataList s). Para usar la opción de editar plantillas, primero haga clic en el vínculo Editar plantillas en la etiqueta inteligente y, a continuación, seleccione la `EditItemTemplate` elemento de la lista desplegable.


[![Participación para trabajar con la plantilla EditItemTemplate s de DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figura 8**: participar para trabajar con el control DataList s `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


A continuación, escriba el nombre de producto: y precio: y, a continuación, arrastre dos controles de cuadro de texto del cuadro de herramientas en el `EditItemTemplate` interfaz en el diseñador. Establecer los cuadros de texto `ID` propiedades `ProductName` y `UnitPrice`.


[![Agregar un cuadro de texto para el nombre del producto s y el precio](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figura 9**: agregar un cuadro de texto para el s nombre de producto y el precio ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


Es necesario enlazar los valores de campo de datos de producto correspondiente a la `Text` propiedades de los dos cuadros de texto. En las etiquetas inteligentes de cuadros de texto, haga clic en el vínculo Editar DataBindings y, a continuación, asocie el campo de datos adecuado con el `Text` propiedad, como se muestra en la figura 10.

> [!NOTE]
> Al enlazar la `UnitPrice` campo de datos para el precio del cuadro de texto s `Text` campo, puede formatearlo según un valor de divisa (`{0:C}`), un número general (`{0:N}`), o dejarla sin formato.


![Enlazar los campos de datos de UnitPrice y ProductName a las propiedades de texto de los cuadros de texto](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figura 10**: enlazar el `ProductName` y `UnitPrice` campos de datos a la `Text` propiedades de los cuadros de texto


Observe cómo funciona el cuadro de diálogo Editar DataBindings en la figura 10 *no* incluyen la casilla de verificación de enlace de datos bidireccional que está presente cuando se edita un TemplateField en GridView o DetailsView o una plantilla de FormView. La característica de enlace de datos bidireccional permite el valor especificado en el control de Web de entrada que se asigne automáticamente a las operaciones de asignación ObjectDataSource correspondiente `InsertParameters` o `UpdateParameters` al insertar o actualizar datos. El control DataList no admite enlace de datos bidireccional como veremos más adelante en este tutorial, después el usuario realiza su cambia y está preparado para actualizar los datos, tendrá que obtener acceso mediante programación a estos cuadros de texto `Text` propiedades y los valores que se pase el adecuado `UpdateProduct` método en la `ProductsBLL` clase.

Por último, tenemos que agregar actualización botones y cancelar la `EditItemTemplate`. Como vimos en el [principal/detalle con una lista con viñetas de registros maestros DataList detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial, cuando un botón, LinkButton o ImageButton cuyo `CommandName` se establece la propiedad se hace clic en desde dentro de un repetidor o DataList, el S repetidor o DataList `ItemCommand` evento se desencadena. Para el control DataList, si la `CommandName` propiedad está establecida en un valor determinado, un evento adicional se puede producir también. Especial `CommandName` los valores de propiedad son, entre otros:

- Cancelar genera el `CancelCommand` eventos
- Editar genera el `EditCommand` eventos
- Actualizar genera el `UpdateCommand` eventos

Tenga en cuenta que estos eventos se generan *además* el `ItemCommand` eventos.

Agregar a la `EditItemTemplate` dos controles de botón Web, uno cuyo `CommandName` se establece en la actualización y las otras operaciones de asignación establecida en Cancel. Después de agregar estos dos controles de botón Web, el diseñador debe ser similar al siguiente:


[![Actualización de agregar botones a la plantilla EditItemTemplate y cancelar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figura 11**: agregar actualizar y cancelar botones a la `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Con el `EditItemTemplate` completa el marcado declarativo DataList s debería ser similar al siguiente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Paso 5: Agregar la mecánica para entrar en modo de edición

En este momento nuestro DataList tiene una interfaz de edición definida a través de su `EditItemTemplate`; sin embargo, hay s actualmente ninguna manera para que un usuario visite nuestra página para indicar que desea editar la información de producto s. Tenemos que agregar un botón Editar para cada producto que, al hacer clic, representa esa DataList artículos en modo de edición. Empiece por agregar un botón Editar para el `ItemTemplate`, ya sea a través del diseñador o mediante declaración. Asegúrese de establecer el botón Editar s `CommandName` propiedad a editar.

Después de haber agregado este botón de edición, tómese un momento para ver la página a través de un explorador. Con esta adición, cada lista de productos debe incluir un botón Editar.


[![Actualización de agregar botones a la plantilla EditItemTemplate y cancelar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figura 12**: agregar actualizar y cancelar botones a la `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Haga clic en el botón provoca una devolución de datos, pero *no* poner el listado en modo de edición del producto. Para que pueda modificar el producto, es necesario:

1. Establecer el control DataList s [ `EditItemIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) en el índice de la `DataListItem` cuyo botón Editar simplemente se ha hecho clic.
2. Volver a enlazar los datos al control DataList. Cuando se vuelven a representar el control DataList la `DataListItem` cuyo `ItemIndex` se corresponde con el control DataList s `EditItemIndex` se representarán mediante su `EditItemTemplate`.

Desde el control DataList s `EditCommand` evento se desencadena cuando se hace clic en el botón de edición, cree un `EditCommand` controlador de eventos con el código siguiente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

El `EditCommand` controlador de eventos se pasa en un objeto de tipo `DataListCommandEventArgs` como su segundo parámetro de entrada, que incluye una referencia a la `DataListItem` cuyo botón Edición se hizo clic (`e.Item`). El controlador de eventos establece primero la DataList s `EditItemIndex` a la `ItemIndex` de la editable `DataListItem` y, a continuación, vuelve a enlazar los datos al control DataList mediante una llamada a DataList s `DataBind()` método.

Después de agregar este controlador de eventos, volver a visitar la página en un explorador. Haga clic en el botón Editar ahora hace que el producto ha hecho clic editable (consulte figura 13).


[![Al hacer clic en hace de botón de edición del producto Editable](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figura 13**: haga clic en el botón Editar hace que el producto puede editar ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Paso 6: Guardar los cambios de s de usuario

Al hacer clic en los botones de cancelación o actualización del producto editado s no hace nada en este momento; Para agregar esta funcionalidad es necesario para crear controladores de eventos para el control DataList s `UpdateCommand` y `CancelCommand` eventos. Comience creando el `CancelCommand` controlador de eventos, que se ejecutará cuando se hace clic en el botón de cancelación de producto editado s y ha asignado la tarea devolviendo el control DataList a su estado anterior edición.

Para que el control DataList representar todos sus elementos en el modo de solo lectura, es necesario:

1. Establecer el control DataList s [ `EditItemIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) en el índice de un inexistente `DataListItem` índice. `-1` es una opción segura, ya que la `DataListItem` índices empiezan en `0`.
2. Volver a enlazar los datos al control DataList. Desde no `DataListItem` `ItemIndex` es corresponden al control DataList s `EditItemIndex`, DataList completo se representará en un modo de solo lectura.

Estos pasos pueden realizarse con el siguiente código de controlador de eventos:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Con esta adición, haga clic en el botón Cancelar, devuelve DataList a su estado anterior de edición.

Es el último controlador de eventos es necesario para completar la `UpdateCommand` controlador de eventos. Este controlador de eventos debe:

1. Obtener acceso mediante programación el nombre del producto introducidos por el usuario y el precio, así como el producto editado s `ProductID`.
2. Iniciar el proceso de actualización mediante una llamada a la correspondiente `UpdateProduct` sobrecarga en el `ProductsBLL` clase.
3. Establecer el control DataList s [ `EditItemIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) en el índice de un inexistente `DataListItem` índice. `-1` es una opción segura, ya que la `DataListItem` índices empiezan en `0`.
4. Volver a enlazar los datos al control DataList. Desde no `DataListItem` `ItemIndex` es corresponden al control DataList s `EditItemIndex`, DataList completo se representará en un modo de solo lectura.

Los pasos 1 y 2 son responsables de guardar al usuario los cambios de s; los pasos 3 y 4 devuelven DataList a su estado anterior edición después de que los cambios se guardaron y son idénticos a los pasos realizados en el `CancelCommand` controlador de eventos.

Para obtener el nombre de producto actualizado y el precio, debemos usar la `FindControl` método a referencia mediante programación de controles de la Web de cuadro de texto dentro de la `EditItemTemplate`. También es necesario obtener el producto editado s `ProductID` valor. Cuando se enlaza inicialmente el ObjectDataSource al control DataList, Visual Studio asigna DataList s `DataKeyField` propiedad al valor de clave principal del origen de datos (`ProductID`). Este valor, a continuación, se puede recuperar desde el control DataList s `DataKeys` colección. Tómese un momento para asegurarse de que el `DataKeyField` propiedad realmente está establecida en `ProductID`.

El código siguiente implementa los cuatro pasos:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

El controlador de eventos se inicia mediante la lectura en el producto editado s `ProductID` desde el `DataKeys` colección. A continuación, los dos cuadros de texto en el `EditItemTemplate` se hace referencia y sus `Text` propiedades almacenadas en variables locales, `productNameValue` y `unitPriceValue`. Usamos el `Decimal.Parse()` método para leer el valor de la `UnitPrice` para ese if el valor especificado del cuadro de texto tiene un símbolo de moneda, todavía se puede convertir correctamente en un `Decimal` valor.

> [!NOTE]
> Los valores de la `ProductName` y `UnitPrice` cuadros de texto solo se asignan a las variables productNameValue y unitPriceValue si las propiedades de texto de cuadros de texto tienen un valor especificado. En caso contrario, un valor de `Nothing` se usa para las variables, que tiene el efecto de actualizar los datos con una base de datos `NULL` valor. Es decir, el código trata convierte cadenas a base de datos vacías `NULL` valores, que es el comportamiento predeterminado de la interfaz de edición en los controles GridView, DetailsView y FormView.


Después de leer los valores, el `ProductsBLL` clase s `UpdateProduct` se llama al método, pasando el nombre de producto s, precios, y `ProductID`. El controlador de eventos completa devolviendo el control DataList a su estado de edición previamente con la misma lógica exacta que en el `CancelCommand` controlador de eventos.

Con el `EditCommand`, `CancelCommand`, y `UpdateCommand` completar controladores de eventos, un visitante puede editar el nombre y el precio de un producto. Las figuras 14 a 16 muestran este flujo de trabajo de edición en acción.


[![Al primer visitar la página, todos los productos están en modo de solo lectura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figura 14**: en primer lugar, visite la página, todos los productos están en modo de solo lectura ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Para actualizar un producto s nombre o precio, haga clic en el botón Editar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figura 15**: para actualizar un producto es nombre o el precio, haga clic en el botón Editar ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Después de cambiar el valor, haga clic en Actualizar para volver al modo de solo lectura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figura 16**: después de cambiar el valor, haga clic en Actualizar para volver al modo de sólo lectura ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Paso 7: Agregar capacidades de eliminación

Los pasos para agregar capacidades de eliminación a un control DataList son similares a las de agregar capacidades de edición. En resumen, tenemos que agregar un botón Eliminar para el `ItemTemplate` que, al hacer clic en:

1. Lee en el producto correspondiente s `ProductID` a través de la `DataKeys` colección.
2. Lleva a cabo la eliminación mediante una llamada a la `ProductsBLL` clase s. `DeleteProduct` método.
3. Vuelve a enlazar los datos al control DataList.

Permiten s empiece agregando un botón Eliminar para el `ItemTemplate`.

Al hacer clic en un botón cuyo `CommandName` es editar, actualizar, o cancelar genera DataList s `ItemCommand` eventos junto con un evento adicional (por ejemplo, cuando se utiliza Editar el `EditCommand` evento se desencadena también). Del mismo modo, cualquier botón, LinkButton o ImageButton en DataList cuyo `CommandName` propiedad está establecida en eliminar causas el `DeleteCommand` activación del evento (junto con `ItemCommand`).

Agregar un botón Eliminar situada junto al botón de edición en el `ItemTemplate`, y establece su `CommandName` propiedad en el valor Delete. Después de agregar este botón control DataList s `ItemTemplate` debería ser la sintaxis declarativa similar:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el control DataList s `DeleteCommand` evento usando el código siguiente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Haga clic en el botón Eliminar provoca una devolución de datos y se activa el control DataList s `DeleteCommand` eventos. En el controlador de eventos, el producto ha hecho clic s `ProductID` valor se obtiene acceso desde el `DataKeys` colección. A continuación, se elimina el producto mediante una llamada a la `ProductsBLL` clase s. `DeleteProduct` método.

Después de eliminar el producto, lo importante que se vuelva a enlazar los datos al control DataList (`DataList1.DataBind()`), en caso contrario, el control DataList seguirán mostrando el producto que solo se eliminó.

## <a name="summary"></a>Resumen

Mientras el control DataList no tiene el punto y haga clic en Editar y eliminar la compatibilidad con su experiencia con GridView, con un poco de código corto puede mejorarse para incluir estas características. En este tutorial, hemos visto cómo crear una lista de dos columnas de los productos que se ha podido eliminar y cuyo nombre y el precio pueden editarse. Agregar edición y eliminación de soporte técnico es una cuestión de inclusión de los controles Web adecuados de la `ItemTemplate` y `EditItemTemplate`, crear los controladores de eventos correspondiente, leer los valores de clave principales y escrito por el usuario e interactúa con el negocio Capa de lógica.

Aunque hemos agregado la edición básica y eliminar funciones al control DataList, carece de características más avanzadas. Por ejemplo, no hay ninguna validación de campo de entrada - si un usuario escribe un precio de demasiado costosa, una excepción se producirá por `Decimal.Parse` al intentar convertir demasiado costosa en un `Decimal`. De forma similar, si hay un problema en la actualización de los datos en la lógica de negocios o los niveles de acceso a datos se presentará al usuario con la pantalla de error estándar. Sin ningún tipo de confirmación en el botón Eliminar, la eliminación accidental de un producto es probable todo demasiado.

Experimentan de tutoriales veremos cómo mejorar el usuario de edición en el futuro.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Zack Jones, Ken Pespisa y Randy Schmidt. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](customizing-the-datalist-s-editing-interface-cs.md)
> [Siguiente](performing-batch-updates-vb.md)
