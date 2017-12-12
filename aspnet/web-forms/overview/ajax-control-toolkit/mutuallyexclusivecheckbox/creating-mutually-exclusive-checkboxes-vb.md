---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: "Creación de casillas de verificación mutuamente excluyentes (VB) | Documentos de Microsoft"
author: wenz
description: "Cuando se puede seleccionar sólo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Hay una desventaja, sin embargo: una vez que se selecciona un botón de radio en un grupo,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 023ca0b145de8147a98e78f4dba20846dc344f06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Creación de casillas de verificación mutuamente excluyentes (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Cuando se puede seleccionar sólo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Hay una desventaja, sin embargo: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio. Casillas de verificación se puede deshabilitar en cualquier momento, pero no se excluyen mutuamente. Este tutorial ofrece lo mejor de ambos enfoques: casillas de verificación que se excluyen mutuamente.


## <a name="overview"></a>Información general

Cuando se puede seleccionar sólo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Hay una desventaja, sin embargo: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio. Casillas de verificación se puede deshabilitar en cualquier momento, pero no se excluyen mutuamente. Este tutorial ofrece lo mejor de ambos enfoques: casillas de verificación que se excluyen mutuamente.

## <a name="steps"></a>Pasos

El Kit de herramientas de Control de AJAX de ASP.NET contiene el extensor MutuallyExclusiveCheckBox. Esto permite a los programadores asignar cualquier casilla a un nombre de grupo (`Key` atributo). En todas las casillas de verificación dentro del mismo grupo, sólo uno puede seleccionarse al mismo tiempo.

Comencemos con dos casillas de verificación a colocar en una nueva página ASP.NET. Puede haber más, pero dos de ellos son suficientes para demostrar la entidad de seguridad:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Para ambas casillas de verificación, un control de MutuallyExclusiveCheckBoxExtender debe situarse en la página. Ambos atributos de clave deben tener el mismo valor, el valor de los atributos de elementos de botón de radio HTML deben ser idénticos para denotar el grupo al que pertenecen. La propiedad TargetControlID del extensor de apunta al identificador de la casilla de verificación.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Por último, incluya el AJAX de ASP.NET `ScriptManager` necesario para todos los elementos del Kit de herramientas del Control de AJAX de ASP.NET:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Guarde y ejecute la página: puede comprobar y desactive las casillas de verificación, sin embargo en ningún momento ambas casillas de verificación se puede comprobar.


[![Se puede comprobar solo una casilla a la vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Se puede comprobar solo una casilla a la vez ([haga clic aquí para ver la imagen a tamaño completo](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](creating-mutually-exclusive-checkboxes-cs.md)
