---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Usar TextBoxWatermark con controles de validación (VB) | Documentos de Microsoft
author: wenz
description: El control TextBoxWatermark en el Kit de herramientas de Control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, lo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>Usar TextBoxWatermark con controles de validación (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> El control TextBoxWatermark en el Kit de herramientas de Control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto podrían entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero se pueden solucionar estos problemas.


## <a name="overview"></a>Información general

El `TextBoxWatermark` control en el Kit de herramientas de Control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto podrían entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero se pueden solucionar estos problemas.

## <a name="steps"></a>Pasos

La configuración básica del ejemplo es el siguiente: una `TextBox` control se marca de agua utilizando un `TextBoxWatermarkExtender` control. Un botón desencadena una devolución de datos y se usará más adelante para activar los controles de validación en la página. Además, un `ScriptManager` control debe inicializar AJAX de ASP.NET:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Ahora, agregue un `RequiredFieldValidator` control que comprueba si hay texto en el campo cuando se envía el formulario. El `InitialValue` propiedad del validador debe establecerse en el mismo valor que se utiliza en el `TextBoxWatermarkExtender` control: cuando se envía el formulario, el valor de un cuadro de texto sin cambios es el valor de marca de agua dentro de él:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Sin embargo, es un problema con este enfoque: si el cliente deshabilita JavaScript, el campo de texto que no se rellena previamente con el texto de marca de agua, por lo tanto, la `RequiredFieldValidator` desencadenar un mensaje de error. Por lo tanto, un segundo `RequiredFieldValidator` se requiere control que comprueba si hay un cuadro de texto vacío (si se omite la `InitialValue` atributo).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Dado que ambos validadores usa `Display` = `"Dynamic"`, el usuario final no puede distinguir de la apariencia visual que de los dos validadores se activó; en su lugar, parece que hubo solo uno de ellos.

Finalmente, agregue el código del servidor para el texto en el campo de salida si ningún validador emite un mensaje de error:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![El validador se queja de que no hay ningún texto en el campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

El validador se queja de que no hay ningún texto en el campo ([haga clic aquí para ver la imagen a tamaño completo](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-vb.md)
