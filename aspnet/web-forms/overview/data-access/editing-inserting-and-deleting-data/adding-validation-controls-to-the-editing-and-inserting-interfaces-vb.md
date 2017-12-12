---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: "Agregar controles de validación para la edición e insertar Interfaces (VB) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial veremos lo fácil que es agregar controles de validación a las plantillas EditItemTemplate e InsertItemTemplate de un control Web de datos para proporcionar una más..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a181be8d07ff15c33818077dc75b5cc6c6bbf65d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Agregar controles de validación para la edición e insertar Interfaces (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) o [descarga de PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> En este tutorial veremos lo fácil que es agregar controles de validación a las plantillas EditItemTemplate e InsertItemTemplate de un control Web de datos para proporcionar una interfaz de usuario más infalible.


## <a name="introduction"></a>Introducción

Los controles GridView y DetailsView en los ejemplos hemos analizado durante los últimos tres tutoriales han se compone de BoundFields y CheckBoxFields (los tipos de campo se agrega automáticamente mediante Visual Studio cuando se enlaza un GridView o DetailsView a un origen de datos controlar a través de la etiqueta inteligente). Cuando se edita una fila en un DetailsView o GridView, esas BoundFields que no son de solo lectura se convierten en cuadros de texto, desde el que el usuario final puede modificar los datos existentes. De forma similar, cuando insertar un nuevo registro en un control DetailsView, esas BoundFields cuya `InsertVisible` propiedad está establecida en `True` (valor predeterminado) se representan como cuadros de texto vacío, en la que el usuario puede proporcionar valores de campo del nuevo registro. Del mismo modo, CheckBoxFields, que están deshabilitadas en la interfaz estándar de solo lectura, se convierten en las casillas de verificación habilitado en la edición y la inserción de interfaces.

El valor predeterminado, editar e insertar interfaces para los BoundField y CampoCasillaVerificación puede ser útil, la interfaz no tiene a ningún tipo de validación. Si un usuario comete un error de entrada de datos: por ejemplo, si se omite la `ProductName` campo o escriba un valor no válido para `UnitsInStock` (por ejemplo, -50) se generará una excepción desde dentro de las profundidades de la arquitectura de la aplicación. Mientras esta excepción puede controlarse correctamente como se muestra en el [tutorial anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), lo ideal es que la edición o inserción de interfaz de usuario incluye controles de validación para evitar que un usuario escribe estos datos no válidos en el colocar en primer lugar.

Para proporcionar una interfaz insertar o modificar personalizada, es necesario reemplazar el BoundField o CampoCasillaVerificación con TemplateField. TemplateFields, que son el tema de discusión en el [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) y [utilizando TemplateFields en el DetailsView Control](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) tutoriales, puede constar de varios plantillas de definir separan las interfaces para los Estados de fila diferente. El TemplateField `ItemTemplate` se utiliza para al representar los campos de solo lectura o filas de los controles DetailsView o GridView, mientras que la `EditItemTemplate` y `InsertItemTemplate` indican las interfaces que se va a usar para la edición e insertar los modos, respectivamente.

