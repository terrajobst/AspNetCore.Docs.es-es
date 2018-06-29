---
title: Actualizar las páginas generadas en una aplicación ASP.NET Core
author: rick-anderson
description: Sepa cómo actualizar las páginas generadas en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278075"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="3a9db-103">Actualizar las páginas generadas en una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a9db-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="3a9db-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3a9db-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3a9db-105">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="3a9db-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="3a9db-106">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).</span><span class="sxs-lookup"><span data-stu-id="3a9db-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="3a9db-108">Actualización del código generado</span><span class="sxs-lookup"><span data-stu-id="3a9db-108">Update the generated code</span></span>

<span data-ttu-id="3a9db-109">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a9db-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="3a9db-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="3a9db-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3a9db-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span><span class="sxs-lookup"><span data-stu-id="3a9db-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span></span>
::: moniker-end

<span data-ttu-id="3a9db-112">Haga clic con el botón derecho en una línea ondulada roja > **Acciones rápidas y refactorizaciones**.</span><span class="sxs-lookup"><span data-stu-id="3a9db-112">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

<span data-ttu-id="3a9db-114">Seleccione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="3a9db-114">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  <span data-ttu-id="3a9db-116">Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="3a9db-116">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="3a9db-117">[Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Adición de búsqueda](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="3a9db-117">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
