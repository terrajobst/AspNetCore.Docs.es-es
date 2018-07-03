---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Preconfigurar entradas de lista con CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 56093ad098d69039aafa9b4e6ba4e4c2ad91e49b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362004"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Preconfigurar entradas de lista con CascadingDropDown (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. Es posible que un elemento de lista es el valor una vez que los datos se ha cargado dinámicamente con un poco de código.


## <a name="overview"></a>Información general

El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) Es posible que un elemento de lista es el valor una vez que los datos se ha cargado dinámicamente con un poco de código.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

A continuación, se requiere un control de DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Para esta lista, se agrega un extensor CascadingDropDown, que proporciona información de dirección URL y el método de servicio web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la firma del método siguiente:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

El método devuelve una matriz de tipo de valor CascadingDropDown. El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo). Si el tercer argumento se establece en true, la lista de elemento se selecciona automáticamente en el explorador.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Carga de la página en el explorador rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionado.


[![La lista se rellena y preseleccionada automáticamente](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

La lista se rellena y preseleccionada automáticamente ([haga clic aquí para ver imagen en tamaño completo](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-cascadingdropdown-with-a-database-cs.md)
> [Siguiente](using-auto-postback-with-cascadingdropdown-cs.md)
