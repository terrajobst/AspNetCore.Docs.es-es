---
title: "Vistas y métodos de controlador"
author: rick-anderson
description: "Trabajar con los métodos de controlador, vistas y DataAnnotations"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e279b6c2852f1aea49685381ccaa5f7854f7c418
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="80101-103">Vistas y métodos de controlador</span><span class="sxs-lookup"><span data-stu-id="80101-103">Controller methods and views</span></span>

<span data-ttu-id="80101-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="80101-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="80101-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="80101-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="80101-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.</span><span class="sxs-lookup"><span data-stu-id="80101-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="80101-108">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="80101-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="80101-109">Haga clic con el botón derecho en una línea ondulada roja **> Acciones rápidas y refactorizaciones**.</span><span class="sxs-lookup"><span data-stu-id="80101-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="80101-111">Pulse `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="80101-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](controller-methods-views/_static/da.png)

  <span data-ttu-id="80101-113">Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="80101-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="80101-114">Vamos a quitar las instrucciones `using` que no son necesarias.</span><span class="sxs-lookup"><span data-stu-id="80101-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="80101-115">Se muestran de forma predeterminada con una fuente de color gris claro.</span><span class="sxs-lookup"><span data-stu-id="80101-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="80101-116">Haga clic con el botón derecho en cualquier lugar del archivo *Movie.cs* **> Eliminar y ordenar instrucciones Using**.</span><span class="sxs-lookup"><span data-stu-id="80101-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Eliminar y ordenar instrucciones Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="80101-118">El código actualizado:</span><span class="sxs-lookup"><span data-stu-id="80101-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="80101-119">[Anterior](working-with-sql.md)
[Siguiente](search.md)</span><span class="sxs-lookup"><span data-stu-id="80101-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
