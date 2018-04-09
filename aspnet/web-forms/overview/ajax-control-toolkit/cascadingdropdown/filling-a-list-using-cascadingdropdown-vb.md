---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Rellenar una lista con CascadingDropDown (VB) | Documentos de Microsoft
author: wenz
description: El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores de anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e488a30443970d5e2ce825abd96d8e4a027585d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>Rellenar una lista con CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) El primer desafío para resolver es realmente rellenar una lista desplegable con este control.


## <a name="overview"></a>Información general

El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) El primer desafío para resolver es realmente rellenar una lista desplegable con este control.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

A continuación, se requiere un control de DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Para esta lista, se agrega un extensor CascadingDropDown. Enviará una solicitud asincrónica a un servicio web que devolverá una lista de entradas que se mostrará en la lista. Para que funcione, deben establecerse los atributos CascadingDropDown siguientes:

- `ServicePath`: Dirección URL de un servicio web entrega las entradas de lista
- `ServiceMethod`: Método web entrega de las entradas de lista
- `TargetControlID`: Id. de la lista desplegable
- `Category`: Información de categoría que se envía al método web cuando se llama
- `PromptText`: Texto que aparece cuando asincrónicamente Cargando datos de la lista desde el servidor

Este es el marcado para la `CascadingDropDown` elemento. La única diferencia entre C# y VB es el nombre del servicio web asociado:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

El código de JavaScript procedente de la `CascadingDropDown` extender llama a un método de servicio web con la siguiente firma:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Por lo que el aspecto importante es que el método debe devolver una matriz de tipo `CascadingDropDownNameValue` (definido por el Kit de herramientas de Control de AJAX de ASP.NET). En el `CascadingDropDownNameValue` constructor, primero deben proporcionar el texto de la entrada de lista y, a continuación, su valor, al igual que `<option value="VALUE">NAME</option>` sería hacerlo en HTML. Le mostramos algunos datos de ejemplo:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Cargar la página en el explorador se activará la lista que se debe satisfacer con tres proveedores.


[![La lista se rellena automáticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

La lista se rellena automáticamente ([haga clic aquí para ver la imagen a tamaño completo](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
> [Siguiente](using-cascadingdropdown-with-a-database-vb.md)
