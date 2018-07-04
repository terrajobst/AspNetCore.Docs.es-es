---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Agregar animación a un Control (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Este tutorial se muestra cómo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3909422dc5d261b39f3efd7d7eaeb5cfb1976f2b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364931"
---
<a name="adding-animation-to-a-control-vb"></a>Agregar animación a un Control (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Este tutorial muestra cómo configurar este tipo de una animación.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Este tutorial muestra cómo configurar este tipo de una animación.

## <a name="steps"></a>Pasos

El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

La animación en este escenario se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

La clase CSS asociada para el panel define un ancho y un color de fondo:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

A continuación, necesitamos el `AnimationExtender`. Después de proporcionar un `ID` y el habitual `runat="server"`, el `TargetControlID` atributo debe establecerse para el control se va a animar en nuestro caso, el panel:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Mediante declaración, se aplica la animación completa mediante una sintaxis XML, lamentablemente actualmente no admitida totalmente IntelliSense de Visual Studio. El nodo raíz es `<Animations>;` dentro de este nodo, se permiten varios eventos que determinan cuándo la animación adopten lugar:

- `OnClick` (clic del mouse)
- `OnHoverOut` (cuando el mouse sale de un control)
- `OnHoverOver` (al mantener el mouse sobre un control, deteniendo el `OnHoverOut` animación)
- `OnLoad` (cuando se ha cargado la página)
- `OnMouseOut` (cuando el mouse sale de un control)
- `OnMouseOver` (al mantener el mouse sobre un control, no Deteniendo el `OnMouseOut` animación)

El marco de trabajo incluye un conjunto de animaciones, cada uno representado por su propio elemento XML. Aquí es una selección:

- `<Color>` (cambiar un color)
- `<FadeIn>` (resaltar)
- `<FadeOut>` (difuminado)
- `<Property>` (cambiar la propiedad de un control)
- `<Pulse>` (en alza)
- `<Resize>` (cambiar el tamaño)
- `<Scale>` (cambiar proporcionalmente el tamaño)

En este ejemplo, el panel será fundido de salida. La animación adoptarán 1,5 segundos (`Duration` atributo), mostrar (pasos de animación) de 24 fotogramas por segundo (`Fps` attributs). Aquí está el marcado completo para el `AnimationExtender` control:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Al ejecutar este script, el panel se muestra y atenúa en segundos de uno y medio.


[![El panel se atenúa](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

El panel se atenúa ([haga clic aquí para ver imagen en tamaño completo](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Siguiente](executing-several-animations-at-the-same-time-vb.md)
