---
title: Actualizar las páginas generadas en una aplicación ASP.NET Core
author: rick-anderson
description: Sepa cómo actualizar las páginas generadas en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 78490be1cfa3018c465cb1e8125918404a7e4525
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011617"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Actualizar las páginas generadas en una aplicación ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación de películas pinta bien, pero la presentación no es ideal. No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Actualización del código generado

Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

Haga clic con el botón derecho en una línea ondulada roja > **Acciones rápidas y refactorizaciones**.

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

Seleccione `using System.ComponentModel.DataAnnotations;`

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Adición de búsqueda](xref:tutorials/razor-pages/search)
