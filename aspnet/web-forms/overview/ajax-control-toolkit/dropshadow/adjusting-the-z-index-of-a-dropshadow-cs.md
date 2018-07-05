---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Ajustar el índice Z de un control DropShadow (C#) | Microsoft Docs
author: wenz
description: El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles para insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 22732bc44a56819a2ec73346dc32659d027eaee0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369575"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Ajustar el índice Z de un control DropShadow (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET. Cuando una entrada de menú emergente, aparece detrás de la sombra paralela.


## <a name="overview"></a>Información general

El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET. Cuando una entrada de menú emergente, aparece detrás de la sombra paralela.

## <a name="steps"></a>Pasos

El código comienza con el Panel, que contiene texto suficiente para que el panel contiene el texto suficiente para que el efecto sea visible:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Otro panel se coloca directamente antes de la `panelShadow` panel. Contiene un menú con orientación horizontal para que aparezcan entradas de menú sobre (o más bien: en) la `dropShadow` panel):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

A continuación, la `DropShadowExtender` se agrega para ampliar el `panelShadow` panel con un efecto de sombra:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Por último, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Al ejecutar esta secuencia de comandos, las entradas de menú aparecen debajo del panel. Sin embargo, el menú usa la clase CSS `panel` donde solo se debe definir dos cosas para hacer que los elementos aparecen delante de otro panel:

- Ubicación relativa
- Un índice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

A continuación, la `DropShadowExtender` control no entra en conflicto ya con el control de menú.


[![Antes: La entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Antes: La entrada de menú no está visible ([haga clic aquí para ver imagen en tamaño completo](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Después: La entrada de menú aparece](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Después: La entrada de menú aparece ([haga clic aquí para ver imagen en tamaño completo](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Siguiente](manipulating-dropshadow-properties-from-client-code-cs.md)
