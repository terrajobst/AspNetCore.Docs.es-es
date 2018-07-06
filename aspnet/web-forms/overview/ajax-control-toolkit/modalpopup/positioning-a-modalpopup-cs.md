---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Colocar un ModalPopup (C#) | Microsoft Docs
author: wenz
description: El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo el control no ofrece un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f94c0a473b88c0155847d700c48faa34b84c940
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807069"
---
<a name="positioning-a-modalpopup-c"></a>Colocar un ModalPopup (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo el control no ofrece una funcionalidad integrada para colocar el elemento emergente.


## <a name="overview"></a>Información general

El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo el control no ofrece una funcionalidad integrada para colocar el elemento emergente.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager`. control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente modal. Se utiliza un botón para cerrar la ventana emergente:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Cada vez que se muestra la ventana emergente, deberá se coloca en un lugar determinado en la página. Para esta tarea, se crea una función de JavaScript del lado cliente. En primer lugar intenta tener acceso al panel. Si se realiza correctamente, la posición del panel se establece con CSS y JavaScript (cambiar la posición del elemento emergente en le). Sin embargo, el `ModalPopupExtender` control también intenta colocar la ventana emergente. Por lo tanto, el código JavaScript repetidamente coloca la ventana emergente, cada décima de segundo.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Como puede ver, el valor devuelto de la `setTimeout()` método JavaScript se guarda en una variable global. Esto permite detener el posicionamiento repetidas de la ventana emergente a petición, utilizando el `clearTimeout()` método:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Ahora todo lo que queda por hacer es hacer que el explorador llamar a estas funciones siempre que sea apropiado. El `movePanel()` se debe llamar la función de JavaScript cuando se presiona el botón que desencadena el panel:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

Y el `stopMoving()` función entra en juego cuando se cierra el menú emergente puede activarse en el `ModalPopupExtender` control:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![El elemento emergente modal aparece en la posición designada](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

El elemento emergente modal aparece en la posición designada ([haga clic aquí para ver imagen en tamaño completo](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-modalpopup-cs.md)
> [Siguiente](launching-a-modal-popup-window-from-server-code-vb.md)
