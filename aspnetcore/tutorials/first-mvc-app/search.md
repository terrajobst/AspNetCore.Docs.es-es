---
title: "Agregar una búsqueda"
author: rick-anderson
description: "Se muestra cómo agregar la búsqueda a una aplicación sencilla de ASP.NET Core MVC"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 3ab9086275ec4c3651383c4c845e40db55f67f4c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="40a6a-103">Puede cambiar fácilmente el nombre del parámetro `searchString` por `id` con el comando **rename**.</span><span class="sxs-lookup"><span data-stu-id="40a6a-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="40a6a-104">Haga clic con el botón derecho en `searchString` **> Cambiar nombre**.</span><span class="sxs-lookup"><span data-stu-id="40a6a-104">Right click on `searchString` **> Rename**.</span></span>

![Menú contextual](search/_static/rename.png)

<span data-ttu-id="40a6a-106">Se resaltarán los objetivos del cambio de nombre.</span><span class="sxs-lookup"><span data-stu-id="40a6a-106">The rename targets are highlighted.</span></span>

![Editor de código que muestra la variable resaltada con el método Index ActionResult](search/_static/rename2.png)

<span data-ttu-id="40a6a-108">Cambie el parámetro por `id` y todas las apariciones de `searchString` se modificarán por `id`.</span><span class="sxs-lookup"><span data-stu-id="40a6a-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor de código que muestra que la variable se ha modificado por id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="40a6a-110">Observe cómo IntelliSense le ayuda a actualizar el marcado.</span><span class="sxs-lookup"><span data-stu-id="40a6a-110">Notice how intelliSense helps us update the markup.</span></span>

![Menú contextual de IntelliSense con el atributo method seleccionado en la lista de atributos del elemento de formulario](search/_static/int_m.png)

![Menú contextual de IntelliSense con el método get seleccionado en la lista de valores de atributos de método](search/_static/int_get.png)

<span data-ttu-id="40a6a-113">Observe que la fuente es diferente en la etiqueta `<form>`.</span><span class="sxs-lookup"><span data-stu-id="40a6a-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="40a6a-114">Esta fuente distinta indica que la etiqueta es compatible con las [aplicaciones auxiliares de etiquetas](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="40a6a-114">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![etiqueta form con el texto de color morado](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="40a6a-116">[Anterior](controller-methods-views.md)
[Siguiente](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="40a6a-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
