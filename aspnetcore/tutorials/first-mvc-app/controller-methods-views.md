---
title: "Vistas y métodos de controlador"
author: rick-anderson
description: "Trabajar con los métodos de controlador, vistas y DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
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
