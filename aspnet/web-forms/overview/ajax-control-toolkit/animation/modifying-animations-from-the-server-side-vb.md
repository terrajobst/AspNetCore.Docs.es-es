---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modificación de las animaciones del lado servidor (VB) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones también pueden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869275"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Modificación de las animaciones del lado servidor (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones también podrán modificarse en el servidor


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones también podrán modificarse en el servidor

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

El resto del código se ejecuta en el servidor y no usa marcado; en su lugar, utiliza código para crear el `AnimationExtender` control:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Sin embargo, el Kit de herramientas de Control actualmente no proporciona un acceso de API para crear las animaciones individuales. Sin embargo, es posible establecer el `AnimationExtender`del propiedad animaciones en una cadena que contiene el marcado XML que se utilizan al asignar las animaciones mediante declaración. Para crear el XML que no debe contener el `<Animations>` elemento podría utilizar XML de .NET Framework admiten o, como se muestra en el siguiente código, basta con que proporcione la cadena:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Por último, agregue el `AnimationExtender` en el control a la página actual, el `<form runat="server">` elemento, asegurándose de que la animación se incluye y se ejecuta:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![La animación se crea mediante código del lado servidor C# /VB](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

La animación se crea mediante código del lado servidor C# /VB ([haga clic aquí para ver la imagen a tamaño completo](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](triggering-an-animation-in-another-control-vb.md)
> [Siguiente](executing-animations-using-client-side-code-vb.md)
