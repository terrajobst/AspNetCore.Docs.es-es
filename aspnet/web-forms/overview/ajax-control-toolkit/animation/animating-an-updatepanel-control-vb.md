---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animar un Control UpdatePanel (VB) | Documentos de Microsoft
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d1056fc798e22254e94e5cad54436576a297f7d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-vb"></a>Animar un Control UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: UpdatePanelAnimation. Este tutorial muestra cómo configurar una animación de este tipo para un UpdatePanel.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: `UpdatePanelAnimation`. Este tutorial muestra cómo configurar estas una animación para un `UpdatePanel`.

## <a name="steps"></a>Pasos

El primer paso es como de costumbre incluir la `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

La animación en este escenario se aplicarán a un ASP.NET `Wizard` control web que residen en un `UpdatePanel`. Tres pasos (arbitrarios) proporcionan suficiente opciones para desencadenar las devoluciones de datos:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

El marcado que sea necesario para la `UpdatePanelAnimationExtender` control es muy similar para el marcado usado para la `AnimationExtender`. En el `TargetControlID` atributo proporcionamos el `ID` de la `UpdatePanel` a animar; dentro de la `UpdatePanelAnimationExtender` (control), el `<Animations>` elemento contiene el marcado XML de la animación. Sin embargo, es una diferencia: la cantidad de eventos y controladores de eventos se limita en comparación con `AnimationExtender`. Para `UpdatePanels`, sólo dos de ellos existe:

- `<OnUpdated>`Cuando se ha actualizado el UpdatePanel
- `<OnUpdating>`Cuando inicia la actualización UpdatePanel

En este escenario, el nuevo contenido de la `UpdatePanel` (después de la devolución de datos) serán fundido de entrada. Éste es el formato necesario para:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Ahora cada vez que se produce un postback en UpdatePanel, el nuevo contenido del panel se atenúan sin problemas.


[![El siguiente paso del asistente se difuminado](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

El siguiente paso del asistente se difuminado ([haga clic aquí para ver la imagen a tamaño completo](animating-an-updatepanel-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](changing-an-animation-using-client-side-code-vb.md)
[Siguiente](dynamically-controlling-updatepanel-animations-vb.md)
