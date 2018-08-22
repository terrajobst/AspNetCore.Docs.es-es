---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Cambiar una animación con código de cliente (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. La animación puede también...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 253377ef6019a672680c6e819349357627ef111b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838539"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Cambiar una animación con código de cliente (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También se puede cambiar la animación mediante código personalizado de JavaScript del lado cliente.


## <a name="overview"></a>Información general

El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También se puede cambiar la animación mediante código personalizado de JavaScript del lado cliente.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene este aspecto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

La animación real se inicia mediante un botón HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Tenga en cuenta que no hay ningún `<Animations>` nodo dentro de la `AnimationExtender` control. Código JavaScript personalizado se utiliza para proporcionar las animaciones que se usará con el control.

Igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil para asignar una animación para el extensor todavía. Sin embargo, el extensor exponer varios métodos para leer y escribir las animaciones registrado con los distintos eventos (`OnClick`, `OnLoad`, y así sucesivamente). A continuación se muestran algunos ejemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

El formato del valor devuelto de la `get_*()` funciones y el formato del argumento para el `set_*()` functions es una cadena JSON, que proporciona una representación de objeto de cuál sería el marcado XML. Actualmente, no hay ninguna manera de pasar un objeto, pero es posible leer un objeto de una animación (`get_OnXXXBehavior()` métodos).

Esta es una cadena JSON (sin las comillas delimitadoras y presentarla convenientemente) que representa una animación que se desencadene el botón, pero el panel de la animación, cambiar su tamaño y atenúa al mismo tiempo:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

El siguiente código JavaScript asigna este descripting JSON para el `OnClick` animación del extensor actual y lo ejecuta:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco de marcado)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco de marcado) ([haga clic aquí para ver imagen en tamaño completo](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-cs.md)
> [Siguiente](animating-an-updatepanel-control-cs.md)
