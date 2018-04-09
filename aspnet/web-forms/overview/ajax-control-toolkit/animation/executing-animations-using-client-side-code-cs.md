---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Ejecutar animaciones utilizando el código de cliente (C#) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. La ejecución de la animación...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-c"></a>Ejecutar animaciones utilizando el código de cliente (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También se puede desencadenar la ejecución de animación utilizando el código de JavaScript de cliente personalizada.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También se puede desencadenar la ejecución de animación utilizando el código de JavaScript de cliente personalizada.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

En el `<Animations>` nodo, use `<OnClick>` ejecutar las animaciones una vez el usuario hace clic en el panel. Agregue dos animaciones que se ejecute parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

En aras de la demostración, esta animación (y cualquier otra animación creados mediante el Kit de herramientas de Control) se ejecutan utilizando el código de JavaScript, una vez que se ejecuta la página. En primer lugar se necesitan tener acceso a la `AnimationExtender` control. La biblioteca de AJAX de ASP.NET proporciona la `$find()` función para esta tarea:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

El `AnimationExtender` control expone una API enriquecida, incluidos los métodos con nombres idénticos a los controladores de eventos que se utiliza en el marcado XML: `OnClick()`, `OnLoad()`, y así sucesivamente. Por ejemplo, una llamada de la `OnClick()` el método ejecuta la animación en la `<OnClick>` elemento de la `AnimationExtender` control:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Este es el código de JavaScript completo de cliente que emula el haga clic en el panel de una vez que la página se ha cargado completamente tenga en cuenta que el `pageLoad()` es usar el nombre de función que se llama mediante AJAX de ASP.NET una vez la página e incluyen han sido bibliotecas de JavaScript cargar.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![La animación se ejecuta inmediatamente, sin un clic del mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

La animación se ejecuta inmediatamente, sin un clic del mouse ([haga clic aquí para ver la imagen a tamaño completo](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](modifying-animations-from-the-server-side-cs.md)
> [Siguiente](changing-an-animation-using-client-side-code-cs.md)
