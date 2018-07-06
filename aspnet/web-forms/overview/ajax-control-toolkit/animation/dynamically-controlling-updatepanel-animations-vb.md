---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlar dinámicamente las animaciones UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: b93ed1c7994c7561396298d876af213ae872f787
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803660"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Controlar dinámicamente las animaciones UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un extensor especial que se basa principalmente en el marco de animación: UpdatePanelAnimation. También puede trabajar junto con los desencadenadores UpdatePanel.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa principalmente en el marco de animación: `UpdatePanelAnimation`. También puede trabajar junto con `UpdatePanel` desencadenadores.

## <a name="steps"></a>Pasos

El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

La animación en este escenario se aplicarán a una presentación de la hora actual. Esta información se puede escribir en una etiqueta con el `Page_Load()` método, o (por motivos de simplicidad) se usa el siguiente código en línea:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Además, se crea un botón para desencadenar la actualización de la hora:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Este código, a continuación, se coloca en el `<ContentTemplate>` sección de un `UpdatePanel` elemento. El panel `UpdateMode` atributo debe establecerse en `"Conditional"`, ya que solo los desencadenadores pueden actualizar el contenido del panel. En el `<Triggers>` sección de la `UpdatePanel`, crea un desencadenador de postback asincrónico y vinculado a la `Click` eventos del botón. Por lo tanto, si el usuario hace clic en el botón, el `UpdatePanel` se actualiza. Aquí está el marcado para el `UpdatePanel` control:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Por último, el `UpdatePanelAnimationExtender` debe configurarse: establecer el `TargetControlID` para el Id. del panel de atributo y define una animación en el dispositivo extender. Difuminación en hace sentido, lo que crea un énfasis visual interesante en la hora de actualización. El marcado de extensor es posible que, a continuación, este aspecto:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Ejecute el archivo en el explorador. Al hacer clic en el botón, se muestra la hora actual en el panel, siempre desvanece durante un segundo.


[![Se desvanece la hora actual](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

La hora actual se desvanece ([haga clic aquí para ver imagen en tamaño completo](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-an-updatepanel-control-vb.md)
