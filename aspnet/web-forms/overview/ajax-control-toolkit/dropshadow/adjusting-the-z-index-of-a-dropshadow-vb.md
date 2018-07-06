---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustar el índice Z de un control DropShadow (VB) | Microsoft Docs
author: wenz
description: El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles para insta...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 78697f51a09dfaad315255efa23120d4c456bfea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829549"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Ajustar el índice Z de un control DropShadow (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET. Cuando una entrada de menú emergente, aparece detrás de la sombra paralela.


## <a name="overview"></a>Información general

El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET. Cuando una entrada de menú emergente, aparece detrás de la sombra paralela.

## <a name="steps"></a>Pasos

El código comienza con el Panel, que contiene texto suficiente para que el panel contiene el texto suficiente para que el efecto sea visible:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Otro panel se coloca directamente antes de la `panelShadow` panel. Contiene un menú con orientación horizontal para que aparezcan entradas de menú sobre (o más bien: en) la `dropShadow` panel):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

A continuación, la `DropShadowExtender` se agrega para ampliar el `panelShadow` panel con un efecto de sombra:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Por último, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Al ejecutar esta secuencia de comandos, las entradas de menú aparecen debajo del panel. Sin embargo, el menú usa la clase CSS `panel` donde solo se debe definir dos cosas para hacer que los elementos aparecen delante de otro panel:

- Ubicación relativa
- Un índice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

A continuación, la `DropShadowExtender` control no entra en conflicto ya con el control de menú.


[![Antes: La entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Antes: La entrada de menú no está visible ([haga clic aquí para ver imagen en tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Después: La entrada de menú aparece](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Después: La entrada de menú aparece ([haga clic aquí para ver imagen en tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Siguiente](manipulating-dropshadow-properties-from-client-code-vb.md)
