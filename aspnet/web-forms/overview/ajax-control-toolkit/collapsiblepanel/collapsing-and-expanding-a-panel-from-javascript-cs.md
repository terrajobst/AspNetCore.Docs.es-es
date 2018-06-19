---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Contraer y expandir un Panel desde JavaScript (C#) | Documentos de Microsoft
author: wenz
description: El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad de expandir o contraer su contenido un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 7baa3be7144946bde7d11afd9b1cb5f14ad9dede
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875544"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Contraer y expandir un Panel desde JavaScript (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad para contraer su contenido y expanda de nuevo. Estas dos acciones también pueden realizarse desde código de JavaScript personalizado.


## <a name="overview"></a>Información general

El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad para contraer su contenido y expanda de nuevo. Estas dos acciones también pueden realizarse desde código de JavaScript personalizado.

## <a name="steps"></a>Pasos

En primer lugar, cree una nueva página ASP.NET e incluya la `ScriptManager` dentro del `<form>` elemento. Esto carga la biblioteca de AJAX de ASP.NET y es necesario para el Kit de herramientas de Control:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

A continuación, cree un panel con algún texto, por lo que puede verse el efecto de expandir y contraer:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Como puede ver, el panel hace referencia a una clase CSS que se muestra aquí (y básicamente define un color de fondo y el ancho del panel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

El `CollapsiblePanelExtender` control requiere el `TargetControlID` atributo para que el Kit de herramientas sepa cuál de los paneles para contraer o expandir tras la solicitud:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Desafortunadamente, el dispositivo extender actualmente no expone una API específica para contraer o expandir el panel, pero algunos métodos sin documentar llevará a cabo. En primer lugar, agregue tres botones HTML a la página que, a continuación, desencadenará el JavaScript del lado cliente para contraer o expandir el contenido del panel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

En el código de JavaScript del lado cliente (partió `<script type="text/javascript">`), el `$find()` método debe usarse para tener acceso a la `CollapsiblePanelExtender`. `$find("cpe")` Devuelve una referencia a él. Desde aquí, los métodos específicos resolverá la tarea en cuestión.

El método de apertura (expandir) se denomina el panel `_doOpen()`; el código siguiente implementa la `doOpen()` función se llama cuando se hace clic en el primer botón:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Para cerrar o contraer el panel, el `_doClose()` método debe ejecutarse. Por lo que cuando el usuario hace clic en el segundo botón, se llama al código de JavaScript siguiente:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

El tercer botón alterna el estado del panel: de contraer para expandir y viceversa. El `CollapsiblePanelExtender` expone el `toggle()` método que hace exactamente eso: invierte el estado del panel. Sin embargo, también es otro método (que se usa internamente en el `toggle()` (método)): el `get_Collapsed()` método de la `CollapsiblePanelExtender()` nos indica si el panel está contraído o no. Dependiendo del valor devuelto de esta función, el panel es, a continuación, ya sea expandido (`_doOpen()` método) o contraer (`_doClose()`) método:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![El tercer botón cambia el estado del panel: desde contraído a expandido y viceversa](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

El tercer botón cambia el estado del panel: desde contraído a expandido y viceversa ([haga clic aquí para ver la imagen a tamaño completo](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](collapsing-and-expanding-a-panel-from-javascript-vb.md)
