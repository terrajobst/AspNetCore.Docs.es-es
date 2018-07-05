---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Agregar controles de validación para el control DataList de la edición (interfaz) (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos lo fácil que es agregar los controles de validación a EditItemTemplate de DataList con el fin de proporcionar un int de usuario de edición más infalible...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 8a9fe298b7b9cc9fadc4dfd131a7a61d6029da04
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842710"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Agregar controles de validación para la interfaz de edición de DataList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) o [descargar PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> En este tutorial veremos lo fácil que es agregar los controles de validación a EditItemTemplate de DataList para proporcionar una interfaz de usuario de edición más infalible.


## <a name="introduction"></a>Introducción

En el control DataList edición tutoriales hasta ahora, las interfaces de edición de DataList no incluyó ninguna validación de entrada de usuario automático, aunque el usuario no válido de entrada como un nombre de producto que faltan o los resultados de precio negativo en una excepción. En el [tutorial anterior](handling-bll-and-dal-level-exceptions-cs.md) hemos visto cómo agregar código para el control DataList s de control de excepciones `UpdateCommand` controlador de eventos con el fin de detectar y mostrar correctamente información sobre las excepciones que se han producido. Lo ideal es que, sin embargo, la interfaz de edición incluye controles de validación para impedir que un usuario entre dichos datos no válidos en primer lugar.

En este tutorial veremos lo fácil que es agregar los controles de validación para el control DataList s `EditItemTemplate` con el fin de proporcionar una interfaz de usuario de edición más infalible. En concreto, este tutorial toma el ejemplo creado en el tutorial anterior y aumenta la interfaz de edición para incluir validación adecuado.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Paso 1: Replicación en el ejemplo de[control de excepciones de nivel BLL y DAL](handling-bll-and-dal-level-exceptions-cs.md)

En el [BLL - control y las excepciones de nivel DAL](handling-bll-and-dal-level-exceptions-cs.md) tutorial hemos creado una página que muestra los nombres y los precios de los productos en un control DataList de dos columnas, puede editar. El objetivo de este tutorial es aumentar la interfaz de edición de DataList s para incluir controles de validación. En concreto, nuestra lógica de validación hará lo siguiente:

- Requerir que se proporcione el nombre del producto s
- Asegúrese de que el valor especificado para el precio es un formato de moneda válidos
- Asegúrese de que el valor especificado para el precio es mayor o igual a cero, desde un negativo `UnitPrice` valor no es válido

Antes de que podemos observar enriquecidas con el ejemplo anterior para incluir la validación, primero es necesario replicar el ejemplo desde el `ErrorHandling.aspx` página en el `EditDeleteDataList` carpeta a la página para este tutorial, `UIValidation.aspx`. Para lograr esto es necesario para copiar a través de ambos el `ErrorHandling.aspx` marcado declarativo s y su código fuente de página. Copiar el marcado declarativo realizando los pasos siguientes:

1. Abra el `ErrorHandling.aspx` página en Visual Studio
2. Vaya al marcado declarativo página s (haga clic en el botón de origen en la parte inferior de la página)
3. Copie el texto dentro de la `<asp:Content>` y `</asp:Content>` etiquetas (las líneas 3 a 32), como se muestra en la figura 1.


[![Copie el texto dentro de la &lt;asp: Content&gt; Control](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: copiar el texto dentro de la `<asp:Content>` Control ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Abra el `UIValidation.aspx` página
2. Vaya al marcado declarativo s página
3. Pegue el texto dentro de la `<asp:Content>` control.

Para copiar el código de origen, abra el `ErrorHandling.aspx.vb` página y copie el texto *en* el `EditDeleteDataList_ErrorHandling` clase. Copie los controladores de eventos de tres (`Products_EditCommand`, `Products_CancelCommand`, y `Products_UpdateCommand`) junto con el `DisplayExceptionDetails` método, pero no **no** copiar la declaración de clase o `using` instrucciones. Pegue el texto copiado *en* el `EditDeleteDataList_UIValidation` clase `UIValidation.aspx.vb`.

Después de migrar el contenido y el código de `ErrorHandling.aspx` a `UIValidation.aspx`, dedique un momento para probar las páginas en un explorador. Debería ver la misma salida y experimenta la misma funcionalidad en cada una de estas dos páginas (consulte la figura 2).


[![La página UIValidation.aspx imita la funcionalidad en ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: el `UIValidation.aspx` página imita la funcionalidad en `ErrorHandling.aspx` ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Paso 2: Agregar los controles de validación a DataList s EditItemTemplate

Al construir los formularios de entrada de datos, es importante que los usuarios escriban los campos obligatorios y que todas sus entradas proporcionadas son valores legales, tiene el formato correcto. Para ayudar a garantizar que un usuario de s es válidas, ASP.NET proporciona cinco controles de validación integradas que están diseñados para validar el valor de un control Web de entrada:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantiza que se ha proporcionado un valor
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida un valor con otro valor de control de Web o un valor constante, o se asegura de que el formato de valor s es válido para un tipo de datos especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantiza que un valor está dentro del intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida un valor con un [expresión regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida un valor con un método personalizado definido por el usuario

Para obtener más información sobre estos cinco controles hacer referencia a la [agregar controles de validación a las Interfaces de inserción y edición](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial o desproteger el [sección de controles de validación](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) de la [Tutoriales rápidos de ASP.NET](https://quickstarts.asp.net).

En este tutorial se deberá usar un control RequiredFieldValidator para asegurarse de que se ha proporcionado un valor para el nombre del producto y un control CompareValidator para asegurarse de que el precio indicado tiene un valor mayor o igual que 0 y se presenta en un formato de moneda válidos.

> [!NOTE]
> Aunque ASP.NET 1.x tenía estos mismos controles de validación de cinco, ASP.NET 2.0 ha agregado una serie de mejoras, principal dos de script de cliente compatibilidad con exploradores, además de Internet Explorer y la capacidad de los controles de validación de partición en una página en grupos de validación. Para obtener más información sobre las nuevas características de control de validación de 2.0, consulte [Disecar los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Permiten s empiece agregando los controles de validación necesarias para el control DataList s `EditItemTemplate`. Esta tarea puede realizarse a través del diseñador, haga clic en el vínculo Editar plantillas de la etiqueta inteligente de DataList s o a través de la sintaxis declarativa. Permiten paso s a través del proceso mediante la opción Editar plantillas de la vista de diseño. Después de editar el control DataList s `EditItemTemplate`, agregue un control RequiredFieldValidator arrastrándolo desde el cuadro de herramientas en la interfaz de edición de plantilla, colóquela después de la `ProductName` cuadro de texto.


[![Agregue un control RequiredFieldValidator a EditItemTemplate después el cuadro de texto ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: agregar un control RequiredFieldValidator a la `EditItemTemplate After` el `ProductName` TextBox ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Todos los controles de validación funcionan mediante la validación de la entrada de un solo control Web de ASP.NET. Por lo tanto, debemos indicar que el control RequiredFieldValidator que acabamos de agregar debería validar la `ProductName` TextBox; Esto se hace estableciendo el control de validación s [ `ControlToValidate` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) a la `ID` de el control Web adecuado (`ProductName`, en este caso). A continuación, establezca el [ `ErrorMessage` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) a debe proporcionar el nombre del producto s y la [ `Text` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a \*. El `Text` valor de propiedad, si se proporciona, es el texto que se muestra el control de validación si se produce un error en la validación. El `ErrorMessage` el valor de propiedad, que es necesario, se utiliza el control ValidationSummary; si el `Text` se omite el valor de propiedad, el `ErrorMessage` se muestra el valor de propiedad mediante el control de validación de entrada no válida.

Después de establecer estas tres propiedades de RequiredFieldValidator, la pantalla debe ser similar a la figura 4.


[![Establecer las propiedades de texto, ErrorMessage y RequiredFieldValidator s ControlToValidate](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: establecer la s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, y `Text` propiedades ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Con el control RequiredFieldValidator agregado a la `EditItemTemplate`, todos los que queda es agregar la validación es necesaria para el precio del producto s cuadro de texto. Puesto que la `UnitPrice` es opcional cuando se edita un registro, no queremos t necesidad de agregar un control RequiredFieldValidator. , Sin embargo, necesitamos agregar un control CompareValidator para asegurarse de que el `UnitPrice`, si se proporciona, se tiene el formato correcto como moneda y es mayor o igual que 0.

Agregar CompareValidator en el `EditItemTemplate` y establezca su `ControlToValidate` propiedad `UnitPrice`, sus `ErrorMessage` propiedad en el precio debe ser mayor o igual que cero y no puede incluir el símbolo de moneda y su `Text` propiedad \*. Para indicar que el `UnitPrice` valor debe ser mayor o igual que 0, establezca la s CompareValidator [ `Operator` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) a `GreaterThanEqual`, sus [ `ValueToCompare` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) en 0, y su [ `Type` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) a `Currency`.

Después de agregar estos controles de validación de dos, el control DataList s `EditItemTemplate` sintaxis declarativa s debe ser similar al siguiente:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Después de realizar estos cambios, abra la página en un explorador. Si intenta omitir el nombre o escriba un valor del precio no válido al editar un producto, aparece un asterisco junto al cuadro de texto. Como se muestra en la figura 5, un valor del precio que incluye el símbolo de moneda, como $19,95 se considera no válido. Las operaciones de asignación CompareValidator `Currency` `Type` permite separadores de dígitos (por ejemplo, comas ni puntos, dependiendo de la configuración de la referencia cultural) y un líder plus o signo menos, pero *no* permiten un símbolo de moneda. Este comportamiento puede perplex a los usuarios como la interfaz de edición actualmente representa la `UnitPrice` con el formato de moneda.


[![Aparece un asterisco junto a los cuadros de texto con la entrada no válida](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: un asterisco aparece junto a los cuadros de texto con la entrada no válida ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Mientras que las obras de validación como-es, el usuario debe quitar manualmente el símbolo de moneda al editar un registro, que no es aceptable. Además, si hay no válido de entradas en la edición interfaz ni la actualización ni cancelar botones, al hacer clic en, se invocará una devolución de datos. Idealmente, el botón Cancelar devolvería al control DataList a su estado previo edición independientemente de la validez de las entradas de usuario s. Además, debemos asegurarnos de que los datos de la página s están válidos antes de actualizar la información del producto en el control DataList s `UpdateCommand` controlador de eventos, como los controles de validación del lado cliente lógica puede omitirse si los usuarios con exploradores don soporte t JavaScript o bien tienen su compatibilidad deshabilitado.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Quitar el símbolo de moneda del cuadro de texto de EditItemTemplate s UnitPrice

Cuando se usa la s CompareValidator `Currency``Type`, la entrada que se va a validar no puede contener cualquier símbolo de divisa. La presencia de estos símbolos hace CompareValidator marcar la entrada como no válida. Sin embargo, nuestra interfaz de edición actualmente incluye un símbolo de moneda en la `UnitPrice` cuadro de texto, lo que significa que el usuario debe quitar explícitamente el símbolo de divisa antes de guardar sus cambios. Para solucionar este problema, tiene tres opciones:

1. Configurar la `EditItemTemplate` para que el `UnitPrice` valor del cuadro de texto no tiene el formato como una moneda.
2. Permitir al usuario que escriba un símbolo de moneda quitando CompareValidator y reemplazarlo por un control RegularExpressionValidator que busca un valor de moneda con formato correcto. Aquí el desafío es que la expresión regular para validar un valor de moneda no es tan sencilla como CompareValidator y requeriría la escritura de código si quisiéramos incorporar las configuraciones de referencia cultural.
3. Quitar totalmente el control de validación y se basan en la lógica de validación del lado servidor personalizada en la s GridView `RowUpdating` controlador de eventos.

Permiten s ir con la opción 1 para este tutorial. Actualmente la `UnitPrice` se da formato como un valor de moneda a causa de la expresión de enlace de datos para el cuadro de texto en el `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Cambiar el `Eval` instrucción `Eval("UnitPrice", "{0:n2}")`, lo que da formato al resultado como un número con dos dígitos de precisión. Esto se puede hacer directamente a través de la sintaxis declarativa o haciendo clic en el vínculo Editar DataBindings de la `UnitPrice` cuadro de texto en el control DataList s `EditItemTemplate`.

Con este cambio, el precio con formato en la interfaz de edición incluye la coma como separador de grupo y un punto como separador decimal, pero se cierra el símbolo de moneda.

> [!NOTE]
> Al quitar el formato de moneda de la interfaz editable, encuentro útil para colocar el símbolo de moneda como texto fuera del cuadro de texto. Esto sirve como una sugerencia al usuario que no se necesitan para proporcionar el símbolo de moneda.


## <a name="fixing-the-cancel-button"></a>Corregir el botón Cancelar

De forma predeterminada, los controles de validación Web emiten JavaScript para realizar la validación del cliente. Cuando se hace clic en un botón, LinkButton o ImageButton, los controles de validación en la página se comprueban en el lado cliente antes de que se produzca la devolución. Si hay datos no válidos, se cancela la devolución de datos. Para determinados botones, sin embargo, la validez de los datos podría ser irrelevante; En tal caso, tener el postback cancelado debido a que los datos no válidos es una molestia.

El botón Cancelar es un ejemplo. Imagine que un usuario escribe datos no válidos, por ejemplo, si se omite el nombre del producto s y, a continuación, decide quieres guardar después de todo el producto y presiona el botón Cancelar. Actualmente, el botón Cancelar desencadena la en la página, los controles de validación que informan de que el nombre del producto falta y evitar la devolución de datos. El usuario tiene que escriba algún texto en el `ProductName` TextBox para cancelar el proceso de edición.

Afortunadamente, el botón, LinkButton y ImageButton tienen un [ `CausesValidation` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) que puede indicar si hace clic en el botón debe iniciar la lógica de validación (el valor predeterminado es `True`). Establezca el botón Cancelar de s `CausesValidation` propiedad `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Garantizar las entradas son válidos en el controlador de eventos UpdateCommand

Debido a la secuencia de comandos de cliente emitido por los controles de validación, los controles de validación cancelar las devoluciones de datos iniciadas por el botón LinkButton, si un usuario escribe una entrada no válida o ImageButton controles cuya propiedad `CausesValidation` propiedades son `True` (el valor predeterminado). Sin embargo, si un usuario visita con un explorador anticuado o cuya compatibilidad de JavaScript se ha deshabilitado, no se ejecutan las comprobaciones de validación del lado cliente.

Todos los controles de validación ASP.NET repita su lógica de validación inmediatamente tras la devolución y notificar la validez general de las entradas de página s a través de la [ `Page.IsValid` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Sin embargo, el flujo de la página no se interrumpe o se detiene en cualquier forma según el valor de `Page.IsValid`. Como desarrolladores, es nuestra responsabilidad para asegurarse de que el `Page.IsValid` propiedad tiene un valor de `True` antes de continuar con el código que se da por supuesto válido los datos de entrada.

Si un usuario tiene JavaScript deshabilitado, visita nuestra página, edita un producto, entra en un valor del precio de demasiado costosa y hace clic en el botón de actualización, se omitirá la validación del lado cliente y se produzca un postback. En el postback, las páginas de ASP.NET `UpdateCommand` ejecuta el controlador de eventos y se produce una excepción al intentar analizar demasiado costoso para una `Decimal`. Puesto que tenemos control de excepciones, se controlarán correctamente este tipo de excepción, pero podemos impedir que los datos no válidos de pospuestas a través en primer lugar, al continuar solo con el `UpdateCommand` controlador de eventos si `Page.IsValid` tiene un valor de `True`.

Agregue el código siguiente al principio de la `UpdateCommand` controlador de eventos, inmediatamente antes de la `Try` bloque:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Con esta versión, el producto intentará actualizarse sólo si los datos enviados están válidos. Poder ganada t mayoría de los usuarios a una devolución de datos no válidos debido a los scripts de cliente de los controles de validación, pero los usuarios con exploradores don soporte t JavaScript o que tienen JavaScript admite deshabilitado, pueden omitir las comprobaciones de cliente y enviar datos no válidos.

> [!NOTE]
> El lector astuto Recuerde que al actualizar datos con el control GridView, es necesario comprobar explícitamente la `Page.IsValid` propiedad en nuestra clase de código subyacente de la página s. Esto es porque el control GridView consulta el `Page.IsValid` nos y solo se inicia con la actualización solo si se devuelve un valor de propiedad de `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Paso 3: Resumen de problemas de entrada de datos

Además de los controles de validación de cinco, ASP.NET incluye el [control ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que muestra la `ErrorMessage` s de los controles de validación que ha detectado datos no válidos. Estos datos de resumen se pueden mostrar como texto en la página web o a través de un cuadro de mensaje modal, del lado cliente. Permiten s mejorar este tutorial para que incluya un cuadro de mensajes del lado cliente que resume los problemas de validación.

Para ello, arrastre un control ValidationSummary desde el cuadro de herramientas hasta el diseñador. La ubicación del control ValidationSummary t importa realmente, desde que creamos re va a configurar para mostrar solo el resumen como un cuadro de mensajes. Después de agregar el control, establezca su [ `ShowSummary` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) a `False` y su [ `ShowMessageBox` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) a `True`. Con esta versión, se resumen los errores de validación en un cuadro de mensajes del lado cliente (consulte la figura 6).


[![Los errores de validación se resumen en un cuadro de mensajes del lado cliente](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: los errores de validación se resumen en un cuadro de mensajes del lado cliente ([haga clic aquí para ver imagen en tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo reducir la probabilidad de excepciones mediante el uso de controles de validación para asegurarse de forma proactiva que nuestras entradas de los usuarios son válidas antes de intentar usarlos en el flujo de trabajo de actualización. ASP.NET proporciona cinco controles Web de validación que están diseñados para inspeccionar un sitio Web determinado controlan las entradas s e informan sobre la validez de s de entrada. En este tutorial hemos usado dos de estos cinco controles RequiredFieldValidator y CompareValidator para asegurarse de que se proporcionó el nombre de producto s y que el precio tenía un formato de moneda con un valor mayor o igual que cero.

Agregar controles de validación al editar interfaz DataList s es tan sencillo como arrastrarlos hasta la `EditItemTemplate` desde el cuadro de herramientas y la configuración de un conjunto de propiedades. De forma predeterminada, los controles de validación emiten automáticamente script de validación del lado cliente; También proporcionan validación del lado servidor en el postback, almacena el resultado acumulado en el `Page.IsValid` propiedad. Para omitir la validación del lado cliente cuando se hace clic en un botón, LinkButton o ImageButton, establezca el botón s `CausesValidation` propiedad `False`. Además, antes de realizar las tareas con los datos presentados en el postback, asegúrese de que el `Page.IsValid` propiedad devuelve `True`.

Todo el control DataList edición tutoriales se ve examinar hasta ahora han tenido muy sencillas interfaces edición un cuadro de texto para el nombre del producto s y otra para el precio. Sin embargo, la interfaz de edición, puede contener una mezcla de diferentes controles de Web, como listas desplegables, calendarios, botones de opción, casillas y así sucesivamente. En nuestro siguiente tutorial daremos un vistazo a la creación de una interfaz que usa una variedad de controles Web.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Liz Shulok, Dennis Patterson y Ken Pespisa. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-cs.md)
> [Siguiente](customizing-the-datalist-s-editing-interface-cs.md)
