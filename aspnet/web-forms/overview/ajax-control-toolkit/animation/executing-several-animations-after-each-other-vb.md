---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Ejecutar varias animaciones uno tras otro (VB) | Documentos de Microsoft
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: e949d181c0b742ee38ebbcc46e0e08efc678a1f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-vb"></a>Ejecutar varias animaciones uno tras otro (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Permite para ejecutar varias animaciones uno tras otro.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Permite para ejecutar varias animaciones uno tras otro.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria`runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

En el `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones una vez que la página se ha cargado completamente. Por lo general, `<OnLoad>` solo acepta una animación. El marco de trabajo de animación le permite unir varios animaciones en uno solo mediante la `<Sequence>` elemento. Todas las animaciones en `<Sequence>` son ejecutada uno tras otro. Aquí es el marcado posibles para el `AnimationExtender` control, en primer lugar hacer que el panel más amplio y, a continuación, disminuir su alto:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Al ejecutar este script, el panel de primera obtiene más anchos y, a continuación, más pequeños.


[![En primer lugar se ha aumentado el ancho](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

En primer lugar se ha aumentado el ancho ([haga clic aquí para ver la imagen a tamaño completo](executing-several-animations-after-each-other-vb/_static/image3.png))


[![A continuación, se reduce el alto](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

A continuación, se reduce el alto ([haga clic aquí para ver la imagen a tamaño completo](executing-several-animations-after-each-other-vb/_static/image6.png))

>[!div class="step-by-step"]
[Anterior](executing-several-animations-at-the-same-time-vb.md)
[Siguiente](animation-depending-on-a-condition-vb.md)
