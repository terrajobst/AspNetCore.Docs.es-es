---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Luchar contra Bots (VB) | Documentos de Microsoft
author: wenz
description: Bots automatizados pegar blogs y otros sitios Web con spam, envío de formularios de comentario sin ninguna interacción del usuario. El control de NoBot en el timo de AJAX de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d5984ac7ff3422bab07a759c93fef3914a22f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874062"
---
<a name="fighting-bots-vb"></a>Luchar contra Bots (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Bots automatizados pegar blogs y otros sitios Web con spam, envío de formularios de comentario sin ninguna interacción del usuario. El control de NoBot en el Kit de herramientas de Control de AJAX de ASP.NET puede ayudar a combatir los robots.


## <a name="overview"></a>Información general

Bots automatizados pegar blogs y otros sitios Web con spam, envío de formularios de comentario sin ninguna interacción del usuario. El control de NoBot en el Kit de herramientas de Control de AJAX de ASP.NET puede ayudar a combatir los robots.

## <a name="steps"></a>Pasos

Un método común para anular bots es usar pruebas CAPTCHAs completamente automatizada pública Turing para indicar a los equipos y los seres humanos separadas. Una prueba Turing originalmente era una prueba de que alguien necesita para decidir si un asociado de comunicación es una persona o una máquina. En la web, un CAPTCHA suele estar compuesto de una imagen con algunas letras distorsionado en él. La idea es que solo una persona puede leer las letras de la imagen, mientras que los algoritmos de OCR se producirá un error.

Hay varias ventajas y desventajas de este enfoque, pero una descripción de este está fuera del ámbito de este tutorial. Sin embargo hay un control en el Kit de herramientas de Control de AJAX de ASP.NET que proporciona un enfoque similar: `NoBot`. Es más fácil de evitar que un CAPTCHA, pero es muy fácil de usar y se rechaza, tarifas muy bien en sitios Web como blogs donde se considera correcta si la mayoría de los intentos de correo no deseado que el `NoBot` control puede hacer.

`NoBot` intercepta la devolución de datos del formulario web ASP.NET actual si se cumple al menos una de estas condiciones:

- Se produce un error en el explorador resolver un rompecabezas de JavaScript (por ejemplo si JavaScript está desactivada)
- El usuario envía el formulario para rápido
- La dirección IP del cliente envía el formulario con demasiada frecuencia en un período de tiempo determinado.

Para comprobar estas condiciones, el `NoBot` control requiere estos atributos (todos ellos opcionales):

- `ResponseMinimumDelaySeconds` cantidad mínima de segundos entre las devoluciones de datos
- `CutoffWindowSeconds` longitud del intervalo de tiempo en el que las devoluciones de datos de una IP son medidas
- `CutoffMaximumInstances` cantidad máxima de segundos por intervalo de tiempo

Las demandas de marcado que al menos dos segundos siguientes transcurran entre las devoluciones de datos y que hay postbacks solo cinco o menos dentro de un intervalo de 30 segundos:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

A continuación, como de costumbre no olvide incluir el `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Como la mayoría de las comprobaciones `NoBot` realiza se producen en el servidor, deberá comprobar el resultado de estas validaciones. Esto puede hacerse mediante una llamada a `NoBot`del `IsValid()` método. Tiene un argumento (como un `out` parámetro /`ByRef` parámetro) que es de tipo `NoBotState`. La representación de cadena contiene la razón cuando se produce un error en la comprobación y `Valid` en caso contrario. El código siguiente genera un mensaje de acuerdo con `NoBot`del resultado:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Por último, necesita un formulario para enviar y un elemento de etiqueta para el mensaje de salida y haya terminado.

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Al ejecutar este script y desactivar JavaScript o enviar el formulario dentro de los dos primeros segundos o enviar el formulario siete veces dentro de treinta segundos, obtendrá un mensaje de error. Sin embargo usar este control con cuidado, ya que sólo 90-95% de los usuarios tener JavaScript activado, por lo tanto, entre 5 y 10% de los usuarios se producirá un error `NoBot`de la prueba.


[![Este mensaje de error puede deberse a un componente](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Este mensaje de error puede deberse a un bot ([haga clic aquí para ver la imagen a tamaño completo](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](fighting-bots-cs.md)
