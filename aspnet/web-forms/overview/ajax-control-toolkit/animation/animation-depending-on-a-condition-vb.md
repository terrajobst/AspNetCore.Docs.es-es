---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animación según una condición (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si una animación es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 160b53519382c215db039fd6297bb907688b81c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368562"
---
<a name="animation-depending-on-a-condition-vb"></a>Animación según una condición (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si se ejecuta una animación o no puede también dependen de una condición en forma de código JavaScript.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si se ejecuta una animación o no puede también dependen de una condición en forma de código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente. En lugar de una de las animaciones regulares, el `<Condition>` elemento entra en juego. El código JavaScript proporcionado como el valor de la `ConditionScript` atributo se ejecuta en tiempo de ejecución. Si se evalúa como true, la animación se ejecuta, en caso contrario, no. El siguiente marcado proporciona dos animaciones, cada uno de ellos que se está ejecutando en el 50% de los casos al azar. Puesto que sólo puede haber una animación en `<OnLoad>`, los dos `<Condition>` las animaciones se unen entre sí mediante el `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Tenga en cuenta que el signo menor que (`<`) en el `ConditionScript` atributo debe ser de escape (de). Cuando ejecute este script, no hay ejecuciones de animación, o uno de los dos, o ambas lo hacen.


[![El panel es difuminación sin cambiar el tamaño, por lo que no las segundas ejecuciones de animación, la primera de ellas](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

El panel es difuminación sin cambiar el tamaño, por lo que no las segundas ejecuciones de animación, la primera de ellas ([haga clic aquí para ver imagen en tamaño completo](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-vb.md)
> [Siguiente](picking-one-animation-out-of-a-list-vb.md)
