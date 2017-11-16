---
title: "Agregar una búsqueda"
author: rick-anderson
description: "Se muestra cómo agregar la búsqueda a una aplicación sencilla de ASP.NET Core MVC"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="1b7c8-104">Puede cambiar fácilmente el nombre del parámetro `searchString` por `id` con el comando **rename**.</span><span class="sxs-lookup"><span data-stu-id="1b7c8-104">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="1b7c8-105">Haga clic con el botón derecho en `searchString` **> Cambiar nombre**.</span><span class="sxs-lookup"><span data-stu-id="1b7c8-105">Right click on `searchString` **> Rename**.</span></span>

![Menú contextual](search/_static/rename.png)

<span data-ttu-id="1b7c8-107">Se resaltarán los objetivos del cambio de nombre.</span><span class="sxs-lookup"><span data-stu-id="1b7c8-107">The rename targets are highlighted.</span></span>

![Editor de código que muestra la variable resaltada con el método Index ActionResult](search/_static/rename2.png)

<span data-ttu-id="1b7c8-109">Cambie el parámetro por `id` y todas las apariciones de `searchString` se modificarán por `id`.</span><span class="sxs-lookup"><span data-stu-id="1b7c8-109">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor de código que muestra que la variable se ha modificado por id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="1b7c8-111">Observe cómo IntelliSense le ayuda a actualizar el marcado.</span><span class="sxs-lookup"><span data-stu-id="1b7c8-111">Notice how intelliSense helps us update the markup.</span></span>

![Menú contextual de IntelliSense con el atributo method seleccionado en la lista de atributos del elemento de formulario](search/_static/int_m.png)

![Menú contextual de IntelliSense con el método get seleccionado en la lista de valores de atributos de método](search/_static/int_get.png)

<span data-ttu-id="1b7c8-114">Observe que la fuente es diferente en la etiqueta `<form>`.</span><span class="sxs-lookup"><span data-stu-id="1b7c8-114">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="1b7c8-115">Esta fuente distinta indica que la etiqueta es compatible con las [aplicaciones auxiliares de etiquetas](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="1b7c8-115">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![etiqueta form con el texto de color morado](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="1b7c8-117">[Anterior](controller-methods-views.md)
[Siguiente](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="1b7c8-117">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
