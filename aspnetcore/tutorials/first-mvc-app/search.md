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

Puede cambiar fácilmente el nombre del parámetro `searchString` por `id` con el comando **rename**. Haga clic con el botón derecho en `searchString` **> Cambiar nombre**.

![Menú contextual](search/_static/rename.png)

Se resaltarán los objetivos del cambio de nombre.

![Editor de código que muestra la variable resaltada con el método Index ActionResult](search/_static/rename2.png)

Cambie el parámetro por `id` y todas las apariciones de `searchString` se modificarán por `id`.

![Editor de código que muestra que la variable se ha modificado por id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Observe cómo IntelliSense le ayuda a actualizar el marcado.

![Menú contextual de IntelliSense con el atributo method seleccionado en la lista de atributos del elemento de formulario](search/_static/int_m.png)

![Menú contextual de IntelliSense con el método get seleccionado en la lista de valores de atributos de método](search/_static/int_get.png)

Observe que la fuente es diferente en la etiqueta `<form>`. Esta fuente distinta indica que la etiqueta es compatible con las [aplicaciones auxiliares de etiquetas](../../mvc/views/tag-helpers/intro.md).

![etiqueta form con el texto de color morado](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Anterior](controller-methods-views.md)
[Siguiente](new-field.md)  
