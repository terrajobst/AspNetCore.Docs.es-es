---
title: "Actualización de las páginas generadas"
author: rick-anderson
description: "Actualice las páginas generadas con una mejor visualización."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="7d5c0-103">Actualización de las páginas generadas</span><span class="sxs-lookup"><span data-stu-id="7d5c0-103">Update the generated pages</span></span>

<span data-ttu-id="7d5c0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d5c0-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="7d5c0-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).</span><span class="sxs-lookup"><span data-stu-id="7d5c0-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="7d5c0-108">Actualización del código generado</span><span class="sxs-lookup"><span data-stu-id="7d5c0-108">Update the generated code</span></span>

<span data-ttu-id="7d5c0-109">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d5c0-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="7d5c0-110">Haga clic con el botón derecho en una línea ondulada roja > ** Acciones rápidas y refactorizaciones**.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

<span data-ttu-id="7d5c0-112">Seleccione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="7d5c0-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  <span data-ttu-id="7d5c0-114">Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
<span data-ttu-id="7d5c0-115">[Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adición de búsqueda](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
