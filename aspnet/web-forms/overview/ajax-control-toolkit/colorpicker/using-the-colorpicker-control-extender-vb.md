---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Usar el extensor de Control de ColorPicker (VB) | Documentos de Microsoft
author: microsoft
description: ColorPicker es un extensor de AJAX de ASP.NET que proporciona funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control de menú emergente. Se puede adjuntar a cualquier ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874530"
---
<a name="using-the-colorpicker-control-extender-vb"></a>Usar el extensor de Control de ColorPicker (VB)
====================
por [Microsoft](https://github.com/microsoft)

> ColorPicker es un extensor de AJAX de ASP.NET que proporciona funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control de menú emergente. Se puede adjuntar a cualquier control de cuadro de texto de ASP.NET. Se.


El objetivo de este tutorial es explicar cómo se puede utilizar el extensor de control AJAX Control Toolkit ColorPicker. El extensor de control de ColorPicker muestra un cuadro de diálogo emergente que le permite seleccionar un color. El componente ColorPicker es útil cuando desea proporcionar una interfaz de usuario intuitiva para que un usuario elige un color.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Extender un Control de cuadro de texto con el extensor de Control de ColorPicker

Por ejemplo, imagine que desea crear un sitio Web que permite a los visitantes crear tarjetas de presentación personalizadas. Los visitantes pueden escribir el texto para una tarjeta de presentación y seleccionar el color. La página ASP.NET en el listado 1 contiene dos controles de cuadro de texto denominado txtCardText y txtCardColor. Cuando se envía el formulario, se muestran los valores seleccionados (consulte la figura 1).


[![Forma sencilla para crear una tarjeta de presentación](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: formulario Simple para crear una tarjeta de presentación ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-vb/_static/image2.png))


**Lista 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

El formulario en el listado 1 funciona, pero no proporciona una experiencia óptima al usuario. El usuario debe escribir un color en el cuadro de texto. Si el usuario desea un color especializado - por ejemplo, solo el derecho tonalidad de verde pea -, a continuación, el usuario debe averiguar el código de color HTML sin ningún tipo de ayuda.

Puede utilizar el extensor de control de ColorPicker para crear una mejor experiencia de usuario. El componente ColorPicker muestra un cuadro de diálogo color al mover el foco a un control de cuadro de texto (consulte la figura 2).


[![El extensor de Control de ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: el extensor de Control de ColorPicker ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-vb/_static/image4.png))


Debe completar dos pasos para usar el extensor de control ColorPicker con el formulario en el listado 1:

1. Agregar un control ScriptManager a la página
2. Agregue el extensor de control de ColorPicker a la página

Para poder usar el componente ColorPicker, debe agregar un ScriptManager a la página. Es un buen lugar para agregar el objeto ScriptManager justo debajo del servidor de apertura &lt;formulario&gt; etiqueta. Puede arrastrar el objeto ScriptManager a la página del cuadro de herramientas (el objeto ScriptManager se encuentra en la ficha Extensiones AJAX). Como alternativa, puede escribir la siguiente etiqueta en la vista del origen de debajo de la etiqueta de formulario de servidor de apertura:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

Es la manera más fácil de agregar el extensor de control de ColorPicker a la página en la vista Diseño. Si se mantenga el mouse sobre el cuadro de texto txtCardColor, una opción de tarea inteligente aparece permite agregar un dispositivo extender (consulte la figura 3). Si elige esta opción, aparece el Asistente de Extender (consulte la figura 4).


[![Agregar un dispositivo extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: agregar un dispositivo extender ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Al seleccionar un extensor de control con el Asistente para Extender](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: seleccionar un extensor de control con el Asistente para Extender ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-vb/_static/image8.png))


Puede elegir el extensor de ColorPicker para ampliar el cuadro de texto txtCardColor con el dispositivo extender ColorPicker. Haga clic en Aceptar para cerrar el cuadro de diálogo.

Después de realizar estos cambios, el origen de la página parece listado 2.

**La lista 2 - CreateCard.aspx (con ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Observe que la página contiene ahora un control ColorPickerExtender que aparece directamente debajo de la txtCardColor TextBox (control). El control ColorPickerExtender amplía el control de txtCardColor para que se muestre un cuadro de diálogo de selector de color.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usar un botón para iniciar el cuadro de diálogo de selector de Color

El extensor de ColorPicker admite las siguientes propiedades:

- PopupButtonId: el identificador de un botón en la página que hace que el cuadro de diálogo de selector de color que aparezca.
- PopupPosition: la posición en relación con el control de destino, del cuadro de diálogo de selector de color. Valores posibles son absolutos, centro, BottomLeft, BottomRight, TopLeft, TopRight, derecha e izquierda (el valor predeterminado es BottomLeft).
- SampleControlId: el identificador de un control que muestra el color seleccionado.
- SelectedColor - el color inicial seleccionado por el componente ColorPicker.

Puede usar estas propiedades para personalizar cómo se muestra el cuadro de diálogo de selector de color y cómo se muestra el color seleccionado. La página en la lista 3 muestra cómo puede usar algunas de estas propiedades.

**El listado 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La página en la lista 3 incluye un seleccionar Color botón (Véase la figura 5). Al hacer clic en este botón, aparece el cuadro de diálogo de selector de color sobre el cuadro de texto. Si selecciona un color en el cuadro de diálogo del color seleccionado aparece como el color de fondo de la lblSample Label (control).

La propiedad ColorPicker PopupButtonID se utiliza para asociar el dispositivo extender ColorPicker en el botón Seleccionar Color. Al proporcionar un valor para la propiedad PopupButtonID, el cuadro de diálogo de selector de color ya no aparece cuando el control de destino tiene el foco. Debe hacer clic en el botón para mostrar el cuadro de diálogo.

La propiedad SampleControlID se utiliza para asociar un control que muestra el color seleccionado con el componente ColorPicker. El componente ColorPicker cambia el color de fondo de este control para el color seleccionado actualmente.


[![Mostrar el cuadro de diálogo de selector de color con un botón](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: mostrar el cuadro de diálogo de selector de color con un botón ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Resumen

En este tutorial, aprendió a utilizar el extensor de control de ColorPicker para mostrar un cuadro de diálogo de selector de colores emergente. En primer lugar, se examina cómo puede mostrar el cuadro de diálogo cuando se mueve el foco a un control de cuadro de texto. A continuación, aprendió a crear un botón que muestra el cuadro de diálogo de selector de color cuando se hace clic en el botón.

> [!div class="step-by-step"]
> [Anterior](using-the-colorpicker-control-extender-cs.md)
