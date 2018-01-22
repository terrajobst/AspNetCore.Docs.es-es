---
title: "Agregar una búsqueda"
author: rick-anderson
description: "Se muestra cómo agregar la búsqueda a una aplicación sencilla de ASP.NET Core MVC"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 2d8a18365a0d46d6468d708e1cd02def071309b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="0ff6e-103">Nota: SQLlite distingue mayúsculas de minúsculas, por lo que tendrá que buscar "Ghost" y no "ghost".</span><span class="sxs-lookup"><span data-stu-id="0ff6e-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="0ff6e-104">Cambie la etiqueta `<form>` en la vista de Razor *Views\movie\Index.cshtml* para especificar `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="0ff6e-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="0ff6e-105">[Anterior: Vistas y métodos de controlador](controller-methods-views.md)
[Siguiente: Agregar un campo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="0ff6e-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
