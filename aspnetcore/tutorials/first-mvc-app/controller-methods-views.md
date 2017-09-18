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
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="4906b-104">Vistas y métodos de controlador</span><span class="sxs-lookup"><span data-stu-id="4906b-104">Controller methods and views</span></span>

<span data-ttu-id="4906b-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4906b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4906b-106">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="4906b-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="4906b-107">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.</span><span class="sxs-lookup"><span data-stu-id="4906b-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="4906b-109">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="4906b-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="4906b-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="4906b-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>

<span data-ttu-id="4906b-111">Haga clic con el botón derecho en una línea ondulada roja **> Acciones rápidas y refactorizaciones**.</span><span class="sxs-lookup"><span data-stu-id="4906b-111">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="4906b-113">Pulse `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="4906b-113">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](controller-methods-views/_static/da.png)

  <span data-ttu-id="4906b-115">Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="4906b-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="4906b-116">Vamos a quitar las instrucciones `using` que no son necesarias.</span><span class="sxs-lookup"><span data-stu-id="4906b-116">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="4906b-117">Se muestran de forma predeterminada con una fuente de color gris claro.</span><span class="sxs-lookup"><span data-stu-id="4906b-117">They show up by default in a light grey font.</span></span> <span data-ttu-id="4906b-118">Haga clic con el botón derecho en cualquier lugar del archivo *Movie.cs* **> Eliminar y ordenar instrucciones Using**.</span><span class="sxs-lookup"><span data-stu-id="4906b-118">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Eliminar y ordenar instrucciones Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="4906b-120">El código actualizado:</span><span class="sxs-lookup"><span data-stu-id="4906b-120">The updated code:</span></span>

<span data-ttu-id="4906b-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="4906b-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span></span>

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="4906b-122">[Anterior](working-with-sql.md)
[Siguiente](search.md)</span><span class="sxs-lookup"><span data-stu-id="4906b-122">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
