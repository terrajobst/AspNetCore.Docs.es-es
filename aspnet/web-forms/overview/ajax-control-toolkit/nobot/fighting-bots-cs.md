---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Combatir los Bots (C#) | Microsoft Docs
author: wenz
description: Inicios automáticos pegar los weblogs y otros sitios Web con el correo no deseado, envío de formularios de comentario sin ninguna interacción del usuario. El control NoBot en la desventaja de AJAX de ASP.NET...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 52ed34e7640cd125a3b4c3b50ab760a7c1d713f1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837287"
---
<a name="fighting-bots-c"></a>Combatir los Bots (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Inicios automáticos pegar los weblogs y otros sitios Web con el correo no deseado, envío de formularios de comentario sin ninguna interacción del usuario. El control NoBot de ASP.NET AJAX Control Toolkit puede ayudar a combatir los bots.


## <a name="overview"></a>Información general

Inicios automáticos pegar los weblogs y otros sitios Web con el correo no deseado, envío de formularios de comentario sin ninguna interacción del usuario. El control NoBot de ASP.NET AJAX Control Toolkit puede ayudar a combatir los bots.

## <a name="steps"></a>Pasos

Un método común para combatir los bots es usar CAPTCHAs completamente automatizada pública test de Turing para indicar a los equipos y los seres humanos separados. Un test de Turing era originalmente una prueba donde alguien necesarios para decidir si un asociado de comunicación es una persona o una máquina. En la web, un CAPTCHA normalmente consta de una imagen con algunas letras distorsionadas en él. La idea es que sólo una persona puede leer las letras de la imagen, mientras que los algoritmos de reconocimiento óptico de caracteres se producirá un error.

Hay varias ventajas y desventajas con este enfoque, pero una explicación de este queda fuera del ámbito de este tutorial. Sin embargo hay un control de ASP.NET AJAX Control Toolkit que proporciona un enfoque similar: `NoBot`. Es más fácil solución que un CAPTCHA, pero es muy fácil de usar y tarifas muy bien en los sitios Web, como blogs, donde se considera correcta si la mayoría de los intentos de correo no deseado se rechaza, lo que el `NoBot` control puede hacer.

`NoBot` intercepta la devolución de datos del formulario web ASP.NET actual si se cumple al menos una de estas condiciones:

- Se produce un error en el explorador resolver un rompecabezas de JavaScript (por ejemplo cuando JavaScript está desactivado)
- El usuario enviado el formulario a rápida
- La dirección IP de cliente había enviado el formulario con demasiada frecuencia en un período de tiempo determinado.

Para comprobar estas condiciones, el `NoBot` control requiere estos atributos (todos ellos opcionales):

- `ResponseMinimumDelaySeconds` cantidad mínima de segundos entre las devoluciones de datos
- `CutoffWindowSeconds` longitud del intervalo de tiempo en el que las devoluciones de datos desde una dirección IP son medidas
- `CutoffMaximumInstances` cantidad máxima de segundos por intervalo de tiempo

Las demandas de marcado siguiente que al menos dos segundos deben transcurrir entre las devoluciones de datos y que hay devoluciones de datos solo cinco o menos dentro de un intervalo de 30 segundos:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

A continuación, como de costumbre no olvide incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Porque la mayoría de las comprobaciones de `NoBot` está haciendo se producen en el servidor, deberá comprobar el resultado de estas validaciones. Esto puede hacerse mediante una llamada a `NoBot`del `IsValid()` método. Tiene un argumento (como un `out` parámetro /`ByRef` parámetro) que es de tipo `NoBotState`. Representación de cadena contiene el motivo, cuando se produce un error en la comprobación y `Valid` en caso contrario. El código siguiente genera un mensaje conforme a `NoBot`del resultado:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Por último, necesita un formulario para enviar y un elemento de etiqueta para el mensaje de salida, y ha terminado!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Al ejecutar esta secuencia de comandos y desactivar JavaScript o enviar el formulario dentro de los primeros dos segundos o enviar el formulario siete veces dentro de treinta segundos, obtendrá un mensaje de error. Sin embargo con inteligencia a usar este control, puesto que sólo unos 90 95% de los usuarios tienen activados en JavaScript, por lo tanto, 5-10% de los usuarios se producirá un error `NoBot`de la prueba.


[![Este mensaje de error podría deberse a un bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Este mensaje de error podría deberse a un bot ([haga clic aquí para ver imagen en tamaño completo](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](fighting-bots-vb.md)
