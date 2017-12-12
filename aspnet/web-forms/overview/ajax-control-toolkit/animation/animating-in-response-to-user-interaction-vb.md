---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: "Animar en respuesta a la interacción del usuario (VB) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones pueden estrellas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 3219e9d126b3225bfc78d08fb3ac7ef4cc3dca75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-vb"></a>Animar en respuesta a la interacción del usuario (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones pueden iniciar automáticamente o se pueden desencadenar mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones pueden iniciar automáticamente o se pueden desencadenar mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

En el `<Animations>` nodo, hay cinco formas de iniciar la animación a través de la interacción del usuario (el elemento que falta es `<OnLoad>` que se ejecuta una vez que se ha cargado completamente toda la página):

- `<OnClick>`(el mouse (ratón) haga clic en el control)
- `<OnHoverOut>`(mouse deja el control)
- `<OnHoverOver>`(se sitúa el mouse sobre un control, detener la `<OnHoverOut>` animación)
- `<OnMouseOut>`(el mouse sale de un control)
- `<OnMouseOver>`(se sitúa el mouse sobre un control, no se detiene la `<OnMouseOut>` animación)

En este escenario, `<OnClick>` se utiliza. Cuando el usuario hace clic en el panel, se cambia el tamaño y fundido de salida al mismo tiempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![La animación inicia un clic del mouse](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

La animación inicia un clic del mouse ([haga clic aquí para ver la imagen a tamaño completo](animating-in-response-to-user-interaction-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](picking-one-animation-out-of-a-list-vb.md)
[Siguiente](disabling-actions-during-animation-vb.md)
