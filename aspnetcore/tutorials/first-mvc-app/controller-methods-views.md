---
title: Vistas y métodos de controlador en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo trabajar con métodos de controlador, vistas y DataAnnotations en ASP.NET Core.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 42a63044cd14873ff334a728c6c8304214ee8575
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011344"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="28fc3-103">Vistas y métodos de controlador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28fc3-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="28fc3-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="28fc3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="28fc3-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="28fc3-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="28fc3-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.</span><span class="sxs-lookup"><span data-stu-id="28fc3-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="28fc3-108">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="28fc3-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="28fc3-109">[Anterior](working-with-sql.md)
> [Siguiente](search.md)</span><span class="sxs-lookup"><span data-stu-id="28fc3-109">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
