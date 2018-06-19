---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Agregar animación a un Control (VB) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Este tutorial se muestra cómo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3da98e478c45213875b3829e51351d03571a05b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870640"
---
<a name="adding-animation-to-a-control-vb"></a>Agregar animación a un Control (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Este tutorial muestra cómo configurar una animación de este tipo.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Este tutorial muestra cómo configurar una animación de este tipo.

## <a name="steps"></a>Pasos

El primer paso es como de costumbre incluir la `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

La animación en este escenario se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

La clase CSS asociada para el panel define un ancho y un color de fondo:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

A continuación, necesitamos la `AnimationExtender`. Después de proporcionar un `ID` y el habitual `runat="server"`, el `TargetControlID` atributo debe establecerse en el control para animar en nuestro caso, el panel:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Toda la animación se aplica mediante declaración, utilizando una sintaxis XML, que Desafortunadamente actualmente no se admiten totalmente IntelliSense de Visual Studio. El nodo raíz es `<Animations>;` dentro de este nodo, se permiten varios eventos que determinan si la animación adopten lugar:

- `OnClick` (haga clic del mouse)
- `OnHoverOut` (cuando el mouse sale de un control)
- `OnHoverOver` (cuando se sitúa el mouse sobre un control, detener la `OnHoverOut` animación)
- `OnLoad` (cuando se ha cargado la página)
- `OnMouseOut` (cuando el mouse sale de un control)
- `OnMouseOver` (cuando se sitúa el mouse sobre un control, no detiene el `OnMouseOut` animación)

El marco de trabajo incluye un conjunto de animaciones, cada uno de ellos representado por su propio elemento XML. Esta es una selección:

- `<Color>` (cambiar un color)
- `<FadeIn>` (resaltar)
- `<FadeOut>` (difuminado)
- `<Property>` (cambiar la propiedad del control)
- `<Pulse>` (pulsating)
- `<Resize>` (cambiar el tamaño)
- `<Scale>` (y cambiar el tamaño proporcionalmente)

En este ejemplo, el panel será fundido de salida. La animación adoptarán 1,5 segundos (`Duration` atributo), mostrar 24 fotogramas (pasos de animación) por segundo (`Fps` attributs). Este es el marcado completo para el `AnimationExtender` control:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Al ejecutar este script, el panel se muestra y atenúa en segundos de uno y medio.


[![El panel se atenúa](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

El panel se atenúa ([haga clic aquí para ver la imagen a tamaño completo](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Siguiente](executing-several-animations-at-the-same-time-vb.md)
