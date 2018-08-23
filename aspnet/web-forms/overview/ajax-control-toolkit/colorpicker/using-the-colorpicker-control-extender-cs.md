---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Usar el extensor del Control ColorPicker (C#) | Microsoft Docs
author: microsoft
description: ColorPicker es un extensor de AJAX de ASP.NET que proporciona la funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control emergente. Se puede conectar a cualquier ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b8d581ed426227ed77435e22c84e9ea5d62ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836998"
---
<a name="using-the-colorpicker-control-extender-c"></a>Usar el extensor del Control ColorPicker (C#)
====================
por [Microsoft](https://github.com/microsoft)

> ColorPicker es un extensor de AJAX de ASP.NET que proporciona la funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control emergente. Se puede conectar a cualquier control de cuadro de texto de ASP.NET. Él.


El objetivo de este tutorial es explicar cómo puede usar el extensor de control de AJAX Control Toolkit ColorPicker. El extensor de control ColorPicker muestra un cuadro de diálogo emergente que le permite seleccionar un color. ColorPicker es útil cuando desea proporcionar una interfaz de usuario intuitiva para un usuario elegir un color.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Ampliación de un Control de cuadro de texto con el extensor del Control ColorPicker

Por ejemplo, imagine que desea crear un sitio Web que permite a los visitantes crear tarjetas de presentación personalizadas. Los visitantes pueden escribir el texto para una tarjeta de presentación y seleccionar el color. La página ASP.NET en el listado 1 contiene dos controles TextBox denominados txtCardText y txtCardColor. Cuando se envía el formulario, se muestran los valores seleccionados (consulte la figura 1).


[![Formulario simple para crear una tarjeta de presentación](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Figura 01**: un formulario Simple para crear una tarjeta de presentación ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Listado 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

El formulario en el listado 1 funciona, pero no proporciona una experiencia de usuario excelente. El usuario debe escribir un color en el cuadro de texto. Si el usuario desea un color especializado - por ejemplo, solo el derecho tono verde pea -, a continuación, el usuario debe averiguar el código de color HTML sin ayuda.

Puede usar el extensor de control ColorPicker para crear una mejor experiencia de usuario. ColorPicker muestra un cuadro de diálogo color al mover el foco a un control de cuadro de texto (consulte la figura 2).


[![El extensor del Control ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Figura 02**: el extensor del Control ColorPicker ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image4.png))


Es preciso realizar dos pasos para usar el extensor de control ColorPicker con el formulario en el listado 1:

1. Agregar un control ScriptManager a la página
2. Agregue el extensor de control ColorPicker a la página

Para poder usar ColorPicker, debe agregar un ScriptManager en la página. Es un buen lugar para agregar el control ScriptManager justo debajo de la apertura del servidor &lt;formulario&gt; etiqueta. Puede arrastrar el control ScriptManager hasta la página del cuadro de herramientas (ScriptManager se encuentra en la pestaña Extensiones de AJAX). Como alternativa, puede escribir la siguiente etiqueta en la vista del origen de debajo de la etiqueta de formulario de servidor de apertura:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;

Es la manera más fácil de agregar el extensor de control ColorPicker a la página en la vista Diseño. Si se mantenga el mouse sobre el cuadro de texto txtCardColor, una opción de tarea inteligente aparece permite agregar un extensor (consulte la figura 3). Si elige esta opción, aparece el Asistente de extensor (consulte la figura 4).


[![Adición de un extensor](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Figura 03**: adición de un extensor ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Seleccionar un extensor de control con el Asistente para Extender](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Figura 04**: selección de un extensor de control con el Asistente para Extender ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image8.png))


Puede elegir el extensor ColorPicker para ampliar el cuadro de texto txtCardColor con el extensor ColorPicker. Haga clic en Aceptar para cerrar el cuadro de diálogo.

Después de realizar estos cambios, el origen de la página parece listado 2.

Listado 2 - CreateCard.aspx (con ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Tenga en cuenta que la página ahora contiene un control ColorPickerExtender que aparece justo debajo del control TextBox txtCardColor. El control ColorPickerExtender extiende el control de txtCardColor para que se muestre un cuadro de diálogo del selector de color.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usar un botón para iniciar el cuadro de diálogo del selector de Color

El extensor ColorPicker admite las siguientes propiedades:

- PopupButtonId - el identificador de un botón en la página que hace que el cuadro de diálogo del selector de color que aparezca.
- PopupPosition: la posición en relación con el control de destino, del cuadro de diálogo de selector de color. Los valores posibles son absolutos, centro, BottomLeft, BottomRight, TopLeft, superior, derecha e izquierda (el valor predeterminado es BottomLeft).
- SampleControlId - el identificador de un control que muestra el color seleccionado.
- SelectedColor - color seleccionado por el componente ColorPicker inicial.

Puede usar estas propiedades para personalizar cómo se muestra el cuadro de diálogo del selector de color y cómo se muestra el color seleccionado. La página en el listado 3 muestra cómo puede usar varias de estas propiedades.

**Listado 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

La página en el listado 3 incluye seleccionar un Color (consulte la figura 5). Al hacer clic en este botón, aparece el cuadro de diálogo del selector de color por encima del cuadro de texto. Si selecciona un color en el cuadro de diálogo color seleccionado aparece como el color de fondo de la lblSample control de etiqueta.

La propiedad ColorPicker PopupButtonID se utiliza para asociar el extensor ColorPicker en el botón Seleccionar Color. Al proporcionar un valor para la propiedad PopupButtonID, el cuadro de diálogo del selector de color ya no aparece cuando el control de destino tiene el foco. Debe hacer clic en el botón para mostrar el cuadro de diálogo.

La propiedad SampleControlID se utiliza para asociar un control que muestra el color seleccionado con ColorPicker. ColorPicker cambia el color de fondo de este control para el color seleccionado actualmente.


[![Mostrar el cuadro de diálogo del selector de color con un botón](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Figura 05**: mostrar el cuadro de diálogo del selector de color con un botón ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a usar el extensor de control ColorPicker para mostrar un cuadro de diálogo de selector de colores emergente. En primer lugar, hemos visto cómo se puede mostrar el cuadro de diálogo cuando se mueve el foco a un control de cuadro de texto. A continuación, ha aprendido a crear un botón que muestra el cuadro de diálogo del selector de color cuando se hace clic en el botón.

> [!div class="step-by-step"]
> [Siguiente](using-the-colorpicker-control-extender-vb.md)
