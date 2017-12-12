---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: "Ajustar el índice Z de una sombra paralela (VB) | Documentos de Microsoft"
author: wenz
description: El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, para insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 844ea00c2ef1c974aa72c7dd627819b0429d612e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Ajustar el índice Z de una sombra paralela (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET. Cuando una entrada de menú aparece, aparece detrás de la sombra paralela.


## <a name="overview"></a>Información general

El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET. Cuando una entrada de menú aparece, aparece detrás de la sombra paralela.

## <a name="steps"></a>Pasos

El código comienza con el Panel, que contiene texto suficiente como para que el panel contiene texto suficiente para que el efecto sea visible:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Otro panel se coloca directamente antes de la `panelShadow` panel. Contiene un menú con orientación horizontal para que aparezcan las entradas de menú sobre (o en su lugar: en) la `dropShadow` panel):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

A continuación, la `DropShadowExtender` se agrega al ampliar el `panelShadow` panel con un efecto de sombra paralela:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Por último, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Al ejecutar este script, las entradas de menú aparecen debajo del panel. Sin embargo, el menú utiliza la clase CSS `panel` donde sólo tiene que definir dos cosas para que los elementos aparecen delante del otro panel:

- Posición relativa
- Un índice de z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

A continuación, la `DropShadowExtender` control no entra en conflicto ya con el control de menú.


[![Antes: La entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Antes: La entrada de menú no está visible ([haga clic aquí para ver la imagen a tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Después: La entrada de menú aparece](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Después: La entrada de menú aparece ([haga clic aquí para ver la imagen a tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

>[!div class="step-by-step"]
[Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
[Siguiente](manipulating-dropshadow-properties-from-client-code-vb.md)
