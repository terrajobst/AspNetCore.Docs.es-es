---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Usar DynamicPopulate con un Control de usuario y JavaScript (VB) | Documentos de Microsoft
author: wenz
description: El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873006"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Usar DynamicPopulate con un Control de usuario y JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página. También es posible desencadenar la población utilizando el código de JavaScript de cliente personalizada. Sin embargo, un cuidado especial tiene que tener cuidado cuando el dispositivo extender reside en un control de usuario.


## <a name="overview"></a>Información general

El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página. También es posible desencadenar la población utilizando el código de JavaScript de cliente personalizada. Sin embargo, un cuidado especial tiene que tener cuidado cuando el dispositivo extender reside en un control de usuario.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web de ASP.NET que implementa el método que invocará el `DynamicPopulateExtender` control. El servicio web implementa el método `getDate()` que espera un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web. Este es el código (archivos `DynamicPopulate.vb.asmx`) que recupera la fecha actual en uno de estos tres formatos:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

En el paso siguiente, creará un nuevo control de usuario (`.ascx` archivo), como indica el icono por la declaración siguiente en la primera línea:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

A &lt; `label` &gt; elemento se usará para mostrar los datos procedentes del servidor.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

También en el archivo de control de usuario, se usará tres botones de radio, cada uno de ellos que representa uno de los tres formatos de fecha posible admitidos por el servicio web. Cuando el usuario hace clic en uno de los botones de radio, el explorador ejecutará el código de JavaScript que el siguiente aspecto:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Este código tiene acceso a la `DynamicPopulateExtender` (no se preocupe el identificador extraño todavía, esto se explicará más adelante) y desencadena el rellenado con datos dinámico. En el contexto del botón de radio actual, `this.value` hace referencia a su valor que es `format1`, `format2` o `format3` lo que espera el método web.

Lo único que falta en el control de usuario todavía es el `DynamicPopulateExtender` control lo que vincula los botones de radio con el servicio web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Nuevo se puede tener en cuenta el identificador extraño utilizado en el control: `mcd1$myDate` en lugar de `myDate`. Anteriormente, el código de JavaScript usa `mcd1_dpe1` para tener acceso a la `DynamicPopulateExtender` en lugar de `dpe1`. Esta estrategia de nomenclatura es un requisito especial cuando se usa `DynamicPopulateExtender` dentro de un control de usuario. Además, tendrá que incrustar el control de usuario de una manera específica para que todo funcione. Crear una nueva página ASP.NET y registrar un prefijo de etiqueta para el control de usuario que acaba de implementar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

A continuación, incluya el AJAX de ASP.NET `ScriptManager` control en la página nueva:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Por último, agregue el control de usuario a la página. Solo tiene que establecer su `ID` atributo (y `runat="server"`, evidentemente), pero también tiene que establecer para un nombre específico: `mcd1` puesto que este es el prefijo usado en el control de usuario para acceder a él mediante JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

Y listo. La página se comporta según lo esperado: un usuario hace clic en uno de los botones de opción, el control en el Kit de herramientas llama al servicio web y muestra la fecha actual en el formato deseado.


[![Los botones de radio residen en un control de usuario](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Los botones de radio residen en un control de usuario ([haga clic aquí para ver la imagen a tamaño completo](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-using-javascript-code-vb.md)
