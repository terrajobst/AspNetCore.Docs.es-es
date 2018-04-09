---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Controla las devoluciones de datos desde un ModalPopup (C#) | Documentos de Microsoft
author: wenz
description: El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener especial cuidado cuando un punto de venta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 183725db62ba8b4037f368ed9d87d5059e3f1bcb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Controla las devoluciones de datos desde un ModalPopup (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener un cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.


## <a name="overview"></a>Información general

El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener un cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente modal. No existe, el usuario puede escribir un nombre y una dirección de correo electrónico. Se utiliza un botón para cerrar la ventana emergente y guardar la información. Tenga en cuenta que el `OnClick` atributo está establecido para que se produce un postback cuando se hace clic en este botón:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

La propia página consta de dos etiquetas para exactamente la misma información: nombre y direcciones de correo. Un botón se usa para desencadenar la ventana emergente modal:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Para hacer que aparezca la ventana emergente, agregue el `ModalPopupExtender` control. Establecer el `PopupControlID` atributo ID del panel y `TargetControlID` para el identificador del botón:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Ahora cada vez que la `Save` dentro de la ventana emergente modal se hace clic en botón, el lado del servidor `SaveData()` se ejecuta el método. Allí, puede guardar los datos introducidos en un almacén de datos. Por simplicidad, los nuevos datos solo se genera en la etiqueta:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Además, los controles de cuadro de texto dentro de la ventana emergente modal deben rellenarse con el nombre actual y el correo electrónico. Pero esto solo es necesario cuando se produce ninguna devolución de datos. Si hay una devolución de datos, la característica de estado de vista ASP.NET rellenará automáticamente con los valores apropiados en los cuadros de texto.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![La ventana emergente modal provoca una devolución de datos](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

La ventana emergente modal provoca una devolución de datos ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-modalpopup-with-a-repeater-control-cs.md)
> [Siguiente](positioning-a-modalpopup-cs.md)
