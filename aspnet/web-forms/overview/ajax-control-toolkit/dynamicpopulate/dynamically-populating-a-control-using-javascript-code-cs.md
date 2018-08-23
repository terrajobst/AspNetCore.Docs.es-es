---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Rellenar dinámicamente un Control utilizando el código de JavaScript (C#) | Microsoft Docs
author: wenz
description: El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino de t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e8b49f41f132cc31ca458ce0af3b74dbb54f225e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837272"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Rellenar dinámicamente un Control utilizando el código de JavaScript (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página. También es posible desencadenar la población mediante código personalizado de JavaScript del lado cliente.


## <a name="overview"></a>Información general

El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página. También es posible desencadenar la población mediante código personalizado de JavaScript del lado cliente.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web de ASP.NET que implementa el método al que llamará el `DynamicPopulateExtender` control. El servicio web implementa el método `getDate()` que espera un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web. Este es el código (archivo `DynamicPopulate.cs.asmx`) que recupera la fecha actual en uno de los tres formatos:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

En el paso siguiente, cree un nuevo sitio ASP.NET y comenzar con el control ScriptManager de ASP.NET AJAX:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

A continuación, agregue un control de etiqueta (por ejemplo mediante el control HTML del mismo nombre, o el `<asp:Label />` control web) que posteriormente mostrará el resultado de llamar al servicio web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

A continuación, incluir un `DynamicPopulateExtender` controlan y proporcionan información del servicio web, control de destino, pero no el nombre del control que desencadena el rellenado se realizará más adelante con JavaScript personalizado.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Ahora a la parte de JavaScript. El `$find()` función, definido por la biblioteca AJAX de ASP.NET, devuelve una referencia a objetos de servidor de ASP.NET AJAX Control Toolkit como `DynamicPopulateExtender`. En el archivo actual, `$find("dpe")` devuelve una referencia a la `DynamicPopulateExtender` control en la página. Expone un método llamado `populate()` que desencadena el proceso de rellenado dinámico. El `populate()` método requiere un argumento: la clave de contexto que servirá como argumento para el `getDate()` método web. Por ejemplo, `$find("dpe").populate("format1")` rellenaría la etiqueta con la fecha actual en formato de mes-año.

Con el fin de que el ejemplo un poco más flexible, el usuario puede elegir entre varios formatos de fecha. Para cada uno de ellos, se muestra un botón de radio. Una vez que el usuario hace clic en un botón de radio, código JavaScript rellena dinámicamente la etiqueta con el formato de fecha seleccionado. Estos son los botones de radio:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Tenga en cuenta que dentro del contexto de un botón de radio, la expresión de JavaScript `this.value` hace referencia al valor del botón actual, que es exactamente la misma información de la `getDate()` método puede trabajar con.


[![Hacer clic en el botón recupera la fecha del servidor, en el formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Hacer clic en el botón recupera la fecha del servidor, en el formato especificado ([haga clic aquí para ver imagen en tamaño completo](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-cs.md)
> [Siguiente](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
