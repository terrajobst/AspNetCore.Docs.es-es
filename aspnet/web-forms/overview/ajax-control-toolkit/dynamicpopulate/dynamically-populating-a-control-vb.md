---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Rellenar dinámicamente un Control (VB) | Microsoft Docs
author: wenz
description: El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino de t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fd55b59f9375eb320711ffea8d971a8d86179c43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368665"
---
<a name="dynamically-populating-a-control-vb"></a>Rellenar dinámicamente un Control (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página.


## <a name="overview"></a>Información general

El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página. Este tutorial muestra cómo configurar esta opción.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web de ASP.NET que implementa el método al que llamará `DynamicPopulate`. La clase de servicio web requiere la `ScriptService` atributo que se define dentro de `Microsoft.Web.Script.Services`; en caso contrario, ASP.NET AJAX no se puede crear el proxy de JavaScript del lado cliente para el servicio web que a su vez requiere `DynamicPopulate`.

El método web debe esperar un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web. El siguiente servicio web devuelve la fecha actual en un formato representado por la `contextKey` argumento:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

El servicio web, a continuación, se guarda como `DynamicPopulate.vb.asmx`. Como alternativa, podría implementar el `getDate()` método como un método de página en la página ASP.NET real con el `DynamicPopulate` control.

En el paso siguiente, creará un nuevo archivo ASP.NET. Como siempre, el primer paso es incluir la `ScriptManager` en la página actual para cargar la biblioteca AJAX de ASP.NET y para realizar el trabajo del Kit de herramientas de Control:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

A continuación, agregue un control de etiqueta (por ejemplo mediante el control HTML del mismo nombre, o la &lt; `asp:Label`  / &gt; control web) que posteriormente mostrará el resultado de llamar al servicio web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Un botón HTML (como un control HTML, ya que no se requiere un postback al servidor), a continuación, se usará para desencadenar el rellenado dinámico:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Por último, necesitamos el `DynamicPopulateExtender` control a la unificación. Se establecerán los siguientes atributos (excepto los obvios, `ID` y `runat` = `"server"`):

- `TargetControlID` dónde colocar el resultado de llamar al servicio web
- `ServicePath` ruta de acceso al servicio web (omitir si desea usar un método de página)
- `ServiceMethod` nombre del método web o método de página
- `ContextKey` información de contexto para enviarse al servicio web
- `PopulateTriggerControlID` elemento que se activa la llamada de servicio web
- `ClearContentsDuringUpdate` Si desea vaciar el elemento de destino durante la llamada de servicio web

Como puede ver, el control requiere cierta información pero puesta en marcha de todo lo que es bastante sencillo. Aquí está el marcado para el `DynamicPopulateExtender` control en el escenario actual:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Ejecute la página ASP.NET en el explorador y haga clic en el botón; recibirá la fecha actual en formato de mes-año.


[![Hacer clic en el botón recupera la fecha del servidor](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Hacer clic en el botón recupera la fecha del servidor ([haga clic aquí para ver imagen en tamaño completo](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Siguiente](dynamically-populating-a-control-using-javascript-code-vb.md)
