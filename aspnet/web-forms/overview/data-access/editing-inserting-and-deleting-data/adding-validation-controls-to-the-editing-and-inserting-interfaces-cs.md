---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: Agregar controles de validación a la edición e inserción de Interfaces (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos lo fácil que es agregar los controles de validación a las plantillas EditItemTemplate e InsertItemTemplate de un control Web de datos para proporcionar más...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: fedcad6265e7247d59a74a2dcaccb71fabfe0b4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834208"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>Agregar controles de validación a la edición e inserción de Interfaces (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe) o [descargar PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> En este tutorial veremos lo fácil que es agregar los controles de validación a las plantillas EditItemTemplate e InsertItemTemplate de un control Web de datos para proporcionar una interfaz de usuario infalible.


## <a name="introduction"></a>Introducción

Los controles GridView y DetailsView en los ejemplos hemos explorado durante los últimos tres tutoriales han sido compuestos por BoundFields y CheckBoxFields (los tipos de campo se agrega automáticamente mediante Visual Studio al enlazar un control GridView o DetailsView a un origen de datos el control a través de la etiqueta inteligente). Cuando edita una fila en GridView o DetailsView, esas BoundFields que no son de solo lectura se convierten en cuadros de texto, desde el que el usuario final puede modificar los datos existentes. De forma similar, cuando insertar un nuevo registro en un control DetailsView, esas BoundFields cuya `InsertVisible` propiedad está establecida en `true` (valor predeterminado) se representan como cuadros de texto vacío, en la que el usuario puede proporcionar valores de campo del nuevo registro. Del mismo modo, CheckBoxFields, que están deshabilitadas en la interfaz estándar, de solo lectura, se convierten en casillas de verificación habilitado en la edición de interfaces e inserción.

Aunque el valor predeterminado, edición e inserción de interfaces para el BoundField y CampoCasillaVerificación puede ser útil, la interfaz no tiene a algún tipo de validación. Si un usuario comete un error de entrada de datos - por ejemplo, si se omite el `ProductName` campo o escriba un valor no válido para `UnitsInStock` (por ejemplo, -50), se generará una excepción desde dentro de las profundidades de la arquitectura de aplicación. Aunque puede controlar esta excepción correctamente como se muestra en el [tutorial anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md), lo ideal es que la edición o insertar la interfaz de usuario incluye controles de validación para impedir que un usuario entre dichos datos no válidos en el punto de partida.

Para proporcionar una interfaz de inserción o edición personalizada, es necesario reemplazar el BoundField o CampoCasillaVerificación TemplateField. TemplateFields, que estaban en el tema de discusión en la [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) y [uso de TemplateFields en el DetailsView Control](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md) tutoriales, puede constar de varios las plantillas de definición de separan las interfaces para los Estados de fila diferente. El TemplateField `ItemTemplate` se usa al representar los campos de solo lectura o las filas de los controles DetailsView o GridView, mientras que el `EditItemTemplate` y `InsertItemTemplate` indican las interfaces que se usará para la edición e inserción de los modos, respectivamente.

En este tutorial veremos lo fácil que es agregar los controles de validación a la TemplateField `EditItemTemplate` y `InsertItemTemplate` para proporcionar una interfaz de usuario infalible. En concreto, este tutorial realiza en el ejemplo se crea en el [examinando los eventos asociados con la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) interfaces tutorial y amplía la edición e inserción para incluir validación adecuado.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>Paso 1: Replicación en el ejemplo de[examinar los eventos asociados con la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

En el [examinando los eventos asociados con la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial hemos creado una página que muestra los nombres y los precios de los productos en un control GridView editable. Además, la página incluye un DetailsView cuyo `DefaultMode` propiedad se estableció en `Insert`, representación, por tanto, siempre en modo de inserción. Desde este DetailsView, el usuario podría escriba el nombre y el precio de un producto nuevo, haga clic en Insertar y hacer que se agregue al sistema (consulte la figura 1).


[![El ejemplo anterior permite a los usuarios agregar productos nuevos y editar las existentes](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**Figura 1**: el anterior ejemplo permite a los usuarios agregar productos nuevos y editar los existentes ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


El objetivo de este tutorial es aumentar la DetailsView y GridView para proporcionar controles de validación. En concreto, nuestra lógica de validación hará lo siguiente:

- Requerir que se proporcione el nombre al insertar o editar un producto
- Requerir que el precio se proporcione al insertar un registro. al editar un registro, se necesitará un precio, pero usará la lógica de programación en el control de GridView `RowUpdating` ya está presente en el tutorial anterior de controlador de eventos
- Asegúrese de que el valor especificado para el precio es un formato de moneda válidos

Antes de que podemos observar enriquecidas con el ejemplo anterior para incluir la validación, primero es necesario replicar el ejemplo desde el `DataModificationEvents.aspx` página a la página para este tutorial, `UIValidation.aspx`. Para lograr esto es necesario para copiar a través de ambos el `DataModificationEvents.aspx` marcado declarativo de la página y su código fuente. Copiar el marcado declarativo realizando los pasos siguientes:

1. Abra el `DataModificationEvents.aspx` página en Visual Studio
2. Vaya al marcado declarativo de la página (haga clic en el botón de origen en la parte inferior de la página)
3. Copie el texto dentro de la `<asp:Content>` y `</asp:Content>` etiquetas (las líneas 3 a 44), como se muestra en la figura 2.


[![Copie el texto dentro de la &lt;asp: Content&gt; Control](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**Figura 2**: copiar el texto dentro de la `<asp:Content>` Control ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. Abra el `UIValidation.aspx` página
2. Vaya al marcado declarativo de la página
3. Pegue el texto dentro de la `<asp:Content>` control.

Para copiar el código de origen, abra el `DataModificationEvents.aspx.cs` página y copie el texto *en* el `EditInsertDelete_DataModificationEvents` clase. Copie los controladores de eventos de tres (`Page_Load`, `GridView1_RowUpdating`, y `ObjectDataSource1_Inserting`), pero sí **no** copiar la declaración de clase o `using` instrucciones. Pegue el texto copiado *en* el `EditInsertDelete_UIValidation` clase `UIValidation.aspx.cs`.

Después de migrar el contenido y el código de `DataModificationEvents.aspx` a `UIValidation.aspx`, dedique un momento para probar su progreso en un explorador. Debería ver la misma salida y experimenta la misma funcionalidad en cada una de estas dos páginas (hacer referencia a la figura 1 para una captura de pantalla de `DataModificationEvents.aspx` en acción).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Paso 2: Convertir el BoundFields en TemplateFields

Para agregar controles de validación a las interfaces de edición e inserción, debe convertirse en TemplateFields el BoundFields utilizados por los controles GridView y DetailsView. Para ello, haga clic en los vínculos Editar columnas y campos de edición de GridView y de DetailsView etiquetas inteligentes, respectivamente. Allí, seleccione cada uno de los BoundFields y haga clic en el vínculo "Convert this field into a TemplateField".


[![Cada una de la de DetailsView y de GridView BoundFields convertir TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**Figura 3**: convertir cada uno de los de DetailsView y de GridView BoundFields en TemplateFields ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


Convertir un BoundField en TemplateField a través del cuadro de diálogo campos genera TemplateField que exhibe las mismas interfaces de solo lectura, edición e Insertar como el BoundField propio. El marcado siguiente muestra la sintaxis declarativa para el `ProductName` campo DetailsView después de se ha convertido en TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

Tenga en cuenta que este TemplateField tenía tres plantillas que se crean automáticamente `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate`. El `ItemTemplate` muestra un valor de campo de datos única (`ProductName`) con un control Web Label, mientras el `EditItemTemplate` y `InsertItemTemplate` presentar el valor del campo de datos en un control de cuadro de texto Web que asocia el campo de datos en el cuadro de texto `Text` propiedad con enlace de datos bidireccional. Puesto que estamos usando solo DetailsView en esta página para insertar, puede quitar el `ItemTemplate` y `EditItemTemplate` desde los dos TemplateFields, aunque no hay ningún riesgo en dejarlos.

Puesto que la GridView no admite la integradas de inserción de las características de DetailsView, convertir la GridView `ProductName` en TemplateField produce solo una `ItemTemplate` y `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

Al hacer clic en la función "Convert this field into a TemplateField", Visual Studio ha creado un TemplateField cuyas plantillas imitan la interfaz de usuario de la BoundField convertido. Para comprobarlo, puede visitar esta página a través de un explorador. Encontrará que es idéntica a la experiencia de la apariencia y comportamiento de la TemplateFields BoundFields que se usaron en su lugar.

> [!NOTE]
> No dude en Personalizar las interfaces de edición de las plantillas según sea necesario. Por ejemplo, puede que convenga tener el cuadro de texto el `UnitPrice` TemplateFields se representa como un cuadro de texto más pequeño que el `ProductName` cuadro de texto. Para realizar esta acción puede establecer el cuadro de texto `Columns` propiedad en un valor adecuado o proporcionar un ancho absoluto a través de la `Width` propiedad. En el siguiente tutorial veremos cómo personalizar completamente la interfaz de edición si se reemplaza el cuadro de texto con una entrada de datos alternativos control Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Paso 3: Agregar los controles de validación a la GridView`EditItemTemplate` s

Al construir los formularios de entrada de datos, es importante que los usuarios escriban los campos obligatorios y que todas las entradas proporcionadas son valores legales, tiene el formato correcto. Para ayudar a garantizar que las entradas del usuario son válidas, ASP.NET proporciona cinco controles de validación integradas que están diseñados para usarse para validar el valor de un control de entrada:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantiza que se ha proporcionado un valor
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida un valor con otro valor de control de Web o un valor constante, o se asegura de que el formato del valor es válido para un tipo de datos especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantiza que un valor está dentro del intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida un valor con un [expresión regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida un valor con un método personalizado definido por el usuario

Para obtener más información sobre estos cinco controles, consulte el [sección de controles de validación](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) de la [tutoriales de ASP.NET](https://asp.net/QuickStart/aspnet/).

En este tutorial se deberá usar un control RequiredFieldValidator en las de GridView y DetailsView `ProductName` TemplateFields y un control RequiredFieldValidator v ovládacím prvku DetailsView `UnitPrice` TemplateField. Además, se deberá agregar un control CompareValidator para ambos controles `UnitPrice` TemplateFields que garantiza que el precio indicado tiene un valor mayor o igual que 0 y se presenta en un formato de moneda válidos.

> [!NOTE]
> Aunque ASP.NET 1.x tenía estos mismos controles de validación de cinco, ASP.NET 2.0 ha agregado una serie de mejoras, los principales dos de script de cliente compatibilidad con exploradores distintos de Internet Explorer y la capacidad de los controles de validación de partición en una página en grupos de validación. Para obtener más información sobre las nuevas características de control de validación de 2.0, consulte [Disecar los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Comencemos por agregar los controles de validación necesaria para la `EditItemTemplate` s en TemplateFields de GridView. Para ello, haga clic en el vínculo Editar plantillas de etiqueta inteligente de GridView para que aparezca la interfaz de edición de plantilla. Desde aquí, puede seleccionar qué plantilla para editar en la lista desplegable. Puesto que deseamos aumentar la interfaz de edición, tenemos que agregar los controles de validación para el `ProductName` y `UnitPrice`del `EditItemTemplate` s.


[![Es necesario ampliar el ProductName y EditItemTemplates de UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**Figura 4**: es necesario extender el `ProductName` y `UnitPrice`del `EditItemTemplate` s ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


En el `ProductName` `EditItemTemplate`, agregue un control RequiredFieldValidator arrastrándolo desde el cuadro de herramientas en la interfaz de edición de plantilla, colocar después el cuadro de texto.


[![Agregar un control RequiredFieldValidator a ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**Figura 5**: agregar un control RequiredFieldValidator a la `ProductName` `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


Todos los controles de validación funcionan mediante la validación de la entrada de un solo control Web de ASP.NET. Por lo tanto, debemos indicar que debe validar el control RequiredFieldValidator que acabamos de agregar en el cuadro de texto en el `EditItemTemplate`; Esto se logra estableciendo el control de validación [propiedad ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) a la `ID` del control Web adecuado. El cuadro de texto tiene actualmente en su lugar no distintivo `ID` de `TextBox1`, pero vamos a cambiar a algo más adecuado. Haga clic en el cuadro de texto en la plantilla y, a continuación, en la ventana Propiedades, cambie la `ID` desde `TextBox1` a `EditProductName`.


[![Cambie el identificador del cuadro de texto a EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**Figura 6**: cambiar el cuadro de texto `ID` a `EditProductName` ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


A continuación, establezca el RequiredFieldValidator `ControlToValidate` propiedad `EditProductName`. Por último, establezca el [propiedad ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "Debe proporcionar el nombre del producto" y el [propiedad Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a "\*". El `Text` valor de propiedad, si se proporciona, es el texto que se muestra el control de validación si se produce un error en la validación. El `ErrorMessage` el valor de propiedad, que es necesario, se utiliza el control ValidationSummary; si el `Text` se omite el valor de propiedad, el `ErrorMessage` valor de propiedad también es el texto mostrado por el control de validación de entrada no válida.

Después de establecer estas tres propiedades de RequiredFieldValidator, la pantalla debe ser similar a la figura 7.


[![Establecer el RequiredFieldValidator ControlToValidate ErrorMessage y propiedades de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**Figura 7**: establecer el RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, y `Text` propiedades ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


Con el control RequiredFieldValidator agregado a la `ProductName` `EditItemTemplate`, todos los que queda es agregar la validación necesaria para la `UnitPrice` `EditItemTemplate`. Puesto que hemos decidido que, para esta página, el `UnitPrice` opcional cuando edita un registro, no tenemos que agregar un control RequiredFieldValidator. , Sin embargo, necesitamos agregar un control CompareValidator para asegurarse de que el `UnitPrice`, si se proporciona, se tiene el formato correcto como moneda y es mayor o igual que 0.

Antes de agregar el control CompareValidator para el `UnitPrice` `EditItemTemplate`, vamos a cambiar primero Id. del control de cuadro de texto Web desde `TextBox2` a `EditUnitPrice`. Después de realizar este cambio, agregar CompareValidator, establecer su `ControlToValidate` propiedad `EditUnitPrice`, sus `ErrorMessage` propiedad "el precio debe ser mayor o igual que cero y no puede incluir el símbolo de moneda" y su `Text` propiedad "\*".

Para indicar que el `UnitPrice` valor debe ser mayor o igual que 0, establezca el CompareValidator [propiedad Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, sus [propiedad ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" y su [ La propiedad Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`. La siguiente sintaxis declarativa la `UnitPrice` del TemplateField `EditItemTemplate` una vez realizados estos cambios:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

Después de realizar estos cambios, abra la página en un explorador. Si intenta omitir el nombre o escriba un valor del precio no válido al editar un producto, aparece un asterisco junto al cuadro de texto. Como se muestra en la figura 8, un valor del precio que incluye el símbolo de moneda, como $19,95 se considera no válido. El control de CompareValidator `Currency` `Type` permite separadores de dígitos (por ejemplo, comas ni puntos, dependiendo de la configuración de la referencia cultural) y un líder plus o signo menos, pero *no* permiten un símbolo de moneda. Este comportamiento puede perplex a los usuarios como la interfaz de edición actualmente representa la `UnitPrice` con el formato de moneda.

> [!NOTE]
> Recuerde que en el *eventos asociados con la inserción, actualización y eliminación* tutorial establecemos el BoundField `DataFormatString` propiedad `{0:c}` con el fin de dar el formato de una moneda. Además, hemos establecido el `ApplyFormatInEditMode` propiedad en true, lo que hace que el control GridView de edición del interfaz para dar formato a la `UnitPrice` como una moneda. Al convertir el BoundField en TemplateField, Visual Studio se ha indicado a esta configuración y con el formato del cuadro de texto `Text` propiedad como una moneda mediante la sintaxis de enlace de datos `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Aparece un asterisco junto a los cuadros de texto con la entrada no válida](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**Figura 8**: un asterisco aparece junto a los cuadros de texto con la entrada no válida ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


Mientras que las obras de validación como-es, el usuario debe quitar manualmente el símbolo de moneda al editar un registro, que no es aceptable. Para solucionar esto, tenemos tres opciones:

1. Configurar la `EditItemTemplate` para que el `UnitPrice` valor no tiene el formato como una moneda.
2. Permitir al usuario que escriba un símbolo de moneda quitando CompareValidator y reemplazarlo por un control RegularExpressionValidator que comprueba correctamente un valor de moneda con formato correcto. El problema aquí es que la expresión regular para validar un valor de moneda no es más bonito del mundo y requeriría la escritura de código si quisiéramos incorporar las configuraciones de referencia cultural.
3. Quitar totalmente el control de validación y se basan en la lógica de validación del lado servidor en el control de GridView `RowUpdating` controlador de eventos.

Vamos a con la opción #1 para este ejercicio. Actualmente la `UnitPrice` se da formato como una moneda debido a la expresión de enlace de datos para el cuadro de texto en el `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Cambie la instrucción de enlace a `Bind("UnitPrice", "{0:n2}")`, lo que da formato al resultado como un número con dos dígitos de precisión. Esto se puede hacer directamente a través de la sintaxis declarativa o haciendo clic en el vínculo Editar DataBindings de la `EditUnitPrice` cuadro de texto en el `UnitPrice` del TemplateField `EditItemTemplate` (consulte las figuras 9 y 10).


[![Haga clic en vínculo de Editar DataBindings del cuadro de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**Figura 9**: haga clic en vínculo de Editar DataBindings del cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![Especifique el especificador de formato en la instrucción de enlace](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**Figura 10**: especifique el especificador de formato en el `Bind` instrucción ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


Con este cambio, el precio con formato en la interfaz de edición incluye la coma como separador de grupo y un punto como separador decimal, pero se cierra el símbolo de moneda.

> [!NOTE]
> El `UnitPrice` `EditItemTemplate` no incluye un control RequiredFieldValidator, lo que permite negativas derivadas de la devolución de datos y la lógica de actualización para comenzar. Sin embargo, el `RowUpdating` controlador de eventos que se copian desde el *examinando los eventos asociados con la inserción, actualización y eliminación* tutorial incluye una comprobación de programación que garantiza que el `UnitPrice` se proporciona. No dude en quitar esta lógica, déjelo en como-es, o agregar un control RequiredFieldValidator a la `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Paso 4: Resumen de problemas de entrada de datos

Además de los controles de validación de cinco, ASP.NET incluye el [control ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que muestra la `ErrorMessage` s de los controles de validación que ha detectado datos no válidos. Estos datos de resumen se pueden mostrar como texto en la página web o a través de un cuadro de mensaje modal, del lado cliente. Vamos a mejorar este tutorial para que incluya un cuadro de mensajes del lado cliente que resume los problemas de validación.

Para ello, arrastre un control ValidationSummary desde el cuadro de herramientas hasta el diseñador. La ubicación del control de validación no importa realmente, puesto que vamos a configurarlo para mostrar únicamente el resumen como un cuadro de mensajes. Después de agregar el control, establezca su [propiedad ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) a `false` y su [propiedad ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `true`. Con esta versión, se resumen los errores de validación en un cuadro de mensajes del lado cliente.


[![Los errores de validación se resumen en un cuadro de mensajes del lado cliente](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**Figura 11**: los errores de validación se resumen en un cuadro de mensajes del lado cliente ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Paso 5: Agregar los controles de validación al prvku DetailsView.`InsertItemTemplate`

Todo lo que queda de este tutorial es agregar los controles de validación a la interfaz de inserción de DetailsView. El proceso de agregar controles de validación a las plantillas de DetailsView es idéntico a la se examinan en el paso 3; por lo tanto, podrá avanzar la tarea en este paso. Igual que hicimos con la GridView `EditItemTemplate` s, le animo a cambiar el nombre de la `ID` s de los cuadros de texto de la no distintivo `TextBox1` y `TextBox2` a `InsertProductName` y `InsertUnitPrice`.

Agregue un control RequiredFieldValidator a la `ProductName` `InsertItemTemplate`. Establecer el `ControlToValidate` a la `ID` del cuadro de texto en la plantilla, su `Text` propiedad en "\*" y su `ErrorMessage` propiedad "Debe proporcionar el nombre del producto".

Puesto que la `UnitPrice` es necesario para esta página cuando se agrega un nuevo registro, agregue un control RequiredFieldValidator a la `UnitPrice` `InsertItemTemplate`, estableciendo su `ControlToValidate`, `Text`, y `ErrorMessage` propiedades adecuadamente. Por último, agregue un control CompareValidator para los `UnitPrice` `InsertItemTemplate` , y configurar su `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, y `ValueToCompare` propiedades tal como se hizo con la `UnitPrice`de CompareValidator en el control de GridView `EditItemTemplate`.

Después de agregar estos controles de validación, un producto nuevo no se puede agregar al sistema si no se proporciona su nombre o si su precio es un número negativo o forma no autorizada con formato.


[![Lógica de validación se ha agregado a la interfaz de inserción de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**Figura 12**: lógica de validación se ha agregado a la interfaz de inserción de DetailsView ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Paso 6: Crear particiones de los controles de validación en los grupos de validación

Nuestra página consta de dos conjuntos dispares de forma lógica de los controles de validación: los que corresponden a la GridView de la interfaz de edición y los que corresponden a DetailsView de la inserción de interfaz. De forma predeterminada, se produce un postback *todas* se comprueban los controles de validación de la página. Sin embargo, al editar un registro no queremos controles de validación insertar de la interfaz de DetailsView para validar. Figura 13 se muestra este dilema actual cuando un usuario edita un producto con los valores legales perfectamente, haciendo clic en Update produce un error de validación porque los valores de nombre y el precio de la interfaz de inserción están en blanco.


[![Actualizar un producto hace que los controles de validación de la interfaz insertar Fire](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**Figura 13**: actualizar un producto hace que los controles de validación de la interfaz insertar Fire ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


Los controles de validación en ASP.NET 2.0 se pueden crear particiones en grupos de validación a través de sus `ValidationGroup` propiedad. Para asociar un conjunto de controles de validación de un grupo, basta con establecer sus `ValidationGroup` propiedad en el mismo valor. En este tutorial, establezca la `ValidationGroup` las propiedades de los controles de validación de TemplateFields de GridView para `EditValidationControls` y el `ValidationGroup` propiedades de TemplateFields ovládacího prvku DetailsView a `InsertValidationControls`. Estos cambios pueden realizarse directamente en el marcado declarativo o a través de la ventana Propiedades cuando se usa el diseñador edición la interfaz de la plantilla.

Además de la validación de controles, el botón y los controles de botón en ASP.NET 2.0 también incluyen un `ValidationGroup` propiedad. Validadores de un grupo de validación se comprueban la validez solo cuando una devolución de datos sea inducido por un botón que tiene el mismo `ValidationGroup` configuración de la propiedad. Por ejemplo, en orden en de DetailsView botón de inserción desencadenar la `InsertValidationControls` grupo de validación se debe establecer el CommandField `ValidationGroup` propiedad `InsertValidationControls` (vea la figura 14). Además, establezca la GridView del CommandField `ValidationGroup` propiedad `EditValidationControls`.


[![Propiedad de ValidationGroup del CommandField a InsertValidationControls de conjunto DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**Figura 14**: establecer el DetailsView de CommandField `ValidationGroup` propiedad a `InsertValidationControls` ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


Después de estos cambios, el DetailsView de GridView TemplateFields y CommandFields debe ser similar al siguiente:

El DetailsView TemplateFields y CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

La GridView CommandField y TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

En este momento los controles de validación específica de la edición activan sólo cuando se hace clic botón de actualización de GridView y el fuego de controles de validación específica de la inserción solo cuando se hace clic en botón de inserción de DetailsView, resolver el problema de resaltado en la figura 13. Sin embargo, con este cambio nuestro control ValidationSummary ya no se muestra cuando se ingresan datos no válidos. El control ValidationSummary también contiene un `ValidationGroup` propiedad y únicamente se muestra información de resumen para los controles de validación en su grupo de validación. Por lo tanto, necesitamos tener dos controles de validación en esta página, uno para el `InsertValidationControls` grupo de validación y otro para `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

Con esta adición finalizada en este tutorial.

## <a name="summary"></a>Resumen

Si bien pueden proporcionar BoundFields tanto una interfaz de inserción y edición, la interfaz no es personalizable. Normalmente, queremos agregar controles de validación para la edición y la inserción de interfaz para asegurarse de que el usuario escribe las entradas necesarias en un formato válido. Para ello, debemos convertir la BoundFields TemplateFields y agregar los controles de validación a las plantillas adecuadas. En este tutorial se ha ampliado el ejemplo desde el *examinando los eventos asociados con la inserción, actualización y eliminación* tutorial, agregar controles de validación a ambos DetailsView de la inserción de interfaz y la GridView interfaz de edición. Además, hemos visto cómo mostrar información de resumen de validación mediante el control ValidationSummary y cómo dividir los controles de validación en la página en grupos distintos de validación.

Como hemos visto en este tutorial, TemplateFields permiten a las interfaces de edición e inserción para ampliarse para incluir controles de validación. TemplateFields también puede ampliar para incluir controles de entrada Web adicionales, habilitar el cuadro de texto se reemplacen por un control Web más adecuado. En nuestro siguiente tutorial veremos cómo reemplazar el control de cuadro de texto con un control DropDownList enlazados a datos, que es ideal cuando se edita una clave externa (como `CategoryID` o `SupplierID` en el `Products` tabla).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Liz Shulok y Zack Jones. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
> [Siguiente](customizing-the-data-modification-interface-cs.md)
