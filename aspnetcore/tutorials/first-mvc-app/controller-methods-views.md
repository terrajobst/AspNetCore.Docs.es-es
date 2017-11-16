---
title: "Vistas y métodos de controlador"
author: rick-anderson
description: "Trabajar con los métodos de controlador, vistas y DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a>Vistas y métodos de controlador

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación de películas pinta bien, pero la presentación no es ideal. No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](working-with-sql/_static/m55.png)

Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Haga clic con el botón derecho en una línea ondulada roja **> Acciones rápidas y refactorizaciones**.

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](controller-methods-views/_static/qa.png)


Pulse `using System.ComponentModel.DataAnnotations;`.

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](controller-methods-views/_static/da.png)

  Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.

Vamos a quitar las instrucciones `using` que no son necesarias. Se muestran de forma predeterminada con una fuente de color gris claro. Haga clic con el botón derecho en cualquier lugar del archivo *Movie.cs* **> Eliminar y ordenar instrucciones Using**.

![Eliminar y ordenar instrucciones Using](controller-methods-views/_static/rm.png)

El código actualizado:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Anterior](working-with-sql.md)
[Siguiente](search.md)  
