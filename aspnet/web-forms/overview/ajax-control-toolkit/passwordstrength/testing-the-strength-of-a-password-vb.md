---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Probar la seguridad de una contraseña (VB) | Microsoft Docs
author: wenz
description: Prácticamente cualquier lugar, las contraseñas son necesarias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El control PasswordStrength en ASP. N...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 7073a06186ba3ff6c6a12d558445d75d9301589a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805906"
---
<a name="testing-the-strength-of-a-password-vb"></a>Probar la seguridad de una contraseña (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Prácticamente cualquier lugar, las contraseñas son necesarias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El control PasswordStrength de ASP.NET AJAX Control Toolkit puede comprobar la calidad es de una contraseña.


## <a name="overview"></a>Información general

Prácticamente cualquier lugar, las contraseñas son necesarias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El `PasswordStrength` control de ASP.NET AJAX Control Toolkit puede comprobar la calidad es de una contraseña.

## <a name="steps"></a>Pasos

El `PasswordStrength` control extiende un cuadro de texto y comprueba si la contraseña es suficientemente buena. Ofrece una gran variedad de opciones a través de los atributos. Estos son sólo algunas de ellas:

- `MinimumNumericCharacters` número mínimo de caracteres numéricos que requiere la contraseña
- `MinimumSymbolCharacters` número mínimo de caracteres de símbolos (no letras y dígitos) requerido la contraseña
- `PreferredPasswordLength` longitud mínima de la contraseña
- `RequiresUpperAndLowerCaseCharacters` Si la contraseña debe usar mayúsculas y minúsculas

El `StrengthIndicatorType` proporciona la información de presentación de la seguridad de la contraseña, como texto (valor `"Text"`) o como un tipo de barra de progreso (valor `"BarIndicator"`). En el `DisplayPosition` atributo, configurar dónde aparece la información. Este es un ejemplo completo, incluido ASP.NET AJAX `ScriptManager` (control), el `PasswordStrength` control y por supuesto un cuadro de texto donde el usuario puede escribir una contraseña. Modo de demostración, el campo de formulario de este último es un campo de texto normal y no un campo de contraseña para que se puedan ver lo que está escribiendo durante el desarrollo.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Ejecute la página y escriba distancia: solo después de escribir letras minúsculas, letras mayúsculas, dígitos y símbolos, la contraseña se considera como indivisible.


[![Ahora, la contraseña es válida (bastante)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

Ahora, la contraseña es válida (bastante) ([haga clic aquí para ver imagen en tamaño completo](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](testing-the-strength-of-a-password-cs.md)
