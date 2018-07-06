---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Usar el Postback automático con CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: bece4f78b46c44db988e04e0e987450d94c37aab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819385"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Usar el Postback automático con CascadingDropDown (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de la red DropDownList no funciona, ya que se carga asincrónicamente datos en la lista genera una respuesta (innecesario) por sí mismo. Con algún código JavaScript, se puede evitar este efecto.


## <a name="overview"></a>Información general

El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de la red DropDownList no funciona, ya que se carga asincrónicamente datos en la lista genera una respuesta (innecesario) por sí mismo. Con algún código JavaScript, se puede evitar este efecto.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del &lt; `form` &gt; elemento):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

A continuación, se requiere un control de DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Para esta lista, se agrega un extensor CascadingDropDown, que proporciona información de dirección URL y el método de servicio web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la firma del método siguiente:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

El método devuelve una matriz de tipo de valor CascadingDropDown. El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Carga de la página en el explorador rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionado. Además, ASP.NET define el `__doPostBack()` método JavaScript. Una vez que se ha cargado la página, esta llamada de JavaScript se agrega a la lista desplegable, pero solo si hay elementos en ella. Si no hay ningún elemento en la lista, el Kit de herramientas de Control está cargando actualmente, por lo que el código de JavaScript usa un tiempo de espera y se intenta de nuevo en un medio segundo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

De este modo, una devolución de datos solo se ejecuta cuando hay elementos realmente en la lista y el usuario selecciona una entrada.


[![Al seleccionar un elemento de lista, una devolución de datos](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Al seleccionar un elemento de lista, una devolución de datos ([haga clic aquí para ver imagen en tamaño completo](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Siguiente](filling-a-list-using-cascadingdropdown-vb.md)
