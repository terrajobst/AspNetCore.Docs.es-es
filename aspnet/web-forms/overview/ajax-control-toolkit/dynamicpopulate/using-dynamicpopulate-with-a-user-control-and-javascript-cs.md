---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Usar DynamicPopulate con un Control de usuario y JavaScript (C#) | Microsoft Docs
author: wenz
description: El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino de t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: cba1647e69cbaebf0accb745278590e357d8c2c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370424"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Usar DynamicPopulate con un Control de usuario y JavaScript (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página. También es posible desencadenar la población mediante código personalizado de JavaScript del lado cliente. Sin embargo un cuidado especial tiene que ser tomada cuando el dispositivo extender reside en un control de usuario.


## <a name="overview"></a>Información general

El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página. También es posible desencadenar la población mediante código personalizado de JavaScript del lado cliente. Sin embargo un cuidado especial tiene que ser tomada cuando el dispositivo extender reside en un control de usuario.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web de ASP.NET que implementa el método al que llamará el `DynamicPopulateExtender` control. El servicio web implementa el método `getDate()` que espera un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web. Este es el código (archivo `DynamicPopulate.cs.asmx`) que recupera la fecha actual en uno de los tres formatos:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

En el paso siguiente, creará un nuevo control de usuario (`.ascx` archivo), conocido por la declaración siguiente en la primera línea:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Un &lt; `label` &gt; elemento que se usará para mostrar los datos procedentes del servidor.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

También en el archivo de control de usuario, usaremos tres botones de radio, cada uno que representa uno de los tres formatos de fecha admitidos por el servicio web. Cuando el usuario hace clic en uno de los botones de radio, el explorador ejecutará el código de JavaScript que tiene este aspecto:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Este código tiene acceso a la `DynamicPopulateExtender` (no se preocupe el identificador extraño todavía, esto se explicará más adelante) y desencadena el rellenado con datos dinámico. En el contexto del botón de radio actual, `this.value` hace referencia a su valor que es `format1`, `format2` o `format3` exactamente lo que espera el método web.

Lo único que no se encuentra en el control de usuario todavía es el `DynamicPopulateExtender` control lo que vincula los botones de radio con el servicio web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Nuevo es posible que tenga en cuenta el identificador extraño utilizado en el control: `mcd1$myDate` en lugar de `myDate`. Anteriormente, el código de JavaScript usado `mcd1_dpe1` para tener acceso a la `DynamicPopulateExtender` en lugar de `dpe1`. Esta estrategia de nomenclatura es un requisito especial cuando se usa `DynamicPopulateExtender` dentro de un control de usuario. Además, tiene que insertar el control de usuario de forma específica para que todo funcione. Crear una nueva página ASP.NET y registrar un prefijo de etiqueta del control de usuario que acaba de implementar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

A continuación, se incluyen ASP.NET AJAX `ScriptManager` control en la página nueva:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Por último, agregue el control de usuario a la página. Solo tiene que establecer su `ID` atributo (y `runat="server"`, por supuesto), pero también debe establecer en un nombre específico: `mcd1` puesto que es el prefijo usado en el control de usuario para acceder a él mediante JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Y listo. La página se comporta según lo esperado: un usuario hace clic en uno de los botones de radio, el control en el Kit de herramientas, llama al servicio web y muestra la fecha actual en el formato deseado.


[![Los botones de radio residen en un control de usuario](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Los botones de radio residen en un control de usuario ([haga clic aquí para ver imagen en tamaño completo](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Siguiente](dynamically-populating-a-control-vb.md)
