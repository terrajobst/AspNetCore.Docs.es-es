---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Seleccionar una animación de una lista (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. El marco de trabajo también permitir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c60d7cff7c841d23185fbdf07abf0e894b21cf5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836086"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Seleccionar una animación de una lista (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. El marco también permite al programador seleccionar una animación de una lista de animaciones, dependiendo de la evaluación de código JavaScript.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. El marco también permite al programador seleccionar una animación de una lista de animaciones, dependiendo de la evaluación de código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente. En lugar de una de las animaciones regulares, el `<Case>` elemento entra en juego. Se evalúa el valor de su atributo SelectScript; el valor devuelto debe ser numérico. Según este número, una de las subanimaciones dentro de &lt;caso&gt; se ejecuta. Por ejemplo, si se evalúa como SelectScript a 2, el Kit de herramientas de Control se ejecuta la animación terceros dentro de &lt;caso&gt; (contando comienza en 0).

El marcado siguiente define tres de las subanimaciones: ajustar el tamaño, cambiar el alto y atenúa. El código de JavaScript (`Math.floor(3 * Math.random())`), a continuación, elige un número entre 0 y 2, por lo que se ejecuta una de las tres animaciones:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Una de las animaciones de tres posibles: el panel obtiene más amplio](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Una de las animaciones de tres posibles: el panel obtiene más amplio ([haga clic aquí para ver imagen en tamaño completo](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animation-depending-on-a-condition-vb.md)
> [Siguiente](animating-in-response-to-user-interaction-vb.md)
