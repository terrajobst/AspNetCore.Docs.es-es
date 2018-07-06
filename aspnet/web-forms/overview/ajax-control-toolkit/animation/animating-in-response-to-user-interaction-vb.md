---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animaciones en respuesta a la interacción del usuario (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden star...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b774a058e715e4a98e767daf92886f24e627822
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815530"
---
<a name="animating-in-response-to-user-interaction-vb"></a>Animaciones en respuesta a la interacción del usuario (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden iniciar automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden iniciar automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, existen cinco formas de iniciar la animación a través de la interacción del usuario (el elemento que falta es `<OnLoad>` que se ejecuta una vez que se ha cargado completamente toda la página):

- `<OnClick>` (clic del mouse en el control)
- `<OnHoverOut>` (mouse deja el control)
- `<OnHoverOver>` (se mantiene el mouse sobre un control, deteniendo el `<OnHoverOut>` animación)
- `<OnMouseOut>` (el mouse sale de un control)
- `<OnMouseOver>` (se mantiene el mouse sobre un control, no Deteniendo el `<OnMouseOut>` animación)

En este escenario, `<OnClick>` se utiliza. Cuando el usuario hace clic en el panel, se cambia el tamaño y atenúa al mismo tiempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![La animación inicia un clic del mouse](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

La animación inicia un clic del mouse ([haga clic aquí para ver imagen en tamaño completo](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-vb.md)
> [Siguiente](disabling-actions-during-animation-vb.md)
