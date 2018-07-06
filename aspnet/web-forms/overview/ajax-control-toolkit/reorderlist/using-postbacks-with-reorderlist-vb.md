---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Usar Postbacks con ReorderList (VB) | Microsoft Docs
author: wenz
description: El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. Cada vez que se puede reordenar la lista, un pedido de compra...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 20214db1c038f417944bd374bdaa8ce55d6d44aa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839004"
---
<a name="using-postbacks-with-reorderlist-vb"></a>Usar Postbacks con ReorderList (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. Cada vez que se puede reordenar la lista, una devolución informará al servidor del cambio.


## <a name="overview"></a>Información general

El `ReorderList` control de AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. Cada vez que se puede reordenar la lista, una devolución informará al servidor del cambio.

## <a name="steps"></a>Pasos

Hay varios orígenes de datos posibles para el `ReorderList` control. Una consiste en usar un `XmlDataSource` control:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Para enlazar este XML a un `ReorderList` se deben establecer el control y habilitar las devoluciones de datos, los siguientes atributos:

- `DataSourceID`: El identificador del origen de datos
- `SortOrderField`: La propiedad para ordenar por
- `AllowReorder`: Si se permite al usuario reordenar los elementos de lista
- `PostBackOnReorder`: Si desea crear una devolución de datos cada vez que se reorganiza la lista

Este es el formato adecuado para el control:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

Dentro de la `ReorderList` control, los datos específicos del origen de datos puede estar enlazado con el `Eval()` método:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

En una posición arbitraria en la página, una etiqueta contendrá la información cuando ocurrió la última reordenación:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Esta etiqueta se rellena con texto en el código de servidor, que controla la devolución de datos:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Por último, para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en la página:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Cada reordenación desencadena una devolución de datos](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Cada reordenación desencadena una devolución de datos ([haga clic aquí para ver imagen en tamaño completo](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Siguiente](drag-and-drop-via-reorderlist-vb.md)
