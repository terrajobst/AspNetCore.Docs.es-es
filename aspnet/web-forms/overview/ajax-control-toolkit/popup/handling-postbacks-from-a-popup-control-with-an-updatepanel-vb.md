---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Controla las devoluciones de datos de un Control de elemento emergente con un UpdatePanel (VB) | Documentos de Microsoft
author: wenz
description: El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Debe tenerse especial cuidado...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Controla las devoluciones de datos de un Control de elemento emergente con un UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Un cuidado especial tiene que tener cuidado cuando se produce un postback dentro de este tipo un menú emergente.


## <a name="overview"></a>Información general

El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Un cuidado especial tiene que tener cuidado cuando se produce un postback dentro de este tipo un menú emergente.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con una devolución de datos, un `UpdatePanel` puede evitar la actualización de página causada por la devolución de datos. El marcado siguiente define un par de elementos importantes:

- Un `ScriptManager` controlar la manera en que funciona el Kit de herramientas de Control de AJAX de ASP.NET
- Dos `TextBox` controla qué ambos, se desencadenará un menú emergente
- Un `Panel` control que actúa como el elemento emergente
- En el panel, un `Calendar` control está incrustado en un `UpdatePanel` control
- Dos `PopupControlExtender` controles que asignar el panel a los cuadros de texto

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Tenga en cuenta que la `OnSelectionChanged` atributo de la `Calendar` se establece el control. Por lo que cuando el usuario selecciona una fecha en el calendario, se produce una devolución de datos y el método de servidor `c1_SelectionChanged()` se ejecuta. Dentro de ese método, se debe recuperar la fecha actual y vuelve a escribir en el cuadro de texto.

La sintaxis para el que es como sigue: en primer lugar, un proxy del objeto para el `PopupControlExtender` en la página se debe generar. El Kit de herramientas de Control de AJAX de ASP.NET ofrece el `GetProxyForCurrentPopup()` método. El objeto que devuelve este método es compatible con la `Commit()` método que envía un valor al control que desencadena el menú emergente (no el control que desencadenó la llamada al método!). El código siguiente proporciona la fecha seleccionada como argumento para el `Commit()` método, haciendo que el código escribir la fecha seleccionada en el cuadro de texto:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Al hacer clic en una fecha del calendario, la fecha seleccionada aparece en el cuadro de texto asociado, crear un control de selector de fecha que puede actualmente se encuentra ahora en muchos sitios Web.


[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](using-multiple-popup-controls-vb.md)
> [Siguiente](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
