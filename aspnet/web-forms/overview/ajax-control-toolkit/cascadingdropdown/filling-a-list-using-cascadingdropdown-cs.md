---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Rellenar una lista con CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a4271b2697df8e24fca5f7ff30797b1e4e077a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385426"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Rellenar una lista con CascadingDropDown (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) El primer reto es rellenar realmente una lista desplegable usar este control.


## <a name="overview"></a>Información general

El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) El primer reto es rellenar realmente una lista desplegable usar este control.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

A continuación, se requiere un control de DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Para esta lista, se agrega un extensor CascadingDropDown. Le enviará una solicitud asincrónica a un servicio web que, a continuación, devolverá una lista de entradas que se mostrará en la lista. Para que funcione, los siguientes atributos CascadingDropDown deben establecerse:

- `ServicePath`: Dirección URL de un servicio web entrega las entradas de lista
- `ServiceMethod`: Método web entrega de las entradas de lista
- `TargetControlID`: Id. de la lista desplegable
- `Category`: Información de categoría que se envía al método web cuando se llama
- `PromptText`: Texto que aparece al cargar asincrónicamente los datos de la lista desde el servidor

Aquí está el marcado para el `CascadingDropDown` elemento. La única diferencia entre C# y VB es el nombre del servicio web asociado:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

El código de JavaScript procedente de la `CascadingDropDown` extensor llama a un método de servicio web con la firma siguiente:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Por lo que lo más importante es que el método debe devolver una matriz de tipo `CascadingDropDownNameValue` (definido por ASP.NET AJAX Control Toolkit). En el `CascadingDropDownNameValue` constructor, primero se deben proporcionar texto de la entrada de la lista y, a continuación, su valor, al igual que `<option value="VALUE">NAME</option>` lo haría en HTML. Estos son algunos datos de ejemplo:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Carga de la página en el explorador se desencadenará la lista que se va a rellenar con tres proveedores.


[![La lista se rellena automáticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

La lista se rellena automáticamente ([haga clic aquí para ver imagen en tamaño completo](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-cascadingdropdown-with-a-database-cs.md)
