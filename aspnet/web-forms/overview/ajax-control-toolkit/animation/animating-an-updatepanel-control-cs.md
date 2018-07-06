---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animar un Control UpdatePanel (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3214ef85938c374a8fb083bc075bd9390e37a209
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808258"
---
<a name="animating-an-updatepanel-control-c"></a>Animar un Control UpdatePanel (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un extensor especial que se basa principalmente en el marco de animación: UpdatePanelAnimation. Este tutorial muestra cómo configurar este tipo de una animación de un UpdatePanel.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa principalmente en el marco de animación: `UpdatePanelAnimation`. Este tutorial muestra cómo configurar este tipo de una animación para un `UpdatePanel`.

## <a name="steps"></a>Pasos

El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

La animación en este escenario se aplicarán a ASP.NET `Wizard` control web que residen en un `UpdatePanel`. Tres pasos (arbitrarios) proporcionan suficiente opciones para desencadenar devoluciones de datos:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

El marcado necesario para la `UpdatePanelAnimationExtender` control es bastante semejante al marcado que se usa para el `AnimationExtender`. En el `TargetControlID` atributo proporcionamos el `ID` de la `UpdatePanel` se va a animar; dentro de la `UpdatePanelAnimationExtender` (control), el `<Animations>` elemento contiene el marcado XML de la animación. Sin embargo hay una diferencia: la cantidad de eventos y controladores de eventos está limitada en comparación con `AnimationExtender`. Para `UpdatePanels`, sólo dos de ellas existen:

- `<OnUpdated>` Cuando se ha actualizado el UpdatePanel
- `<OnUpdating>` Cuando inicia la actualización de UpdatePanel

En este escenario, el nuevo contenido de la `UpdatePanel` (después de la devolución de datos) serán fundido de entrada. Este es el marcado necesario para:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Ahora cada vez que se produce un postback en UpdatePanel, el nuevo contenido del panel de fundido de entrada sin problemas.


[![El siguiente paso del asistente se desvanece](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

El siguiente paso del asistente se desvanece ([haga clic aquí para ver imagen en tamaño completo](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](changing-an-animation-using-client-side-code-cs.md)
> [Siguiente](dynamically-controlling-updatepanel-animations-cs.md)
