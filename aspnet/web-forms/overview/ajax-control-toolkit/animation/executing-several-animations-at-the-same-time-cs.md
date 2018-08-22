---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Ejecutar varias animaciones al mismo tiempo (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: d70f7b9170cbd3307dae4cdb4f9ee735e3c5bee8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829733"
---
<a name="executing-several-animations-at-the-same-time-c"></a>Ejecutar varias animaciones al mismo tiempo (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar varias animaciones en paralelo.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar varias animaciones en paralelo.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente. Por lo general, `<OnLoad>` solo acepta una animación. El marco de animación le permite unir varias animaciones en uno solo mediante la `<Parallel>` elemento. Todas las animaciones en `<Parallel>` se ejecutan al mismo tiempo.

Este es el un marcado posible para el `AnimationExtender` control difuminación y el tamaño del panel al mismo tiempo:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

Y realmente: al ejecutar este script, el panel se muestra, a continuación, se cambia el tamaño (más que threefolding su ancho y halfing su alto) y el fundido de salida al mismo tiempo.


[![El panel se atenúa y cambiar el tamaño (incluido su contenido, gracias al motor de representación del explorador)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

El panel se atenúa y cambiar el tamaño (incluido su contenido, gracias al motor de representación del explorador) ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adding-animation-to-a-control-cs.md)
> [Siguiente](executing-several-animations-after-each-other-cs.md)
