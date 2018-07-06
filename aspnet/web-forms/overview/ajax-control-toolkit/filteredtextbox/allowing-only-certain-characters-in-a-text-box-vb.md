---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Permitir solo determinados caracteres en un cuadro de texto (VB) | Microsoft Docs
author: wenz
description: Controles de validación ASP.NET pueden asegurarse de que solo determinados caracteres se permiten en la entrada del usuario. Esto todavía no impide que los usuarios escriban no válidos...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: e53d7282237c94b88c712e53bf607dcb94ccf1f2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830751"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="32249-104">Permitir solo determinados caracteres en un cuadro de texto (VB)</span><span class="sxs-lookup"><span data-stu-id="32249-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="32249-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="32249-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="32249-106">[Descargar código](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="32249-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="32249-107">Controles de validación ASP.NET pueden asegurarse de que solo determinados caracteres se permiten en la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="32249-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="32249-108">Sin embargo esto todavía no impide que los usuarios escribir caracteres no válidos e intenta enviar el formulario.</span><span class="sxs-lookup"><span data-stu-id="32249-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="32249-109">Información general</span><span class="sxs-lookup"><span data-stu-id="32249-109">Overview</span></span>

<span data-ttu-id="32249-110">Controles de validación ASP.NET pueden asegurarse de que solo determinados caracteres se permiten en la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="32249-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="32249-111">Sin embargo esto todavía no impide que los usuarios escribir caracteres no válidos e intenta enviar el formulario.</span><span class="sxs-lookup"><span data-stu-id="32249-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="32249-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="32249-112">Steps</span></span>

<span data-ttu-id="32249-113">ASP.NET AJAX Control Toolkit contiene el `FilteredTextBox` control que amplía un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="32249-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="32249-114">Una vez activado, solo un determinado conjunto de caracteres puede especificarse en el campo.</span><span class="sxs-lookup"><span data-stu-id="32249-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="32249-115">Para que funcione, primero es necesario como de costumbre ASP.NET AJAX `ScriptManager` que carga las bibliotecas de JavaScript que también se usan en ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="32249-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="32249-116">A continuación, necesitamos un cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="32249-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="32249-117">Por último, el `FilteredTextBoxExtender` control se encarga de restringir los caracteres que el usuario puede escribir.</span><span class="sxs-lookup"><span data-stu-id="32249-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="32249-118">En primer lugar, establezca el `TargetControlID` atributo a la `ID` de la `TextBox` control.</span><span class="sxs-lookup"><span data-stu-id="32249-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="32249-119">A continuación, elija uno de los disponibles `FilterType` valores:</span><span class="sxs-lookup"><span data-stu-id="32249-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="32249-120">`Custom` de forma predeterminada; tendrá que proporcionar una lista de caracteres válidos</span><span class="sxs-lookup"><span data-stu-id="32249-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="32249-121">`LowercaseLetters` sólo letras en minúscula</span><span class="sxs-lookup"><span data-stu-id="32249-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="32249-122">`Numbers` sólo dígitos</span><span class="sxs-lookup"><span data-stu-id="32249-122">`Numbers` digits only</span></span>
- <span data-ttu-id="32249-123">`UppercaseLetters` solo las letras mayúsculas</span><span class="sxs-lookup"><span data-stu-id="32249-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="32249-124">Si el `Custom FilterType` se usa, la `ValidChars` propiedad debe configurarse y proporcionar una lista de caracteres que se puede escribir.</span><span class="sxs-lookup"><span data-stu-id="32249-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="32249-125">Por cierto: si intenta pegar texto en el cuadro de texto, se quitan todos los caracteres no válidos.</span><span class="sxs-lookup"><span data-stu-id="32249-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="32249-126">Aquí está el marcado para la `FilteredTextBoxExtender` control que solo permite dígitos (algo que también habría sido posible con `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="32249-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="32249-127">Ejecutar la página y vuelva a escribir una carta si JavaScript está habilitado, no funcionará; Sin embargo, dígitos aparecen en la página.</span><span class="sxs-lookup"><span data-stu-id="32249-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="32249-128">Sin embargo, observe que la protección `FilteredTextBox` proporciona no es infalible: si JavaScript está habilitado, todos los datos pueden especificarse en el cuadro de texto, por lo que debe usar medios de validación adicional, es decir, ASP. Controles de validación de la red.</span><span class="sxs-lookup"><span data-stu-id="32249-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="32249-129">[![Pueden especificarse solo dígitos](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32249-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="32249-130">Pueden especificarse solo dígitos ([haga clic aquí para ver imagen en tamaño completo](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="32249-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="32249-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="32249-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
