---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: "Controlar dinámicamente UpdatePanel animaciones (C#) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0408556e322a46211388fbd557d7a967044129ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-c"></a>Controlar dinámicamente UpdatePanel animaciones (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: UpdatePanelAnimation. También puede trabajar junto con desencadenadores de UpdatePanel.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: `UpdatePanelAnimation`. También puede trabajar junto con `UpdatePanel` desencadenadores.

## <a name="steps"></a>Pasos

El primer paso es como de costumbre incluir la `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

La animación en este escenario se aplicarán a una presentación de la hora actual. Esta información se puede escribir en una etiqueta con el `Page_Load()` (método), o (por simplicidad) se utiliza el código de la línea siguiente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Además, se crea un botón para desencadenar la actualización de la hora:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Este código, a continuación, se coloca en el `<ContentTemplate>` sección de un `UpdatePanel` elemento. El panel `UpdateMode` atributo debe establecerse en `"Conditional"`, ya que solo los desencadenadores pueden actualizar el contenido del panel. En el `<Triggers>` sección de la `UpdatePanel`, un desencadenador de postback asincrónico creado y vinculado a la `Click` eventos del botón. Por lo tanto, si el usuario hace clic en el botón, el `UpdatePanel` se actualiza. Este es el marcado para el `UpdatePanel` control:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Por último, el `UpdatePanelAnimationExtender` debe configurarse: establecer el `TargetControlID` de atributo para el Id. del panel y definir una animación en el dispositivo extender. Resaltar hace sentido, que crea un "nice" énfasis visual en el tiempo actualizado. El marcado del extensor, a continuación, puede ser similar al siguiente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Ejecute el archivo en el explorador. Al hacer clic en el botón, se muestra la hora actual en el panel, siempre resaltar para la duración de un segundo.


[![La hora actual es difuminado](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

La hora actual es difuminado ([haga clic aquí para ver la imagen a tamaño completo](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](animating-an-updatepanel-control-cs.md)
[Siguiente](adding-animation-to-a-control-vb.md)
