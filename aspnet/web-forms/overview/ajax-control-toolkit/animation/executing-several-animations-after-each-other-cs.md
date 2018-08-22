---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Ejecutar varias animaciones una tras otra (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 60fb7dacfe59eaafef71fcc9cde61b7840624343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838913"
---
<a name="executing-several-animations-after-each-other-c"></a>Ejecutar varias animaciones una tras otra (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Permite ejecutar varias animaciones una tras otra.


El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Permite ejecutar varias animaciones una tras otra.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente. Por lo general, `<OnLoad>` solo acepta una animación. El marco de animación le permite unir varias animaciones en uno solo mediante la `<Sequence>` elemento. Todas las animaciones en `<Sequence>` son ejecutada uno tras otro. Aquí está el un marcado posible para el `AnimationExtender` control realizar primero la más amplia de panel y, a continuación, reduce su alto:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Al ejecutar este script, el panel de primera obtiene más anchos y, a continuación, más pequeños.


[![En primer lugar el ancho aumenta](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

En primer lugar se aumenta el ancho ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-after-each-other-cs/_static/image3.png))


[![A continuación, se reduce la altura](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

A continuación, se reduce el alto ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-at-the-same-time-cs.md)
> [Siguiente](animation-depending-on-a-condition-cs.md)
