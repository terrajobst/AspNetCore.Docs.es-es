---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: "Agregar controles de validación al control DataList de edición (interfaz) (C#) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial veremos lo fácil que es agregar controles de validación al EditItemTemplate del control DataList con el fin de proporcionar un int de usuario de edición más infalible..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 06f3e59d0e6fd59a83934084422816360e915bd7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Agregar controles de validación a la interfaz de edición del control DataList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) o [descarga de PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> En este tutorial veremos lo fácil que es agregar controles de validación al EditItemTemplate del control DataList con el fin de proporcionar una interfaz de usuario de edición más infalible.


## <a name="introduction"></a>Introducción

En el control DataList edición tutoriales hasta ahora, la edición de interfaces de DataList no incluyó ninguna validación de entrada de usuario automático incluso si un usuario no válido de entrada como un nombre de producto que faltan o los resultados de precio negativo en una excepción. En el [tutorial anterior](handling-bll-and-dal-level-exceptions-cs.md) se examina cómo agregar código al control DataList s de control de excepciones `UpdateCommand` controlador de eventos con el fin de detectar y mostrar correctamente información sobre las excepciones que se produjeron. Lo ideal es que, sin embargo, la interfaz de edición incluye controles de validación para evitar que un usuario especifique estos datos no válidos en primer lugar.

En este tutorial veremos lo fácil que es agregar controles de validación al control DataList s `EditItemTemplate` con el fin de proporcionar una interfaz de usuario de edición más infalible. En concreto, este tutorial toma el ejemplo creado en el tutorial anterior y aumenta la interfaz de edición para incluir la validación adecuada.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Paso 1: Replicar en el ejemplo de[control de excepciones de nivel de DAL y BLL](handling-bll-and-dal-level-exceptions-cs.md)

En el [BLL - control y excepciones de nivel de la capa DAL](handling-bll-and-dal-level-exceptions-cs.md) tutorial se crea una página que muestra los nombres y los precios de los productos en un control DataList de dos columnas, puede editar. El objetivo de este tutorial consiste en aumentar la interfaz de edición de DataList s para incluir controles de validación. En concreto, nuestra lógica de validación hará lo siguiente:

- Requieren que se proporcione el nombre del producto s
- Asegúrese de que el valor especificado para el precio es un formato de moneda válidos
- Asegúrese de que el valor especificado para el precio es mayor o igual a cero, desde un negativo `UnitPrice` valor no es válido

Antes de que podemos observar aumentar el ejemplo anterior para incluir la validación, primero es necesario replicar en el ejemplo de la `ErrorHandling.aspx` página en el `EditDeleteDataList` carpeta a la página de este tutorial, `UIValidation.aspx`. Para lograr esto es necesario copiar a través de ambos la `ErrorHandling.aspx` página marcado declarativo s y su código fuente. En primer lugar copie en el marcado declarativo realizando los pasos siguientes:

1. Abra la `ErrorHandling.aspx` página en Visual Studio
2. Vaya al marcado declarativo de la página s (haga clic en el botón de origen en la parte inferior de la página)
3. Copie el texto dentro de la `<asp:Content>` y `</asp:Content>` etiquetas (líneas de 3 a 32), como se muestra en la figura 1.


