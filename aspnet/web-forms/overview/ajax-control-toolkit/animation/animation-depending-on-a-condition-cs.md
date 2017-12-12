---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: "Animación según una condición (C#) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Si es una animación..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a>Animación según una condición (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Si se ejecuta una animación o no también puede depender de una condición en forma de código JavaScript.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Si se ejecuta una animación o no también puede depender de una condición en forma de código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

En el `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones una vez que la página se ha cargado completamente. En lugar de una de las animaciones regulares, el `<Condition>` elemento entra en juego. El código de JavaScript que se proporciona como el valor de la `ConditionScript` atributo se ejecuta en tiempo de ejecución. Si se evalúa como true, la animación se ejecuta, en caso contrario, no. El siguiente marcado proporciona dos animaciones, cada uno de ellos que se está ejecutando en el 50% de los casos en aleatorio. Dado que solo puede haber una animación en `<OnLoad>`, los dos `<Condition>` animaciones se combinan entre sí mediante el `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Tenga en cuenta que el signo menor que (`<`) en el `ConditionScript` atributo debe ser de escape (). Al ejecutar este script, no hay ejecuciones de animación, o realiza una de las dos o ninguna lo hace.


[![El panel se atenúa, sin cambiar el tamaño, por lo que el segundo ejecuciones de animación, la primera de ellas no](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

El panel se atenúa, sin cambiar el tamaño, por lo que el segundo ejecuciones de animación, la primera de ellas no ([haga clic aquí para ver la imagen a tamaño completo](animation-depending-on-a-condition-cs/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](executing-several-animations-after-each-other-cs.md)
[Siguiente](picking-one-animation-out-of-a-list-cs.md)
