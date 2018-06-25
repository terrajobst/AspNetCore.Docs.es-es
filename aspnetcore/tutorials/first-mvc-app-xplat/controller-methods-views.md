---
title: Vistas y métodos de controlador en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo trabajar con métodos de controlador, vistas y DataAnnotations en ASP.NET Core.
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 7cf42807eaba356cd090a08bba9357c3ec237087
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278445"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="cbf50-103">Vistas y métodos de controlador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbf50-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="cbf50-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cbf50-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cbf50-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="cbf50-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="cbf50-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.</span><span class="sxs-lookup"><span data-stu-id="cbf50-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="cbf50-108">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="cbf50-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="cbf50-109">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cbf50-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="cbf50-110">[Anterior: Trabajar con SQLite](working-with-sql.md)
> [Siguiente: Agregar búsqueda](search.md)</span><span class="sxs-lookup"><span data-stu-id="cbf50-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