[![Copie el texto dentro de la &lt;asp: Content&gt; Control](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: copiar el texto dentro de la `<asp:Content>` Control ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Abra la `UIValidation.aspx` página
2. Vaya al marcado declarativo de s de página
3. Pegue el texto dentro de la `<asp:Content>` control.

Para copiar sobre el código fuente, abra el `ErrorHandling.aspx.vb` página y copie solamente el texto *en* la `EditDeleteDataList_ErrorHandling` clase. Copie los tres controladores de eventos (`Products_EditCommand`, `Products_CancelCommand`, y `Products_UpdateCommand`) junto con el `DisplayExceptionDetails` método, pero no **no** copiar la declaración de clase o `using` las instrucciones. Pegar el texto copiado *en* el `EditDeleteDataList_UIValidation` clase `UIValidation.aspx.vb`.

Después de mover sobre el contenido y el código de `ErrorHandling.aspx` a `UIValidation.aspx`, tómese un momento para probar las páginas en un explorador. Debe ver el mismo resultado y experiencia de la misma funcionalidad en cada una de estas dos páginas (consulte la figura 2).


[![La página UIValidation.aspx imita la funcionalidad de ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: el `UIValidation.aspx` página imita la funcionalidad de `ErrorHandling.aspx` ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Paso 2: Agregar los controles de validación a la plantilla EditItemTemplate s de DataList

Al construir los formularios de entrada de datos, es importante que los usuarios especifiquen los campos obligatorios y que todas sus entradas proporcionadas son valores válidos, con un formato correcto. Para ayudar a garantizar que un entradas de s de usuario son válidas, ASP.NET proporciona cinco controles de validación integradas que están diseñados para validar el valor de un control Web de entrada único:

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx) garantiza que se ha proporcionado un valor
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx) valida un valor con otro valor de control de Web o un valor constante, o se asegura de que el formato de valor s es válido para un tipo de datos especificado
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx) garantiza que un valor está dentro del intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx) valida un valor con un [expresión regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) valida un valor con un método personalizado definido por el usuario

