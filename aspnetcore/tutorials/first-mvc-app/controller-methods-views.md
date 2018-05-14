---
title: Vistas y métodos de controlador en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo trabajar con métodos de controlador, vistas y DataAnnotations en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Vistas y métodos de controlador en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación de películas pinta bien, pero la presentación no es ideal. No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](working-with-sql/_static/m55.png)

Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Haga clic con el botón derecho en una línea ondulada roja **> Acciones rápidas y refactorizaciones**.

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](controller-methods-views/_static/qa.png)


Pulse `using System.ComponentModel.DataAnnotations;`.

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](controller-methods-views/_static/da.png)

  Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.

Vamos a quitar las instrucciones `using` que no son necesarias. Se muestran de forma predeterminada con una fuente de color gris claro. Haga clic con el botón derecho en cualquier lugar del archivo *Movie.cs* **> Eliminar y ordenar instrucciones Using**.

![Eliminar y ordenar instrucciones Using](controller-methods-views/_static/rm.png)

El código actualizado:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Anterior](working-with-sql.md)
> [Siguiente](search.md)  
