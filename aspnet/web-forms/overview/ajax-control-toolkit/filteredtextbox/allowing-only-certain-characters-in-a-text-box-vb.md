---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Permitir solo determinados caracteres en un cuadro de texto (VB) | Microsoft Docs
author: wenz
description: Controles de validación ASP.NET pueden asegurarse de que solo determinados caracteres se permiten en la entrada del usuario. Esto todavía no impide que los usuarios escriban no válidos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: e44b69a4f7d46f1f1278f7de07a2e6c025f5c316
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386765"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Permitir solo determinados caracteres en un cuadro de texto (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Controles de validación ASP.NET pueden asegurarse de que solo determinados caracteres se permiten en la entrada del usuario. Sin embargo esto todavía no impide que los usuarios escribir caracteres no válidos e intenta enviar el formulario.


## <a name="overview"></a>Información general

Controles de validación ASP.NET pueden asegurarse de que solo determinados caracteres se permiten en la entrada del usuario. Sin embargo esto todavía no impide que los usuarios escribir caracteres no válidos e intenta enviar el formulario.

## <a name="steps"></a>Pasos

ASP.NET AJAX Control Toolkit contiene el `FilteredTextBox` control que amplía un cuadro de texto. Una vez activado, solo un determinado conjunto de caracteres puede especificarse en el campo.

Para que funcione, primero es necesario como de costumbre ASP.NET AJAX `ScriptManager` que carga las bibliotecas de JavaScript que también se usan en ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

A continuación, necesitamos un cuadro de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Por último, el `FilteredTextBoxExtender` control se encarga de restringir los caracteres que el usuario puede escribir. En primer lugar, establezca el `TargetControlID` atributo a la `ID` de la `TextBox` control. A continuación, elija uno de los disponibles `FilterType` valores:

- `Custom` de forma predeterminada; tendrá que proporcionar una lista de caracteres válidos
- `LowercaseLetters` sólo letras en minúscula
- `Numbers` sólo dígitos
- `UppercaseLetters` solo las letras mayúsculas

Si el `Custom FilterType` se usa, la `ValidChars` propiedad debe configurarse y proporcionar una lista de caracteres que se puede escribir. Por cierto: si intenta pegar texto en el cuadro de texto, se quitan todos los caracteres no válidos.

Aquí está el marcado para la `FilteredTextBoxExtender` control que solo permite dígitos (algo que también habría sido posible con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Ejecutar la página y vuelva a escribir una carta si JavaScript está habilitado, no funcionará; Sin embargo, dígitos aparecen en la página. Sin embargo, observe que la protección `FilteredTextBox` proporciona no es infalible: si JavaScript está habilitado, todos los datos pueden especificarse en el cuadro de texto, por lo que debe usar medios de validación adicional, es decir, ASP. Controles de validación de la red.


[![Pueden especificarse solo dígitos](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Pueden especificarse solo dígitos ([haga clic aquí para ver imagen en tamaño completo](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](allowing-only-certain-characters-in-a-text-box-cs.md)
