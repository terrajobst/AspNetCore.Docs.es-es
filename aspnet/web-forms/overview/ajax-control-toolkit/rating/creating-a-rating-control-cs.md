---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Crear un Control de clasificación (C#) | Documentos de Microsoft
author: wenz
description: Muchos sitios Web, de comercio electrónico para sitios de Comunidades, ofrecen a los usuarios de artículos de velocidad o elementos. Esto normalmente requiere algunos esfuerzo de codificación, pero tenemos el...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-rating-control-c"></a>Crear un Control de clasificación (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Muchos sitios Web, de comercio electrónico para sitios de Comunidades, ofrecen a los usuarios de artículos de velocidad o elementos. Esto normalmente requiere algunos esfuerzo de codificación, pero tenemos el Kit de herramientas de Control para la eliminación.


## <a name="overview"></a>Información general

Muchos sitios Web, de comercio electrónico para sitios de Comunidades, ofrecen a los usuarios de artículos de velocidad o elementos. Esto normalmente requiere algunos esfuerzo de codificación, pero tenemos el Kit de herramientas de Control para la eliminación.

## <a name="steps"></a>Pasos

En primer lugar, necesita (por lo menos) dos tipos de imágenes: uno para una forma rellena el elemento clasificación y uno para un elemento vacío clasificación. Un elemento de clasificación es normalmente una estrella o una cara. En este escenario, encontrará tres archivos, smiley.png y empty.png y done.png sonriente como parte de las descargas de código de origen para este tutorial.

A continuación, cree un nuevo archivo ASP.NET y comience por agregar una `ScriptManager` control a él:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

A continuación, agregue el `Rating` control desde el Kit de herramientas de Control de AJAX de ASP.NET. Los siguientes atributos deben establecerse en este ejemplo:

- `CurrentRating` la clasificación inicial que se usará
- `MaxRating` el valor máximo de clasificación
- `EmptyStarCssClass` la clase CSS que se usará cuando un elemento de clasificación (estrella) está vacía
- `FilledStarCssClass` la clase CSS que se usará cuando se rellena un elemento de clasificación (estrella)
- `StarCssClass` la clase CSS que se usará para un estado visible
- `WaitingStarCssClass` la clase CSS que use mientras una clasificación por estrellas se envía al servidor

Y aquí es el marcado que crea un control de clasificación con cinco elementos (emoticones) de los cuales ninguno se rellenan inicialmente:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Las tres clases CSS que se hace referencia ahora necesitan mostrar los archivos de imagen adecuada, que es fácil de hacer uso de CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Asegúrese de que proporciona el ancho y alto de las tres imágenes, en caso contrario, la visualización puede parecer un poco punto seguridad.

Por último, el resultado de la clasificación debe mostrar al usuario (o, al menos se guardan en una base de datos). Por lo tanto, agregue una etiqueta para la salida de un mensaje de texto y un botón de envío para enviar el formulario de clasificación en el servidor de nuevo:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

En el código del lado servidor, tenga acceso al control de clasificación a través de su `ID` y, a continuación, obtener acceso a su `CurrentRating` propiedad que es el número de los elementos de clasificación seleccionado, en nuestro ejemplo, un valor entre 0 y 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Guarde la página y cargarlo en el explorador. Al mantener el mouse sobre los elementos de la clasificación (inicialmente vacía), se produce un efecto de JavaScript: los cambios de clasificación. Al hacer clic en el conjunto de estrellas, se conserva la clasificación actual. Por último, cuando se envía el formulario, el código del lado servidor genera el valor de clasificación seleccionado.


[![Creación de un sistema de clasificación con código mínimo](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Creación de un sistema de clasificación con código mínimo ([haga clic aquí para ver la imagen a tamaño completo](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](creating-a-rating-control-vb.md)
