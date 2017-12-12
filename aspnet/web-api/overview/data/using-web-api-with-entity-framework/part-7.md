---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Crear la vista (UI) | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>Crear la vista (UI)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, se iniciará definir el código HTML de la aplicación y Agregar enlace de datos entre el código HTML y el modelo de vista.

Abra el archivo Views/Home/Index.cshtml. Reemplace todo el contenido de ese archivo con la siguiente.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La mayoría de los `div` elementos existen para [arranque](http://getbootstrap.com/) aplicación de estilos. Los elementos importantes son aquellas con `data-bind` atributos. Este atributo vincula el código HTML para el modelo de vista.

Por ejemplo:

[!code-html[Main](part-7/samples/sample2.html)]

En este ejemplo, el &quot; `text` &quot; enlace causas el `<p>` elemento que se va a mostrar el valor de la `error` propiedad desde el modelo de vista. Recuerde que `error` se ha declarado como un `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Cada vez que se asigna un nuevo valor a `error`, Knockout actualiza el texto en el `<p>` elemento.

El `foreach` enlace indica Knockout recorrer el contenido de la `books` matriz. Para cada elemento de la matriz, crea un nuevo Knockout &lt;li&gt; elemento. Enlaces dentro del contexto de la `foreach` hacer referencia a propiedades en el elemento de matriz. Por ejemplo:

[!code-html[Main](part-7/samples/sample4.html)]

Aquí el `text` enlace lee la propiedad del autor de cada libro.

Si se ejecuta la aplicación ahora, debería ser similar al siguiente:

![](part-7/_static/image1.png)

La lista de libros carga de forma asincrónica, después de la página se cargue. Ahora, el &quot;detalles&quot; vínculos no son funcionales. Vamos a agregar esta funcionalidad en la sección siguiente.

>[!div class="step-by-step"]
[Anterior](part-6.md)
[Siguiente](part-8.md)
