---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Ejecutar varias animaciones una tras otra (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d9e1ce0c156752ce0e97b91f253ced26360a721
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364758"
---
<a name="executing-several-animations-after-each-other-vb"></a>Ejecutar varias animaciones una tras otra (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Permite ejecutar varias animaciones una tras otra.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Permite ejecutar varias animaciones una tras otra.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente. Por lo general, `<OnLoad>` solo acepta una animación. El marco de animación le permite unir varias animaciones en uno solo mediante la `<Sequence>` elemento. Todas las animaciones en `<Sequence>` son ejecutada uno tras otro. Aquí está el un marcado posible para el `AnimationExtender` control realizar primero la más amplia de panel y, a continuación, reduce su alto:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Al ejecutar este script, el panel de primera obtiene más anchos y, a continuación, más pequeños.


[![En primer lugar el ancho aumenta](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

En primer lugar se aumenta el ancho ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-after-each-other-vb/_static/image3.png))


[![A continuación, se reduce la altura](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

A continuación, se reduce el alto ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-at-the-same-time-vb.md)
> [Siguiente](animation-depending-on-a-condition-vb.md)
