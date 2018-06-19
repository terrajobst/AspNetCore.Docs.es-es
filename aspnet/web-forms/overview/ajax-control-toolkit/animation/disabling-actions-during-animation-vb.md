---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deshabilite las acciones durante la animación (VB) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También admite la acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868651"
---
<a name="disabling-actions-during-animation-vb"></a>Deshabilite las acciones durante la animación (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También admite acciones, como clics del mouse. Sin embargo cuando un clic del mouse, inicia una animación, es deseable deshabilitar clics del mouse durante la animación.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También admite acciones, como clics del mouse. Sin embargo cuando un clic del mouse, inicia una animación, es deseable deshabilitar clics del mouse durante la animación.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

La animación se aplicarán a un botón HTML similar al siguiente:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Tenga en cuenta que se utiliza un HTML Control en lugar de un Control Web, puesto que deseamos que el botón para crear un valor devuelto; solo debe iniciar la animación de cliente para que podamos.

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

En el `<Animations>` nodo, `<OnClick>` es el elemento correcto para controlar el mouse (ratón), haga clic en. Sin embargo, puede hacer clic en el botón durante la animación, también. El `<EnableAction>` elemento puede ocuparse de ese. Establecer `Enabled="false"` deshabilita el botón como parte de la animación. Puesto que estamos usando varias animaciones individuales (deshabilitar el botón y las animaciones reales), el `<Parallel>` elemento es necesario para pegar las animaciones juntos en una sola. Este es el marcado completo para `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

También sería posible volver a habilitar al botón después de la animación, utilizando el elemento XML siguiente al final de la lista:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Sin embargo en el escenario que nos ocupa sería inservible desde el botón atenúa y no está visible al final de la animación.


[![El botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic aquí para ver la imagen a tamaño completo](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-vb.md)
> [Siguiente](triggering-an-animation-in-another-control-vb.md)
