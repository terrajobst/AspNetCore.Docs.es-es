---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Creación de casillas de verificación mutuamente excluyentes (C#) | Documentos de Microsoft
author: wenz
description: 'Cuando se puede seleccionar sólo uno de un conjunto de opciones, normalmente se utilizan los botones de radio. Hay una desventaja, sin embargo: una vez que se selecciona un botón de radio en un grupo,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="5c3cd-104">Creación de casillas de verificación mutuamente excluyentes (C#)</span><span class="sxs-lookup"><span data-stu-id="5c3cd-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="5c3cd-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5c3cd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5c3cd-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5c3cd-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="5c3cd-107">Cuando se puede seleccionar sólo uno de un conjunto de opciones, normalmente se utilizan los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="5c3cd-108">Hay una desventaja, sin embargo: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="5c3cd-109">Casillas de verificación se puede deshabilitar en cualquier momento, pero no se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="5c3cd-110">Este tutorial ofrece lo mejor de ambos enfoques: casillas de verificación que se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="5c3cd-111">Información general</span><span class="sxs-lookup"><span data-stu-id="5c3cd-111">Overview</span></span>

<span data-ttu-id="5c3cd-112">Cuando se puede seleccionar sólo uno de un conjunto de opciones, normalmente se utilizan los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="5c3cd-113">Hay una desventaja, sin embargo: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="5c3cd-114">Casillas de verificación se puede deshabilitar en cualquier momento, pero no se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="5c3cd-115">Este tutorial ofrece lo mejor de ambos enfoques: casillas de verificación que se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="5c3cd-116">Pasos</span><span class="sxs-lookup"><span data-stu-id="5c3cd-116">Steps</span></span>

<span data-ttu-id="5c3cd-117">El Kit de herramientas de Control de AJAX de ASP.NET contiene el extensor MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="5c3cd-118">Esto permite a los programadores asignar cualquier casilla a un nombre de grupo (`Key` atributo).</span><span class="sxs-lookup"><span data-stu-id="5c3cd-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="5c3cd-119">En todas las casillas de verificación dentro del mismo grupo, sólo uno puede seleccionarse al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="5c3cd-120">Comencemos con dos casillas de verificación a colocar en una nueva página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="5c3cd-121">Puede haber más, pero dos de ellos son suficientes para demostrar la entidad de seguridad:</span><span class="sxs-lookup"><span data-stu-id="5c3cd-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="5c3cd-122">Para ambas casillas de verificación, un control de MutuallyExclusiveCheckBoxExtender debe situarse en la página.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="5c3cd-123">Ambos atributos de clave deben tener el mismo valor, el valor de los atributos de elementos de botón de radio HTML deben ser idénticos para denotar el grupo al que pertenecen.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="5c3cd-124">La propiedad TargetControlID del extensor de apunta al identificador de la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="5c3cd-125">Por último, incluya el AJAX de ASP.NET `ScriptManager` necesario para todos los elementos del Kit de herramientas del Control de AJAX de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5c3cd-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="5c3cd-126">Guarde y ejecute la página: puede comprobar y desactive las casillas de verificación, sin embargo en ningún momento ambas casillas de verificación se puede comprobar.</span><span class="sxs-lookup"><span data-stu-id="5c3cd-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="5c3cd-127">[![Se puede comprobar solo una casilla a la vez](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5c3cd-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="5c3cd-128">Se puede comprobar solo una casilla a la vez ([haga clic aquí para ver la imagen a tamaño completo](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5c3cd-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5c3cd-129">Siguiente</span><span class="sxs-lookup"><span data-stu-id="5c3cd-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
