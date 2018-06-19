---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Mostrar detalles de los elementos | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868092"
---
<a name="display-item-details"></a>Mostrar detalles del elemento
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de ver los detalles de cada libro. En app.js, agregue el código siguiente en el modelo de vista:

[!code-javascript[Main](part-8/samples/sample1.js)]

En Views/Home/Index.cshtml, agregue un elemento de enlace de datos para el vínculo de detalles:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Esto enlaza el controlador de clic de los &lt;una&gt; elemento a la `getBookDetail` función en el modelo de vista.

En el mismo archivo, reemplace el marcado siguiente:

[!code-html[Main](part-8/samples/sample3.html)]

por este:

[!code-html[Main](part-8/samples/sample4.html)]

Este marcado crea una tabla que está enlazado a datos a las propiedades de la `detail` observable en el modelo de vista.

El "&lt;!--ko--&gt; &quot; sintaxis permite incluir un enlace Knockout fuera de un elemento DOM. En este caso, el `if` enlace hace que esta sección de marcado que se mostrará solo cuando `details` es distinto de null.

[!code-html[Main](part-8/samples/sample5.html)]

Ahora, si ejecuta la aplicación y haga clic en uno de los &quot;detalle&quot; vínculos, la aplicación mostrará los detalles del libro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](part-7.md)
> [Siguiente](part-9.md)
