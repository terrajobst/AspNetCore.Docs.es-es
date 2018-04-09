---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Enlace de datos del Control deslizante (VB) | Documentos de Microsoft
author: wenz
description: El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible enlazar el sición actual...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="databinding-the-slider-control-vb"></a>Enlace de datos del Control deslizante (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.


## <a name="overview"></a>Información general

El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

A continuación, agregue dos `TextBox` controles a la página. Uno se transformará en un control deslizante gráfico y el otro se mantendrá la posición del control deslizante.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

El siguiente paso ya es el último paso. El `SliderExtender` control desde el Kit de herramientas de Control de AJAX de ASP.NET realiza un control deslizante del primer cuadro de texto y el segundo cuadro de texto se actualiza automáticamente cuando el control deslizante posición cambios. En orden para que esto funcione, el `SliderExtender`del `TargetControlID` atributo debe establecerse en el identificador del primer cuadro de texto; el `BoundControlID` atributo debe establecerse en el identificador del segundo cuadro de texto.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: escribir un nuevo valor en el cuadro de texto actualiza la posición del control deslizante. Si realiza el segundo cuadro de texto de solo lectura, puede agregar una protección no segura para el campo de texto para que resulte más difícil para el usuario actualizar manualmente el valor de allí.


[![Control deslizante y cuadro de texto están sincronizadas](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Control deslizante y el cuadro de texto están sincronizados ([haga clic aquí para ver la imagen a tamaño completo](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-the-slider-control-with-auto-postback-vb.md)
