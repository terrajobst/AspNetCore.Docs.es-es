---
title: Actualizar las páginas generadas en una aplicación ASP.NET Core
author: rick-anderson
description: Sepa cómo actualizar las páginas generadas en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 7633c0a40764cc18a656f0497e3280e4067cb59f
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045580"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="da9aa-103">Actualizar las páginas generadas en una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="da9aa-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="da9aa-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="da9aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="da9aa-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="da9aa-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="da9aa-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).</span><span class="sxs-lookup"><span data-stu-id="da9aa-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="da9aa-108">Actualización del código generado</span><span class="sxs-lookup"><span data-stu-id="da9aa-108">Update the generated code</span></span>

<span data-ttu-id="da9aa-109">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="da9aa-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="da9aa-110">Haga clic con el botón derecho en una línea ondulada roja > **Acciones rápidas y refactorizaciones**.</span><span class="sxs-lookup"><span data-stu-id="da9aa-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

<span data-ttu-id="da9aa-112">Seleccione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="da9aa-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  <span data-ttu-id="da9aa-114">Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="da9aa-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="da9aa-115">[Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Adición de búsqueda](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="da9aa-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
