---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Controlar los Postbacks desde un Control emergente con un UpdatePanel (C#) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Debe tenerse especial cuidado...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f886ba0f3e79bc6d5daf193eaedfd627bc9b937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837566"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Controlar los Postbacks desde un Control emergente con un UpdatePanel (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Tiene un cuidado especial que se realizará cuando se produce un postback dentro de este tipo una ventana emergente.


## <a name="overview"></a>Información general

El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Tiene un cuidado especial que se realizará cuando se produce un postback dentro de este tipo una ventana emergente.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con una devolución de datos, un `UpdatePanel` puede evitar que la actualización de la página causada por la devolución de datos. El marcado siguiente define un par de elementos importantes:

- Un `ScriptManager` controlar para que funcione de ASP.NET AJAX Control Toolkit
- Dos `TextBox` controla qué ambos, se desencadenará un menú emergente
- Un `Panel` control que actúa como el elemento emergente
- Dentro del panel, un `Calendar` control incrustado en un `UpdatePanel` control
- Dos `PopupControlExtender` controles que asignación el panel a los cuadros de texto

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Tenga en cuenta que el `OnSelectionChanged` atributo de la `Calendar` se establece el control. Por lo que cuando el usuario selecciona una fecha del calendario, se produce un postback y el método de servidor `c1_SelectionChanged()` se ejecuta. Dentro de ese método, se debe recuperar la fecha actual y volver a escribir en el cuadro de texto.

La sintaxis que es como sigue: en primer lugar, un proxy de objetos para el `PopupControlExtender` debe haberse generado en la página. ASP.NET AJAX Control Toolkit ofrece el `GetProxyForCurrentPopup()` método. El objeto que devuelve este método admite la `Commit()` método que envía un valor al control que desencadena la ventana emergente (no el control que desencadenó la llamada al método!). El código siguiente proporciona la fecha seleccionada como argumento para el `Commit()` método, lo que provocaría el código escribir la fecha seleccionada en el cuadro de texto:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Ahora al hacer clic en una fecha del calendario, la fecha seleccionada aparece en el cuadro de texto asociado, crear un control de selector de fecha que puede actualmente se encuentra en muchos sitios Web.


[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](using-multiple-popup-controls-cs.md)
> [Siguiente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
