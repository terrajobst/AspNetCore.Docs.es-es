---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: "Desencadenar una animación en otro Control (VB) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Por lo general, iniciar un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a>Desencadenar una animación en otro Control (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Por lo general, iniciar una animación desencadenan la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Por lo general, iniciar una animación desencadenan la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Para iniciar el panel de la animación, se usa un botón HTML. Tenga en cuenta que `<input type="button" />` favorecido sobre `<asp:Button />` puesto que no deseamos una devolución de datos cuando el usuario hace clic en ese botón.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`. Es importante establecer `TargetControlID` con el identificador del botón (el elemento desencadenar la animación), no para el Id. del panel (es decir, el elemento que se está animando)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

En el `<Animations>` nodo, animaciones de contexto como de costumbre. Para que pueda cambiar el panel, no en el botón, establezca la `AnimationTarget` atributo para cada elemento de animación en `AnimationExtender`. El valor de `AnimationTarget` es el identificador del panel, por supuesto. De este modo, las animaciones suceder con el panel, no con el botón de activación. Este es el `AnimationExtender` marcado para este escenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Tenga en cuenta el orden especial en el que aparecen las animaciones individuales. En primer lugar, el botón obtiene desactiva una vez que se ejecuta la animación. Dado que no hay ningún `AnimationTarget` de atributo en el `<EnableAction>` elemento, esta animación se aplica al control de origen: el botón. Los pasos siguientes dos animación se llevará a cabo parallelly (`<Parallel>` elemento). Ambos tienen sus `AnimationTarget` atributos establecidos en `"Panel1"`, animación, por tanto, el panel, no en el botón.


[![Un clic del mouse en el botón inicia la animación de panel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Un clic del mouse en el botón inicia la animación de panel ([haga clic aquí para ver la imagen a tamaño completo](triggering-an-animation-in-another-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](disabling-actions-during-animation-vb.md)
[Siguiente](modifying-animations-from-the-server-side-vb.md)
