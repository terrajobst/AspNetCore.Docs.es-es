---
title: "Vistas y métodos de controlador"
author: rick-anderson
description: "Trabajar con los métodos de controlador, vistas y DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: b72fb5077d3bd577cd6282e7a6231a0806ee1fd2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="a255f-104">Vistas y métodos de controlador</span><span class="sxs-lookup"><span data-stu-id="a255f-104">Controller methods and views</span></span>

<span data-ttu-id="a255f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a255f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a255f-106">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="a255f-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="a255f-107">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.</span><span class="sxs-lookup"><span data-stu-id="a255f-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="a255f-109">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="a255f-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="a255f-110">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a255f-110">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="a255f-111">[Anterior: Trabajar con SQLite](working-with-sql.md)
[Siguiente: Agregar búsqueda](search.md)</span><span class="sxs-lookup"><span data-stu-id="a255f-111">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
