---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: "Modificación de las animaciones del lado servidor (C#) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones también pueden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: e1f9bda03b3e3edf3bbdc591cde9858944d39321
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-c"></a>Modificación de las animaciones del lado servidor (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones también podrán modificarse en el servidor


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones también podrán modificarse en el servidor

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

El resto del código se ejecuta en el servidor y no usa marcado; en su lugar, utiliza código para crear el `AnimationExtender` control:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Sin embargo, el Kit de herramientas de Control actualmente no proporciona un acceso de API para crear las animaciones individuales. Sin embargo, es posible establecer el `AnimationExtender`del propiedad animaciones en una cadena que contiene el marcado XML que se utilizan al asignar las animaciones mediante declaración. Para crear el XML que no debe contener el `<Animations>` elemento podría utilizar XML de .NET Framework admiten o, como se muestra en el siguiente código, basta con que proporcione la cadena:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Por último, agregue el `AnimationExtender` en el control a la página actual, el `<form runat="server">` elemento, asegurándose de que la animación se incluye y se ejecuta:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![La animación se crea mediante código del lado servidor C# /VB](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

La animación se crea mediante código del lado servidor C# /VB ([haga clic aquí para ver la imagen a tamaño completo](modifying-animations-from-the-server-side-cs/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](triggering-an-animation-in-another-control-cs.md)
[Siguiente](executing-animations-using-client-side-code-cs.md)