Para obtener más información sobre estos cinco controles hacen referencia a la [agregar controles de validación a las Interfaces de insertar y modificar](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial o desproteger el [sección de controles de validación](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) de la [Tutoriales rápidos de ASP.NET](https://quickstarts.asp.net).

En nuestro tutorial necesitamos usar un control RequiredFieldValidator para asegurarse de que se ha proporcionado un valor para el nombre del producto y un control CompareValidator para asegurarse de que el precio indicado tiene un valor mayor o igual que 0 y se presenta en un formato de moneda válidos.

> [!NOTE]
> Mientras que ASP.NET 1.x tenía estos mismos controles de validación de cinco, ASP.NET 2.0 se agregado una serie de mejoras, los principales dos que se va a script de cliente compatibilidad con exploradores además de Internet Explorer y la capacidad de los controles de validación de partición en una página en grupos de validación. Para obtener más información sobre las nuevas características de control de validación de 2.0, consulte [Diseccionando los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Permiten s empiece agregando los controles de validación necesarios para el control DataList s `EditItemTemplate`. Esta tarea puede realizarse a través del diseñador haciendo clic en el vínculo Editar plantillas de la etiqueta inteligente de DataList s, o a través de la sintaxis declarativa. Permiten paso s a través del proceso mediante la opción de editar plantillas de la vista de diseño. Después de elegir Editar el control DataList s `EditItemTemplate`, agregue un control RequiredFieldValidator arrastrándolo desde el cuadro de herramientas en la interfaz de edición de plantilla, colóquela después de la `ProductName` cuadro de texto.


[![Agregue un control RequiredFieldValidator al EditItemTemplate después el cuadro de texto ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: agregar un control RequiredFieldValidator a la `EditItemTemplate After` el `ProductName` cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Todos los controles de validación de trabajo mediante la validación de la entrada de un único control Web de ASP.NET. Por lo tanto, es necesario indicar que el control RequiredFieldValidator que acabamos de agregar debe validar contra la `ProductName` cuadro de texto; Esto se hace estableciendo el control de validación s [ `ControlToValidate` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) a la `ID` de el control Web adecuado (`ProductName`, en este caso). A continuación, establezca el [ `ErrorMessage` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) que debe proporcionar el nombre del producto s y [ `Text` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a \*. La `Text` valor de propiedad, si se proporciona, es el texto que se muestra el control de validación si se produce un error en la validación. El `ErrorMessage` se utiliza el valor de propiedad, que es necesario, mediante el control del sistema; si la `Text` se omite el valor de propiedad, el `ErrorMessage` valor de propiedad se muestra el control de validación de entrada no válida.

Después de establecer estas tres propiedades de RequiredFieldValidator, la pantalla debe ser similar a la figura 4.


[![Establecer las propiedades de texto, RequiredFieldValidator s ControlToValidate y ErrorMessage](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: establecer la s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, y `Text` propiedades ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Con el control RequiredFieldValidator agregado a la `EditItemTemplate`, todos los que queda es agregar la validación necesaria para el precio del producto s cuadro de texto. Puesto que la `UnitPrice` es opcional cuando se edita un registro, no queremos t se necesita agregar un control RequiredFieldValidator. , Sin embargo, necesitamos agregar un control CompareValidator para asegurarse de que el `UnitPrice`, si se proporciona, con un formato correcto como moneda y es mayor o igual que 0.

Agregar el control CompareValidator en el `EditItemTemplate` y establecer su `ControlToValidate` propiedad `UnitPrice`, sus `ErrorMessage` propiedad al precio debe ser mayor o igual que cero y no puede incluir el símbolo de moneda y su `Text` propiedad \*. Para indicar que la `UnitPrice` valor debe ser mayor o igual que 0, establezca la s CompareValidator [ `Operator` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, sus [ `ValueToCompare` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) en 0, y su [ `Type` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`.

Después de agregar estos controles de validación de dos, el control DataList s `EditItemTemplate` la sintaxis declarativa s debería ser similar al siguiente:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Después de realizar estos cambios, abra la página en un explorador. Si intenta omitir el nombre o escriba un valor de precios no válido durante la edición de un producto, aparece un asterisco junto al cuadro de texto. Como se muestra en la figura 5, un valor de precio que incluye el símbolo de moneda, como $19.95 se considera no válido. Las operaciones de asignación CompareValidator `Currency` `Type` permite separadores de dígitos (por ejemplo, comas o puntos, dependiendo de la configuración de la referencia cultural) y un signo más iniciales o signo, pero *no* permitir un símbolo de moneda. Este comportamiento puede perplex a los usuarios como la interfaz de edición representa actualmente la `UnitPrice` con el formato de moneda.


[![Aparece un asterisco junto a los cuadros de texto con entradas no válidas](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: un asterisco aparece junto a los cuadros de texto con entradas no válidas ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Mientras que las obras de validación como-es, el usuario debe quitar manualmente el símbolo de moneda al editar un registro, que no es aceptable. Además, si hay no válido de entradas en la edición interfaz ni la actualización ni cancelar botones, al hacer clic en, va a invocar una devolución de datos. Idealmente, el botón Cancelar devolvería a DataList a su estado anterior edición independientemente de la validez de las entradas de s de usuario. Además, necesitamos asegurarnos de que los datos de página s son válidos antes de actualizar la información del producto en el control DataList s `UpdateCommand` controlador de eventos, como los controles de validación lógica del lado del cliente puede omitirse si los usuarios cuyos exploradores don soporte t JavaScript o bien tienen su compatibilidad deshabilitado.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Quitar el símbolo de moneda en el cuadro de texto de EditItemTemplate s UnitPrice

Cuando se usa la s CompareValidator `Currency``Type`, la entrada que se está validada no debe incluir cualquier símbolo de divisa. La presencia de estos símbolos hace que el control CompareValidator marcar la entrada como no válida. Sin embargo, la interfaz de edición actualmente incluye un símbolo de moneda en la `UnitPrice` cuadro de texto, lo que significa que el usuario debe quitar explícitamente el símbolo de moneda antes de guardar sus cambios. Para solucionar esto tenemos tres opciones:

1. Configurar la `EditItemTemplate` para que la `UnitPrice` valor de cuadro de texto no tiene el formato como una moneda.
2. Permitir que el usuario escriba un símbolo de moneda quitando el control CompareValidator y reemplazarlo con un control RegularExpressionValidator que busca un valor de divisa con el formato correcto. El desafío es que la expresión regular para validar un valor de moneda no es tan sencilla como el control CompareValidator y requeriría escribir código si quería volver a incorporar la configuración de referencia cultural.
3. Quite por completo el control de validación y se basan en la lógica de validación de servidor personalizada en las operaciones de asignación GridView `RowUpdating` controlador de eventos.

Permiten s vaya con la opción 1 para este tutorial. Actualmente el `UnitPrice` se da formato como un valor de moneda a causa de la expresión de enlace de datos para el cuadro de texto en el `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Cambiar el `Eval` instrucción `Eval("UnitPrice", "{0:n2}")`, que da formato al resultado como un número con dos dígitos de precisión. Esto puede hacerse directamente a través de la sintaxis declarativa o haciendo clic en el vínculo Editar DataBindings de la `UnitPrice` cuadro de texto en el control DataList s `EditItemTemplate`.

Con este cambio, el precio con formato en la interfaz de edición incluye comas como separador de grupo y un punto como separador decimal, pero deja fuera el símbolo de moneda.

> [!NOTE]
> Al quitar el formato de moneda de la interfaz editable, resultarle útil para colocar el símbolo de moneda como texto fuera del cuadro de texto. Esto actúa como una sugerencia para el usuario que no necesitan proporcionar el símbolo de moneda.


## <a name="fixing-the-cancel-button"></a>Corregir el botón Cancelar

De forma predeterminada, los controles de validación Web emiten código JavaScript para realizar la validación en el lado del cliente. Cuando se hace clic en un botón, LinkButton o ImageButton, los controles de validación en la página se comprueban en el cliente antes de que se produzca la devolución. Si no hay ningún dato no válido, se cancela la devolución de datos. Para ciertos botones, sin embargo, la validez de los datos podría ser irrelevante; En tal caso, tener el postback cancelado por datos no válidos es una molestia.

El botón de cancelación es un ejemplo. Imagine que un usuario escribe datos no válidos, por ejemplo, si se omite el nombre del producto s y, a continuación, decide quieres guardar después de que todos los productos y presiona el botón Cancelar. Actualmente, el botón Cancelar desencadena los controles de validación en la página, que informan de que el nombre del producto falta y evitar que la devolución de datos. El usuario tiene que escriba algún texto en el `ProductName` cuadro de texto para cancelar para salir del proceso de edición.

Afortunadamente, el botón, LinkButton y ImageButton tienen un [ `CausesValidation` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.causesvalidation.aspx) que puede indicar si o no al hacer clic en el botón debe iniciar la lógica de validación (valor predeterminado es `True`). Establecer el botón Cancelar s `CausesValidation` propiedad `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Garantizar las entradas son válidos en el controlador de eventos UpdateCommand

Debido a la secuencia de comandos de cliente emitido por los controles de validación, los controles de validación cancelar las devoluciones de datos iniciadas por Button, LinkButton, si un usuario escribe una entrada no válida o ImageButton controla cuya `CausesValidation` propiedades son `True` (el valor predeterminado). Sin embargo, si está visitando a un usuario con un explorador anticuado o uno cuya compatibilidad de JavaScript se ha deshabilitado, no se ejecutan las comprobaciones de validación del lado cliente.

Todos los controles de validación de ASP.NET se repiten su lógica de validación inmediatamente después de la devolución de datos y la validez general de las entradas de página s a través de informes la [ `Page.IsValid` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.page.isvalid.aspx). Sin embargo, el flujo de la página no se interrumpe o detenido en cualquier forma según el valor de `Page.IsValid`. Como los desarrolladores, es la responsabilidad de garantizar que el `Page.IsValid` propiedad tiene un valor de `True` antes de continuar con el código que supone válido los datos de entrada.

Si un usuario tiene deshabilitado JavaScript, visite nuestra página, edita un producto, entra en un valor del precio de demasiado caro y hace clic en el botón de actualización, se omitirá la validación del lado cliente y se produzca una devolución de datos. En la devolución de datos, las páginas de ASP.NET `UpdateCommand` ejecuta el controlador de eventos y se produce una excepción al intentar analizar demasiado caro un `Decimal`. Puesto que disponemos de control de excepciones, una de estas excepciones se controlarán correctamente, pero podemos impedir que los datos no válidos de elementos secundarios a través en primer lugar por solo continuando con el `UpdateCommand` controlador de eventos si `Page.IsValid` tiene un valor de `True`.

Agregue el código siguiente al principio de la `UpdateCommand` controlador de eventos, inmediatamente antes de la `Try` bloque:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Con esta adición, el producto intentará actualizar solo si los datos enviados están válidos. Poder won t mayoría de los usuarios a una devolución de datos no válido debido a las secuencias de comandos del lado cliente de controles de validación, pero los usuarios cuyos exploradores don soporte t JavaScript o que tienen JavaScript admite deshabilitado, pueden omitir las comprobaciones de cliente y enviar datos no válidos.

> [!NOTE]
> El lector astuto Recuerde que al actualizar datos con el control GridView, se Professionals t tendrá que comprobar explícitamente la `Page.IsValid` propiedad en nuestra clase de código subyacente de la página s. Esto es porque la consulta de GridView el `Page.IsValid` propiedad para nosotros y lleva a cabo sólo con la actualización solo si devuelve un valor de `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Paso 3: Resumir los problemas de entrada de datos

Además de los controles de validación de cinco, ASP.NET incluye la [control ValidationSummary](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx), que muestra la `ErrorMessage` s de los controles de validación que ha detectado datos no válidos. Estos datos de resumen se pueden mostrar como texto en la página web o a través de un cuadro de mensaje modal, de cliente. Permiten s mejorar este tutorial para incluir un cuadro de mensajes del lado cliente resumir los problemas de validación.

Para ello, arrastre un control del cuadro de herramientas hasta el diseñador. La ubicación del control ValidationSummary t realmente importan, desde que se re va a configurar para mostrar solo el resumen como un cuadro de mensajes. Después de agregar el control, establezca su [ `ShowSummary` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) a `False` y su [ `ShowMessageBox` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `True`. Con esta adición, se resumen los errores de validación en un cuadro de mensajes de cliente (consulte la figura 6).


[![Los errores de validación se resumen en un cuadro de mensajes de cliente](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: los errores de validación se resumen en un cuadro de mensajes de cliente ([haga clic aquí para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo reducir la probabilidad de excepciones mediante el uso de controles de validación para asegurarse de forma proactiva que nuestro entradas de los usuarios sean válidas antes de intentar utilizarlos en el flujo de trabajo de actualización. ASP.NET proporciona cinco controles Web de validación que están diseñados para inspeccionar un Web determinado controlan entrada s e informan sobre la validez de s de entrada. En este tutorial se utilizaron dos de los cinco controles RequiredFieldValidator y el control CompareValidator para asegurarse de que se proporcionó el nombre del producto s y que el precio tenía un formato de moneda con un valor mayor o igual que cero.

Agregar controles de validación a las operaciones de asignación DataList edición interfaz es tan sencillo como arrastrarlos hasta la `EditItemTemplate` desde el cuadro de herramientas y la configuración de un conjunto de propiedades. De forma predeterminada, los controles de validación emiten automáticamente el script de validación del lado cliente; También proporcionan validación del lado servidor en la devolución de datos, almacenar el resultado acumulado en el `Page.IsValid` propiedad. Para omitir la validación del lado cliente cuando se hace clic en un botón, LinkButton o ImageButton, establezca el botón s `CausesValidation` propiedad `False`. Además, antes de realizar las tareas con los datos presentados en devolución de datos, asegúrese de que el `Page.IsValid` propiedad devuelve `True`.

Todos del control DataList tutoriales de edición se ha examinado hasta el momento han tenido muy sencillas interfaces de edición de un cuadro de texto para el nombre del producto s y otro para el precio. Sin embargo, la interfaz de edición, puede contener una combinación de controles Web diferentes, como listas desplegables, calendarios, botones de opción, casillas y así sucesivamente. En nuestro tutorial siguiente trataremos crear una interfaz que se usa una serie de controles Web.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisores para este tutorial fueron Liz Shulok, Dennis Patterson y Ken Pespisa. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](handling-bll-and-dal-level-exceptions-cs.md)
[Siguiente](customizing-the-datalist-s-editing-interface-cs.md)
