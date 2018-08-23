---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Desencadenar una animación en otro Control (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Por lo general, iniciar un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 132f9f85eccabc890308984b9e78ed1d2212c57a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836620"
---
<a name="triggering-an-animation-in-another-control-vb"></a>Desencadenar una animación en otro Control (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Por lo general, iniciar una animación se desencadena mediante la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Por lo general, iniciar una animación se desencadena mediante la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Para iniciar el panel de la animación, se usa un botón HTML. Tenga en cuenta que `<input type="button" />` es favorecido sobre `<asp:Button />` puesto que no deseamos una devolución de datos cuando el usuario hace clic en ese botón.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`. Es importante establecer `TargetControlID` al identificador del botón (el elemento desencadenar la animación), no para el Id. del panel (el elemento que se está animando)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

Dentro de la `<Animations>` nodo, las animaciones de contexto como de costumbre. Para que sean cambiar el panel, no en el botón, establezca la `AnimationTarget` atributo para cada elemento de animación en `AnimationExtender`. El valor de `AnimationTarget` es el identificador del panel, por supuesto. De este modo, las animaciones que se producen con el panel, no con el botón de desencadenamiento. Este es el `AnimationExtender` marcado para este escenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Tenga en cuenta el orden en especial en el que aparecen las animaciones individuales. En primer lugar, el botón desactiva una vez que se ejecuta la animación. Dado que no hay ningún `AnimationTarget` atributo el `<EnableAction>` elemento, esta animación se aplica al control de origen: el botón. Los pasos de la animación en los dos próximos se llevará a cabo parallelly (`<Parallel>` elemento). Ambos tienen sus `AnimationTarget` atributos establecidos en `"Panel1"`, animar, por tanto, el panel, no en el botón.


[![Un clic del mouse en el botón inicia la animación de panel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Un clic del mouse en el botón inicia la animación de panel ([haga clic aquí para ver imagen en tamaño completo](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-vb.md)
> [Siguiente](modifying-animations-from-the-server-side-vb.md)
