---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Crear casillas de verificación mutuamente excluyentes (C#) | Microsoft Docs
author: wenz
description: 'Cuando se puede seleccionar solo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Un inconveniente, sin embargo hay: una vez que se selecciona un botón de radio en un grupo,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: b5850c2e8d053d61e6230d3a24ddbbfe215bc531
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396141"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Crear casillas de verificación mutuamente excluyentes (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Cuando se puede seleccionar solo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Un inconveniente, sin embargo hay: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio. Las casillas de verificación puede desactivarse en cualquier momento, sin embargo, no son mutuamente excluyentes. Este tutorial ofrece lo mejor de ambos enfoques: las casillas de verificación que se excluyen mutuamente.


## <a name="overview"></a>Información general

Cuando se puede seleccionar solo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Un inconveniente, sin embargo hay: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio. Las casillas de verificación puede desactivarse en cualquier momento, sin embargo, no son mutuamente excluyentes. Este tutorial ofrece lo mejor de ambos enfoques: las casillas de verificación que se excluyen mutuamente.

## <a name="steps"></a>Pasos

ASP.NET AJAX Control Toolkit contiene los extensores MutuallyExclusiveCheckBox. Esto permite a los programadores asignar cualquier casilla a un nombre de grupo (`Key` atributo). En todas las casillas de verificación dentro del mismo grupo, sólo uno puede seleccionarse al mismo tiempo.

Comencemos con la colocación de dos casillas de verificación en una nueva página ASP.NET. Puede haber más, pero dos de ellos es suficiente para demostrar el principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Para ambas casillas de verificación, se debe colocar un control MutuallyExclusiveCheckBoxExtender en la página. Ambos atributos de clave deben tener el mismo valor, al igual que el valor de los atributos de elementos de botón de radio HTML deben ser idénticos para denotar el grupo al que pertenece. La propiedad TargetControlID del extensor apunta al identificador de la casilla de verificación.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Por último, se incluyen ASP.NET AJAX `ScriptManager` que es necesario para todos los elementos de ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Guarde y ejecute la página: se puede activar y desactivar ambas casillas de verificación, pero en ningún momento ambas casillas de verificación se puede comprobar.


[![Se puede comprobar solo una casilla a la vez](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Se puede comprobar solo una casilla a la vez ([haga clic aquí para ver imagen en tamaño completo](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](creating-mutually-exclusive-checkboxes-vb.md)
