---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: "Utilizar el Control deslizante con devolución de datos automática (VB) | Documentos de Microsoft"
author: wenz
description: "El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible hacer que el control deslizante Autocontab..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: fedd3ff947c6f5d5d4d00791087e9fd825fdf9d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Utilizar el Control deslizante con devolución de datos automática (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible hacer que el control deslizante autopostback una vez su valor cambia.


## <a name="overview"></a>Información general

El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible hacer que el control deslizante autopostback una vez su valor cambia.

## <a name="steps"></a>Pasos

Para realizar el control deslizante postback automáticamente tras un cambio, ambos cuadros de texto necesitan el atributo `AutoPostBack="true"`: el cuadro de texto que se convertirá en el control deslizante propio y el cuadro de texto que contiene la posición del control deslizante. Este es el marcado necesario para:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

El `SliderExtender` control desde el Kit de herramientas de Control de AJAX de ASP.NET asigna la funcionalidad de control deslizante a los dos cuadros de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Un elemento de etiqueta adicionales se pueden utilizar posteriormente para informar al usuario de una devolución de datos:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Por último, el `ScriptManager` control de AJAX de ASP.NET carga el código de JavaScript necesario para el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Ahora está enviando el control deslizante hacia atrás; en el servidor, este evento se puede detectar y ha actuado en consecuencia:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Mover el control deslizante desencadena una devolución de datos](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Mover el control deslizante desencadena una devolución de datos ([haga clic aquí para ver la imagen a tamaño completo](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Después, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Después, la fecha de este cambio se escribe en la etiqueta ([haga clic aquí para ver la imagen a tamaño completo](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

>[!div class="step-by-step"]
[Anterior](databinding-the-slider-control-cs.md)
[Siguiente](databinding-the-slider-control-vb.md)
