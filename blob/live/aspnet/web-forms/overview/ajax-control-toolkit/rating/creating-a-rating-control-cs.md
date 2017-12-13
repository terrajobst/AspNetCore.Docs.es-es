---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: "Crear un Control de clasificación (C#) | Documentos de Microsoft"
author: wenz
description: "Muchos sitios Web, de comercio electrónico para sitios de Comunidades, ofrecen a los usuarios de artículos de velocidad o elementos. Esto normalmente requiere algunos esfuerzo de codificación, pero tenemos el..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c004522ac72b848e42320862d77bced0c11ca15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="3a13e-104">Crear un Control de clasificación (C#)</span><span class="sxs-lookup"><span data-stu-id="3a13e-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="3a13e-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3a13e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3a13e-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3a13e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="3a13e-107">Muchos sitios Web, de comercio electrónico para sitios de Comunidades, ofrecen a los usuarios de artículos de velocidad o elementos.</span><span class="sxs-lookup"><span data-stu-id="3a13e-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="3a13e-108">Esto normalmente requiere algunos esfuerzo de codificación, pero tenemos el Kit de herramientas de Control para la eliminación.</span><span class="sxs-lookup"><span data-stu-id="3a13e-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="3a13e-109">Información general</span><span class="sxs-lookup"><span data-stu-id="3a13e-109">Overview</span></span>

<span data-ttu-id="3a13e-110">Muchos sitios Web, de comercio electrónico para sitios de Comunidades, ofrecen a los usuarios de artículos de velocidad o elementos.</span><span class="sxs-lookup"><span data-stu-id="3a13e-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="3a13e-111">Esto normalmente requiere algunos esfuerzo de codificación, pero tenemos el Kit de herramientas de Control para la eliminación.</span><span class="sxs-lookup"><span data-stu-id="3a13e-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="3a13e-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="3a13e-112">Steps</span></span>

<span data-ttu-id="3a13e-113">En primer lugar, necesita (por lo menos) dos tipos de imágenes: uno para una forma rellena el elemento clasificación y uno para un elemento vacío clasificación.</span><span class="sxs-lookup"><span data-stu-id="3a13e-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="3a13e-114">Un elemento de clasificación es normalmente una estrella o una cara.</span><span class="sxs-lookup"><span data-stu-id="3a13e-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="3a13e-115">En este escenario, encontrará tres archivos, smiley.png y empty.png y done.png sonriente como parte de las descargas de código de origen para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3a13e-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="3a13e-116">A continuación, cree un nuevo archivo ASP.NET y comience por agregar una `ScriptManager` control a él:</span><span class="sxs-lookup"><span data-stu-id="3a13e-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="3a13e-117">A continuación, agregue el `Rating` control desde el Kit de herramientas de Control de AJAX de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3a13e-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="3a13e-118">Los siguientes atributos deben establecerse en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3a13e-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="3a13e-119">`CurrentRating`la clasificación inicial que se usará</span><span class="sxs-lookup"><span data-stu-id="3a13e-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="3a13e-120">`MaxRating`el valor máximo de clasificación</span><span class="sxs-lookup"><span data-stu-id="3a13e-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="3a13e-121">`EmptyStarCssClass`la clase CSS que se usará cuando un elemento de clasificación (estrella) está vacía</span><span class="sxs-lookup"><span data-stu-id="3a13e-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="3a13e-122">`FilledStarCssClass`la clase CSS que se usará cuando se rellena un elemento de clasificación (estrella)</span><span class="sxs-lookup"><span data-stu-id="3a13e-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="3a13e-123">`StarCssClass`la clase CSS que se usará para un estado visible</span><span class="sxs-lookup"><span data-stu-id="3a13e-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="3a13e-124">`WaitingStarCssClass`la clase CSS que use mientras una clasificación por estrellas se envía al servidor</span><span class="sxs-lookup"><span data-stu-id="3a13e-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="3a13e-125">Y aquí es el marcado que crea un control de clasificación con cinco elementos (emoticones) de los cuales ninguno se rellenan inicialmente:</span><span class="sxs-lookup"><span data-stu-id="3a13e-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="3a13e-126">Las tres clases CSS que se hace referencia ahora necesitan mostrar los archivos de imagen adecuada, que es fácil de hacer uso de CSS:</span><span class="sxs-lookup"><span data-stu-id="3a13e-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="3a13e-127">Asegúrese de que proporciona el ancho y alto de las tres imágenes, en caso contrario, la visualización puede parecer un poco punto seguridad.</span><span class="sxs-lookup"><span data-stu-id="3a13e-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="3a13e-128">Por último, el resultado de la clasificación debe mostrar al usuario (o, al menos se guardan en una base de datos).</span><span class="sxs-lookup"><span data-stu-id="3a13e-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="3a13e-129">Por lo tanto, agregue una etiqueta para la salida de un mensaje de texto y un botón de envío para enviar el formulario de clasificación en el servidor de nuevo:</span><span class="sxs-lookup"><span data-stu-id="3a13e-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="3a13e-130">En el código del lado servidor, tenga acceso al control de clasificación a través de su `ID` y, a continuación, obtener acceso a su `CurrentRating` propiedad que es el número de los elementos de clasificación seleccionado, en nuestro ejemplo, un valor entre 0 y 5.</span><span class="sxs-lookup"><span data-stu-id="3a13e-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="3a13e-131">Guarde la página y cargarlo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="3a13e-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="3a13e-132">Al mantener el mouse sobre los elementos de la clasificación (inicialmente vacía), se produce un efecto de JavaScript: los cambios de clasificación.</span><span class="sxs-lookup"><span data-stu-id="3a13e-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="3a13e-133">Al hacer clic en el conjunto de estrellas, se conserva la clasificación actual.</span><span class="sxs-lookup"><span data-stu-id="3a13e-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="3a13e-134">Por último, cuando se envía el formulario, el código del lado servidor genera el valor de clasificación seleccionado.</span><span class="sxs-lookup"><span data-stu-id="3a13e-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="3a13e-135">[![Creación de un sistema de clasificación con código mínimo](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3a13e-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="3a13e-136">Creación de un sistema de clasificación con código mínimo ([haga clic aquí para ver la imagen a tamaño completo](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3a13e-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="3a13e-137">Siguiente</span><span class="sxs-lookup"><span data-stu-id="3a13e-137">Next</span></span>](creating-a-rating-control-vb.md)
