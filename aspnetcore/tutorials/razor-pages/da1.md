---
title: Actualizar las páginas generadas en una aplicación ASP.NET Core
author: rick-anderson
description: Sepa cómo actualizar las páginas generadas en una aplicación ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 5c188799b7a42bcd5e9d5eab8dfe8cdad8002fe5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Actualizar las páginas generadas en una aplicación ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación de películas pinta bien, pero la presentación no es ideal. No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Actualización del código generado

Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Haga clic con el botón derecho en una línea ondulada roja > ** Acciones rápidas y refactorizaciones**.

  ![En el menú contextual se muestra **> Acciones rápidas y refactorizaciones**.](da1/qa.png)

Seleccione `using System.ComponentModel.DataAnnotations;`

  ![uso de System.ComponentModel.DataAnnotations en la parte superior de la lista](da1/da.png)

  Visual Studio agrega `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Anterior: Trabajar con SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Adición de búsqueda](xref:tutorials/razor-pages/search)
