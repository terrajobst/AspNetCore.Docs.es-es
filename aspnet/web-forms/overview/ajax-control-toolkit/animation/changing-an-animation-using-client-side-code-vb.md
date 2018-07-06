---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Cambiar una animación con código del lado cliente (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. La animación puede también...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: afb275e4e5155ceccb7e696cbf9d00de99a455f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840737"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Cambiar una animación con código del lado cliente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También se puede cambiar la animación mediante código personalizado de JavaScript del lado cliente.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También se puede cambiar la animación mediante código personalizado de JavaScript del lado cliente.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

La animación real se inicia mediante un botón HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Tenga en cuenta que no hay ningún `<Animations>` nodo dentro de la `AnimationExtender` control. Código JavaScript personalizado se utiliza para proporcionar las animaciones que se usará con el control.

Igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil para asignar una animación para el extensor todavía. Sin embargo, el extensor exponer varios métodos para leer y escribir las animaciones registrado con los distintos eventos (`OnClick`, `OnLoad`, y así sucesivamente). A continuación se muestran algunos ejemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

El formato del valor devuelto de la `get_*()` funciones y el formato del argumento para el `set_*()` functions es una cadena JSON, que proporciona una representación de objeto de cuál sería el marcado XML. Actualmente, no hay ninguna manera de pasar un objeto, pero es posible leer un objeto de una animación (`get_OnXXXBehavior()` métodos).

Esta es una cadena JSON (sin las comillas delimitadoras y presentarla convenientemente) que representa una animación que se desencadene el botón, pero el panel de la animación, cambiar su tamaño y atenúa al mismo tiempo:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

El siguiente código JavaScript asigna este descripting JSON para el `OnClick` animación del extensor actual y lo ejecuta:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco de marcado)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco de marcado) ([haga clic aquí para ver imagen en tamaño completo](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-vb.md)
> [Siguiente](animating-an-updatepanel-control-vb.md)
