---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Controlar los Postbacks desde un Control emergente sin un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Cuando se produce un postback en unidades de búsqueda...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ef1d0300cba42eefb5f6cda77693aeafb93a31a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363849"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Controlar los Postbacks desde un Control emergente sin un UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar qué panel se ha hecho clic.


## <a name="overview"></a>Información general

El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar qué panel se ha hecho clic.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con una devolución de datos, pero sin tener un `UpdatePanel` en la página, el Kit de herramientas de Control no ofrece una manera de determinar qué elemento de cliente ha desencadenado la ventana emergente que a su vez la originaron. Sin embargo, un pequeño truco proporciona una solución alternativa para este escenario.

En primer lugar, aquí es la configuración básica: dos cuadros de texto que desencadenen la mismo ventana emergente, un calendario. Dos `PopupControlExtenders` reunir los cuadros de texto y el elemento emergente.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

La idea básica consiste en Agregar un campo de formulario oculto en el &lt; `form` &gt; elemento que contiene el cuadro de texto que se inicia la ventana emergente:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Cuando se carga la página, el código de JavaScript agrega un controlador de eventos en ambos cuadros de texto: cada vez que el usuario hace clic en un cuadro de texto, se escribe su nombre en el campo de formulario oculto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

En el código del lado servidor, se debe leer el valor del campo oculto. Puesto que son triviales para manipular campos ocultos de formulario, se requiere un enfoque de lista de permitidos para validar el valor hidden. Una vez identificado el cuadro de texto correcto, la fecha del calendario se escribe en él.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
