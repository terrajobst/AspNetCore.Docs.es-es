---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animación según una condición (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si una animación es...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: cb08c330d6fbc86035a2f21ad382cc009411bcd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808429"
---
<a name="animation-depending-on-a-condition-c"></a>Animación según una condición (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si se ejecuta una animación o no puede también dependen de una condición en forma de código JavaScript.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si se ejecuta una animación o no puede también dependen de una condición en forma de código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente. En lugar de una de las animaciones regulares, el `<Condition>` elemento entra en juego. El código JavaScript proporcionado como el valor de la `ConditionScript` atributo se ejecuta en tiempo de ejecución. Si se evalúa como true, la animación se ejecuta, en caso contrario, no. El siguiente marcado proporciona dos animaciones, cada uno de ellos que se está ejecutando en el 50% de los casos al azar. Puesto que sólo puede haber una animación en `<OnLoad>`, los dos `<Condition>` las animaciones se unen entre sí mediante el `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Tenga en cuenta que el signo menor que (`<`) en el `ConditionScript` atributo debe ser de escape (de). Cuando ejecute este script, no hay ejecuciones de animación, o uno de los dos, o ambas lo hacen.


[![El panel es difuminación sin cambiar el tamaño, por lo que no las segundas ejecuciones de animación, la primera de ellas](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

El panel es difuminación sin cambiar el tamaño, por lo que no las segundas ejecuciones de animación, la primera de ellas ([haga clic aquí para ver imagen en tamaño completo](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-cs.md)
> [Siguiente](picking-one-animation-out-of-a-list-cs.md)
