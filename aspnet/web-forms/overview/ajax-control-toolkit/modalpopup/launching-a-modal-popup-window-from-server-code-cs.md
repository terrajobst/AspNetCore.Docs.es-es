---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Iniciar una ventana emergente Modal desde el código de servidor (C#) | Microsoft Docs
author: wenz
description: El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo, algunos escenarios requieren que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b59997d5c3e841d36d475431b02d3df2d1a4b666
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835931"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Iniciar una ventana emergente Modal desde el código de servidor (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo algunos escenarios requieren que se desencadena la apertura de la ventana emergente modal en el lado del servidor.


## <a name="overview"></a>Información general

El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo algunos escenarios requieren que se desencadena la apertura de la ventana emergente modal en el lado del servidor.

## <a name="steps"></a>Pasos

En primer lugar, se requiere un control de botón de ASP.NET web para demostrar cómo funciona el control ModalPopup. Agregar un botón dentro de la &lt;formulario&gt; elemento en una página nueva:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

A continuación, necesita el marcado para la ventana emergente que desea crear. Defínalo como un `<asp:Panel>` controlar y asegúrese de que incluye un control de botón. El control ModalPopup ofrece la funcionalidad para hacer que un botón cerrar la ventana emergente; en caso contrario, no hay ninguna manera fácil para permitirle desaparecer.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

A continuación, agregue el control ModalPopup de ASP.NET AJAX Toolkit a la página. Establecer propiedades para el botón que carga el control, el botón que hace desaparecer y el identificador del elemento emergente real.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Igual que con todas las páginas web basadas en ASP.NET AJAX; el director de scripts es necesaria para cargar las bibliotecas de JavaScript necesarias para los exploradores de destino diferente:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Ejecute el ejemplo en el explorador. Al hacer clic en el botón, aparece la ventana emergente modal. Para lograr el mismo efecto mediante el código del lado servidor, se requiere un nuevo botón:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Como puede ver, hacer clic en el botón genera un postback y ejecuta el `ServerButton_Click()` método en el servidor. En este método, llama una función de JavaScript `launchModal()` se ejecuta para ser exactos, la función de JavaScript se ejecutará una vez que se ha cargado la página:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

El trabajo de `launchModal()` es mostrar la ModalPopup. El `launchModal()` función se ejecuta una vez que se ha cargado la página HTML completa. En ese momento, sin embargo, el marco de ASP.NET AJAX no ha sido totalmente cargado aún. Por lo tanto, el `launchModal()` función solo establece una variable que se debe mostrar el control ModalPopup más adelante:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

El `pageLoad()` función de JavaScript es una función especial que se ejecuta una vez que AJAX de ASP.NET se ha cargado completamente. Por lo tanto, agregue código a esta función para mostrar el control ModalPopup, pero solo si `launchModal()` ha llamado antes de:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

El `$find()` función busca un elemento con nombre en la página y se espera que el identificador del lado servidor como un parámetro. Por lo tanto, `$find("mpe")` devuelve la representación del cliente del control ModalPopup; su `show()` método permite que aparezca la ventana emergente.


[![Aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones ([haga clic aquí para ver imagen en tamaño completo](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-modalpopup-with-a-repeater-control-cs.md)
