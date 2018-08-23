---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Contraer y expandir un Panel desde JavaScript (VB) | Microsoft Docs
author: wenz
description: El control CollapsiblePanel en ASP.NET AJAX Control Toolkit extiende un panel y le proporciona la capacidad de su contenido de contraer y expandir esta un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f80a6979966a887db0557b4f1b98570e10b1ab7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828656"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Contraer y expandir un Panel desde JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> El control CollapsiblePanel en ASP.NET AJAX Control Toolkit extiende un panel y lo proporciona la capacidad necesaria para contraer su contenido y volver a ampliar. Estas dos acciones también pueden activarse desde código JavaScript personalizado.


## <a name="overview"></a>Información general

El control CollapsiblePanel en ASP.NET AJAX Control Toolkit extiende un panel y lo proporciona la capacidad necesaria para contraer su contenido y volver a ampliar. Estas dos acciones también pueden activarse desde código JavaScript personalizado.

## <a name="steps"></a>Pasos

En primer lugar, cree una nueva página ASP.NET e incluya el `ScriptManager` dentro de la `<form>` elemento. Esto carga la biblioteca de AJAX de ASP.NET que se requiere el Kit de herramientas de Control:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

A continuación, cree un panel con algún texto para que se puede ver el efecto de expandir y contraer:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Como puede ver, el panel hace referencia a una clase CSS que se muestra aquí (y básicamente define un color de fondo y el ancho del panel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

El `CollapsiblePanelExtender` control requiere el `TargetControlID` atributo para que el Kit de herramientas sepa qué panel para contraer o expandir a petición:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Lamentablemente, el extensor actualmente no expone una API específica para contraer o expandir el panel, pero basta con algunos métodos sin documentar. En primer lugar, agregue tres botones HTML a la página que, a continuación, se desencadenará el JavaScript del lado cliente para contraer o expandir el contenido del panel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

En el código de JavaScript del lado cliente (Introducción a `<script type="text/javascript">`), el `$find()` método debe utilizarse para tener acceso a la `CollapsiblePanelExtender`. `$find("cpe")` devolverá una referencia a él. Desde aquí, los métodos específicos resolverá la tarea en cuestión.

El método de apertura (expandir) se denomina el panel `_doOpen()`; el código siguiente implementa el `doOpen()` función se llama cuando se hace clic en el primer botón:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Para cerrar o contraer el panel, el `_doClose()` método se debe ejecutar. Así que cuando el usuario hace clic en el segundo botón, se llama el código JavaScript siguiente:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

El tercer botón alterna el estado del panel: desde contraída en ampliado y viceversa. El `CollapsiblePanelExtender` expone el `toggle()` método que hace exactamente eso: invierte el estado del panel. Pero también hay otro enfoque (que se usa internamente por la `toggle()` método): el `get_Collapsed()` método de la `CollapsiblePanelExtender()` nos indica si el panel se contrae o no. Dependiendo del valor devuelto de esta función, el panel es, a continuación, ya sea expandido (`_doOpen()` método) o se contrae (`_doClose()`) método:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![El tercer botón cambia el estado del panel: desde contraído expandida y back-](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

El tercer botón cambia el estado del panel: desde contraído expandida y back-([haga clic aquí para ver imagen en tamaño completo](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](collapsing-and-expanding-a-panel-from-javascript-cs.md)
