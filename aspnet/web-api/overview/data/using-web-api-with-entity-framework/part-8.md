---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Mostrar detalles del elemento | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 83f291326307a4964afdd5e8b50f2c375348ed0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838135"
---
<a name="display-item-details"></a>Mostrar detalles del elemento
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de ver los detalles de cada libro. En app.js, agregue el código siguiente en el modelo de vista:

[!code-javascript[Main](part-8/samples/sample1.js)]

En Views/Home/Index.cshtml, agregue un elemento de enlace de datos para el vínculo de detalles:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Esto enlaza el controlador de clics para el &lt;un&gt; elemento a la `getBookDetail` función en el modelo de vista.

En el mismo archivo, reemplace la marca de seguridad siguiente:

[!code-html[Main](part-8/samples/sample3.html)]

por este:

[!code-html[Main](part-8/samples/sample4.html)]

Este marcado crea una tabla que está enlazado a datos a las propiedades de la `detail` perceptible en el modelo de vista.

El "&lt;!--ko--&gt; &quot; sintaxis permite incluir un enlace de Knockout fuera de un elemento de DOM. En este caso, el `if` enlace hace que esta sección de marcado que se mostrará solo cuando `details` es distinto de null.

[!code-html[Main](part-8/samples/sample5.html)]

Ahora, si ejecuta la aplicación y haga clic en uno de los &quot;detalle&quot; vínculos, la aplicación mostrará los detalles del libro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](part-7.md)
> [Siguiente](part-9.md)
