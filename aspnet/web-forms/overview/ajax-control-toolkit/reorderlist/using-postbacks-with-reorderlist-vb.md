---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Con las devoluciones de datos ReorderList (VB) | Documentos de Microsoft
author: wenz
description: "El control de ReorderList en el Kit de herramientas de Control de AJAX proporciona una lista que se puede ordenar por el usuario a través de arrastrar y colocar. Cada vez que se reordena la lista, un pedido de compra..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a>Uso de las devoluciones de datos con ReorderList (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> El control de ReorderList en el Kit de herramientas de Control de AJAX proporciona una lista que se puede ordenar por el usuario a través de arrastrar y colocar. Cada vez que se reordena la lista, una devolución de datos debe informar al servidor del cambio.


## <a name="overview"></a>Información general

El `ReorderList` control en el Kit de herramientas de Control de AJAX proporciona una lista que se puede ordenar por el usuario a través de arrastrar y colocar. Cada vez que se reordena la lista, una devolución de datos debe informar al servidor del cambio.

## <a name="steps"></a>Pasos

Hay varios orígenes de datos posibles para el `ReorderList` control. Una consiste en usar un `XmlDataSource` control:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Para enlazar este código XML a un `ReorderList` se deben establecer el control y habilitar las devoluciones de datos, los siguientes atributos:

- `DataSourceID`: El identificador del origen de datos
- `SortOrderField`: La propiedad que se va a ordenar por
- `AllowReorder`: Si se permite al usuario volver a ordenar los elementos de lista
- `PostBackOnReorder`: Si desea crear una devolución de datos cada vez que se reorganiza la lista

Este es el formato adecuado para el control:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

En el `ReorderList` control, los datos específicos del origen de datos puede estar enlazado con el `Eval()` método:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

En una posición arbitraria en la página, una etiqueta contendrá la información cuando ocurrió reordenar los últimos:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Esta etiqueta se rellena con texto en el código del lado servidor, control de la devolución de datos:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Por último, con el fin de activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe situarse en la página:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Cada organización desencadena una devolución de datos](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Cada organización desencadena una devolución de datos ([haga clic aquí para ver la imagen a tamaño completo](using-postbacks-with-reorderlist-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](drag-and-drop-via-reorderlist-cs.md)
[Siguiente](drag-and-drop-via-reorderlist-vb.md)
