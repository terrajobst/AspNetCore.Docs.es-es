---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Controla las devoluciones de datos de un Control emergente sin un UpdatePanel (C#) | Documentos de Microsoft
author: wenz
description: El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Cuando se produce un postback en su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 59ffa05945289de6e01e2c21dd5a0f82ca1fa374
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Controla las devoluciones de datos de un Control emergente sin un UpdatePanel (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar cuál de los paneles se ha hecho clic.


## <a name="overview"></a>Información general

El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar cuál de los paneles se ha hecho clic.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con una devolución de datos, pero sin necesidad de un `UpdatePanel` en la página, el Kit de herramientas de Control no ofrece una manera de determinar qué elemento de cliente ha desencadenado la ventana emergente que a su vez produjo el postback. Sin embargo, un pequeño truco proporciona una solución alternativa para este escenario.

En primer lugar, mostramos la configuración básica: dos cuadros de texto que desencadenen la misma ventana emergente, un calendario. Dos `PopupControlExtenders` reunir los cuadros de texto y elementos emergentes.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

La idea básica consiste en Agregar un campo de formulario oculto en el &lt; `form` &gt; elemento que contiene el cuadro de texto que inicia la ventana emergente:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Cuando se carga la página, el código de JavaScript agrega un controlador de eventos a ambos cuadros de texto: cada vez que el usuario hace clic en un cuadro de texto, se escribe su nombre en el campo oculto del formulario:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

En el código del lado servidor, se debe leer el valor del campo oculto. Puesto que son triviales para manipular campos ocultos de formulario, se requiere un enfoque de lista de permitidos para validar el valor hidden. Una vez identificado el cuadro de texto correcto, la fecha del calendario se escribe en él.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Siguiente](using-multiple-popup-controls-vb.md)
