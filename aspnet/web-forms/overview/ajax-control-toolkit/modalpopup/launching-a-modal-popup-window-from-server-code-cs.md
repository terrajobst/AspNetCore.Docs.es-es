---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Inicie una ventana emergente Modal del código del servidor (C#) | Documentos de Microsoft
author: wenz
description: El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo, algunos escenarios requieren que t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Inicie una ventana emergente Modal del código del servidor (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo algunos escenarios requieren que se desencadene la apertura de la ventana emergente modal en el lado del servidor.


## <a name="overview"></a>Información general

El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo algunos escenarios requieren que se desencadene la apertura de la ventana emergente modal en el lado del servidor.

## <a name="steps"></a>Pasos

En primer lugar, se requiere un control de botón de ASP.NET web para demostrar cómo funciona el control ModalPopup. Agregar un botón de este tipo dentro de la &lt;formulario&gt; elemento en una página nueva:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

A continuación, necesitará el marcado para la ventana emergente que desea crear. Definir como un `<asp:Panel>` de control y asegúrese de que incluye un control de botón. El control ModalPopup ofrece la funcionalidad necesaria para realizar este tipo un botón para cerrar la ventana emergente; en caso contrario, no hay ninguna manera fácil de dejar que desaparecer.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

A continuación, agregue el control de ModalPopup desde el Kit de herramientas de AJAX de ASP.NET a la página. Establecer propiedades para el botón que cargue el control, el botón que facilita el desaparecen y el identificador de la ventana emergente real.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Al igual que con todas las páginas web en función de AJAX de ASP.NET; el Administrador de scripts es necesario para cargar las bibliotecas de JavaScript necesarias para los exploradores de destino diferente:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Ejecutar el ejemplo en el explorador. Al hacer clic en el botón, aparece el cuadro emergente modal. Para lograr el mismo efecto mediante código del lado servidor, se requiere un nuevo botón:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Como puede ver, al hacer clic en el botón genera una devolución de datos y ejecuta el `ServerButton_Click()` método en el servidor. En este método, llama a una función de JavaScript `launchModal()` se ejecuta para ser exactos, la función de JavaScript se ejecutará una vez que se ha cargado la página:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

El trabajo de `launchModal()` es mostrar la ModalPopup. El `launchModal()` se ejecuta la función una vez que se ha cargado la página HTML completa. En ese momento, sin embargo, el marco de AJAX de ASP.NET no ha sido totalmente cargado aún. Por lo tanto, la `launchModal()` función simplemente establece una variable que se debe mostrar el control ModalPopup más adelante:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

El `pageLoad()` función de JavaScript es una función especial que se ejecuta una vez que se ha cargado completamente AJAX de ASP.NET. Por lo tanto, se agregue código a esta función para mostrar el control ModalPopup, pero solo si `launchModal()` se ha llamado antes de:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

El `$find()` función busca un elemento con nombre en la página y espera el identificador de servidor como un parámetro. Por lo tanto, `$find("mpe")` devuelve la representación del cliente del control ModalPopup; su `show()` método permite que aparezca la ventana emergente.


[![Aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones ([haga clic aquí para ver la imagen a tamaño completo](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-modalpopup-with-a-repeater-control-cs.md)
