---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Probar la seguridad de una contraseña (C#) | Documentos de Microsoft
author: wenz
description: Casi siempre, las contraseñas son obligatorias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El control PasswordStrength en ASP. N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f4a7128f2edbef4fbe95faf9de19bdae5f436e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869223"
---
<a name="testing-the-strength-of-a-password-c"></a>Probar la seguridad de una contraseña (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Casi siempre, las contraseñas son obligatorias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El control de PasswordStrength en el Kit de herramientas de Control de AJAX de ASP.NET puede comprobar la calidad es de una contraseña.


## <a name="overview"></a>Información general

Casi siempre, las contraseñas son obligatorias para que los usuarios diferidos tienden a elegir contraseñas sencillas que son fáciles de descifrar. El `PasswordStrength` control en el Kit de herramientas de Control de AJAX de ASP.NET puede comprobar la calidad es de una contraseña.

## <a name="steps"></a>Pasos

El `PasswordStrength` control extiende un cuadro de texto y comprueba si la contraseña es suficientemente buena. Ofrece una gran variedad de opciones a través de atributos; Estas son solo algunas de ellas:

- `MinimumNumericCharacters` número mínimo de caracteres numéricos que requiere la contraseña
- `MinimumSymbolCharacters` número mínimo de caracteres de símbolos (no letras y dígitos) requerido la contraseña
- `PreferredPasswordLength` longitud mínima de la contraseña
- `RequiresUpperAndLowerCaseCharacters` Si la contraseña se debe usar caracteres en mayúsculas y minúsculas

El `StrengthIndicatorType` proporciona la información sobre cómo presentar la seguridad de la contraseña, como texto (valor `"Text"`) o como un tipo de barra de progreso (valor `"BarIndicator"`). En el `DisplayPosition` atributo, configurar dónde aparece la información. Este es un ejemplo completo, incluido ASP.NET AJAX `ScriptManager` (control), el `PasswordStrength` control y, por supuesto, un cuadro de texto donde el usuario puede escribir una contraseña. En aras de la demostración, el campo de formulario de este último es un campo de texto normal y no es un campo de contraseña para que puedan ver durante el desarrollo de lo que está escribiendo.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Ejecute la página y escriba inmediata: solo después de escribir letras minúsculas, letras mayúsculas, dígitos y símbolos, la contraseña se considera como separable.


[![Ahora, la contraseña es buena (bastante)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Ahora, la contraseña es buena (bastante) ([haga clic aquí para ver la imagen a tamaño completo](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](testing-the-strength-of-a-password-vb.md)
