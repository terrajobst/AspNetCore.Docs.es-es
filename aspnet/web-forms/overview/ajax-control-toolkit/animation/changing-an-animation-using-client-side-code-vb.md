---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Cambiar una animación con código del lado cliente (VB) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También puede la animación...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879158"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Cambiar una animación con código del lado cliente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. La animación también se puede cambiar utilizando el código de JavaScript de cliente personalizada.


## <a name="overview"></a>Información general

El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. La animación también se puede cambiar utilizando el código de JavaScript de cliente personalizada.

## <a name="steps"></a>Pasos

En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que el siguiente aspecto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Un botón HTML inicia la animación real:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Tenga en cuenta que no hay ningún `<Animations>` nodo dentro de la `AnimationExtender` control. Código de JavaScript personalizado se utiliza para proporcionar las animaciones que se utilizará con el control.

Al igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil de asignar una animación para el dispositivo extender aún. Sin embargo, el dispositivo extender exponer varios métodos para leer y escribir las animaciones registrado con los distintos eventos (`OnClick`, `OnLoad`, y así sucesivamente). A continuación se muestran algunos ejemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

El formato del valor devuelto de la `get_*()` funciones y el formato del argumento para el `set_*()` funciones es una cadena JSON, que proporciona una representación de objeto de lo que sería el marcado XML. Actualmente, no hay ninguna manera de pasar un objeto, pero es posible leer un objeto de una animación (`get_OnXXXBehavior()` métodos).

Esta es una cadena JSON (sin las comillas delimitadoras y un formato bien) que representa una animación desencadenada por el botón, pero el panel de la animación, cambiando su tamaño y desaparecer gradualmente al mismo tiempo:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

El siguiente código de JavaScript asigna este descripting JSON para el `OnClick` animación del extensor actual y la ejecuta:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado) ([haga clic aquí para ver la imagen a tamaño completo](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-vb.md)
> [Siguiente](animating-an-updatepanel-control-vb.md)
