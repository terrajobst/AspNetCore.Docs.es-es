---
title: Agregar búsqueda a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Se muestra cómo agregar la búsqueda a una aplicación sencilla de ASP.NET Core MVC
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: aca0835340977605cc84fad1970ac30fa1a9872a
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961547"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Nota: SQLlite distingue mayúsculas de minúsculas, por lo que tendrá que buscar "Ghost" y no "ghost".

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Cambie la etiqueta `<form>` en la vista de Razor *Views\movie\Index.cshtml* para especificar `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Anterior: Vistas y métodos de controlador](controller-methods-views.md)
> [Siguiente: Agregar un campo](new-field.md)
