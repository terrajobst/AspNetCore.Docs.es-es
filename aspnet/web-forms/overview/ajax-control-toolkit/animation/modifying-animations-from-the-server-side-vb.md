---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modificar las animaciones desde el lado del servidor (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden también...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: d39a40f2776d5b87bf82d1a6c6282ce920f4a907
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401959"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Modificar las animaciones desde el lado del servidor (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También se pueden cambiar las animaciones en el lado servidor


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También se pueden cambiar las animaciones en el lado servidor

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

El resto del código se ejecuta en el lado del servidor y no usa marcado; en su lugar, utiliza código para crear el `AnimationExtender` control:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Sin embargo, el Kit de herramientas de Control actualmente no proporciona un acceso de API para crear las animaciones individuales. Sin embargo, es posible establecer el `AnimationExtender`propiedad Animations en una cadena que contiene el marcado XML que se usa al asignar las animaciones mediante declaración. Con el fin de crear el XML que no debe contener el `<Animations>` elemento podría usar XML de .NET Framework admite o, como se muestra en el código siguiente, basta con que proporcione la cadena:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Por último, agregue el `AnimationExtender` en el control a la página actual, el `<form runat="server">` elemento, asegurándose de que la animación se incluye y se ejecuta:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![La animación se crea con C# / VB código de servidor](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

La animación se crea con C# / VB código de servidor ([haga clic aquí para ver imagen en tamaño completo](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](triggering-an-animation-in-another-control-vb.md)
> [Siguiente](executing-animations-using-client-side-code-vb.md)
