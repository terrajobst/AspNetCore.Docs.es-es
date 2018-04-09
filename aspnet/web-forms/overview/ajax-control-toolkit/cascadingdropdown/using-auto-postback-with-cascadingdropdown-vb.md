---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Con devolución de datos automática CascadingDropDown (VB) | Documentos de Microsoft
author: wenz
description: El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores de anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>Uso de devolución de datos automática con CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de NET DropDownList no funciona, ya que la carga de forma asincrónica datos en la lista genera una respuesta (innecesario) por sí mismo. Con algún código de JavaScript, se puede evitar este efecto.


## <a name="overview"></a>Información general

El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de NET DropDownList no funciona, ya que la carga de forma asincrónica datos en la lista genera una respuesta (innecesario) por sí mismo. Con algún código de JavaScript, se puede evitar este efecto.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el &lt; `form` &gt; elemento):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

A continuación, se requiere un control de DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Para esta lista, se agrega un extensor CascadingDropDown, proporcionar información de dirección URL y el método de servicio web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la siguiente firma de método:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

El método devuelve una matriz de tipo de valor CascadingDropDown. El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Cargar la página en el explorador, se rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionada. Además, ASP.NET define la `__doPostBack()` método JavaScript. Una vez que se ha cargado la página, esta llamada de JavaScript se agrega a la lista desplegable, pero solo si hay elementos en ella. Si no hay ningún elemento en la lista, el Kit de herramientas de Control está cargando actualmente, por lo que el código de JavaScript usa un tiempo de espera y trata de nuevo en un medio segundo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

De esta manera, una devolución de datos solo se ejecuta cuando hay elementos realmente en la lista y el usuario selecciona una entrada.


[![Al seleccionar un elemento de lista, una devolución de datos](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

Al seleccionar un elemento de lista, una devolución de datos ([haga clic aquí para ver la imagen a tamaño completo](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](presetting-list-entries-with-cascadingdropdown-vb.md)
