---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "Uso de varios controles de menú emergente (C#) | Documentos de Microsoft"
author: wenz
description: "El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. También es posible utilizar m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a>Uso de varios controles de menú emergente (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. También es posible usar más de un control de elemento emergente en una sola página.


## <a name="overview"></a>Información general

El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. También es posible usar más de un control de elemento emergente en una sola página.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

A continuación, agregue un panel que actúa como la ventana emergente. En el escenario actual, el panel contiene un `Calendar` control. Para evitar las actualizaciones de página causadas por las devoluciones de datos del calendario, el panel se coloca dentro de un `UpdatePanel` control:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

La página también contiene dos cuadros de texto. Para cada cuadro de texto, el calendario emergente debe aparecer una vez que se activa el cuadro de texto.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Ahora ampliar cada uno de los dos cuadros de texto con un `PopupControlExtender`. El `TargetControlID` atributo proporciona el identificador del control asociado al dispositivo extender. El `PopupControlID` atributo contiene el identificador del panel emergente. En este caso, ambos extensores mostrar el mismo panel, pero también son posibles, paneles diferentes.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Al hacer clic en un campo de texto, un calendario aparece debajo del campo, que le permite seleccionar una fecha. (Recibir la fecha seleccionada en los cuadros de texto se tratará en otro tutorial.)


[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](using-multiple-popup-controls-cs/_static/image3.png))

>[!div class="step-by-step"]
[Siguiente](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
