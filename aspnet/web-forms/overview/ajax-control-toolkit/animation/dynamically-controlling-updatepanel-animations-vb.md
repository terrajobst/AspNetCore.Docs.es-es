---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: "Controlar dinámicamente UpdatePanel animaciones (VB) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b1f169724d8d3f8bcbc46198d320eeb8e7260c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Controlar dinámicamente UpdatePanel animaciones (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: UpdatePanelAnimation. También puede trabajar junto con desencadenadores de UpdatePanel.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: `UpdatePanelAnimation`. También puede trabajar junto con `UpdatePanel` desencadenadores.

## <a name="steps"></a>Pasos

El primer paso es como de costumbre incluir la `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

La animación en este escenario se aplicarán a una presentación de la hora actual. Esta información se puede escribir en una etiqueta con el `Page_Load()` (método), o (por simplicidad) se utiliza el código de la línea siguiente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Además, se crea un botón para desencadenar la actualización de la hora:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Este código, a continuación, se coloca en el `<ContentTemplate>` sección de un `UpdatePanel` elemento. El panel `UpdateMode` atributo debe establecerse en `"Conditional"`, ya que solo los desencadenadores pueden actualizar el contenido del panel. En el `<Triggers>` sección de la `UpdatePanel`, un desencadenador de postback asincrónico creado y vinculado a la `Click` eventos del botón. Por lo tanto, si el usuario hace clic en el botón, el `UpdatePanel` se actualiza. Este es el marcado para el `UpdatePanel` control:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Por último, el `UpdatePanelAnimationExtender` debe configurarse: establecer el `TargetControlID` de atributo para el Id. del panel y definir una animación en el dispositivo extender. Resaltar hace sentido, que crea un "nice" énfasis visual en el tiempo actualizado. El marcado del extensor, a continuación, puede ser similar al siguiente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Ejecute el archivo en el explorador. Al hacer clic en el botón, se muestra la hora actual en el panel, siempre resaltar para la duración de un segundo.


[![La hora actual es difuminado](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

La hora actual es difuminado ([haga clic aquí para ver la imagen a tamaño completo](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](animating-an-updatepanel-control-vb.md)