En este tutorial veremos lo fácil que es agregar controles de validación a la TemplateField `EditItemTemplate` y `InsertItemTemplate` para proporcionar una interfaz de usuario más infalible. En concreto, este tutorial le guía en el ejemplo se crea en el [examinar los eventos asociados con la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) interfaces tutorial y amplía la edición e inserción para incluir la validación adecuada.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Paso 1: Replicar en el ejemplo de[examinar los eventos asociados a insertar, actualizar y eliminar](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

En el [examinar los eventos asociados con la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial se crea una página que muestra los nombres y los precios de los productos en un control GridView editable. Además, la página incluye un DetailsView cuyo `DefaultMode` propiedad se estableció en `Insert`, representación, por tanto, siempre en modo de inserción. Desde este DetailsView, el usuario podría escriba el nombre y el precio de un producto nuevo, haga clic en Insertar y hacer que se agreguen al sistema (consulte la figura 1).


[![El ejemplo anterior permite a los usuarios agregar nuevos productos y editar las existentes](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figura 1**: el anterior ejemplo permite a los usuarios agregar nuevos productos y editar los existentes ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


El objetivo de este tutorial consiste en aumentar el DetailsView y GridView para proporcionar controles de validación. En concreto, nuestra lógica de validación hará lo siguiente:

- Requieren que se proporcione el nombre al insertar o editar un producto
- Requerir que el precio proporcionarse al insertar un registro; al editar un registro, se seguirán requiriendo un precio, pero usará la lógica de programación en la GridView `RowUpdating` ya está presente en el tutorial anterior de controlador de eventos
- Asegúrese de que el valor especificado para el precio es un formato de moneda válidos

Antes de que podemos observar aumentar el ejemplo anterior para incluir la validación, primero es necesario replicar en el ejemplo de la `DataModificationEvents.aspx` página a la página para este tutorial, `UIValidation.aspx`. Para ello es necesario copiar a través de ambos el `DataModificationEvents.aspx` marcado declarativo de la página y su código fuente. En primer lugar copie en el marcado declarativo realizando los pasos siguientes:

1. Abra la `DataModificationEvents.aspx` página en Visual Studio
2. Vaya a marcado declarativo de la página (haga clic en el botón de origen en la parte inferior de la página)
3. Copie el texto dentro de la `<asp:Content>` y `</asp:Content>` etiquetas (líneas 3 a través de 44), como se muestra en la figura 2.


[![Copie el texto dentro de la &lt;asp: Content&gt; Control](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figura 2**: copiar el texto dentro de la `<asp:Content>` Control ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Abra la `UIValidation.aspx` página
2. Vaya a marcado declarativo de la página
3. Pegue el texto dentro de la `<asp:Content>` control.

Para copiar sobre el código fuente, abra el `DataModificationEvents.aspx.vb` página y copie solamente el texto *en* la `EditInsertDelete_DataModificationEvents` clase. Copie los tres controladores de eventos (`Page_Load`, `GridView1_RowUpdating`, y `ObjectDataSource1_Inserting`), pero **no** copiar la declaración de clase. Pegar el texto copiado *en* el `EditInsertDelete_UIValidation` clase `UIValidation.aspx.vb`.

Después de mover sobre el contenido y el código de `DataModificationEvents.aspx` a `UIValidation.aspx`, tómese un momento para probar su progreso en un explorador. Debería ver el mismo resultado y experimentar la misma funcionalidad en cada una de estas dos páginas (hacen referencia a la figura 1 para una captura de pantalla de `DataModificationEvents.aspx` en acción).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Paso 2: Convertir la BoundFields en TemplateFields

Para agregar controles de validación a las interfaces de edición e inserción, el BoundFields utilizados por los controles DetailsView y GridView necesita convertirse en TemplateFields. Para ello, haga clic en los vínculos Editar columnas y campos de edición de GridView y de DetailsView etiquetas inteligentes, respectivamente. Allí, seleccione cada uno de los BoundFields y haga clic en el vínculo "Convertir este campo en TemplateField".


[![Cada una de la de DetailsView y del GridView BoundFields convertir TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figura 3**: convertir cada uno de los de DetailsView y del GridView BoundFields en TemplateFields ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Convertir un BoundField en TemplateField a través del cuadro de diálogo campos genera TemplateField que exhibe las mismas interfaces de solo lectura, edición e Insertar como el BoundField propio. El marcado siguiente muestra la sintaxis declarativa para el `ProductName` campo DetailsView después de que se ha convertido en TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Tenga en cuenta que este TemplateField tenía tres plantillas creadas automáticamente `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate`. El `ItemTemplate` muestra un valor de campo de datos único (`ProductName`) con un control Web Label, mientras el `EditItemTemplate` y `InsertItemTemplate` presentar el valor del campo de datos en un control de cuadro de texto Web que asocia el campo de datos con el cuadro de texto `Text` propiedad con enlace de datos bidireccional. Puesto que estamos usando sólo el DetailsView en esta página para insertar, puede quitar el `ItemTemplate` y `EditItemTemplate` desde los dos TemplateFields, aunque no hay peligro en convirtiéndolas en.

Puesto que la GridView no permite la integradas de inserción de características de DetailsView, convertir la GridView `ProductName` en TemplateField produce solo una `ItemTemplate` y `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Haciendo clic en el "convertir este campo en TemplateField", Visual Studio ha creado un TemplateField cuyas plantillas imitan la interfaz de usuario de la BoundField convertido. Para comprobarlo, puede visitar esta página a través de un explorador. Encontrará que la apariencia y el comportamiento de la TemplateFields es idéntica a la experiencia cuando BoundFields se utilizaban en su lugar.

> [!NOTE]
> No dude en Personalizar las interfaces de edición en las plantillas según sea necesario. Por ejemplo, podemos queremos que el cuadro de texto en el `UnitPrice` TemplateFields se representa como un cuadro de texto más pequeño que los `ProductName` cuadro de texto. Para ello puede establecer el cuadro de texto `Columns` propiedad en un valor apropiado o proporcionar un ancho absoluto a través de la `Width` propiedad. En el siguiente tutorial veremos cómo personalizar completamente la interfaz de edición si se reemplaza el cuadro de texto con una entrada de datos alternativa control Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Paso 3: Agregar los controles de validación a la GridView`EditItemTemplate` s

Al construir los formularios de entrada de datos, es importante que los usuarios especifiquen los campos obligatorios y que todas las entradas proporcionadas son valores válidos, con un formato correcto. Para ayudar a garantizar que las entradas de un usuario son válidas, ASP.NET proporciona cinco controles de validación integradas que están diseñados para usarse para validar el valor de un control de entrada único:

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx) garantiza que se ha proporcionado un valor
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx) valida un valor con otro valor de control de Web o un valor constante, o se asegura de que el formato del valor es válido para un tipo de datos especificado
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx) garantiza que un valor está dentro del intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx) valida un valor con un [expresión regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) valida un valor con un método personalizado definido por el usuario

Para obtener más información sobre estos cinco controles, visite la [sección de controles de validación](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) de la [tutoriales de inicio rápido de ASP.NET](https://asp.net/QuickStart/aspnet/).

En nuestro tutorial debemos usar un control RequiredFieldValidator en las de GridView y DetailsView `ProductName` TemplateFields y un control RequiredFieldValidator en el DetailsView `UnitPrice` TemplateField. Además, se tendrá que agregar un control CompareValidator a ambos controles `UnitPrice` TemplateFields que garantiza que el precio indicado tiene un valor mayor o igual que 0 y se presenta en un formato de moneda válidos.

> [!NOTE]
> Mientras que ASP.NET 1.x tenía estos mismos controles de validación de cinco, ASP.NET 2.0 se agregado una serie de mejoras, los principales dos que se va a script del lado cliente compatibilidad con exploradores que no sea Internet Explorer y la capacidad de los controles de validación de partición en una página en grupos de validación. Para obtener más información sobre las nuevas características de control de validación de 2.0, consulte [Diseccionando los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Puede empezar agregando los controles de validación necesaria para la `EditItemTemplate` s en TemplateFields de GridView. Para ello, haga clic en el vínculo Editar plantillas de etiqueta inteligente de GridView para que aparezca la interfaz de edición de plantilla. Desde aquí, puede seleccionar qué plantilla de la lista desplegable para editarlo. Puesto que deseamos aumentar la interfaz de edición, tenemos que agregar controles de validación para el `ProductName` y `UnitPrice`del `EditItemTemplate` s.


[![Es necesario ampliar el ProductName y EditItemTemplates del UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figura 4**: es necesario extender el `ProductName` y `UnitPrice`del `EditItemTemplate` s ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


En el `ProductName` `EditItemTemplate`, agregue un control RequiredFieldValidator arrastrándolo desde el cuadro de herramientas en la interfaz de edición de plantilla, colocar después el cuadro de texto.


[![Agregar un control RequiredFieldValidator a ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figura 5**: agregar un control RequiredFieldValidator a la `ProductName` `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Todos los controles de validación de trabajo mediante la validación de la entrada de un único control Web de ASP.NET. Por lo tanto, es necesario indicar que debe validar el RequiredFieldValidator que acabamos de agregar en el cuadro de texto en el `EditItemTemplate`; Esto se logra estableciendo el control de validación [propiedad ControlToValidate](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) a la `ID` del control Web adecuado. El cuadro de texto tiene actualmente en su lugar no distintivo `ID` de `TextBox1`, pero vamos a cambiar a algo más apropiado. Haga clic en el cuadro de texto en la plantilla y, a continuación, en la ventana Propiedades, cambie la `ID` de `TextBox1` a `EditProductName`.


[![Cambiar el Id. del cuadro de texto a EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Figura 6**: cambiar el cuadro de texto `ID` a `EditProductName` ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


A continuación, establezca el RequiredFieldValidator `ControlToValidate` propiedad `EditProductName`. Por último, establezca el [propiedad ErrorMessage](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "Debe proporcionar el nombre del producto" y el [propiedad Text](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a "\*". La `Text` valor de propiedad, si se proporciona, es el texto que se muestra el control de validación si se produce un error en la validación. El `ErrorMessage` se utiliza el valor de propiedad, que es necesario, mediante el control del sistema; si la `Text` se omite el valor de propiedad, el `ErrorMessage` valor de la propiedad también es el texto mostrado por el control de validación de entrada no válida.

Después de establecer estas tres propiedades de RequiredFieldValidator, la pantalla debe ser similar a la figura 7.


[![Establecer el RequiredFieldValidator ControlToValidate, ErrorMessage y propiedades de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figura 7**: establecer el RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, y `Text` propiedades ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Con el control RequiredFieldValidator agregado a la `ProductName` `EditItemTemplate`, todos los que queda es agregar la validación necesaria para la `UnitPrice` `EditItemTemplate`. Puesto que hemos decidido que, de esta página, el `UnitPrice` es opcional cuando Editar un registro, no es necesario agregar un control RequiredFieldValidator. , Sin embargo, necesitamos agregar un control CompareValidator para asegurarse de que el `UnitPrice`, si se proporciona, con un formato correcto como moneda y es mayor o igual que 0.

Antes de que se agregue el control CompareValidator para la `UnitPrice` `EditItemTemplate`, vamos a cambiar primero Id. del control de cuadro de texto Web de `TextBox2` a `EditUnitPrice`. Después de realizar este cambio, agregue el control CompareValidator, establecer su `ControlToValidate` propiedad `EditUnitPrice`, sus `ErrorMessage` propiedad a "el precio debe ser mayor o igual que cero y no puede incluir el símbolo de moneda" y su `Text` propiedad "\*".

Para indicar que la `UnitPrice` valor debe ser mayor o igual que 0, establezca el CompareValidator [propiedad Operator](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, sus [propiedad ValueToCompare](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) en "0" y su [ La propiedad Type](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`. La siguiente muestra la sintaxis declarativa la `UnitPrice` del TemplateField `EditItemTemplate` una vez realizados estos cambios:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Después de realizar estos cambios, abra la página en un explorador. Si intenta omitir el nombre o escriba un valor de precios no válido durante la edición de un producto, aparece un asterisco junto al cuadro de texto. Como se muestra en la figura 8, un valor de precio que incluye el símbolo de moneda, como $19.95 se considera no válido. El control de CompareValidator `Currency` `Type` permite separadores de dígitos (por ejemplo, comas o puntos, dependiendo de la configuración de la referencia cultural) y un signo más iniciales o signo, pero *no* permitir un símbolo de moneda. Este comportamiento puede perplex a los usuarios como la interfaz de edición representa actualmente la `UnitPrice` con el formato de moneda.

> [!NOTE]
> Recuerde que en la *eventos asociados con la inserción, actualización y eliminación* tutorial establecemos la BoundField `DataFormatString` propiedad `{0:c}` con el fin de dar el formato de una moneda. Además, establecemos la `ApplyFormatInEditMode` provocando GridView de la edición de propiedades en true, interfaz para dar formato a los `UnitPrice` como una moneda. Al convertir el BoundField en TemplateField, Visual Studio se indicó esta configuración y el cuadro de texto con el formato `Text` propiedad como una moneda mediante la sintaxis de enlace de datos `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Aparece un asterisco junto a los cuadros de texto con entradas no válidas](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figura 8**: un asterisco aparece junto a los cuadros de texto con entradas no válidas ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Mientras que las obras de validación como-es, el usuario debe quitar manualmente el símbolo de moneda al editar un registro, que no es aceptable. Para solucionar esto, tenemos tres opciones:

1. Configurar la `EditItemTemplate` para que la `UnitPrice` valor no tiene el formato como una moneda.
2. Permitir que el usuario escriba un símbolo de moneda quitando el control CompareValidator y reemplazarlo con un control RegularExpressionValidator que busca correctamente un valor de divisa con el formato correcto. El problema es que la expresión regular para validar un valor de moneda no es descriptiva y requeriría escribir código si quería volver a incorporar la configuración de referencia cultural.
3. Quite por completo el control de validación y se basan en la lógica de validación del lado servidor en la GridView `RowUpdating` controlador de eventos.

Seleccionemos opción #1 para este ejercicio. Actualmente el `UnitPrice` se da formato como una moneda debido a la expresión de enlace de datos para el cuadro de texto en el `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Cambie la instrucción de enlace a `Bind("UnitPrice", "{0:n2}")`, que da formato al resultado como un número con dos dígitos de precisión. Esto puede hacerse directamente a través de la sintaxis declarativa o haciendo clic en el vínculo Editar DataBindings de la `EditUnitPrice` cuadro de texto en el `UnitPrice` del TemplateField `EditItemTemplate` (consulte las figuras 9 y 10).


[![Haga clic en vínculo de Editar DataBindings del cuadro de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figura 9**: haga clic en vínculo de Editar DataBindings del cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Especifique el especificador de formato en la instrucción de enlace](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figura 10**: especifique el especificador de formato en el `Bind` instrucción ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Con este cambio, el precio con formato en la interfaz de edición incluye comas como separador de grupo y un punto como separador decimal, pero deja fuera el símbolo de moneda.

> [!NOTE]
> El `UnitPrice` `EditItemTemplate` no incluye un control RequiredFieldValidator, que permite a la devolución de datos Asegúrese y la lógica de actualización para comenzar. Sin embargo, el `RowUpdating` controlador de eventos que se copió desde el *examinar los eventos asociados con la inserción, actualización y eliminación* tutorial incluye una comprobación de programación que garantiza que la `UnitPrice` se proporciona. No dude en quitar esta lógica, déjelo en como-se, o agregar un control RequiredFieldValidator a la `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Paso 4: Resumen de problemas de entrada de datos

Además de los controles de validación de cinco, ASP.NET incluye la [control ValidationSummary](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx), que muestra la `ErrorMessage` s de los controles de validación que ha detectado datos no válidos. Estos datos de resumen se pueden mostrar como texto en la página web o a través de un cuadro de mensaje modal, de cliente. Vamos a mejorar este tutorial para incluir un cuadro de mensajes del lado cliente resumir los problemas de validación.

Para ello, arrastre un control del cuadro de herramientas hasta el diseñador. La ubicación del control de validación realmente no importa, ya que vamos a configurar para mostrar solo el resumen como un cuadro de mensajes. Después de agregar el control, establezca su [propiedad ShowSummary](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) a `False` y su [propiedad ShowMessageBox](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `True`. Con esta adición, se resumen los errores de validación en un cuadro de mensajes del lado cliente.


[![Los errores de validación se resumen en un cuadro de mensajes de cliente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figura 11**: los errores de validación se resumen en un cuadro de mensajes de cliente ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Paso 5: Agregar los controles de validación a la DetailsView`InsertItemTemplate`

Todo lo que queda de este tutorial es agregar los controles de validación a la interfaz de inserción de DetailsView. El proceso de agregar controles de validación a las plantillas de DetailsView es idéntico a la que se examina en el paso 3; por lo tanto, podrá avanzar la tarea en este paso. Igual que hicimos con la GridView `EditItemTemplate` s, le animo a cambiar el nombre de la `ID` s de los cuadros de texto de la no distintivo `TextBox1` y `TextBox2` a `InsertProductName` y `InsertUnitPrice`.

Agregue un control RequiredFieldValidator a la `ProductName` `InsertItemTemplate`. Establecer el `ControlToValidate` a la `ID` del cuadro de texto en la plantilla, su `Text` propiedad a "\*" y su `ErrorMessage` propiedad "Debe proporcionar el nombre del producto".

Puesto que la `UnitPrice` es necesario para esta página cuando se agrega un nuevo registro, agregue un control RequiredFieldValidator a la `UnitPrice` `InsertItemTemplate`, y establece su `ControlToValidate`, `Text`, y `ErrorMessage` propiedades adecuadamente. Por último, agregue un control CompareValidator a la `UnitPrice` `InsertItemTemplate` , configurar su `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, y `ValueToCompare` propiedades al igual que hicimos con la `UnitPrice`del CompareValidator en la GridView `EditItemTemplate`.

Después de agregar estos controles de validación, un nuevo producto no se puede agregar al sistema si no proporciona su nombre o si su precio es un número negativo o forma no autorizada con formato.


[![Se ha agregado lógica de validación a la interfaz de inserción de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figura 12**: lógica de validación se ha agregado a la interfaz de inserción de DetailsView ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Paso 6: Crear particiones de los controles de validación en los grupos de validación

Nuestra página consta de dos conjuntos dispares lógicamente de controles de validación: los que corresponden a la GridView de la edición de interfaz y los que corresponden a DetailsView de inserción de interfaz. De forma predeterminada, cuando se produce un postback *todos los* se comprueban los controles de validación en la página. Sin embargo, cuando se edita un registro no queremos controles de validación de la interfaz insertar de DetailsView para validar. Figura 13 ilustra este dilema actual cuando un usuario está editando un producto con los valores legales perfectamente, al hacer clic en actualización produce un error de validación porque los valores de nombre y el precio de la interfaz de inserción están en blanco.


[![Actualizar un producto hace que los controles de validación de la inserción de una interfaz para desencadenar](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figura 13**: actualizar un producto hace que los controles de validación de la interfaz de inserción de incendio ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Los controles de validación en ASP.NET 2.0 se pueden crear particiones en grupos de validación a través de sus `ValidationGroup` propiedad. Para asociar un conjunto de controles de validación en un grupo, basta con establecer sus `ValidationGroup` propiedad en el mismo valor. Para nuestro tutorial, establezca la `ValidationGroup` propiedades de los controles de validación de TemplateFields de GridView a `EditValidationControls` y `ValidationGroup` propiedades de TemplateFields de DetailsView a `InsertValidationControls`. Estos cambios pueden realizarse directamente en el marcado declarativo o a través de la ventana Propiedades cuando se usa el diseñador edición interfaz de la plantilla.

Además de la validación, el botón y los controles relacionados con el botón en ASP.NET 2.0 también incluyen un `ValidationGroup` propiedad. Validadores de un grupo de validación se comprueban la validez solo cuando una devolución de datos sea inducido por un botón que tiene el mismo `ValidationGroup` configuración de la propiedad. Por ejemplo, en orden botón de DetailsView de inserción desencadenar la `InsertValidationControls` grupo de validación, es necesario establecer la CommandField `ValidationGroup` propiedad `InsertValidationControls` (Véase la figura 14). Además, establecer la GridView del CommandField `ValidationGroup` propiedad `EditValidationControls`.


[![Propiedad de ValidationGroup del CommandField a InsertValidationControls de conjunto DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figura 14**: establecer el DetailsView del CommandField `ValidationGroup` propiedad `InsertValidationControls` ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Después de estos cambios, el DetailsView de GridView TemplateFields y CommandFields debe ser similar al siguiente:

El DetailsView TemplateFields y CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

La GridView CommandField y TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

En este momento los controles de validación específicos de la edición activan sólo cuando se hace clic botón de actualización de GridView y el fuego de controles de validación específicos de la inserción sólo cuando se hace clic en botón de inserción de DetailsView, resolver el problema resaltado de la figura 13. Sin embargo, con este cambio nuestro control ValidationSummary deja de aparecer cuando se escriban datos no válidos. El control también contiene un `ValidationGroup` propiedad y sólo se muestra información de resumen de los controles de validación en su grupo de validación. Por lo tanto, es necesario tener dos controles de validación en esta página, uno para la `InsertValidationControls` grupo de validación y otro para `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Con esta adición nuestro tutorial es completa.

## <a name="summary"></a>Resumen

Aunque BoundFields puede proporcionar tanto una interfaz de inserción y de edición, la interfaz no es personalizable. Normalmente, queremos agregar controles de validación para la edición y la inserción de interfaz para asegurarse de que el usuario escribe entradas necesarias en un formato válido. Para ello, debemos convertir el BoundFields TemplateFields y agregar los controles de validación a las plantillas adecuadas. En este tutorial se extiende el ejemplo de la *examinar los eventos asociados con la inserción, actualización y eliminación* tutorial, para agregar controles de validación para ambos DetailsView del insertar interfaz y la GridView interfaz de edición. Además, hemos visto cómo mostrar información de resumen de validación mediante el control y cómo dividir los controles de validación en la página en grupos distintos de validación.

Como hemos visto en este tutorial, TemplateFields permite que las interfaces de edición e inserción para que se pueden ampliar para incluir controles de validación. TemplateFields también se puede extender para incluir controles de Web de entrada adicionales, habilitar el cuadro de texto que se reemplazará por un control Web más adecuado. En el siguiente tutorial veremos cómo reemplazar el control de cuadro de texto con un control de DropDownList enlazados a datos, que es ideal al editar una clave externa (como `CategoryID` o `SupplierID` en el `Products` tabla).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Liz Shulok y Zack Jones. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
[Siguiente](customizing-the-data-modification-interface-vb.md)
