---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Crear un valor numérico arriba/abajo Control con un back-end de Web Service (C#) | Documentos de Microsoft
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla de verificación, numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar c como más...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 942902bdba93fe4fef8a9122403c6d5c62e6123c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868820"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Creación de un número hacia arriba o hacia abajo el Control con un back-end de Web Service (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar como más cómodo. De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.


## <a name="overview"></a>Información general

En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar como más cómodo. De forma predeterminada, el `NumericUpDown` control siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.

## <a name="steps"></a>Pasos

El Kit de herramientas de Control de AJAX de ASP.NET contiene el `NumericUpDown` extensor que agrega automáticamente dos botones a un cuadro de texto: uno para aumentar su valor, uno para disminuirlo. Sin embargo, el control también admite una llamada al servicio web (o la llamada al método de página). Cada vez que el hacia arriba o hacia abajo del botón se hace clic, el código de JavaScript código se conecta al servidor web y ejecuta un método no existe. La firma del método es el siguiente:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

El `current` argumento es el valor actual en el cuadro de texto; el `tag` atributo es datos de contexto adicional que se pueden establecer como una propiedad de la `NumericUpDown` extender (pero no es necesario).

En este ejemplo, el valor numérico arriba/abajo control solo debe permitir valores que son potencias de dos: 1, 2, 4, 8, 16, 32, 64 y así sucesivamente. Por lo tanto, el método que se ejecuta cuando el usuario desea aumentar el valor debe duplicar el valor antiguo; el otro método debe dividir el valor por dos. Por lo que este es el servicio web completa:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Finalmente, cree una nueva página ASP.NET. Como es habitual, necesita un `ScriptManager` (control), un `TextBox` control y un `NumericUpDownExtender` control. Por último, tendrá que proporcionar la información del servicio web:

- `ServiceDownMethod` nombre de la profundidad método web o página (método)
- `ServiceDownPath` ruta de acceso al servicio web con el método de servicio hacia abajo; Omitir si está utilizando un método de página
- `ServiceUpMethod` nombre de la seguridad método web o página (método)
- `ServiceUpPath` ruta de acceso al servicio web con el método de servicio arriba; Omitir si está utilizando un método de página

Este es el marcado completo para la página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Si ejecuta la página, observe cómo el valor en el cuadro de texto siempre duplica al hacer clic en el botón superior y está a la mitad al hacer clic en el botón inferior.


[![Aparecen solo los números que son una potencia de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Aparecen solo los números que son una potencia de 2 ([haga clic aquí para ver la imagen a tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
