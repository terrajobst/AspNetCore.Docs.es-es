---
title: "Vistas y métodos de controlador"
author: rick-anderson
description: "Trabajar con los métodos de controlador, vistas y DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 34bd73af9bd0e4a7c1e59b491105f959bcbc06c6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="94d21-103">Vistas y métodos de controlador</span><span class="sxs-lookup"><span data-stu-id="94d21-103">Controller methods and views</span></span>

<span data-ttu-id="94d21-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="94d21-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="94d21-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="94d21-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="94d21-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.</span><span class="sxs-lookup"><span data-stu-id="94d21-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="94d21-108">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="94d21-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="94d21-109">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="94d21-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="94d21-110">[Anterior: Trabajar con SQLite](working-with-sql.md)
[Siguiente: Agregar búsqueda](search.md)</span><span class="sxs-lookup"><span data-stu-id="94d21-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
