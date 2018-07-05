---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Usar TextBoxWatermark con los controles de validación (C#) | Microsoft Docs
author: wenz
description: El control TextBoxWatermark de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, lo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fe28f169e5e714ba07d8a71bb33d0f62608d688b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380696"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Usar TextBoxWatermark con los controles de validación (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> El control TextBoxWatermark de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto podrían entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero se pueden solucionar estos problemas.


## <a name="overview"></a>Información general

El `TextBoxWatermark` control de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto podrían entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero se pueden solucionar estos problemas.

## <a name="steps"></a>Pasos

La configuración básica del ejemplo es el siguiente: un `TextBox` control se marca de agua utilizando una `TextBoxWatermarkExtender` control. Un botón desencadena una devolución de datos y se usará después para desencadenar los controles de validación en la página. Además, un `ScriptManager` control es necesario inicializar AJAX de ASP.NET:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Ahora, agregue un `RequiredFieldValidator` control que comprueba si hay texto en el campo cuando se envía el formulario. El `InitialValue` propiedad del validador debe establecerse en el mismo valor que se usa en el `TextBoxWatermarkExtender` control: cuando se envía el formulario, el valor de un cuadro de texto sin cambios es el valor de marca de agua dentro de él:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Sin embargo, es un problema con este enfoque: si el cliente deshabilita JavaScript, el campo de texto no es rellenan previamente con el texto de marca de agua, por lo tanto, el `RequiredFieldValidator` desencadenar un mensaje de error. Por lo tanto, un segundo `RequiredFieldValidator` se requiere un control que comprueba si hay un cuadro de texto vacío (omitiendo el `InitialValue` atributo).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Dado que ambos validadores usan `Display` = `"Dynamic"`, el usuario final no puede distinguir entre la apariencia visual que de los dos controles de validación que se activó; en su lugar, parece que hubo solo uno de ellos.

Por último, agregue el código del lado servidor para el texto en el campo de salida si ningún validador emite un mensaje de error:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![El validador se queja de que no hay ningún texto en el campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

El validador se queja de que no hay ningún texto en el campo ([haga clic aquí para ver imagen en tamaño completo](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-cs.md)
> [Siguiente](using-textboxwatermark-in-a-formview-vb.md)
