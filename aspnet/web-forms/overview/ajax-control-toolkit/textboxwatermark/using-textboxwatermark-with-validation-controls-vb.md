---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Usar TextBoxWatermark con los controles de validación (VB) | Microsoft Docs
author: wenz
description: El control TextBoxWatermark de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, lo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 25a18bf4ad514fbeadd321f50d3b715b38d656cd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376583"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="3119f-104">Usar TextBoxWatermark con los controles de validación (VB)</span><span class="sxs-lookup"><span data-stu-id="3119f-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="3119f-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3119f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3119f-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3119f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="3119f-107">El control TextBoxWatermark de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro.</span><span class="sxs-lookup"><span data-stu-id="3119f-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="3119f-108">Cuando un usuario hace clic en el cuadro, se vacía.</span><span class="sxs-lookup"><span data-stu-id="3119f-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="3119f-109">Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente.</span><span class="sxs-lookup"><span data-stu-id="3119f-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="3119f-110">Esto podrían entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero se pueden solucionar estos problemas.</span><span class="sxs-lookup"><span data-stu-id="3119f-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="3119f-111">Información general</span><span class="sxs-lookup"><span data-stu-id="3119f-111">Overview</span></span>

<span data-ttu-id="3119f-112">El `TextBoxWatermark` control de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro.</span><span class="sxs-lookup"><span data-stu-id="3119f-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="3119f-113">Cuando un usuario hace clic en el cuadro, se vacía.</span><span class="sxs-lookup"><span data-stu-id="3119f-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="3119f-114">Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente.</span><span class="sxs-lookup"><span data-stu-id="3119f-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="3119f-115">Esto podrían entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero se pueden solucionar estos problemas.</span><span class="sxs-lookup"><span data-stu-id="3119f-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="3119f-116">Pasos</span><span class="sxs-lookup"><span data-stu-id="3119f-116">Steps</span></span>

<span data-ttu-id="3119f-117">La configuración básica del ejemplo es el siguiente: un `TextBox` control se marca de agua utilizando una `TextBoxWatermarkExtender` control.</span><span class="sxs-lookup"><span data-stu-id="3119f-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="3119f-118">Un botón desencadena una devolución de datos y se usará después para desencadenar los controles de validación en la página.</span><span class="sxs-lookup"><span data-stu-id="3119f-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="3119f-119">Además, un `ScriptManager` control es necesario inicializar AJAX de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3119f-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="3119f-120">Ahora, agregue un `RequiredFieldValidator` control que comprueba si hay texto en el campo cuando se envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="3119f-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="3119f-121">El `InitialValue` propiedad del validador debe establecerse en el mismo valor que se usa en el `TextBoxWatermarkExtender` control: cuando se envía el formulario, el valor de un cuadro de texto sin cambios es el valor de marca de agua dentro de él:</span><span class="sxs-lookup"><span data-stu-id="3119f-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="3119f-122">Sin embargo, es un problema con este enfoque: si el cliente deshabilita JavaScript, el campo de texto no es rellenan previamente con el texto de marca de agua, por lo tanto, el `RequiredFieldValidator` desencadenar un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="3119f-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="3119f-123">Por lo tanto, un segundo `RequiredFieldValidator` se requiere un control que comprueba si hay un cuadro de texto vacío (omitiendo el `InitialValue` atributo).</span><span class="sxs-lookup"><span data-stu-id="3119f-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="3119f-124">Dado que ambos validadores usan `Display` = `"Dynamic"`, el usuario final no puede distinguir entre la apariencia visual que de los dos controles de validación que se activó; en su lugar, parece que hubo solo uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="3119f-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="3119f-125">Por último, agregue el código del lado servidor para el texto en el campo de salida si ningún validador emite un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="3119f-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="3119f-126">[![El validador se queja de que no hay ningún texto en el campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3119f-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="3119f-127">El validador se queja de que no hay ningún texto en el campo ([haga clic aquí para ver imagen en tamaño completo](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3119f-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3119f-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="3119f-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
